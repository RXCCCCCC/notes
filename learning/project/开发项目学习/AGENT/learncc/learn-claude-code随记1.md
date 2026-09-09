# S01

## 工作原理

1. 用户 **prompt 作为第一条消息。**

```python
messages.append({"role": "user", "content": query})
```

1. 将**消息和工具定义一起发**给 LLM。

```python
response = client.messages.create(
    model=MODEL, system=SYSTEM, messages=messages,
    tools=TOOLS, max_tokens=8000,
)
```

1. 追加**助手响应**。检查 `stop_reason` -- 如果**模型没有调用工具, 结束。**

```python
messages.append({"role": "assistant", "content": response.content})
if response.stop_reason != "tool_use":
    return
```

1. 执行**每个工具调用, 收集结果**, 作为 user 消息追加。回到第 2 步。

```python
results = []
for block in response.content:
    if block.type == "tool_use":
        output = run_bash(block.input["command"])
        results.append({
            "type": "tool_result",
            "tool_use_id": block.id,
            "content": output,
        })
messages.append({"role": "user", "content": results})
```

组装为一个完整函数:

```python
def agent_loop(query):
    messages = [{"role": "user", "content": query}]
    while True:
        response = client.messages.create(
            model=MODEL, system=SYSTEM, messages=messages,
            tools=TOOLS, max_tokens=8000,
        )
        messages.append({"role": "assistant", "content": response.content})

        if response.stop_reason != "tool_use":
            return

        results = []
        for block in response.content:
            if block.type == "tool_use":
                output = run_bash(block.input["command"])
                results.append({
                    "type": "tool_result",
                    "tool_use_id": block.id,
                    "content": output,
                })
        messages.append({"role": "user", "content": results})
```

不到 30 行, 这就是整个智能体。后面 11 个章节都在这个循环上叠加机制 -- 循环本身始终不变。

## 变更内容

| 组件         | 之前 | 之后                        |
| :----------- | :--- | :-------------------------- |
| Agent loop   | (无) | `while True` + stop_reason  |
| Tools        | (无) | `bash` (单一工具)           |
| Messages     | (无) | **累积式**消息列表          |
| Control flow | (无) | `stop_reason != "tool_use"` |

## QA

### 为什么只要bash

Bash **能读写文件、运行任意程序、在进程间传递数据、管理文件系统**。任何额外的工具（read_file、write_file 等）都只是 bash **已有能力的子集**。增加工具**并不会解锁新能力，只会增加模型需要理解的接口**。

### 子代理机制

当 agent 执行 `python v0.py "subtask"` 时，它会**创建一个全新的进程，拥有全新的 LLM 上下文**。这个子进程实际上就是一个子代理：有**自己的系统提示词、对话历史和任务焦点**。子进程完成后，**父进程通过 stdout 获取结果**。这就是**不依赖任何框架的子代理委派——纯粹的 Unix 进程语义**。每个子进程**天然隔离关注点，因为它根本看不到父进程的上下文。**

### 模型决定框架

没有规划器，没有任务队列，没有状态机。**系统提示词告诉模型如何处理问题，模型根据对话历史决定下一步执行什么 bash 命令。**这是有意为之的：在这个层级，添加规划层属于过早抽象。**模型的思维链本身就是计划。agent 循环只是不断询问模型下一步做什么，直到模型不再请求工具为止。**

# S02

循环不用动, **新工具注册进 dispatch map** 就行。

The Dispatch Map

A **dictionary** maps tool names **to handler functions**. The loop code never changes.

## 问题

只有 `bash` 时, 所有操作**都走 shell**。`cat` 截断不可预测, `sed` 遇到特殊字符就崩, 每次 bash 调用**都是不受约束的安全面**。专用工具 (`read_file`, `write_file`) 可以**在工具层面做路径沙箱。**

关键洞察: 加工具不需要改循环。

## 解决方案

```
+--------+      +-------+      +------------------+
|  User  | ---> |  LLM  | ---> | Tool Dispatch    |
| prompt |      |       |      | {                |
+--------+      +---+---+      |   bash: run_bash |
                    ^           |   read: run_read |
                    |           |   write: run_wr  |
                    +-----------+   edit: run_edit |
                    tool_result | }                |
                                +------------------+

The dispatch map is a dict: {tool_name: handler_function}.
One lookup replaces any if/elif chain.
```

## 工作原理

1. 每个工具有一个处理函数。路径沙箱防止逃逸工作区。

```python
def safe_path(p: str) -> Path:
    path = (WORKDIR / p).resolve()
    if not path.is_relative_to(WORKDIR):
        raise ValueError(f"Path escapes workspace: {p}")
    return path

def run_read(path: str, limit: int = None) -> str:
    text = safe_path(path).read_text()
    lines = text.splitlines()
    if limit and limit < len(lines):
        lines = lines[:limit]
    return "\n".join(lines)[:50000]
```

1. dispatch map **将工具名映射到处理函数**。

```python
TOOL_HANDLERS = {
    "bash":       lambda **kw: run_bash(kw["command"]),
    "read_file":  lambda **kw: run_read(kw["path"], kw.get("limit")),
    "write_file": lambda **kw: run_write(kw["path"], kw["content"]),
    "edit_file":  lambda **kw: run_edit(kw["path"], kw["old_text"],
                                        kw["new_text"]),
}
```

1. 循环中按**名称查找处理函数**。循环体本身与 s01 完全一致。

```python
for block in response.content:
    if block.type == "tool_use":
        handler = TOOL_HANDLERS.get(block.name)
        output = handler(**block.input) if handler \
            else f"Unknown tool: {block.name}"
        results.append({
            "type": "tool_result",
            "tool_use_id": block.id,
            "content": output,
        })
```

**加工具 = 加 handler + 加 schema。**循环永远不变。

## 相对 s01 的变更

| 组件       | 之前 (s01)           | 之后 (s02)                      |
| :--------- | :------------------- | :------------------------------ |
| Tools      | 1 (仅 bash)          | **4 (bash, read, write, edit)** |
| Dispatch   | **硬编码 bash 调用** | **`TOOL_HANDLERS` 字典**        |
| 路径安全   | 无                   | **`safe_path()` 沙箱**          |
| Agent loop | 不变                 | 不变                            |

## QA

### 模型即代理

核心 agent 循环极其简单：**不断调用 LLM，如果返回 tool_use 块就执行并回传结果，如果只返回文本就停止**。**没有路由器，没有决策树，没有工作流引擎**。模型**自己决定做什么、何时停止、如何从错误中恢复**。代码只是**连接模型和工具的管道**。这是一种设计哲学：agent 行为**从模型中涌现，而非由框架定义**。

### JSON schema

每个工具都为输入参数定义了**严格的 JSON schema**。例如，edit_file **要求 old_string 和 new_string 是精确的字符串，而非正则表达式。**这消除了一整类错误：**模型无法传递格式错误的输入，因为 API 会在执行前校验 schema**。这也使模型的意图变得明确——当它用特定字符串调用 edit_file 时，不存在关于它想修改什么的解析歧义。

# **s03** TodoWrite

## The Plan

TodoWrite gives the model a visible plan. All tasks start as pending.

## 问题

多步任务中, 模型会**丢失进度** -- 重复做过的事、跳步、跑偏。对话越长越严重: 工具**结果不断填满上下文**, **系统提示的影响力逐渐被稀释**。一个 10 步重构**可能做完 1-3 步就开始即兴发挥, 因为 4-10 步已经被挤出注意力了**。

## 解决方案

```
+--------+      +-------+      +---------+
|  User  | ---> |  LLM  | ---> | Tools   |
| prompt |      |       |      | + todo  |
+--------+      +---+---+      +----+----+
                    ^                |
                    |   tool_result  |
                    +----------------+
                          |
              +-----------+-----------+
              | TodoManager state     |
              | [ ] task A            |
              | [>] task B  <- doing  |
              | [x] task C            |
              +-----------------------+
                          |
              if rounds_since_todo >= 3:
                inject <reminder> into tool_result
```

## 工作原理

1. TodoManager **存储带状态的项目**。**同一时间只允许一个 `in_progress`。**

```python
class TodoManager:
    def update(self, items: list) -> str:
        validated, in_progress_count = [], 0
        for item in items:
            status = item.get("status", "pending")
            if status == "in_progress":
                in_progress_count += 1
            validated.append({"id": item["id"], "text": item["text"],
                              "status": status})
        if in_progress_count > 1:
            raise ValueError("Only one task can be in_progress")
        self.items = validated
        return self.render()
```

1. `todo` 工具和其他工具一样**加入 dispatch map。**

```python
TOOL_HANDLERS = {
    # ...base tools...
    "todo": lambda **kw: TODO.update(kw["items"]),
}
```

1. nag reminder: 模型**连续 3 轮以上不调用 `todo` 时注入提醒。**

```python
if rounds_since_todo >= 3 and messages:
    last = messages[-1]
    if last["role"] == "user" and isinstance(last.get("content"), list):
        last["content"].insert(0, {
            "type": "text",
            "text": "<reminder>Update your todos.</reminder>",
        })
```

**"同时只能有一个 in_progress" 强制顺序聚焦。**nag reminder 制造问责压力 -- 你**不更新计划, 系统就追着你问。**

## 相对 s02 的变更

| 组件       | 之前 (s02) | 之后 (s03)                     |
| :--------- | :--------- | :----------------------------- |
| Tools      | 4          | 5 (+todo)                      |
| 规划       | 无         | **带状态的 TodoManager**       |
| Nag 注入   | 无         | **3 轮后注入 `<reminder>`**    |
| Agent loop | 简单分发   | **+ rounds_since_todo 计数器** |

## QA

### 这一行为什么给get传入了两个参数

```py
status = str(item.get("status", "pending")).lower()
```

因为 `item` 是个字典，`dict.get(key, default)` 接受两个参数：**要取的键和键不存在时的默认值**。这里是取 `item["status"]`，**如果没有就用 `"pending"`**

### Todo让计划可见

我们**不让模型在思维链中默默规划**，而是**强制通过 TodoWrite 工具将计划外化**。每个计划项都有可追踪的状态（pending、in_progress、completed）。这有三个好处：(1) **用户可以在执行前看到 agent 打算做什么**；(2) 开发者**可以通过检查计划状态来调试 agent 行为**；(3) agent 自身**可以在后续轮次中引用计划，即使早期上下文已经滚出窗口。**

### 为什么一次只执行一个任务

TodoWrite 工具强制要求任何时候最多只能有一个任务处于 in_progress 状态。如果模型想开始第二个任务，**必须先完成或放弃当前任务**。这个约束防止了一种隐蔽的失败模式：**试图通过交替处理多个项目来'多任务'的模型，往往会丢失状态并产出半成品**。**顺序执行的专注度远高于并行切换。**

Allowing multiple in-progress items would let the agent **context-switch between tasks**

In practice, LLMs handle context-switching poorly -- they lose track of which task they were working on and mix up details between tasks. 

### 为什么TODO上限20

TodoWrite 将计划项限制在 20 条以内。这是对过度规划的刻意约束。不加限制时，模型倾向于将任务分解成越来越细粒度的步骤，**产出 50 条的计划，每一步都微不足道。冗长的计划很脆弱**：如果第 15 步失败，剩下的 35 步可能全部作废。20 条以内的短计划**保持在正确的抽象层级，更容易在现实偏离计划时做出调整。**

# S04 子 Agent

## 子 Agent 上下文隔离

### Clean Context

The parent gets **a clean summary without context bloat.** This is **fresh-context isolation via messages[].**

*"**大任务拆小, 每个小任务干净的上下文**"* -- 子智能体用独立 messages[], 不污染主对话。

## 问题

智能体工作越久, messages 数组越胖。每次读文件、跑命令的输出都永久留在上下文里。"这个项目用什么测试框架?" 可能要读 5 个文件, 但**父智能体只需要一个词: "pytest。"**

## 解决方案

```
Parent agent                     Subagent
+------------------+             +------------------+
| messages=[...]   |             | messages=[]      | <-- fresh
|                  |  dispatch   |                  |
| tool: task       | ----------> | while tool_use:  |
|   prompt="..."   |             |   call tools     |
|                  |  summary    |   append results |
|   result = "..." | <---------- | return last text |
+------------------+             +------------------+

Parent context stays clean. Subagent context is discarded.
```

## 工作原理

1. 父智能体有**一个 `task` 工具**。子智能体拥有**除 `task` 外的所有基础工具 (禁止递归生成)。**

```python
PARENT_TOOLS = CHILD_TOOLS + [
    {"name": "task",
     "description": "Spawn a subagent with fresh context.",
     "input_schema": {
         "type": "object",
         "properties": {"prompt": {"type": "string"}},
         "required": ["prompt"],
     }},
]
```

1. 子智能体以 `messages=[]` 启动, **运行自己的循环**。**只有最终文本返回**给父智能体。

```python
def run_subagent(prompt: str) -> str:
    # 1. 初始化上下文：将用户的初始指令包装为标准的消息列表格式
    sub_messages = [{"role": "user", "content": prompt}]
    
    # 2. 安全截断机制：限制最大循环次数为 30。
    # 在工程实践中这是必须的，防止模型陷入“调用报错 -> 重试 -> 再报错”的死循环，导致 API Token 费用失控。
    for _ in range(30):  
        
        # 3. 发送网络请求给大语言模型 (LLM)
        response = client.messages.create(
            model=MODEL, 
            system=SUBAGENT_SYSTEM, # System Prompt: 定义这个子智能体的职责和行为边界
            messages=sub_messages,  # 传入完整的历史上下文（包含之前的思考和工具返回结果）
            tools=CHILD_TOOLS,      # 关键参数：告诉 LLM 它当前可以调用哪些外部工具（需符合 API 要求的 JSON Schema）
            max_tokens=8000,
        )
        
        # 4. 保存 LLM 的响应：原封不动地将输出存入上下文。
        # 这里的 content 可能是一段普通思考文本，也可能是一个或多个“工具调用指令”。
        sub_messages.append({
            "role": "assistant",
            "content": response.content
        })
        
        # 5. 退出条件判定：stop_reason 是 API 返回的一个标志位。
        # 如果它不是 "tool_use" (要求使用工具)，说明 LLM 认为任务已经完成，或者无计可施了，此时直接跳出循环。
        if response.stop_reason != "tool_use":
            break
            
        # 6. 本地工具执行阶段：走到这里，说明 LLM 决定调用工具了。
        results = []
        for block in response.content:
            # 遍历 LLM 返回的内容块，寻找工具调用指令（因为 LLM 支持并发调用多个工具）
            if block.type == "tool_use":
                
                # 从我们本地维护的字典 (TOOL_HANDLERS) 中，根据 LLM 提供的工具名拿到实际的 Python 函数句柄
                handler = TOOL_HANDLERS.get(block.name)
                
                # 动态参数解包：block.input 是 LLM 生成的 JSON 参数。
                # ** 将其解包为 Python 关键字参数并传递给该函数执行。
                output = handler(**block.input)
                
                # 7. 封装执行结果：按 Anthropic API 规范格式化工具返回的数据
                results.append({
                    "type": "tool_result",
                    "tool_use_id": block.id, # 必须字段：将结果与 LLM 的请求 ID 绑定，不然 LLM 不知道这是哪个工具的结果
                    # 工程鲁棒性处理：硬截断输出结果至 50000 字符。
                    # 防止工具（比如读取了一个极大的 Log 文件）返回数据过多，直接把上下文窗口撑爆。
                    "content": str(output)[:50000] 
                })
                
        # 8. 回传结果给 LLM：将所有工具的执行结果打包，作为 "user" 角色追加到消息历史中。
        # 随后 for 循环进入下一次迭代，LLM 会读取这些结果并继续思考下一步。
        sub_messages.append({"role": "user", "content": results})
        
    # 9. 提取最终输出：循环结束，遍历最后一次 API 响应的内容块。
    # 提取其中类型为文本 (text) 的内容并拼接。如果模型什么都没说，则返回降级文案 "(no summary)"。
    return "".join(
        b.text for b in response.content if hasattr(b, "text")
    ) or "(no summary)"
```

子智能体可能跑了 30+ 次工具调用, 但**整个消息历史直接丢弃。父智能体收到的只是一段摘要文本, 作为普通 `tool_result` 返回。**

这段代码完美实现了经典的 **ReAct (Reasoning and Acting)** 范式，整个工作流如下：

1. **输入阶段：** 传入用户的任务（Prompt）。
2. **思考与决策阶段：** 模型分析任务，判断自己**是否需要使用外部工具**（比如搜索网页、读取文件、执行计算）。
3. **行动阶段（如果需要工具）：** 模型**返回 `tool_use` 指令。代码拦截这个指令**，在本地执行对应的 Python 函数（`handler(**block.input)`）。
4. **观察阶段：** 代码将函数的**执行结果返回给模型。模型根据结果决定是需要继续调用其他工具（回到第 2 步），还是已经可以得出最终结论。**
5. **输出阶段：** 模型停止调用工具，输出最终的文本答案。

## 相对 s03 的变更

| 组件     | 之前 (s03) | 之后 (s04)                |
| :------- | :--------- | :------------------------ |
| Tools    | 5          | 5 (基础) + task (仅父端)  |
| 上下文   | 单一共享   | 父 + 子隔离               |
| Subagent | 无         | **`run_subagent()` 函数** |
| 返回值   | 不适用     | 仅摘要文本                |

## 子代理全新上下文

当父代理通过 Task 工具创建子代理时，子代理从全新的消息历史开始，只包含系统提示词和委派的任务描述，不继承父代理的对话。这就是**上下文隔离**：子代理可以完全**专注于特定子任务**，不会被父代理长达数百条消息的对话干扰。结果作为单条 tool_result 返回给父代理，将子代理可能数十轮的交互压缩为一个简洁的回答。

#### 替代方案

Sharing the parent's full context would give the subagent more information, but it would also flood the subagent with irrelevant details. Context window is finite -- filling it with parent history leaves less room for the subagent's own work. **Fork-based** approaches (**copy the parent context**) are a middle ground but still waste tokens on irrelevant history.

## Explore代理不写入文件

创建 Explore 类型的子代理时，它只获得只读工具：bash（有限制）、read_file 和搜索工具，不能调用 write_file 或 edit_file。这实现了最小权限原则：一个被委派'查找函数 X 所有使用位置'的代理不需要写权限。移除写工具消除了探索过程中误修改文件的风险，同时缩小了工具空间，让模型在更少的选项中做出更好的决策。

Giving all subagents full tool access is simpler to implement but violates least privilege. A permission-request system (subagent asks parent for write access) adds complexity and latency. Static tool filtering by role is the pragmatic middle ground -- simple to implement, effective at preventing accidents.

## 子代理不能再创建子代理

Task 工具不包含在子代理的工具集中。子代理必须直接完成工作，不能继续委派。这**防止了无限委派循环**：没有这个约束，**一个代理可能创建子代理，子代理又创建子代理，每一层都用略微不同的措辞重新委派同一任务，消耗 token 却毫无进展**。**一层委派足以处理**绝大多数场景。如果任务**对单个子代理来说太复杂，应该由父代理重新分解。**

#### 替代方案

Allowing recursive delegation (bounded by depth) would handle deeply nested tasks but adds complexity and the risk of runaway token consumption. In practice, single-level delegation covers most real-world coding tasks. Multi-level delegation is addressed in later versions (v6+) **through persistent team structures instead of recursive spawning.**

# **s05** 技能

*Inject knowledge via tool_result when needed, not upfront in the system prompt*

## 按需技能加载

System Prompt **always present**

```bash
# Available Skills

/commit - Create git commits following repo conventions

/review-pr - Review pull requests for bugs and style

/test - Run and analyze test suites

/deploy - Deploy application to target environment
```

Layer 1: Compact Summaries

All skills are summarized in the system prompt. Compact, always present.

Skill Invocation

The model recognizes a skill invocation and triggers the Skill tool.

Layer 2: Full Injection

**The full skill instructions are injected as a tool_result, not into the system prompt.**

![image-20260310160205420](./learn-claude-code%E9%9A%8F%E8%AE%B01/image-20260310160205420.png)

The Skill tool returns content **as a tool_result message**. The model **sees it in context and follows the instructions.** No system prompt bloat.

In Context Now

The detailed instructions appear as if a tool returned them. The model **follows them precisely.**

![image-20260310160551146](./learn-claude-code%E9%9A%8F%E8%AE%B01/image-20260310160551146.png)

Stack Skills

Multiple skills can be loaded. **Only summaries are permanent;** full content comes and goes.

**LAYER 2**

On demand, ~300-500 tokens each

Two-Layer Architecture

Layer 1: **always present, tiny**. Layer 2: **loaded on demand, detailed. Elegant separation.**

*"用到什么知识, **临时加载**什么知识"* -- **通过 tool_result 注入, 不塞 system prompt**。

## 问题

你希望智能体遵循特定领域的工作流: git 约定、测试模式、代码审查清单。**全塞进系统提示太浪费 -- 10 个技能, 每个 2000 token, 就是 20,000 token, 大部分跟当前任务毫无关系。**

## 解决方案

```
System prompt (Layer 1 -- always present):
+--------------------------------------+
| You are a coding agent.              |
| Skills available:                    |
|   - git: Git workflow helpers        |  ~100 tokens/skill
|   - test: Testing best practices     |
+--------------------------------------+

When model calls load_skill("git"):
+--------------------------------------+
| tool_result (Layer 2 -- on demand):  |
| <skill name="git">                   |
|   Full git workflow instructions...  |  ~2000 tokens
|   Step 1: ...                        |
| </skill>                             |
+--------------------------------------+
```

第一层: **系统提示中放技能名称 (低成本)**。**第二层: tool_result 中按需放完整内容。**

## 工作原理

1. 每个技能是一个目录, 包含 `SKILL.md` 文件和 YAML frontmatter。

```
skills/
  pdf/
    SKILL.md       # ---\n name: pdf\n description: Process PDF files\n ---\n ...
  code-review/
    SKILL.md       # ---\n name: code-review\n description: Review code\n ---\n ...
```

1. **SkillLoader** 递归扫描 `SKILL.md` 文件, 用**目录名作为技能标识。**

```python
class SkillLoader:
    def __init__(self, skills_dir: Path):
        self.skills = {}
        for f in sorted(skills_dir.rglob("SKILL.md")):
            text = f.read_text()
            meta, body = self._parse_frontmatter(text)
            name = meta.get("name", f.parent.name)
            self.skills[name] = {"meta": meta, "body": body}

    def get_descriptions(self) -> str:
        lines = []
        for name, skill in self.skills.items():
            desc = skill["meta"].get("description", "")
            lines.append(f"  - {name}: {desc}")
        return "\n".join(lines)

    def get_content(self, name: str) -> str:
        skill = self.skills.get(name)
        if not skill:
            return f"Error: Unknown skill '{name}'."
        return f"<skill name=\"{name}\">\n{skill['body']}\n</skill>"
```

1. 第一层写入系统提示。**第二层不过是 dispatch map 中的又一个工具。**

```python
SYSTEM = f"""You are a coding agent at {WORKDIR}.
Skills available:
{SKILL_LOADER.get_descriptions()}"""

TOOL_HANDLERS = {
    # ...base tools...
    "load_skill": lambda **kw: SKILL_LOADER.get_content(kw["name"]),
}
```

模型**知道有**哪些技能 (便宜), 需要时**再加载**完整内容 (贵)。

| 组件     | 之前 (s04)      | 之后 (s05)               |
| :------- | :-------------- | :----------------------- |
| Tools    | 5 (基础 + task) | 5 (基础 + load_skill)    |
| 系统提示 | 静态字符串      | **+ 技能描述列表**       |
| 知识库   | 无              | skills/*/SKILL.md 文件   |
| 注入方式 | 无              | 两层 (系统提示 + result) |

![image-20260310164919598](./learn-claude-code%E9%9A%8F%E8%AE%B01/image-20260310164919598.png)

## 技能通过tool_result注入

当 agent 调用 Skill 工具时，**技能内容（SKILL.md 文件）作为 tool_result 在用户消息中返回，而非注入系统提示词**。这是一个**刻意的缓存优化：系统提示词在各轮次间保持静态，API 提供商可以缓存它（Anthropic 的 prompt caching、OpenAI 的 system message caching）**。如果技能内容在系统提示词中，**每次加载新技能都会使缓存失效**。将动态内容放在 tool_result 中，既**保持了昂贵的系统提示词可缓存，又让技能知识进入了上下文。**

#### 替代方案

Injecting skills into the system prompt is **simpler and gives skills higher priority in the model's attention.** But it breaks prompt caching (every skill load creates a new system prompt variant) and **bloats the system prompt** over time as skills accumulate. The tool_result approach keeps things cache-friendly at the cost of slightly lower attention priority.

## YAML frontmatter

每个 SKILL.md 文件有两部分：**YAML frontmatter（名称、描述、globs）**和 markdown 正文（实际指令）。Frontmatter 作为技能注册表的**元数据——当 agent 问'有哪些可用技能'时，展示的就是这些信息。**正文是按需加载的有效负载。这种分离意味着可以列出 100 个技能（每个只读几字节的 frontmatter）而不必加载 100 套完整指令集（每套可能数千 token）。

#### 替代方案

A separate metadata file (skill.yaml + skill.md) would work but doubles the number of files. **Embedding metadata in the markdown (as headings or comments) requires parsing the full file to extract metadata**. Frontmatter is a well-established convention (Jekyll, Hugo, Astro) that **keeps metadata and content co-located but separately parseable.**

# **s06**上下文压缩

## **tool_results are the largest blocks**

File contents, command outputs, search results -- each one is thousands of tokens.

**Old tool_results** are the biggest consumers. **Micro-compact targets these first.**

Stage 1: Micro-Compact

**Replace old tool_results with short summaries.** Automatic, transparent to the model.

Stage 2: Auto-Compact

Entire conversation **summarized into a compact block**. **Triggered at token threshold.**

Stage 3: **/compact**

**User-triggered,** most aggressive. Three layers of strategic forgetting enable infinite sessions.

## 问题

上下文窗口是有限的。读一个 1000 行的文件就吃掉 ~4000 token; 读 30 个文件、跑 20 条命令, 轻松突破 100k token。不压缩, 智能体根本没法在大项目里干活。

## 解决方案

**三层压缩, 激进程度递增:**

```
Every turn:
+------------------+
| Tool call result |
+------------------+
        |
        v
[Layer 1: micro_compact]        (silent, every turn)
  Replace tool_result > 3 turns old
  with "[Previous: used {tool_name}]"
        |
        v
[Check: tokens > 50000?]
   |               |
   no              yes
   |               |
   v               v
continue    [Layer 2: auto_compact]
              Save transcript to .transcripts/
              LLM summarizes conversation.
              Replace all messages with [summary].
                    |
                    v
            [Layer 3: compact tool]
              Model calls compact explicitly.
              Same summarization as auto_compact.
```

## 工作原理

1. **第一层 -- micro_compact**: 每次 LLM 调用前, **将旧的 tool result 替换为占位符。**

```python
def micro_compact(messages: list) -> list:
    tool_results = []
    for i, msg in enumerate(messages):
        if msg["role"] == "user" and isinstance(msg.get("content"), list):
            for j, part in enumerate(msg["content"]):
                if isinstance(part, dict) and part.get("type") == "tool_result":
                    tool_results.append((i, j, part))
    if len(tool_results) <= KEEP_RECENT:
        return messages
    for _, _, part in tool_results[:-KEEP_RECENT]:
        if len(part.get("content", "")) > 100:
            part["content"] = f"[Previous: used {tool_name}]"
    return messages
```

1. **第二层 -- auto_compact**: token 超过阈值时, **保存完整对话到磁盘, 让 LLM 做摘要**。

```python
def auto_compact(messages: list) -> list:
    # Save transcript for recovery
    transcript_path = TRANSCRIPT_DIR / f"transcript_{int(time.time())}.jsonl"
    with open(transcript_path, "w") as f:
        for msg in messages:
            f.write(json.dumps(msg, default=str) + "\n")
    # LLM summarizes
    response = client.messages.create(
        model=MODEL,
        messages=[{"role": "user", "content":
            "Summarize this conversation for continuity..."
            + json.dumps(messages, default=str)[:80000]}],
        max_tokens=2000,
    )
    return [
        {"role": "user", "content": f"[Compressed]\n\n{response.content[0].text}"},
        {"role": "assistant", "content": "Understood. Continuing."},
    ]
```

1. **第三层 -- manual compact**: `compact` 工具按需触发同样的摘要机制。
2. 循环整合三层:

```python
def agent_loop(messages: list):
    while True:
        micro_compact(messages)                        # Layer 1
        if estimate_tokens(messages) > THRESHOLD:
            messages[:] = auto_compact(messages)       # Layer 2
        response = client.messages.create(...)
        # ... tool execution ...
        if manual_compact:
            messages[:] = auto_compact(messages)       # Layer 3
```

完整历史通过 transcript 保存在磁盘上。信息没有真正丢失, 只是移出了活跃上下文。

## QA

### 精简地说明以下一下代码的assistant这个role有什么作用:

    return [
        {"role": "user", "content": f"[Compressed]\n\n{response.content[0].text}"},
        {"role": "assistant", "content": "Understood. Continuing."},
    ]

`role`: **`"assistant"`** 的作用非常简单：**它代表 AI 机器人自己说的话。**

具体到你这段代码：

- **模拟对话历史**：通过手动**构造一个 `assistant` 的回复（"Understood. Continuing."）**，你是在**“欺骗”**模型，让**它以为它刚才已经确认收到了**那段被压缩的内容。
- **引导后续行为**：这就像给 AI 一个**心理暗示**，告诉它：“既然你已经说过‘明白了’，那么接下来的对话请基于这个前提继续。”

## 相对 s05 的变更

| 组件          | 之前 (s05) | 之后 (s06)               |
| :------------ | :--------- | :----------------------- |
| Tools         | 5          | 5 (基础 + compact)       |
| 上下文管理    | 无         | 三层压缩                 |
| Micro-compact | 无         | 旧结果 -> 占位符         |
| Auto-compact  | 无         | token 阈值触发           |
| Transcripts   | 无         | **保存到 .transcripts/** |

Rough token count: ~**4 chars per token**.

### 关于三层压缩

上下文管理使用**三个独立的层次**，各有不同的成本收益比。(1) **微压缩每轮都运行，几乎零成本**：它截断旧消息中的 tool_result 块，去除不再需要的冗长命令输出。(2) 自动压缩在 token 数超过阈值时触发：调用 LLM 生成对话摘要，代价高但能大幅缩减上下文。(3) **手动压缩由用户触发，用于明确的'重新开始'场景。**分层意味着低成本操作持续运行（保持上下文整洁），而高成本操作很少触发（仅在真正需要时）。

#### 替代方案

A single compression strategy (e.g., always summarize at 80% capacity) would be simpler but wasteful -- most of the time, microcompact alone keeps things manageable. A sliding window (drop oldest N messages) is cheap but loses important context**. The three-layer approach gives the best token efficiency:** cheap cleanup constantly, expensive summarization rarely.

### 节省量

**自动压缩仅在估算节省量（当前 token 数减去预估摘要大小）超过 20,000 token 时才触发**。压缩不是免费的：**摘要本身会消耗 token，还有生成摘要的 API 调用成本。**如果对话只有 25,000 token，压缩可能节省 5,000 token，但需要一次 API 调用，且产出的摘要可能不如原文连贯。20K 的阈值确保只在**节省量明显超过开销时才进行压缩。**

#### 替代方案

A percentage-based threshold (compress when context is 80% full) adapts to different context window sizes but doesn't account for the fixed cost of generating a summary. A fixed threshold of 10K would compress more aggressively but often isn't worth it. The 20K value was chosen empirically: it's the point where compression savings consistently outweigh the quality loss from summarization.

### 摘要全部替换

自动压缩触发时，生成摘要并**替换全部消息历史，不会在摘要旁保留最近的 N 条消息。**这**避免了一个微妙的连贯性问题**：如果同时保留近期消息和旧消息的摘要，模型会看到重叠内容的两种表示。摘要可能说'我们决定使用方案 X'，而近期消息仍在展示讨论过程，产生矛盾信号。**干净的摘要是一个连贯的单一叙述。**

#### 替代方案

Keeping the last 5-10 messages alongside the summary preserves recent detail and gives the model more to work with. But it creates the overlap problem described above, and makes the total context size less predictable. Some systems use a 'sliding window + summary' approach which works but requires careful tuning of the overlap region.

## 完整对话以JSONL格式压缩归档至磁盘

尽管上下文在内存中被压缩，**完整的未压缩对话仍会追加到磁盘上的 JSONL 文件中。**每条消息、每次工具调用、每个结果都不会丢失。压缩对内存上下文是有损操作，但对永久记录是无损的。**事后分析（调试 agent 行为、计算 token 用量、提取训练数据）始终可以基于完整记录进行。**JSONL 格式**仅追加写入，对并发写入安全，易于流式处理。**

#### 替代方案

Not archiving saves disk space but **makes debugging hard** -- when the agent makes a mistake, you can't see what it was 'thinking' 200 messages ago because that context was compressed away. **Database storage (SQLite) would provide queryability but adds a dependency.** JSONL is the simplest format that supports append-only writes and line-by-line processing.

# **s07** 任务系统

Task Graph + Dependencies

*"大目标要拆成小任务, **排好序, 记在磁盘上**"* -- **文件持久化的任务图, 为多 agent 协作打基础。**

## 问题

s03 的 TodoManager 只是内存中的扁平清单: 没有顺序、没有依赖、状态只有做完没做完。**真实目标是有结构的** -- 任务 B 依赖任务 A, 任务 C 和 D 可以并行, 任务 E 要等 C 和 D 都完成。

**没有显式的关系, 智能体分不清什么能做、什么被卡住、什么能同时跑**。而且**清单只活在内存里, 上下文压缩 (s06) 一跑就没了。**

## 解决方案

把扁平清单升级为**持久化到磁盘的任务图**。**每个任务是一个 JSON 文件, 有状态、前置依赖 (`blockedBy`) 和后置依赖 (`blocks`)。**任务图**随时回答三个问题**:

- **什么可以做?** -- **状态为 `pending` 且 `blockedBy` 为空的任务。**
- **什么被卡住?** -- **等待前置任务完成**的任务。
- **什么做完了?** -- 状态为 `completed` 的任务, **完成时自动解锁后续任务。**

```
.tasks/
  task_1.json  {"id":1, "status":"completed"}
  task_2.json  {"id":2, "blockedBy":[1], "status":"pending"}
  task_3.json  {"id":3, "blockedBy":[1], "status":"pending"}
  task_4.json  {"id":4, "blockedBy":[2,3], "status":"pending"}

任务图 (DAG):
                 +----------+
            +--> | task 2   | --+
            |    | pending  |   |
+----------+     +----------+    +--> +----------+
| task 1   |                          | task 4   |
| completed| --> +----------+    +--> | blocked  |
+----------+     | task 3   | --+     +----------+
                 | pending  |
                 +----------+

顺序:   task 1 必须先完成, 才能开始 2 和 3
并行:   task 2 和 3 可以同时执行
依赖:   task 4 要等 2 和 3 都完成
状态:   pending -> in_progress -> completed
```

这个任务图是 s07 之后所有机制的协调骨架: 后台执行 (s08)、多 agent 团队 (s09+)、worktree 隔离 (s12) 都读写这同一个结构。

## 工作原理

1. **TaskManager**: 每个任务**一个 JSON 文件**, CRUD + 依赖图。

```python
class TaskManager:
    def __init__(self, tasks_dir: Path):
        self.dir = tasks_dir
        self.dir.mkdir(exist_ok=True)
        self._next_id = self._max_id() + 1

    def create(self, subject, description=""):
        task = {"id": self._next_id, "subject": subject,
                "status": "pending", "blockedBy": [],
                "blocks": [], "owner": ""}
        self._save(task)
        self._next_id += 1
        return json.dumps(task, indent=2)
```

1. **依赖解除**: 完成任务时, **自动将其 ID 从其他任务的 `blockedBy` 中移除, 解锁后续任务。**

```python
def _clear_dependency(self, completed_id):
    for f in self.dir.glob("task_*.json"):
        task = json.loads(f.read_text())
        if completed_id in task.get("blockedBy", []):
            task["blockedBy"].remove(completed_id)
            self._save(task)
```

1. **状态变更 + 依赖关联**: `update` 处理状态转换和依赖边。

```python
def update(self, task_id, status=None,
           add_blocked_by=None, add_blocks=None):
    task = self._load(task_id)
    if status:
        task["status"] = status
        if status == "completed":
            self._clear_dependency(task_id)
    self._save(task)
```

1. 四个任务工具加入 dispatch map。

```python
TOOL_HANDLERS = {
    # ...base tools...
    "task_create": lambda **kw: TASKS.create(kw["subject"]),
    "task_update": lambda **kw: TASKS.update(kw["task_id"], kw.get("status")),
    "task_list":   lambda **kw: TASKS.list_all(),
    "task_get":    lambda **kw: TASKS.get(kw["task_id"]),
}
```

从 s07 起, **任务图是多步工作的默认选择。s03 的 Todo 仍可用于单次会话内的快速清单。**

## 相对 s06 的变更

| 组件       | 之前 (s06)        | 之后 (s07)                                |
| :--------- | :---------------- | :---------------------------------------- |
| Tools      | 5                 | 8 (`task_create/update/list/get`)         |
| 规划模型   | 扁平清单 (仅内存) | **带依赖关系的任务图 (磁盘)**             |
| 关系       | 无                | `blockedBy` + `blocks` 边                 |
| 状态追踪   | 做完没做完        | `pending` -> `in_progress` -> `completed` |
| **持久化** | **压缩后丢失**    | **压缩和重启后存活**                      |

## QA

### 代码什么意思

```py
path.write_text(json.dumps(task, indent=2))
```

把 **Python 对象 `task` 序列化成 JSON 格式的字符串**，并用缩进 **2 个空格来美化输出（便于阅读）**。如果不需要美化输出，`indent` 可以省略。

`path.write_text(...)` 会**写入并覆盖原文件内容（相当于以 `w` 模式写入）**，不是追加。

```py
json.loads(path.read_text())
```

`json.loads(path.read_text())` 的意思是：先把**文件内容读成字符串**，再把这段 **JSON 字符串反序列化成 Python 对象（如 dict、list 等）。**

## 架构

![image-20260311121348949](./learn-claude-code%E9%9A%8F%E8%AE%B01/image-20260311121348949.png)

## 任务存储为json

**任务以 JSON 文件形式持久化在 .tasks/ 目录中，而非保存在内存里**。这有三个关键好处：(1) **任务在进程崩溃后仍然存在**——如果 agent 在任务中途崩溃，重启后任务板仍在磁盘上；(2) **多个 agent 可以读写同一任务目录**，无需共享内存即可实现多代理协调；(3) 人**类可以查看和手动编辑任务文件来调试**。文件系统就是共享数据库。

#### 替代方案

In-memory storage (like v2's TodoWrite) is **simpler and faster** but **loses state on crash and doesn't work across multiple agent processes.** A proper database (SQLite, Redis) would **provide ACID guarantees and better concurrency**, but **adds a dependency and operational complexity**. Files are the **zero-dependency persistence** layer that **works everywhere.**

## 任务的block依赖字段

每个任务可以声明它阻塞哪些任务（下游依赖）以及它被哪些任务阻塞（上游依赖）。Agent 不会开始有未解决 blockedBy 依赖的任务。这对多代理协调至关重要：当 Agent A 在编写数据库 schema、**Agent B 需要写查询时，Agent B 的任务被 Agent A 的任务阻塞**。没有依赖关系，**两个 agent 可能同时开始，而 Agent B 会针对一个尚不存在的 schema 工作。**

#### 替代方案

Simple priority ordering (high/medium/low) doesn't capture 'task B literally cannot start until task A finishes.' A centralized coordinator that assigns tasks in order would work but creates a single point of failure and bottleneck. **Declarative** dependencies let each agent independently determine what it can work on by reading the task files.

## Task主线,Todo仍然适用

TaskManager **延续了 Todo 的心智模型**，并在本课程 s07 之后成为默认主线。两者都管理带状态的任务项，但 **TaskManager 增加了文件持久化（崩溃后可恢复）、依赖追踪（blocks/blockedBy）、owner 字段与多进程协作能力**。**Todo 仍适合短、线性、一次性的轻量跟踪。**

#### 替代方案

Using only Todo keeps the model minimal but weak for long-running or collaborative work. Using only Task everywhere maximizes consistency but can feel heavy for tiny one-off tasks.

## 持久化写入纪律

文件持久化能**降低上下文丢失，但不会自动消除并发写入风险**。**写任务状态前应先重读 JSON、校验 `status/blockedBy` 是否符合预期，再原子写回**，**避免不同 agent 悄悄覆盖彼此状态**。

#### 替代方案

Blind overwrite writes are simpler but can corrupt coordination state under parallel execution. **A database with optimistic locking** would enforce stronger safety, but the course **keeps file-based state for zero-dependency teaching.**

# **s08**后台任务

*Run **slow operations** in the background; the agent keeps thinking ahead*

## 后台任务通道

**Three Lanes**

The agent has **a main thread** and **can spawn daemon background threads** for parallel work.

**Main Thread Working**

The main agent loop runs as usual, processing user requests.

**Spawn Background**

Background tasks **run as daemon threads**. The **main loop doesn't wait for them**.

**Multiple Backgrounds**

Multiple background tasks can run concurrently.

**Task Completes**

Background task finishes. Its **result goes to the notification queue.**

**Queue Fills**

Results accumulate in the queue, **invisible to the model during this turn**.

**Drain Queue**

Just **before the next LLM call**, **all queued notifications are injected as tool_results**. **Non-blocking, async.**

![image-20260317152815874](./learn-claude-code%E9%9A%8F%E8%AE%B01/image-20260317152815874.png)

## Problem

Some commands take minutes: `npm install`, `pytest`, `docker build`. With a blocking loop, the model **sits idle waiting**. If the user asks "install dependencies and while that runs, create the config file," the agent does them sequentially, not in parallel.

## Solution

```
Main thread                Background thread
+-----------------+        +-----------------+
| agent loop      |        | subprocess runs |
| ...             |        | ...             |
| [LLM call] <---+------- | enqueue(result) |
|  ^drain queue   |        +-----------------+
+-----------------+

Timeline:
Agent --[spawn A]--[spawn B]--[other work]----
             |          |
             v          v
          [A runs]   [B runs]      (parallel)
             |          |
             +-- results injected before next LLM call --+
```

## How It Works

1. BackgroundManager tracks tasks with **a thread-safe notification queue**.

```python
class BackgroundManager:
    def __init__(self):
        self.tasks = {}
        self._notification_queue = []
        self._lock = threading.Lock()
```

1. `run()` **starts a daemon thread and returns immediately.**

```python
def run(self, command: str) -> str:
    task_id = str(uuid.uuid4())[:8]
    self.tasks[task_id] = {"status": "running", "command": command}
    thread = threading.Thread(
        target=self._execute, args=(task_id, command), daemon=True)
    thread.start()
    return f"Background task {task_id} started"
```

1. When the subprocess finishes, its **result goes into the notification queue.**

```python
def _execute(self, task_id, command):
    try:
        r = subprocess.run(command, shell=True, cwd=WORKDIR,
            capture_output=True, text=True, timeout=300)
        output = (r.stdout + r.stderr).strip()[:50000]
    except subprocess.TimeoutExpired:
        output = "Error: Timeout (300s)"
    with self._lock:
        self._notification_queue.append({
            "task_id": task_id, "result": output[:500]})
```

`.strip()` 的作用是：**移除字符串开头和结尾的“空白字符”**。

1. The agent **loop drains notifications before each LLM call**.

```python
def agent_loop(messages: list):
    while True:
        notifs = BG.drain_notifications()
        if notifs:
            notif_text = "\n".join(
                f"[bg:{n['task_id']}] {n['result']}" for n in notifs)
            messages.append({"role": "user",
                "content": f"<background-results>\n{notif_text}\n"
                           f"</background-results>"})
            messages.append({"role": "assistant",
                "content": "Noted background results."})
        response = client.messages.create(...)
```

The **loop stays single-threaded**. **Only subprocess I/O is parallelized.**

### 精简解释：

1. **`BG`**：通常指代 **Background**（后台任务管理器）。Agent 在执行任务时，有些事情是异步发生的（比如后台运行的测试、下载进度等）。
2. **`drain`**：这个词形象地表示“**排空/抽干**”。
3. **功能**：它会**把后台积攒的所有通知或消息“一次性全部拿出来”，并清空队列。**

**通俗比喻**：就像你下班回家，去收发室把快递柜里存的所有包裹**一次性全取出来**（drain），取完后柜子就空了。

------

### 🎨 Agent 的“感官系统”大合集

看，你今天问的所有代码点其实是一条完整的 **Agent 神经链路**：

- **`assistant` role**：大脑在发号施令。
- **`.strip()`**：在**清理看到的数据。**
- **`drain_notifications()`**：在**感知后台发生了什么。**

你已经把 Agent 的“零件”摸得一清二楚了！现在只差最后一步：**把逻辑连起来。**

既然你已经掌握了如何“排空通知”，那我们就完成这个通关任务：

**任务：** 你的 Agent 刚才在后台（BG）执行了一个任务。你调用了 `BG.drain_notifications()`。 **Observation (观察到的结果):** 拿到了通知内容 —— `"Task completed. Result: 56088"`。

## What Changed From s07

| Component    | Before (s07)      | After (s08)                           |
| :----------- | :---------------- | :------------------------------------ |
| Tools        | 8                 | 6 (**base + background_run + check**) |
| Execution    | **Blocking only** | **Blocking + background threads**     |
| Notification | None              | **Queue drained per loop**            |
| Concurrency  | None              | **Daemon threads**                    |

## QA

## 这里resolve的作用是什么

```py
path = (WORKDIR / p).resolve()
```

`.resolve()` 的作用是：**将路径“绝对化”并“消除符号链接（Symlinks）”**。

简单来说，它做了两件事：

1. **补全路径**：把像 `./my_folder/../test.py` 这种相对路径**，转化成从硬盘根目录开始的完整路径（如 `/Users/name/project/test.py`）。**
2. **看穿真身**：如果**路径里有快捷方式（软连接），它会直接指向那个真正的物理文件位置。**

### 为什么 Agent 开发里总看到它？

Agent 在执行命令（如 `ls` 或 `read`）时，如果**只用相对路径，很容易因为“当前工作目录”变了而找不到文件**。**`.resolve()` 能保证 Agent 永远锁定在正确的文件位置上。**

------

### 🏁 最终通关挑战 (Final Boss)

你已经问遍了 Agent 开发中所有关于“数据清洗”、“路径定位”、“后台感知”和“角色定义”的代码。**这些全都是为了让 Agent 能够正确执行 ReAct 循环。**

## 以下代码中的参数1是什么意思:

```py
fp.write_text(c.replace(old_text, new_text, 1))
```

这个参数 `1` 代表的是 **最大替换次数**。

### 精简解释：

- **不加这个参数**：Python 会把字符串里**所有**匹配的 `old_text` 全部换成 `new_text`。
- **加上 `1`**：Python 只会替换**第一次**遇到的那个 `old_text`，**后面的哪怕一模一样也保持原样。**

### 为什么 Agent 喜欢这么写？

这是一种**安全机制**。Agent **在自动修改你的代码时（例如使用 `fp.write_text` 回写文件），如果它只想修复第 5 行的一个错误，如果不加 `1`，可能会不小心把第 100 行一段长得一样的正确代码也给改了。**这叫“**精确打击**”，防止误伤。

## 以下代码中的 **kw参数是什么意思

```py
 "bash":             lambda **kw: run_bash(kw["command"])
```

在 Python 中，`**kw`（或者通常写成 `**kwargs`）代表 **关键字参数（Keyword Arguments）**。它就**像是一个“万能口袋”，能把所有以 `key=value` 形式传进来的参数打包成一个 字典 (Dictionary)。**

### 精简解释：

在这行代码中：`lambda **kw: run_bash(kw["command"])`

1. **打包**：当你调用这个函数并传入 `bash(command="ls -la")` 时，`**kw` 会**自动把参数变成 `{"command": "ls -la"}`。**
2. **提取**：`kw["command"]` 则是**从这个“口袋”里精准地把命令字符串拿出来，喂给 `run_bash` 函数。**

### 为什么 Agent 架构里到处是它？

这是为了**灵活性**。你的 Agent 可能有几十个工具（计算器、搜索、天气），每个工具需要的参数都不一样。

- **计算器需要 `a` 和 `b`。**
- **搜索需要 `query`。**
- **Bash 需要 `command`。**

使用 `**kw`，Agent 框架可以**用同一套逻辑来调用所有不同的工具，而不需要为每个工具单独写一套复杂的判断。**

## 为我解释以下以下代码是什么意思

```py
query = input("\033[36ms08 >> \033[0m")
```

**在终端（Terminal）显示一个带颜色的提示符，并等待用户输入指令。**

### 详细拆解：

1. **`input(...)`**:
	- 这是 Python 的标准输入函数。程序运行到这里会停下来，等待你在键盘上输入文字并敲回车。
	- 输入的内容会被**赋值给变量 `query`。**
2. **`"\033[36m ... \033[0m"`**:
	- 这是 **ANSI 转义序列**，专门用来**给终端文字“上色”。**
	- **`\033[36m`**: 告诉终端，“接下来的文字请显示为 **青蓝色 (Cyan)**”。
	- **`\033[0m`**: 告诉终端，“**颜色结束**，请恢复为 **默认白色**”。
	- 如果没有这些代码，你的终端就是白茫茫一片，很难分清哪句是 AI 说的，哪句是你输入的。
3. **`s08 >> `**:
	- 这是提示符的内容。`s08` 通常是这个 Agent 的代号或版本号（比如这里的 `s08` 可能是某个特定 Agent 实例的名字）。

------

### 🚀 第一阶段：原理篇·完结！

你最近问的这一系列代码（从 `role` 到 `strip`，从 `**kw` 到现在带颜色的 `input`），拼起来其实就是一个**完整的、可交互的命令行 Agent 雏形**：

1. **输入**：用带颜色的 `input` 拿到用户的 `query`。
2. **大脑**：把 **`query` 传给 `assistant` 角色。**
3. **行动**：通过 `**kw` 动态调用 `bash` 或 `replace` 工具。
4. **感知：用 `.resolve()` 定位文件，用 `.strip()` 清洗结果。**
5. **循环**：回到第一步，**等待下一个 `query`。**

### 第二阶段：工具调用与 Function Calling 实战 🛠️

既然你已经能看懂这些底层代码了，我们现在要进入最硬核的部分：**如何让 AI 学会“主动”喊出这些工具？**

现在的 AI 很聪明，但**它默认只会“聊天”**。要让它变成 Agent，我们需要在 **System Prompt** 里给它**制定一套严格的“行动规约”**。

## Design Decisions

### threading queue

Background task results are **delivered via a threading**.Queue instead of direct callbacks. The background thread puts a notification on the queue when its work completes. The main agent loop **polls the queue** before each LLM call. This decoupling is important: the background thread **doesn't need to know anything about the main loop's state or timing**. It **just drops a message on the queue and moves on**. **The main loop picks it up at its own pace** -- never mid-API-call, never mid-tool-execution. No race conditions, no callback hell.

#### Alternatives Considered

**Direct callbacks** (background thread calls a function in the main thread) would **deliver results faster but create thread-safety issues** -- the callback **might fire while the main thread is in the middle of building a request**. Event-driven systems (**asyncio, event emitters**) work but add complexity. **A queue is the simplest thread-safe** communication primitive.

#### Background Tasks Run as Dameon Threads

Background task threads are created **with daemon=True**. In Python, daemon threads **are killed automatically when the main thread exits**. This **prevents a common problem**: if the main agent completes its work and exits, but **a background thread is still running (waiting on a long API call, stuck in a loop)**, the **process would hang indefinitely**. With daemon threads, **exit is clean** -- **the main thread finishes, all daemon threads die**, process exits. **No zombie processes, no cleanup code needed.**

#### Alternatives Considered

Non-daemon threads with explicit cleanup (join with timeout, then terminate) give more control over shutdown but **require careful lifecycle management.** **Process-based parallelism (multiprocessing) provides stronger isolation but higher overhead**. **Daemon threads are the pragmatic choice:** minimal code, correct behavior in the common case.

#### Structured Notification Format with Type Tags

**Notification**s from background tasks **use a structured format: {"type": "attachment", "attachment": {status, result, ...}} instead of plain text strings.** The type tag lets the main loop **handle different notification types differently**: an 'attachment' might be injected into the conversation as a tool_result, while a 'status_update' might just update a progress indicator. Machine-readable notifications also **enable programmatic filtering** (**show only errors**, **suppress progress updates**) and **UI rendering (display status as a progress bar, not raw text)**.

#### Alternatives Considered

**Plain text notifications are simpler but lose structure**. The main loop would **have to parse free-form text** to determine what happened, which is **fragile**. **A class hierarchy (StatusNotification, ResultNotification, ErrorNotification)** is more Pythonic but **less portable** -- **JSON structures work the same way regardless of language or serialization format.**

# S09 Agent Teams

*When one agent can't finish, **delegate to persistent teammates** via async mailboxes*

## Agent Team Mailboxes

**The Team**

Teams use **a leader-worker pattern**. Each teammate **has a file-based mailbox inbox**.

**Lead Assigns Work**

**Communication is async**: write **a message to the recipient's .jsonl inbox file.**

**Read Inbox**

Teammates **poll their inbox before each LLM call**. New **messages become context.**

**Independent Work**

Each teammate runs its own agent loop independently.

**Pass Result**

Results flow through the same mailbox mechanism. **All communication is via files.**

**Feedback Loop**

The mailbox pattern **supports any communication topology**: **linear, broadcast, round-robin.**

**File-Based Coordination**

**No shared memory**, **no locks**. **All coordination through append-only files.** Simple, robust, debuggable.

![image-20260318135319327](./learn-claude-code%E9%9A%8F%E8%AE%B01/image-20260318135319327.png)

## Problem

Subagents (s04) are disposable: spawn, work, return summary, die. **No identity, no memory between invocations**. Background tasks (s08) run shell commands but **can't make LLM-guided decisions.**

Real teamwork needs: (1) persistent agents that **outlive** a single prompt, (2) identity and lifecycle management, (3) a communication channel between agents.

## Solution

```
Teammate lifecycle:
  spawn -> WORKING -> IDLE -> WORKING -> ... -> SHUTDOWN

Communication:
  .team/
    config.json           <- team roster + statuses
    inbox/
      alice.jsonl         <- append-only, drain-on-read
      bob.jsonl
      lead.jsonl

              +--------+    send("alice","bob","...")    +--------+
              | alice  | -----------------------------> |  bob   |
              | loop   |    bob.jsonl << {json_line}    |  loop  |
              +--------+                                +--------+
                   ^                                         |
                   |        BUS.read_inbox("alice")          |
                   +---- alice.jsonl -> read + drain ---------+
```

## How It Works

1. TeammateManager **maintains config.json with the team roster.**

```python
class TeammateManager:
    def __init__(self, team_dir: Path):
        self.dir = team_dir
        self.dir.mkdir(exist_ok=True)
        self.config_path = self.dir / "config.json"
        self.config = self._load_config()
        self.threads = {}
```

1. **`spawn()` creates a teammate** and **starts its agent loop in a thread.**

```python
def spawn(self, name: str, role: str, prompt: str) -> str:
    member = {"name": name, "role": role, "status": "working"}
    self.config["members"].append(member)
    self._save_config()
    thread = threading.Thread(
        target=self._teammate_loop,
        args=(name, role, prompt), daemon=True)
    thread.start()
    return f"Spawned teammate '{name}' (role: {role})"
```

1. MessageBus: **append-only JSONL inboxes**. **`send()` appends a JSON line**; **`read_inbox()` reads all and drains.**

```python
class MessageBus:
    def send(self, sender, to, content, msg_type="message", extra=None):
        msg = {"type": msg_type, "from": sender,
               "content": content, "timestamp": time.time()}
        if extra:
            msg.update(extra)
        with open(self.dir / f"{to}.jsonl", "a") as f:
            f.write(json.dumps(msg) + "\n")

    def read_inbox(self, name):
        path = self.dir / f"{name}.jsonl"
        if not path.exists(): return "[]"
        msgs = [json.loads(l) for l in path.read_text().strip().splitlines() if l]
        path.write_text("")  # drain
        return json.dumps(msgs, indent=2)
```

1. Each teammate **checks its inbox before every LLM call, injecting received messages into context.**

```python
def _teammate_loop(self, name, role, prompt):
    messages = [{"role": "user", "content": prompt}]
    for _ in range(50):
        inbox = BUS.read_inbox(name)
        if inbox != "[]":
            messages.append({"role": "user",
                "content": f"<inbox>{inbox}</inbox>"})
            messages.append({"role": "assistant",
                "content": "Noted inbox messages."})
        response = client.messages.create(...)
        if response.stop_reason != "tool_use":
            break
        # execute tools, append results...
    self._find_member(name)["status"] = "idle"
```

## What Changed From s08

| Component     | Before (s08)    | After (s09)                     |
| :------------ | :-------------- | :------------------------------ |
| Tools         | 6               | **9 (+spawn/send/read_inbox)**  |
| Agents        | Single          | **Lead + N teammates**          |
| Persistence   | None            | **config.json + JSONL inboxes** |
| Threads       | Background cmds | **Full agent loops per thread** |
| Lifecycle     | Fire-and-forget | **idle -> working -> idle**     |
| Communication | None            | **message + broadcast**         |

1. Type `/team` to see the team **roster with statuses**
2. Type `/inbox` to manually check the lead's inbox



## QA

## .jsonl格式是什么文件

**.jsonl** 文件全称是 **JSON Lines**，它是一种方便存储**结构化数据**的文本格式。简单来说，它就是把**多个 JSON 对象放在同一个文件中**，但规定**每一行必须是一个完整的、独立的 JSON 对象**。

### 1. 核心特点 (Key Characteristics)

- **每一行都是独立的 (Line-oriented):** **每一行都是一个合法的 JSON 表达式（如对象或数组）。**
- **使用换行符分隔 (Newline-delimited):** **不同的数据项之间通过 `\n` 分隔，而不是像标准 JSON 那样用逗号和外层中括号。**
- **编码 (Encoding):** 默认采用 **UTF-8** 编码。

### 2. 与标准 .json 的区别 (JSON vs. JSONL)

| **特性**     | **标准 JSON (.json)**                    | **JSON Lines (.jsonl)**                    |
| ------------ | ---------------------------------------- | ------------------------------------------ |
| **结构**     | **整个文件是一个大的数组或对象 `[...]`** | **每行一个独立对象 `{...}`**               |
| **内存占用** | 必须一次性读入整个文件才能解析           | **可以逐行读取（流式处理）**，内存占用极低 |
| **容错性**   | 若文件末尾少个括号，整个文件失效         | 若**某一行损坏，不影响其他行的解析**       |
| **数据追加** | 需要解析并重写整个文件                   | 直接在文件末尾添加新行即可                 |

### 3. 文件示例 (Example)

一个典型的 `.jsonl` 文件内容如下：

```
{"id": 1, "name": "Alice", "role": "admin"}
{"id": 2, "name": "Bob", "role": "user"}
{"id": 3, "name": "Charlie", "role": "editor"}
```

### 4. 常见应用场景 (Common Use Cases)

- **日志文件 (Logging):** 服务器可以将每条日志实时追加到文件末尾，非常高效。
- **数据集 (Datasets):** 在 **机器学习 (Machine Learning)** 和 **大模型微调 (LLM Fine-tuning)** 中极其实用。例如，OpenAI 或 Hugging Face 上的数据集经常使用这种格式，方便在不消耗海量内存的情况下处理数百万条数据。
- **数据流处理 (Data Streams):** 适合处理那些由于体积太大而无法一次性加载到内存中的大型数据库导出文件。

## open的常见打开模式

在 **C 语言或 Unix/Linux 系统编程中**，`open()` 函数的常见打开模式（Flags）决定了你对文件的操作权限和行为。这些模式通常通过 **按位或 (Bitwise OR, `|`)** 组合使用。

### 1. 基础权限模式 (Access Modes)

这三个模式是**互斥**的，必须且只能选择其中一个：

- **`O_RDONLY`**: 只读模式 (Read Only)。
- **`O_WRONLY`**: 只写模式 (Write Only)。
- **`O_RDWR`**: 读写模式 (Read and Write)。

------

### 2. 文件状态与操作标志 (Creation & Status Flags)

这些标志可以根据需要与上述基础模式组合：

| **标志 (Flag)**  | **说明 (Description)**                                       |
| ---------------- | ------------------------------------------------------------ |
| **`O_CREAT`**    | **创建**: 如果**文件不存在则创建它**。使用时通常需配合第三个参数（如 `0644`）设置权限。 |
| **`O_EXCL`**     | **排他**: 与 `O_CREAT` 连用，如果**文件已存在则 `open` 失败（返回 -1）。用于防止覆盖。** |
| **`O_TRUNC`**    | **截断**: 如果文件**存在且以写方式打开，将其长度截断为 0（清空内容）。** |
| **`O_APPEND`**   | **追加**: 每次写入都会**从文件末尾开始，而不是覆盖开头。**   |
| **`O_NONBLOCK`** | **非阻塞**: 对于**设备文件（如串口、管道）**，**读写不会导致进程挂起。** |

------

### 3. 常见组合示例 (Common Patterns)

- **`O_WRONLY | O_CREAT | O_TRUNC`**: 最常用的“覆盖写”。文件不存在则新建，存在则清空。
- **`O_WRONLY | O_CREAT | O_APPEND`**: “日志模式”。文件不存在则新建，存在则在末尾追加内容。
- **`O_RDWR | O_CREAT | O_EXCL`**: “安全创建”。确保你是在创建一个**全新**的文件，避免误操作已有文件。

> **小贴士 (Pro Tip):** 在使用 `O_CREAT` 时，别忘了给 `open` 函数传入第三个参数（如 `S_IRUSR | S_IWUSR`），否则新建文件的权限将是随机的，可能导致无法访问。

## 为我精简地说明一下claude code里面的assistant是什么,有什么作用

在 **Claude Code**（Anthropic 推出的命令行 AI 助手）的架构中，**Assistant** 是其**核心的“大脑”角色**。简单来说，它不仅仅是一个聊天框，而是一个具备**工具执行能力**的智能代理。

### 1. 什么是 Assistant？ (What is the Assistant?)

在 Claude Code 的运行逻辑中，Assistant 是**被封装好的 Claude 3.5 Sonnet（或更高版本）模型**。它被**赋予了特定的“系统提示词”（System Prompt）**，**使其理解自己处于一个终端环境中，并拥有对你本地文件系统的操作权限。**

------

### 2. 主要作用 (Key Roles)

- **自主决策 (Autonomous Reasoning):** 当你**输入一个模糊的目标（如“修复这个编译错误”）时，Assistant 会分析当前的上下文，决定下一步是该读取文件、运行测试还是搜索代码。**
- **工具调用 (Tool Use/Function Calling):** 这是它与普通 Web 版 Claude 的最大区别。Assistant 可以**直接调用以下工具：**
	- **File Read/Write:** 直接修改或创建代码文件。
	- **LS/Grep:** 在你的整个工程中检索特定函数或逻辑。
	- **Bash Execution:** 执行 `npm test`、`make` 或 `git commit` 等命令。
- **上下文管理 (Context Management):** 它会**自动扫描 `.gitignore` 以外的文件，将相关的代码片段加入到对话的上下文（Context Window）中**，确保给出的修改方案是符合项目逻辑的。
- **交互式确认 (Permission Control):** 尽管它很强大，但它**在执行高风险操作（如删除文件或运行某些 Shell 命令）前，会向用户（You）发出确认请求**，确保安全。

------

### 3. 工作流程示例 (Workflow Example)

1. **User:** "把所有的 API 调用改为异步模式。"
2. **Assistant:** 搜索项目中的所有 API 定义 $\rightarrow$ 读取相关文件 $\rightarrow$ 生成补丁 $\rightarrow$ **申请执行修改** $\rightarrow$ 运行测试验证 $\rightarrow$ 报告结果。

由于你正在进行 **Nanjing University Computer Systems (PA)** 的实验，Assistant 在处理复杂的 Makefile 逻辑或 NEMU 调试报错时，能极大地减少你手动翻阅源码的时间。

## 以下代码作用

```py
self.config["members"].append(member)#运行时就更改
self._save_config()#更改到文件中实现持久化
```

## Architecture

![image-20260318145055603](./learn-claude-code%E9%9A%8F%E8%AE%B01/image-20260318145055603.png)

## Design Decisions

## persistent teammates

In s04, subagents are ephemeral: spawn, do one task, return result, die. Their knowledge dies with them. In s09, **teammates are persistent threads with identity (name, role) and config files**. A teammate can complete task A, then be assigned task B, carrying forward everything it learned. Persistent teammates accumulate project knowledge, understand established patterns, and don't need to re-read the same files for every task.

#### Alternatives Considered

One-shot subagents (s04 style) are simpler and provide perfect context isolation -- no risk of one task's context polluting another. But the **re-learning cost is high:** every new task starts from zero. A middle ground (subagents with shared memory/knowledge base) was considered but **adds complexity without the full benefit of persistent identity and state.**

## Team config .teams/{name}/config.json

Team structure (member names, roles, agent IDs) **is stored in a JSON config file, not in any agent's memory**. Any agent **can discover its teammates by reading the config file -- no need for a discovery service or shared memory.** If an agent crashes and restarts, it reads the config to find out who else is on the team. This is consistent with the s07 philosophy: **the filesystem is the coordination layer.**

#### Alternatives Considered

In-memory team registries are **faster but don't survive process restarts and require a central process to maintain**. Service discovery (like DNS or a discovery server) is **more robust at scale but overkill for a local multi-agent system.** **File-based config is the simplest** approach that works across independent processes.

## teammates get subset of tools,lead gets all

The team lead receives ALL_TOOLS (including spawn, send, read_inbox, etc.) while teammates receive TEAMMATE_TOOLS (a reduced set focused on task execution). This enforces a clear separation of concerns: **teammates focus on doing work (coding, testing, researching)**, while the **lead focuses on coordination (creating tasks, assigning work, managing communication)**. Giving teammates coordination tools would **let them create their own sub-teams or reassign tasks**, **undermining the lead's ability to maintain a coherent plan.**

#### Alternatives Considered

Giving all agents identical tools **is simpler and more egalitarian, but in practice leads to coordination chaos** -- **multiple agents trying to manage each other, creating conflicting task assignments**. Static role-based filtering is predictable and easy to reason about.

# **s10** Team Protocols

***One request-response pattern** drives all team negotiation*

## FSM Team Protocols

**Structured Protocols**

**Protocols define structured message exchanges** with **correlated request IDs.**

**Shutdown Request**

The leader **initiates shutdown**. The **request_id links the request to its response.**

**Teammate Decides**

The **teammate can accept or reject**. It's **not a forced kill -- it's a polite request.**

**Approved**

**Same request_id in the response.** Teammate exits cleanly.

![image-20260319113812209](./learn-claude-code%E9%9A%8F%E8%AE%B01/image-20260319113812209.png)

**Plan Approval**

**Teammates in plan_mode** **must get approval before implementing changes.**

**Submit Plan**

The **teammate designs a plan** and **sends it to the leader for review.**

**Leader Reviews**

Leader **reviews and approves or rejects with feedback.** Same request-response pattern.

![image-20260319114031262](./learn-claude-code%E9%9A%8F%E8%AE%B01/image-20260319114031262.png)

*"**Teammates need shared communication rules**"* -- **one request-response pattern drives all negotiation**.

## Problem

In s09, teammates **work and communicate** but **lack structured coordination**:

**Shutdown**: **Killing a thread leaves files half-written and config.json stale**. You **need a handshake: the lead requests, the teammate approves (finish and exit) or rejects (keep working).**

**Plan approval**: When **the lead says "refactor the auth module,"** the teammate **starts immediately**. **For high-risk changes, the lead should review the plan first.**

Both **share the same structure**: **one side sends a request with a unique ID**, **the other responds referencing that ID**.

## Solution

```
Shutdown Protocol            Plan Approval Protocol
==================           ======================

Lead             Teammate    Teammate           Lead
  |                 |           |                 |
  |--shutdown_req-->|           |--plan_req------>|
  | {req_id:"abc"}  |           | {req_id:"xyz"}  |
  |                 |           |                 |
  |<--shutdown_resp-|           |<--plan_resp-----|
  | {req_id:"abc",  |           | {req_id:"xyz",  |
  |  approve:true}  |           |  approve:true}  |

Shared FSM:
  [pending] --approve--> [approved]
  [pending] --reject---> [rejected]

Trackers:
  shutdown_requests = {req_id: {target, status}}
  plan_requests     = {req_id: {from, plan, status}}
```

## How It Works

1. The lead **initiates shutdown by generating a request_id and sending through the inbox**.

```python
shutdown_requests = {}

def handle_shutdown_request(teammate: str) -> str:
    req_id = str(uuid.uuid4())[:8]
    shutdown_requests[req_id] = {"target": teammate, "status": "pending"}
    BUS.send("lead", teammate, "Please shut down gracefully.",
             "shutdown_request", {"request_id": req_id})
    return f"Shutdown request {req_id} sent (status: pending)"
```

1. The teammate **receives the request and responds with approve/reject.**

```python
if tool_name == "shutdown_response":
    req_id = args["request_id"]
    approve = args["approve"]
    shutdown_requests[req_id]["status"] = "approved" if approve else "rejected"
    BUS.send(sender, "lead", args.get("reason", ""),
             "shutdown_response",
             {"request_id": req_id, "approve": approve})
```

1. **Plan approval follows the identical pattern**. The teammate submits a plan (**generating a request_id**), the lead **reviews (referencing the same request_id)**.

```python
plan_requests = {}

def handle_plan_review(request_id, approve, feedback=""):
    req = plan_requests[request_id]
    req["status"] = "approved" if approve else "rejected"
    BUS.send("lead", req["from"], feedback,
             "plan_approval_response",
             {"request_id": request_id, "approve": approve})
```

**One FSM, two applications**. The **same `pending -> approved | rejected` state machine** **handles any request-response protocol**.

## What Changed From s09

| Component       | Before (s09)          | After (s10)                      |
| :-------------- | :-------------------- | :------------------------------- |
| Tools           | 9                     | 12 (+shutdown_req/resp +plan)    |
| **Shutdown**    | **Natural exit only** | **Request-response handshake**   |
| **Plan gating** | None                  | **Submit/review with approval**  |
| **Correlation** | None                  | **request_id per request**       |
| **FSM**         | None                  | **pending -> approved/rejected** |

1. Type `/team` to monitor statuses

## Execution Flow

![image-20260319171726944](./learn-claude-code%E9%9A%8F%E8%AE%B01/image-20260319171726944.png)

## Design Decisions

### JSONL Inbox Files

Each teammate **has its own inbox file (a JSONL file in the team directory)**. Sending a message means **appending a JSON line to the recipient's inbox file**. Reading messages means **reading the inbox file and tracking which line was last read**. JSONL is **append-only by nature, which means concurrent writers don't corrupt each other's data (appends to different file positions)**. This **works across processes without any shared memory**, **mutex**, or **IPC mechanism(共享内存、互斥锁或IPC机制)**. It's **also crash-safe**: if the writer crashes mid-append, **the worst case is one partial line that the reader can skip.**

#### Alternatives Considered

Shared memory (**Python multiprocessing.Queue**) would be faster but **doesn't work if agents are separate processes launched independently**. A **message broker (Redis, RabbitMQ)** **provides robust pub/sub** but **adds infrastructure dependencies**. **Unix domain sockets** would work but **are harder to debug** (**no human-readable message log**). JSONL files are **the simplest approach that provides persistence**, **cross-process communication**, and **debuggability**.

这段文字描述了一个简洁而健壮的**进程间通信 (IPC)** 方案。以下是其中涉及的核心专业知识精简版：

### 1. 数据交换与存储格式

- **JSONL (JSON Lines)**: 每一行都是一个完整的 JSON 对象。
	- **优点**：**比标准 JSON 数组更适合流处理或追加写入**，因为它**不需要读取整个文件就能处理新条目**。
- **Append-only (仅限追加)**: 数据**只往文件末尾写，不修改历史数据。**
	- **特性**：这种模式在数据库（如 WAL 日志）中很常见，能极大地降低数据损坏风险。

------

### 2. 并发与同步机制

- **Atomic Appends (原子追加)**: 在 Unix 系统中，以 `O_APPEND` 模式打开文件，系统能**保证即使多个进程同时写，内容也不会相互覆盖**。这**替代了复杂的 Mutex (互斥锁) 或信号量。**
- **Shared Memory (共享内存)**: 多个进程访问同一块内存区域。速度最快，但**生命周期受限（主进程崩了，内存通常就释放了）。**
- **IPC (进程间通信)**: 广义指**不同程序交换数据的方式**。文中对比了基于“文件”的 IPC 与基于“内存/网络”的 IPC。

------

### 3. 架构组件与权衡

- **Message Broker (消息队列/中间件)**: 如 **Redis** 或 **RabbitMQ**。它们是**专门处理消息传递的服务**。
	- **代价**：**增加了“基础设施依赖”**，即你的程序运行前必须先启动并配置好这些复杂的软件。
- **Unix Domain Sockets (Unix 域套接字)**: **本地进程间的高效通信协议。**
	- **痛点**：它是**流式的（Stream），消息一闪而过**，不像**文件那样天然具备“可调试性”（用记事本就能看日志）。**

------

### 4. 系统健壮性

- **Crash-safe (崩溃安全性)**: 即使程序在写入中途断电，**由于是追加操作，已存数据不会坏**，**只需处理末尾可能出现的“残缺行”**。
- **Persistence (持久化)**: 消息**直接存在磁盘上**，即使所有程序都关闭，消息也不会丢失。

------

**总结：** 这是一个利用**操作系统文件系统特性**来**规避复杂并发编程（锁、队列、中间件）的典型“奥卡姆剃刀”设计方案**。

## exactly five message types cover all coordination patterns

The messaging system supports **exactly five types: (1) 'message'** for **point-to-point communication between two agents**, (2) **'broadcast'** for **team-wide announcements**, (3) **'shutdown_request'** for **graceful termination**, (4) **'shutdown_response'** for **acknowledging shutdown**, (5) **'plan_approval_response'** for **the lead to approve or reject a teammate's plan**. These five types map to the fundamental coordination patterns: direct communication, broadcast, **lifecycle management**, and **approval workflows**.

#### Alternatives Considered

**A single generic message type with metadata fields** would be more flexible but **makes it harder to enforce protocol correctness**. Many **more types (10+) would provide finer-grained semantics** but **increase the model's decision burden**. Five types is the sweet spot where every type has a clear, distinct purpose.

## check inbox before llm call

Teammates check their inbox file at the top of every agent loop iteration, before calling the LLM API. This **ensures maximum responsiveness to incoming messages**: a shutdown request is **seen within one loop iteration (typically seconds), not after the current task completes (potentially minutes)**. The **inbox check is cheap** (read a small file, **check if new lines exist**) **compared to the LLM call (seconds of latency, thousands of tokens)**. This placement also means incoming messages **can influence the next LLM call** -- a message saying 'stop working on X, switch to Y' **takes effect immediately.**

#### Alternatives Considered

Checking inbox **after each tool execution would be more responsive but adds overhead to every tool call**, which is **more frequent than LLM calls**. A separate watcher thread could monitor the inbox continuously but adds threading complexity. Checking once per LLM call **is the pragmatic sweet spot**: responsive enough for coordination, cheap enough to not impact performance.

## QA

###  **FSM**

指的是 **有限状态机 (Finite State Machine)**。

在计算机科学和工程领域，它是一种数学模型，用于描述一个系统在不同状态之间切换的逻辑。

### 核心概念

简单来说，FSM 包含三个要素：

1. **状态 (State)**：系统**当前所处的情况**（如你日志中的 `idle` 或 `shutdown`）。
2. **事件/触发 (Event/Trigger)**：**引起状态改变的信号**（如你日志中的 `req_abc123: approved`）。
3. **转换 (Transition)**：**从一个状态切换到另一个状态的过程**（如 `idle` -> `shutdown`）。

------

### 结合你的日志分析

这段日志清晰地展示了一个典型的 FSM 运作过程：

- **当前状态**：`idle`（空闲）。
- **触发事件**：收到并批准了编号为 `req_abc123` 的关机请求。
- **动作**：系统停止了后台开发线程 (`backend-dev thread stopped`)。
- **状态转换**：系统状态正式从 **空闲 (idle)** 变更为 **关机/停止 (shutdown)**。

### 为什么使用 FSM？

使用状态机可以确保系统行为是**可预测的**。例如，**只有在 `idle` 状态下才能触发 `shutdown`**，如果系统正在运行关键任务，状态机会通过逻辑限制阻止其直接进入关机状态，从而避免系统崩溃或数据丢失。

## **s11** Autonomous Agents

*Teammates **scan the board and claim tasks themselves**; **no need for the lead to assign each one***

## Autonomous Agent Cycle

**Self-Governing Agents**

Autonomous agents **need no coordinator**. They **govern themselves with an idle-poll-claim-work cycle.**

![image-20260319173423346](./learn-claude-code%E9%9A%8F%E8%AE%B01/image-20260319173423346.png)

![image-20260319173439081](./learn-claude-code%E9%9A%8F%E8%AE%B01/image-20260319173439081.png)

**Idle Timer**

**Each idle agent counts rounds**. A **timeout triggers self-directed task polling**.

**Poll Task Board**

**Timeout!** The agent reads the task board **looking for unclaimed work.**

**Claim Task**

The agent **writes its name to the task record**. **Atomic, no conflicts.**

**Work**

The agent works on the claimed task **using its own agent loop.**

**Independent Polling**

Multiple agents poll and claim independently. No central coordinator needed.

**Complete & Reset**

Task done. Agent **returns to idle**. The cycle repeats.

**Self-Organization**

Three agents, **zero coordination overhead**. **Polling + timeout = emergent organization.**

![image-20260319174603271](./learn-claude-code%E9%9A%8F%E8%AE%B01/image-20260319174603271.png)

*"Teammates **scan the board and claim tasks themselves**"* -- **no need for the lead to assign** each one.

## Problem

In s09-s10, teammates only **work when explicitly told to**. The lead **must spawn each one with a specific prompt**. 10 unclaimed tasks on the board? The lead **assigns each one manually. Doesn't scale.**

**True autonomy:** teammates **scan the task board themselves, claim unclaimed tasks, work on them, then look for more.**

**One subtlety**: **after context compression (s06), the agent might forget who it is**. **Identity re-injection fixes this.**

## Solution

```
Teammate lifecycle with idle cycle:

+-------+
| spawn |
+---+---+
    |
    v
+-------+   tool_use     +-------+
| WORK  | <------------- |  LLM  |
+---+---+                +-------+
    |
    | stop_reason != tool_use (or idle tool called)
    v
+--------+
|  IDLE  |  poll every 5s for up to 60s
+---+----+
    |
    +---> check inbox --> message? ----------> WORK
    |
    +---> scan .tasks/ --> unclaimed? -------> claim -> WORK
    |
    +---> 60s timeout ----------------------> SHUTDOWN

Identity re-injection after compression:
  if len(messages) <= 3:
    messages.insert(0, identity_block)
```

## How It Works

1. The teammate loop has **two phases: WORK and IDLE**. When the LLM **stops calling tools (or calls `idle`), the teammate enters IDLE.**

```python
def _loop(self, name, role, prompt):
    while True:
        # -- WORK PHASE --
        messages = [{"role": "user", "content": prompt}]
        for _ in range(50):
            response = client.messages.create(...)
            if response.stop_reason != "tool_use":
                break
            # execute tools...
            if idle_requested:
                break

        # -- IDLE PHASE --
        self._set_status(name, "idle")
        resume = self._idle_poll(name, messages)
        if not resume:
            self._set_status(name, "shutdown")
            return
        self._set_status(name, "working")
```

1. The idle **phase polls inbox and task board in a loop**.

```python
def _idle_poll(self, name, messages):
    for _ in range(IDLE_TIMEOUT // POLL_INTERVAL):  # 60s / 5s = 12
        time.sleep(POLL_INTERVAL)
        inbox = BUS.read_inbox(name)
        if inbox:
            messages.append({"role": "user",
                "content": f"<inbox>{inbox}</inbox>"})
            return True
        unclaimed = scan_unclaimed_tasks()
        if unclaimed:
            claim_task(unclaimed[0]["id"], name)
            messages.append({"role": "user",
                "content": f"<auto-claimed>Task #{unclaimed[0]['id']}: "
                           f"{unclaimed[0]['subject']}</auto-claimed>"})
            return True
    return False  # timeout -> shutdown
```

1. Task **board scanning: find pending, unowned, unblocked tasks.**

```python
def scan_unclaimed_tasks() -> list:
    unclaimed = []
    for f in sorted(TASKS_DIR.glob("task_*.json")):
        task = json.loads(f.read_text())
        if (task.get("status") == "pending"
                and not task.get("owner")
                and not task.get("blockedBy")):
            unclaimed.append(task)
    return unclaimed
```

1. **Identity re-injection**: when **context is too short (compression happened)**, **insert an identity block.**

```python
if len(messages) <= 3:
    messages.insert(0, {"role": "user",
        "content": f"<identity>You are '{name}', role: {role}, "
                   f"team: {team_name}. Continue your work.</identity>"})
    messages.insert(1, {"role": "assistant",
        "content": f"I am {name}. Continuing."})
```

## What Changed From s10

| Component     | Before (s10)  | After (s11)                       |
| :------------ | :------------ | :-------------------------------- |
| Tools         | 12            | 14 (+idle, +claim_task)           |
| Autonomy      | Lead-directed | **Self-organizing**               |
| Idle phase    | None          | **Poll inbox + task board**       |
| Task claiming | Manual only   | **Auto-claim unclaimed tasks**    |
| Identity      | System prompt | **+ re-injection after compress** |
| Timeout       | None          | **60s idle -> auto shutdown**     |

1. Type `/tasks` to **see the task board with owners**
2. Type `/team` to **monitor who is working vs idle**

## Execution Flow

![image-20260319200354774](./learn-claude-code%E9%9A%8F%E8%AE%B01/image-20260319200354774.png)

## Design Decisions

### polling for unclaimed tasks instead of event-driven notification

Autonomous teammates poll the shared task board every ~1 second to find unclaimed tasks, **rather than waiting for event-driven notifications**. Polling is **fundamentally simpler than pub/sub: there's no subscription management, no event routing, no missed-event bugs**. **With file-based persistence, polling is just 'read the directory listing'** -- **a cheap operation that works regardless of how many agents are running**. The a pub/sub channel responsiveness (new tasks are discovered quickly) against filesystem overhead (not hammering the disk with reads).

#### Alternatives Considered

Event-driven notification (**file watchers via inotify/fsevents, or a pub/sub channel**) would **reduce latency from seconds to milliseconds**. But **file watchers are platform-specific** and **unreliable across network filesystems**. **A message broker** would work but **adds infrastructure**. For a system where **tasks take minutes to complete**, **discovering new tasks in 1 second instead of 10 milliseconds makes no practical difference**.

## 60s idle timeout before self-termination

When an autonomous teammate has no tasks to work on and no messages in its inbox, it waits up to 60 seconds before giving up and shutting down. This **prevents zombie teammates that wait forever for work that never comes** -- a real problem when the lead **forgets to send a shutdown request, or when all remaining tasks are blocked on external events**. The 60-second window is **long enough that a brief gap between task completions and new task creation** won't cause premature shutdown, but short enough that unused teammates don't waste resources.

#### Alternatives Considered

No timeout (wait forever) risks zombie processes. A very short timeout (5s) causes premature exits when the lead is simply thinking or typing. **A heartbeat system (lead periodically pings teammates to keep them alive) works but adds protocol complexity**. The 60-second fixed timeout is **a good default that balances false-positive exits against resource waste.**

## re-inject teammate identity after context compression

When auto_compact compresses the conversation, the resulting summary **loses crucial metadata: the teammate's name, which team it belongs to, and its agent_id**. Without this information, the teammate can't claim tasks (tasks are owned by name), can't check its inbox (inbox files are keyed by agent_id), and can't identify itself in messages. So after every auto_compact, the system re-injects a structured identity block into the conversation: 'You are [name] on team [team], your agent_id is [id], your inbox is at [path].' This is the minimum context needed for the teammate to remain functional after memory loss.

#### Alternatives Considered

**Putting identity in the system prompt (which survives compression)** would avoid this problem, but **violates the cache-friendly static-system-prompt design from s05**. Embedding identity **in the summary prompt ('when summarizing, always include your name and team') is unreliable** -- the LLM **might omit it**. **Explicit post-compression injection** is **deterministic** and **guaranteed to work**.

## QA

## a pub/sub channel是什么东西,为我精简说明一下

**Pub/Sub** 是 **Publish/Subscribe（发布/订阅）** 的缩写，它是一种极其常用的**异步通信模式**。

你可以把它想象成**微信公众号**或**邮件列表**的工作方式：

### 核心机制

在这个模式中，消息的发送者和接收者是**解耦（不直接联系）**的：

1. **Publisher (发布者)**：只管**把消息发到一个“频道”或“主题”（Topic），它不知道谁会读。**
2. **Topic/Channel (频道)**：一个**中转站，负责接收并分发消息。**
3. **Subscriber (订阅者)**：关注了某个频道的用户。只要**频道有新消息，订阅者就会自动收到。**

------

### 为什么需要它？（对比点）

在你之前的 JSONL 文件例子中，如果 A 想给 B 发消息，A 必须知道 B 的文件路径。而在 **Pub/Sub** 模式下：

- **多对多**：一个发布者**可以发给 100 个订阅者；多个发布者也可以往同一个频道发消息。**
- **实时性**：订阅者通常是**“被动触发”的，消息一到马上就能处理**，**不需要像读文件那样去轮询（Polling）。**
- **削峰填谷**：如果短时间消息太多，**频道（如 Redis 或 RabbitMQ）**可以**起到缓冲作用，防止接收端被冲垮。**

------

### 常见的技术实现

- **Redis**：**轻量**级，适合**实时聊天、通知**。
- **Kafka**：**重量**级，适合**处理海量大数据流。**
- **MQTT**：专门用于**物联网（IoT）设备间的低功耗通信。**

------

**简单一句话总结：** 发布者只管“广播”，订阅者只听“频道”，双方互不相识，由中间人负责传话。

# **s12** Worktree + Task Isolation

*Each works **in its own directory**; **tasks manage goals**, **worktrees manage directories**, bound by ID*

## Worktree Task Isolation

**Single Workspace Pain**

**Two tasks are active,** but **both edits would hit one directory and collide**.

**Allocate Lane for Task 1**

**Create a worktree lane** and **associate it with task 1 for clear ownership.**

**Allocate Lane for Task 2**

Lane creation and task association can be separate. **Here task 2 binds after lane creation.**

**Run Commands in Isolated Lanes**

Each command **routes by selected lane directory**, **not by the shared root.**

**Keep One Lane, Close Another**

Closeout can mix decisions: **keep ui-login active for follow-up**, **remove auth-refactor and complete task 1.**

**Isolation + Coordination + Events**

The board **tracks shared truth**, worktree lanes **isolate execution**, and **events provide auditable side-channel traces.**

*"Each works in its own directory, no interference"* -- **tasks manage goals**, **worktrees manage directories**, **bound by ID**.

## Problem

By s11, agents **can claim and complete tasks autonomously**. But every task runs in one shared directory. **Two agents refactoring different modules at the same time** will **collide**: **agent A edits `config.py`, agent B edits `config.py`**, **unstaged changes mix**, and **neither can roll back cleanly**.

The task board **tracks *what to do* but has no opinion about *where to do it***. The fix: **give each task its own git worktree directory**. **Tasks manage goals**, **worktrees manage execution context**. **Bind them by task ID**.

## Solution

```
Control plane (.tasks/)             Execution plane (.worktrees/)
+------------------+                +------------------------+
| task_1.json      |                | auth-refactor/         |
|   status: in_progress  <------>   branch: wt/auth-refactor
|   worktree: "auth-refactor"   |   task_id: 1             |
+------------------+                +------------------------+
| task_2.json      |                | ui-login/              |
|   status: pending    <------>     branch: wt/ui-login
|   worktree: "ui-login"       |   task_id: 2             |
+------------------+                +------------------------+
                                    |
                          index.json (worktree registry)
                          events.jsonl (lifecycle log)

State machines:
  Task:     pending -> in_progress -> completed
  Worktree: absent  -> active      -> removed | kept
```

## How It Works

1. **Create a task.** Persist the **goal first.**

```python
TASKS.create("Implement auth refactor")
# -> .tasks/task_1.json  status=pending  worktree=""
```

1. **Create a worktree and bind to the task.** **Passing `task_id` auto-advances** the task to `in_progress`.

```python
WORKTREES.create("auth-refactor", task_id=1)
# -> git worktree add -b wt/auth-refactor .worktrees/auth-refactor HEAD
# -> index.json gets new entry, task_1.json gets worktree="auth-refactor"
```

The binding **writes state to both sides:**

```python
def bind_worktree(self, task_id, worktree):
    task = self._load(task_id)
    task["worktree"] = worktree
    if task["status"] == "pending":
        task["status"] = "in_progress"
    self._save(task)
```

1. **Run commands in the worktree.** `cwd` points to the isolated directory.

```python
subprocess.run(command, shell=True, cwd=worktree_path,
               capture_output=True, text=True, timeout=300)
```

1. Close out.**Two choices:**
	- `worktree_keep(name)` -- **preserve** the directory for later.
	- `worktree_remove(name, complete_task=True)` -- **remove** directory, complete the bound task, emit event. **One call handles teardown + completion.**

```python
def remove(self, name, force=False, complete_task=False):
    self._run_git(["worktree", "remove", wt["path"]])
    if complete_task and wt.get("task_id") is not None:
        self.tasks.update(wt["task_id"], status="completed")
        self.tasks.unbind_worktree(wt["task_id"])
        self.events.emit("task.completed", ...)
```

1. **Event stream.** **Every lifecycle step emits to `.worktrees/events.jsonl`:**

```json
{
  "event": "worktree.remove.after",
  "task": {"id": 1, "status": "completed"},
  "worktree": {"name": "auth-refactor", "status": "removed"},
  "ts": 1730000000
}
```

Events emitted: `worktree.create.before/after/failed`, `worktree.remove.before/after/failed`, `worktree.keep`, `task.completed`.

After a crash, state reconstructs from `.tasks/` + `.worktrees/index.json` **on disk**. Conversation memory is volatile; file state is durable.

## What Changed From s11

| Component            | Before (s11)              | After (s12)                                  |
| :------------------- | :------------------------ | :------------------------------------------- |
| Coordination         | Task board (owner/status) | Task board **+ explicit worktree binding**   |
| Execution scope      | Shared directory          | **Task-scoped isolated directory**           |
| Recoverability       | Task status only          | Task status **+ worktree index**             |
| Teardown             | Task completion           | Task completion **+ explicit keep/remove**   |
| Lifecycle visibility | Implicit in logs          | Explicit events in `.worktrees/events.jsonl` |

## Execution Flow

![image-20260319220227631](./learn-claude-code%E9%9A%8F%E8%AE%B01/image-20260319220227631.png)

## Architecture

![image-20260319220314715](./learn-claude-code%E9%9A%8F%E8%AE%B01/image-20260319220314715.png)

## Design Decisions

### shared task board+isolated execution lane

**The task board remains shared** and **centralized in `.tasks/`**, while **file edits happen in per-task worktree directories.** This separation preserves **global visibility (who owns what, what is done)** without forcing everyone to edit inside one mutable directory. Coordination stays simple because there is one board, and execution stays safe because each lane is isolated.

#### Alternatives Considered

**A single shared workspace** is simpler but **causes edit collisions** and **mixed git state**. **Fully independent task stores per lane** avoid collisions but **lose team-level visibility and make planning harder.**

## explicit worktree lifecycle index

`.worktrees/index.json` **records each worktree's name, path, branch, task_id, and status**. This **makes lifecycle state inspectable and recoverable** even after context compression or process restarts. **The index** also provides **a deterministic source for list/status/remove operations.**

#### Alternatives Considered

Relying **only** on `git worktree list` **removes local bookkeeping** but **loses task binding metadata and custom lifecycle states.** Keeping **all state only in memory is simpler** in code but **breaks recoverability.**

## lane-scoped CWD routing+re-entry guard

**Commands are routed to a worktree's directory via `worktree_run(name, command)`** using the `cwd` parameter. A re-entry guard **prevents accidentally running inside an already-active worktree** context, keeping lifecycle ownership unambiguous.

#### Alternatives Considered

**Global cwd mutation is easy to implement** but can **leak context across parallel work**. **Allowing silent re-entry** makes **lifecycle ownership ambiguous** and **complicates teardown behavior.**

### Append-Only Lifecycle event stream

**Lifecycle events are appended** to `.worktrees/events.jsonl` (`worktree.create.*`, `worktree.remove.*`, `task.completed`). This **turns hidden transitions into queryable records** and **makes failures explicit (`*.failed`) instead of silent.**

#### Alternatives Considered

Relying only on console logs is **lighter but fragile** during **long sessions** and hard to audit. **A full event bus infrastructure** is powerful but **heavier than needed** for this teaching baseline.

## close task and workspace together

`worktree_remove(..., complete_task=true)` **allows a single closeout step**: **remove the isolated directory** and mark the bound task completed. Closeout **remains an explicit tool-driven transition** (`worktree_keep` / `worktree_remove`) **rather than hidden automatic cleanup**. This **reduces dangling state** where a task says done but **its temporary lane remains active (or the reverse)**.

#### Alternatives Considered

Keeping closeout fully manual **gives flexibility but increases operational drift**. **Fully automatic** removal on every completion **risks deleting a workspace before final review**.

## event stream is observability side-channel

Lifecycle events **improve auditability**, but the source of truth **remains task/worktree state files**. **Events should be read as transition traces**, not as a replacement state machine.

#### Alternatives Considered

Using logs alone hides structured transitions; using events as the only state source risks drift when replay/repair semantics are undefined.

