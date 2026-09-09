# 这里的表M[N,T]是什么意思,为我解释一下

![image-20260624231934497](./scut%E7%BC%96%E8%AF%91%E5%8E%9F%E7%90%86%E5%A4%8D%E4%B9%A02/image-20260624231934497.png)

在自顶向下语法分析（Top-Down Parsing）特别是 LL(1) 分析法中，**M[N,T]** 代表**预测分析表（Predictive Parsing Table）**。它是驱动整个语法分析器执行匹配动作的“核心控制矩阵”。

### 详细原理解析

#### 1. 符号含义 (Symbol Definitions)

- **M (Matrix / Map)**：代表一个**二维的查询矩阵（表）。**
- **N (Non-terminal)**：**非终结符**。它是**矩阵的行索引（Row Index）。代表当前分析栈（Stack）栈顶即将被展开的语法结构。**
- **T (Terminal)**：**终结符**（**包含输入结束标记 $**）。它是矩阵的**列索引（Column Index）**。代表当前输入流（Input Stream）中正在面临的**向前看符号（Lookahead Symbol）**。

#### 2. 矩阵单元格的值 (Cell Values)

M[N,T] **这个坐标交叉点里存放的内容，是唯一一条确定性的产生式（Production Rule）。** 如果在某个状态下**遇到某个输入字符时不应该有合法的产生式对应，该单元格则为空（或标记为 Error，代表语法错误 Syntax Error）。**

#### 3. 关联之前的知识：表是怎么填出来的？

M[N,T] 的构建完全依赖于之前讲解的 **SELECT 集（预测集）**。 **填表规则**：对于文法中的**每一条产生式 N→α，计算它的 SELECT(N→α) 集合**。如果终结符 a 属于该 SELECT 集，就把**产生式 N→α 填入矩阵的 M[N,a] 单元格中。**

#### 4. 结合图片示例进行映射 (Mapping to the Trace)

图片底部给出了文法 S→(S)S∣ϵ 的分析表定义： M[S,(]=S→(S)S M[S,)]=M[S,$]=S→ϵ

对照图片上半部分的分析过程（Parse Trace）：

- **步骤 1**：**栈顶（Stack）是非终结符 S**，**输入流（Input）面临的字符是 `(`。** *动作逻辑*：**查表 M[S,(]。查出应使用产生式 S→(S)S。**所以栈顶的 S 被替换成了逆序压栈的字符 (S)S。
- **步骤 3**：栈顶是 S，输入流面临字符 `)`。 *动作逻辑*：查表 M[S,)]。查出应使用 S→ϵ。所以执行 S→eps，栈顶 S 直接被弹出（隐身）。
- **步骤 5**：栈顶是 S，输入流面临 $（结束符）。 *动作逻辑*：查表 M[S,$]。查出应使用 S→ϵ，同样将 S 弹出。栈和输入均只剩 $，接受（Accept）。

### 辅助练习题（Exam-Style Practice）

预测分析表是 LL(1) 的大轴戏，考试中往往要求你结合 FIRST 和 FOLLOW 集自己画出这张表，并执行分析栈的操作。

#### 【练习题 1：构造预测分析表 (Constructing Predictive Parsing Table)】

**考查知识点**：将 SELECT 集映射转化为二维分析表的能力。

**题目（Question）**： 已知文法 G: E′→+TE′∣ϵ 且已知这两个产生式的 SELECT 集为： SELECT(E′→+TE′)={+} SELECT(E′→ϵ)={),$} 请构造与非终结符 E′ 相关的预测分析表 M 的对应行（假设终结符全集为 {+,∗,(,),id,$}）。请以数学关系的形式写出该行所有非空和空的单元格定义。

**解答与思路（Answer & Solution）**： *严格按照填表规则：终结符在哪个产生式的 SELECT 集中，就把产生式填入对应的列。*

- 对于列 `+`：+∈SELECT(E′→+TE′)，故 M[E′,+]=E′→+TE′。
- 对于列 `)`：)∈SELECT(E′→ϵ)，故 M[E′,)]=E′→ϵ。
- 对于列 `$`：$∈SELECT(E′→ϵ)，故 M[E′,$]=E′→ϵ。
- 对于其他列 `*`, `(`, `id`：都不在 SELECT 集中，这些单元格为空（Error）。

**最终标准答案**： 非空单元格： M[E′,+]=E′→+TE′ M[E′,)]=E′→ϵ M[E′,$]=E′→ϵ 空单元格（错误标记）： M[E′,∗]=M[E′,(]=M[E′,id]=Error

#### 【练习题 2：使用分析表执行推导动作 (Parsing Trace Execution)】

**考查知识点**：掌握分析栈（Stack）先进后出的特性，以及根据表 M 进行符号替换的过程。

**题目（Question）**： 利用图片中给定的分析表： M[S,(]=S→(S)S M[S,)]=M[S,$]=S→ϵ 请模拟语法分析器，写出对输入串 `()()` 进行分析的前 4 个步骤的栈（Stack）、输入（Input）和动作（Action）。 初始状态：栈内为 `S$`，输入为 `()()$`。 *(注：由于右侧是栈底，产生式右部入栈时应当是逆序的。例如压入 (S)S，实际上栈顶到栈底的顺序是 `(`, `S`, `)`, `S`)*。

**解答与思路（Answer & Solution）**：

- **Step 1**: 栈顶 S，输入 `(`。查 M[S,(]，用 S→(S)S 替换。
- **Step 2**: 栈顶 `(`，输入 `(`。终结符遇终结符，必须相同才能匹配（match）并消耗掉输入字符。
- **Step 3**: 栈顶 S，输入 `)`。查 M[S,)]，用 S→ϵ 替换（直接弹出 S）。
- **Step 4**: 栈顶 `)`，输入 `)`。终结符匹配，消耗输入字符。

**最终标准答案**：

| 步骤 | 栈 (Stack) | 输入 (Input) | 动作 (Action) |
| ---- | ---------- | ------------ | ------------- |
| 1    | S$         | `()()$`      | S→(S)S        |
| 2    | (S)S$      | `()()$`      | match `(`     |
| 3    | S)S$       | `)()$`       | S→ϵ           |
| 4    | )S$        | `)()$`       | match `)`     |



![image-20260624235317465](./scut%E7%BC%96%E8%AF%91%E5%8E%9F%E7%90%86%E5%A4%8D%E4%B9%A02/image-20260624235317465.png)

### 一、 核心概念与数据结构 (Core Concepts & Data Structures)

图片展示的是自底向上语法分析 (Bottom-Up Parsing) 中的 **LR 分析过程。LR 分析器主要由以下三个核心部分构成：**

1. **文法 (Grammar, $G$):** 包含**一组产生式 (Productions)，用于定义语言的语法结构**。图片中给定了4个产生式，并进行了编号 (1)-(4)，这些**编号将在归约 (Reduce) 时被引用。**
2. **栈 (Stack):** 用于**记录分析器的历史状态**。栈中交替存放**状态 (State)** 和 **文法符号 (Grammar Symbol)**。
	- *注：图片中的栈（如 `$0a2A3`）采用了一种紧凑记法，实际上它代表状态 `0`，符号 `a`，状态 `2`，符号 `A`，状态 `3`。栈底标识为 `$`。*
3. **LR 分析表 (LR Parsing Table):** 驱动分析器动作的核心二维矩阵，分为两部分：
	- **ACTION 表:** **列为终结符 (Terminals) 和结束符 `$`;** 用于**决定当前状态面临特定输入符号时应采取的动作。**
	- **GOTO 表:** **列为非终结符 (Non-terminals)**; 用于在**发生归约动作后，决定状态机应当跳转到的新状态。**

### 二、 分析动作的执行机制 (Mechanism of Parsing Actions)

根据 ACTION 表的条目，**分析器会执行以下四种基本动作**（图片中体现了前三种）：

1. **移进 (Shift, 记为 `sN`):**
	- **触发条件:** `ACTION[当前栈顶状态, 当前输入符号] = sN`。
	- **执行操作:** 将**当前的输入符号 (Terminal) 从输入缓冲区移出，并连同新状态 `N` 一起压入栈顶 (Push)。输入指针前移。**
2. **归约 (Reduce, 记为 `rN`):**
	- **触发条件:** `ACTION[当前栈顶状态, 当前输入符号] = rN`。
	- **执行操作 (核心考点):** 按**第 $N$ 个产生式 $A \rightarrow \beta$ 进行归约。**
		1. **弹出 (Pop):** 从栈顶**弹出 $|\beta|$ 个符号及其对应的状态（即弹出产生式右部的长度）。**
		2. **查表 (Lookup):** 弹出后，**暴露出新的栈顶状态，记为 $s'$。查找 `GOTO[s', A]` 得到新状态 $N'$。**
		3. **压栈 (Push):** 将**产生式左部的非终结符 $A$ 及其对应的新状态 $N'$ 压入栈顶。输入指针不移动。**
3. **接受 (Accept, 记为 `acc`):**
	- **触发条件:** **栈中仅剩开始符号，且当前输入为 `$`**，查找 **ACTION 表得到 `acc`。分析成功结束。**
4. **报错 (Error):** ACTION 表中的空白项。表示存在语法错误。

### 三、 结合 Trace 的具体步骤解析 (Trace Walkthrough)

以图片中的几个关键步骤为例，说明上述机制的运转：

- **初始状态:** 栈为 `$0`，**输入串为 `abbcde$`。**
- **第 1 步 (移进示例):** 查 `ACTION[0, a] = s2`。**将 `a` 和状态 `2` 压栈**。栈变为 `$0a2`，输入串变为 `bbcde$`。
- **第 3 步 (归约示例):** 此时栈为 `$0a2b4`，输入串为 `bcde$`。查 `ACTION[4, b] = r2`，即**用产生式 `(2) A -> b` 归约。**
	- 产生式**右部 `b` 长度为 1，因此从栈顶弹出 `b` 和 `4`。**
	- **暴露出的栈顶状态为 `2`。**
	- 查 `GOTO[2, A] = 3`。
	- **将 `A` 和 `3` 压栈。栈更新为 `$0a2A3`，输入串仍为 `bcde$`。**

### 四、 考点实战与自测题 (Exam-Style Questions)

以下题目根据 LR 分析常考的“状态机追踪”与“查表机制”设计，考查深度较高，为必考题型。

**Question 1: Table Lookup and Stack Tracking**

**Question:** Given the grammar $G$ and LR parsing table in the image `image_ced9b9.png`. During the parsing of a specific input string, assume the current parser configuration (分析器格局) is as follows:

- **Stack:** `$ 0 a 2 A 3 c 5`

- **Remaining Input:** `d e $`

	Please detail the next **TWO** parsing actions (including table lookups, stack modifications, and input buffer status).

*(请遮挡下方答案进行自测)*

**答案:**

**第一步 (Step 1):**

- **查表:** 当前栈顶状态为 `5`，当前输入终结符为 `d`。根据 ACTION 表，`ACTION[5, d] = s8`。
- **动作:** 移进 (Shift)。
- **栈与输入更新:** 将输入符号 `d` 与新状态 `8` 压入栈。栈变为 `$0a2A3c5d8`。剩余输入串变为 `e$`。

**第二步 (Step 2):**

- **查表:** 栈顶状态变为 `8`，当前输入终结符为 `e`。根据 ACTION 表，`ACTION[8, e] = r4`。
- **动作:** 归约 (Reduce)。使用产生式 `(4) B -> d` 进行归约。
- **栈与输入更新:**
	1. 产生式 (4) 右部长度为 1，弹出栈顶的 `d8`。
	2. 此时暴露出的栈顶状态为 `5`。查询 GOTO 表，`GOTO[5, B] = 7`。
	3. 将左部非终结符 `B` 与新状态 `7` 压栈。栈变为 `$0a2A3c5B7`。剩余输入串不移动，仍为 `e$`。

**Question 2: Conceptual Understanding of GOTO**

**Question:** In LR parsing (LR 语法分析), under what exact circumstances does the parser consult the `GOTO` table? Explain its mechanical role in maintaining the state machine.

*(请遮挡下方答案进行自测)*

**答案:**

- **触发时机:** 分析器仅在执行 **归约 (Reduce, `rN`)** 操作的中间阶段查阅 `GOTO` 表。在移进 (Shift) 操作中不使用 `GOTO` 表。
- **机制作用:** 当分析器识别出一个句柄 (Handle) 并使用产生式 $A \rightarrow \beta$ 归约时，它需要将已识别的 $\beta$ 所在的子状态序列从栈中抹去（弹出 $|\beta|$ 个元素），从而回退到开始识别该句柄之前的历史状态 $s'$。此时，`GOTO[s', A]` 的作用是相当于执行一次“针对非终结符 $A$ 的状态转移（Shift）”，指导状态机安全地过渡到一个能够继续接受后续语法结构的新状态。

# 那这里面表的每行是什么意思,各个值又是怎么计算出来的呢

### 一、 表的每行含义 (Meaning of Each Row in the Table)

在 LR 分析表中，**每行代表一个状态 (State)**，在理论上对应一个 **项目集闭包 (Item Set Closure)**。

- **项目 (Item)：** 是在**产生式右部的某个位置加上一个圆点 `·` 构成的符号串**。圆点 `·` **反映了语法分析器在当前状态下对该产生式的识别进度 (Parsing Progress)**。圆点**左边的符号表示已经移进或归约好的部分**，圆点**右边的符号表示期望在后续输入中看到的部分。**
- **项目集 (Item Set)：** **处于同一个状态时，可能正在进行的多个产生式识别进度的集合。**例如，状态 0（第 0 行）代表整个分析过程的初始状态，它包含了期望看到文法开始符号的项目。

### 二、 各项数值的计算方法 (How Each Value is Calculated)

分析表中的**每一个单元格（ACTION 表和 GOTO 表的条目）都是通过构建 LR 项目集规范族 (Canonical Collection of LR Items) 并应用特定的填表规则计算得出的**。其核心计算过程包含以下三个核心步骤：

#### 1. 计算项目集闭包 (CLOSURE 函数)

若**一个项目集 $I$ 中包含项目 $A \rightarrow \alpha \cdot B \beta$（圆点后面紧跟非终结符 $B$）**，这意味着**下一步可能要识别由 $B$ 推导出的符号串**。因此，**必须将所有形如 $B \rightarrow \cdot \gamma$ 的初始项目也加入到该项目集中。**

- **计算公式：** 重复上述过程，**直到没有新的项目可以被加入到项目集 $I$ 中为止。**

#### 2. 计算状态转移 (GOTO 函数)

$GOTO(I, X)$ 函数**用于计算当前项目集 $I$ 在读入一个符号 $X$（可以是终结符或非终结符）后，应该转移到的新项目集。**

- **计算方法：** **找到 $I$ 中所有圆点后为 $X$ 的项目（如 $A \rightarrow \alpha \cdot X \beta$）**，将**圆点向后移动一位变成 $A \rightarrow \alpha X \cdot \beta$，然后对这些新项目组成的项目集求闭包**，即：

	$$GOTO(I, X) = CLOSURE(\{A \rightarrow \alpha X \cdot \beta \mid A \rightarrow \alpha \cdot X \beta \in I\})$$

#### 3. 填充分析表条目 (Table Filling Rules)

假设当前计算出**的第 $i$ 个项目集为 $I_i$，对应分析表的第 $i$ 行：**

- **ACTION 移进 (Shift) 项的确定：** 若项目 $A \rightarrow \alpha \cdot a \beta \in I_i$（ $a$ 为终结符 ），且 $GOTO(I_i, a) = I_j$，则**在第 $i$ 行、 $a$ 列填入 `sj`（表示移进并切换到状态 $j$）。**
- **GOTO 跳转 (Goto) 项的确定：** 若 $GOTO(I_i, A) = I_j$（ $A$ 为非终结符 ），**则在第 $i$ 行、 $A$ 列填入状态编号 `j`。**
- **ACTION 归约 (Reduce) 项的确定：** 若项目 $A \rightarrow \alpha \cdot \in I_i$（圆点**已到达产生式末尾，设该产生式编号为 $k$**），则：
	- 在 **SLR(1) 文法** 中：对于**所有属于 $FOLLOW(A)$ 的终结符 $a$，在第 $i$ 行、 $a$ 列填入 `rk`（表示用第 $k$ 个产生式进行归约）。**
- **接受 (Accept) 项的确定：** 若**拓广文法的接收项目 $S' \rightarrow S \cdot \in I_i$，则在第 $i$ 行、结束符 `$` 列填入 `acc`。**

### 三、 结合本题文法的具体状态推导示例 (Concrete Derivation Example)

针对题目中的文法 $G$：

(1) $S \rightarrow aAcBe$

(2) $A \rightarrow b$

(3) $A \rightarrow Ab$

(4) $B \rightarrow d$

我们来**精确推导状态 2（第 2 行）的形成与单元格填充：**

1. **状态 0 读入 `a` 转移：** **状态 0 包含项目 $S \rightarrow \cdot aAcBe$。读入终结符 `a` 后，圆点后移，得到核心项目 $S \rightarrow a \cdot AcBe$。**

2. **求闭包得到状态 2 (Item Set $I_2$)：**

	**由于圆点后是非终结符 $A$，调用 `CLOSURE` 引入 $A$ 的所有产生式开头项目**：

	$$I_2 = \{ S \rightarrow a \cdot AcBe, \; A \rightarrow \cdot b, \; A \rightarrow \cdot Ab \}$$

3. **根据 $I_2$ 填表：**

	- 因为 $A \rightarrow \cdot b \in I_2$，**圆点后为终结符 `b`。计算** $GOTO(I_2, b) = CLOSURE(\{A \rightarrow b \cdot\}) = I_4$。因此**在 第 2 行、`b` 列填入 `s4`。**
	- 因为**圆点后有非终结符 $A$，计算** $GOTO(I_2, A) = CLOSURE(\{S \rightarrow aA \cdot cBe, A \rightarrow A \cdot b\}) = I_3$。因此在 **第 2 行、`A` 列填入 `3`**。

### 四、 考点实战与自测题 (Exam-Style Questions)

**Question 1: Derivation of Reduce Action (归约项的计算)**

**题目:** 假定某一语法分析器使用 SLR(1) 填表规则。已知非终结符 $A$ 的随前符号集 $FOLLOW(A) = \{c, e, \$\}$。在计算项目集规范族时，求得状态 4 (Item Set $I_4$) 的内容仅包含一个项目：$I_4 = \{ A \rightarrow b \cdot \}$。已知产生式 $A \rightarrow b$ 的编号为 (2)。请准确写出分析表第 4 行中，哪些列应当填入动作，具体填入什么值，并说明计算依据。

*(请遮挡下方答案进行自测)*

**答案:**

- **填表结果：** 在第 4 行的 `c` 列、`e` 列和 `$` 列均填入 **`r2`**。**其余列留空（报错）**。
- **计算依据：** 根据 SLR(1) 分析表的构造规则，当一个项目集 $I_i$ 中包含规约项目 $A \rightarrow \alpha \cdot$ 时，分析器不能盲目在所有输入符号下都进行归约，而是必须检查当前的下一个输入符号（向前看符号 Lookahead）。只有当输入符号 $a \in FOLLOW(A)$ 时，才允许执行归约动作。因为 $FOLLOW(A) = \{c, e, \$\}$，且该产生式编号为 (2)，所以在这三列对应的单元格填入 `r2`。

# 为我详细地讲解一下这个LR(0)表完整的构建流程,从头到尾每一步都需要详细地讲清楚,不能有任何省略的地方,我需要完整地理解这整个流程每一步,如果你一次讲不清楚你就分几次讲,不要压缩省略

构建完整的 LR(0) 分析表 (LR(0) Parsing Table) 是自底向上语法分析 (Bottom-Up Parsing) 的核心基础。LR(0) 的核心思想是通过构建一个确定性有限状态自动机 (DFA) 来追踪所有可能的语法分析状态。

为了保证讲解的完整性和精确性，我将使用**一个标准的上下文无关文法 (Context-Free Grammar, CFG) 作为贯穿全流程的示例**，严格按照编译器设计的标准步骤进行推导。

**示例文法 (Grammar $G$):**

(1) $S \rightarrow BB$

(2) $B \rightarrow aB$

(3) $B \rightarrow b$

### 第一步：构造拓广文法 (Step 1: Construct the Augmented Grammar)

**目的:** 确保**语法分析器有且仅有一个唯一的接受状态 (Accepting State)。**

**操作:** 引入一个**新的开始符号 $S'$，并添加一条新的产生式 $S' \rightarrow S$。**

**拓广后的文法 $G'$:**

(0) $S' \rightarrow S$

(1) $S \rightarrow BB$

(2) $B \rightarrow aB$

(3) $B \rightarrow b$

*(注：后续的**归约操作编号基于 (1)-(3)**，**(0) 仅用于接受动作**)*

### 第二步：定义 LR(0) 项目 (Step 2: Define LR(0) Items)

**定义:** 在产生式**右部的某个位置添加一个圆点（`·`）**所**构成的实体称为 LR(0) 项目 (LR(0) Item)**。圆点**左侧表示已经成功匹配的输入部分，右侧表示期望在后续输入中匹配的部分。**

对于**产生式 $B \rightarrow aB$，可产生以下 3 个项目：**

- $B \rightarrow \cdot aB$ （**移进**项目 / Shift Item）
- $B \rightarrow a \cdot B$ （**待约**项目 / Pending Item）
- $B \rightarrow aB \cdot$ （**归约项目 / Reduce Item**）

### 第三步：定义核心函数 CLOSURE 与 GOTO (Step 3: Define CLOSURE & GOTO Functions)

这是构建状态机（DFA）的引擎。

#### 1. 项目集闭包计算 (CLOSURE Function)

如果**已知当前处于某个项目集 $I$，我们需要找出该状态下所有隐式可能发生的项目。**

**算法 (Algorithm):**

1. **将 $I$ 中的所有初始项目加入** $CLOSURE(I)$。
2. 若**项目 $A \rightarrow \alpha \cdot B \beta$ 在 $CLOSURE(I)$ 中（即圆点后是非终结符 $B$）**，则将文法中所有**关于 $B$ 的产生式的初始项目**（形如 $B \rightarrow \cdot \gamma$）加入 $CLOSURE(I)$。
3. 重复步骤 2，**直到没有新的项目可以加入为止。**

#### 2. 状态转移计算 (GOTO Function)

计算处于**状态（项目集）$I$** 时，**读入文法符号 $X$（终结符或非终结符）**后**转移到的新状态。**

**算法 (Algorithm):**

$$GOTO(I, X) = CLOSURE(\{ A \rightarrow \alpha X \cdot \beta \mid A \rightarrow \alpha \cdot X \beta \in I \})$$

*解释：**提取 $I$ 中所有圆点后紧跟 $X$ 的项目**，将**圆点越过 $X$ 向右移动一位，组成一个新的项目集合**，然后**对这个新集合求 $CLOSURE$**。*

### 第四步：构建 LR(0) 项目集规范族 (Step 4: Construct Canonical Collection of LR(0) Item Sets)

这一步就是**完整推导 DFA 的所有状态（从 $I_0$ 开始）。**

**状态 $I_0$ 的推导:**

**初始项目**集**从拓广文法的起始项目开始：$I_0 = CLOSURE(\{ S' \rightarrow \cdot S \})$**

- **圆点后是 $S$，展开 $S$ 的产生式：加入 $S \rightarrow \cdot BB$。**

- 现在有 $S \rightarrow \cdot BB$**，圆点后是 $B$，展开 $B$ 的产生式：加入 $B \rightarrow \cdot aB$ 和 $B \rightarrow \cdot b$。**

	**$I_0$ 完整内容:**

	$I_0$:

	$S' \rightarrow \cdot S$

	$S \rightarrow \cdot BB$

	$B \rightarrow \cdot aB$

	$B \rightarrow \cdot b$

**通过 $I_0$ 计算后继状态 ($I_1$ 至 $I_4$):**

- **$GOTO(I_0, S)$** = $CLOSURE(\{ S' \rightarrow S \cdot \})$ = **$I_1$** (**接受状态**)

- **$GOTO(I_0, B)$** = $CLOSURE(\{ S \rightarrow B \cdot B \})$ -> (**圆点后是 $B$，需展开 $B$)**

	- **$I_2$**:

		$S \rightarrow B \cdot B$

		$B \rightarrow \cdot aB$

		$B \rightarrow \cdot b$

- **$GOTO(I_0, a)$** = $CLOSURE(\{ B \rightarrow a \cdot B \})$ -> (圆点后是 $B$，需展开 $B$)

	- **$I_3$**:

		$B \rightarrow a \cdot B$

		$B \rightarrow \cdot aB$

		$B \rightarrow \cdot b$

- **$GOTO(I_0, b)$** = $CLOSURE(\{ B \rightarrow b \cdot \})$ = **$I_4$** (**归约状态**)

**继续计算 $I_2$ 的后继状态:**

- **$GOTO(I_2, B)$** = $CLOSURE(\{ S \rightarrow BB \cdot \})$ = **$I_5$** (**归约状态**)
- **$GOTO(I_2, a)$** = 目标集合为 $\{ B \rightarrow a \cdot B, B \rightarrow \cdot aB, B \rightarrow \cdot b \}$。这与 **$I_3$** 完全相同，因此指向 $I_3$。
- **$GOTO(I_2, b)$** = 目标集合为 $\{ B \rightarrow b \cdot \}$。这与 **$I_4$** 完全相同，指向 $I_4$。

**继续计算 $I_3$ 的后继状态:**

- **$GOTO(I_3, B)$** = $CLOSURE(\{ B \rightarrow aB \cdot \})$ = **$I_6$** (归约状态)
- **$GOTO(I_3, a)$** = 指向 **$I_3$**。
- **$GOTO(I_3, b)$** = 指向 **$I_4$**。

至此，没有任何新的状态产生，规范族 $\mathcal{C} = \{I_0, I_1, I_2, I_3, I_4, I_5, I_6\}$ 构建完成。

### 第五步：构造 LR(0) 分析表 (Step 5: Constructing the ACTION and GOTO Tables)

**这是最关键的填表规则（Table Filling Rules），也是考试易错点。LR(0) 的归约规则与 SLR(1) 截然不同。**

对于每一个状态 $I_i$（对应表的第 $i$ 行）：

1. **移进 (Shift, sN):** 若 $A \rightarrow \alpha \cdot a \beta \in I_i$ 且 $GOTO(I_i, a) = I_j$ ($a$ 为终结符)，则在 `ACTION[i, a]` 填入 `sj`。
2. **跳转 (Goto, N):** 若 $GOTO(I_i, X) = I_j$ ($X$ 为非终结符)，则在 `GOTO[i, X]` 填入 `j`。
3. **接受 (Accept, acc):** 若 $S' \rightarrow S \cdot \in I_i$，则在 `ACTION[i, $]` 填入 `acc`。
4. **归约 (Reduce, rN) 【LR(0) 核心特征】:** 若项目集 $I_i$ 中存在一个完整的归约项目 $A \rightarrow \alpha \cdot$（设该产生式编号为 $k$），则对于 **所有的终结符** $a \in T \cup \{\$\}$，在 `ACTION[i, a]` 中填入 `rk`。**（注：不计算 FOLLOW 集，直接填满整行）**

**根据上述规则，填充我们的示例表：**

| **状态 (State)** | **ACTION: a** | **ACTION: b** | **ACTION: $** | **GOTO: S** | **GOTO: B** |
| ---------------- | ------------- | ------------- | ------------- | ----------- | ----------- |
| **0**            | s3            | s4            |               | 1           | 2           |
| **1**            |               |               | acc           |             |             |
| **2**            | s3            | s4            |               |             | 5           |
| **3**            | s3            | s4            |               |             | 6           |
| **4**            | r3            | r3            | r3            |             |             |
| **5**            | r1            | r1            | r1            |             |             |
| **6**            | r2            | r2            | r2            |             |             |

- **行 0, 2, 3 解析:** 这些状态包含形如 $\cdot a$ 或 $\cdot b$ 的移进项目，因此填入 `s3` 和 `s4`。
- **行 4 解析:** $I_4$ 仅包含 $B \rightarrow b \cdot$。由于这是 LR(0) 分析表，在该行的所有终结符列 (a, b, $) 均填入 `r3`。
- **行 5 解析:** $I_5$ 仅包含 $S \rightarrow BB \cdot$。在 a, b, $ 列均填入 `r1`。
- **行 6 解析:** $I_6$ 仅包含 $B \rightarrow aB \cdot$。在 a, b, $ 列均填入 `r2`。

*(如果填表过程中，某个单元格既需要填移进，又需要填归约，或者需要填多个归约，则说明该文法不是 LR(0) 文法，产生了冲突 Conflict。)*

### 考点实战与自测题 (Exam-Style Questions)

**Question 1: Computation of CLOSURE**

**Question:** Given the augmented grammar:

(0) $S' \rightarrow S$

(1) $S \rightarrow L = R$

(2) $S \rightarrow R$

(3) $L \rightarrow * R$

(4) $L \rightarrow id$

(5) $R \rightarrow L$

Assume a parser state $I_x$ contains only the initial item derived from state 0 after reading the symbol `$=$`. That is, the core (核心项目) of $I_x$ is $\{ S \rightarrow L = \cdot R \}$.

Please compute the complete $CLOSURE(I_x)$ by listing all items it contains.

*(请遮挡下方答案作答自检)*

**答案:**

$CLOSURE(I_x)$ 包含以下 4 个项目：

1. $S \rightarrow L = \cdot R$ (核心项目)
2. $R \rightarrow \cdot L$ (因为圆点后是非终结符 $R$)
3. $L \rightarrow \cdot * R$ (因为通过上一步引入了 $L$，且圆点后是 $L$)
4. $L \rightarrow \cdot id$ (同上，引入 $L$ 的另一条产生式)

**Question 2: Conflict Identification in LR(0) (LR(0) 冲突识别)**

**Question:** Suppose in an LR(0) DFA construction for a grammar, you generated a state $I_5$ containing the following two items:

$I_5 = \{ A \rightarrow c \cdot d, B \rightarrow c \cdot \}$

Based on the strictly defined table-filling rules of LR(0) (LR(0) 填表规则), explain what error will occur in the ACTION table at row 5, and specify the formal technical term for this error.

*(请遮挡下方答案作答自检)*

**答案:**

- **发生的错误:** 根据 LR(0) 规则，由于存在移进项目 $A \rightarrow c \cdot d$，在 `ACTION[5, d]` 处应当填入移进操作（假设转移到状态 $j$，填入 $sj$）。同时，由于存在归约项目 $B \rightarrow c \cdot$，LR(0) 规则要求在该行的**所有终结符列**（包括 $d$ 列）填入归约操作（假设编号为 $k$，填入 $rk$）。因此，`ACTION[5, d]` 单元格需要同时填入移进和归约动作，导致分析表无法唯一确定动作。
- **专业术语:** 这种现象被称为 **移进-归约冲突 (Shift-Reduce Conflict)**。这证明该文法不是 LR(0) 文法。

# 详细地解释一下这里的GOTO到底是怎么求的

图片中的公式定义了 **LR 语法分析中构建状态机 (Deterministic Finite Automaton, DFA)** 的核心引擎之一：**状态转移函数 (State Transition Function, $GOTO$)**。

它精确描述了：当**分析器处于状态（项目集）$I$ 时，如果接收到一个文法符号 $X$，分析器应当如何计算并转移到下一个确定的状态**。

以下是对该公式的严格拆解与执行流程讲解。

### 一、 公式符号拆解 (Deconstruction of the Formula)

公式整体为：

$$GOTO(I, X) = CLOSURE(\{A \rightarrow \alpha X \cdot \beta \mid A \rightarrow \alpha \cdot X \beta \in I\})$$

我们将其分为四个关键部分来理解：

1. **$I$ 与 $X$ (输入参数):**
	- $I$: 当前所处的**项目集 (Item Set)**，代表**分析器当前的状态。**
	- $X$: 接收到的**文法符号 (Grammar Symbol)**。它可以是**终结符 (Terminal)**，也可以是**非终结符 (Non-terminal)**。
2. **$A \rightarrow \alpha \cdot X \beta \in I$ (筛选条件 / Filter Condition):**
	- 这是**集合构建的先决条件**。它的意思是：**在当前的项目集 $I$ 中，找出所有圆点 `·` 恰好位于符号 $X$ 前面的项目。**
	- **物理意义:** 这些项目表示**分析器在当前状态下，正在“期待”识别出一个符号 $X$。**
3. **$A \rightarrow \alpha X \cdot \beta$ (移进操作 / Shift Operation):**
	- 对于上一步找出的每一个符合条件的项目，**将圆点 `·` 越过 $X$ 向右移动一个位置。**
	- **物理意义:** 圆点**移过 $X$，代表分析器已经成功接收（或归约出）了符号 $X$，该产生式的识别进度向前推进了一步**。由这些**移位后的新项目构成的集合，被称为新状态的核心项目集 (Core Items)。**
4. **$CLOSURE(\dots)$ (求闭包 / Closure Computation):**
	- 对上述**生成的“核心项目集”调用闭包函数。**
	- **物理意义:** 核心项目只是**状态的“骨架”。如果圆点移位后，恰好落在了一个非终结符的前面（即 $\beta$ 以非终结符开头）**，说明**接下来需要展开识别这个新的非终结符。**$CLOSURE$ 操作**会将该非终结符的所有初始项目（形如 $B \rightarrow \cdot \gamma$）拉取**进来，从而**补全整个新状态的所有可能路径。**

### 二、 GOTO 计算的标准化三步流程 (Standard 3-Step Execution Flow)

在考场上计算 $GOTO(I, X)$ 时，请严格按照以下三步执行，绝不可跳步：

- **Step 1: 扫描与提取 (Scan and Extract)**

	遍历项目集 $I$，**提取所有形如 $A \rightarrow \alpha \cdot X \beta$ 的项目。**如果找不到任何这样的项目，则转移为空（在 ACTION/GOTO 表中表现为空白报错项）。

- **Step 2: 核心项目移位 (Shift Core Items)**

	将 Step 1 提取出的项目中的**圆点 `·` 移到 $X$ 的右侧，**得到集合 $J = \{A \rightarrow \alpha X \cdot \beta, \dots\}$。

- **Step 3: 闭包展开 (Expand via CLOSURE)**

	计算 $CLOSURE(J)$。检查集合 $J$ 中的**每个项目，如果存在圆点后紧跟非终结符 $Y$ 的情况（即 $\dots \cdot Y \dots$），则将 $Y$ 的所有产生式以 $Y \rightarrow \cdot \gamma$ 的形式加入集合**，直到没有新的项目可以加入为止。最终得到的集合即为目标状态。

### 三、 实例推主演示 (Concrete Trace Example)

假设我们有以下文法：

(1) $E \rightarrow T + E$

(2) $E \rightarrow T$

(3) $T \rightarrow \textbf{id}$

已知当前处于状态 $I_0$：

$$I_0 = \begin{cases} E' \rightarrow \cdot E \\ E \rightarrow \cdot T + E \\ E \rightarrow \cdot T \\ T \rightarrow \cdot \textbf{id} \end{cases}$$

**任务: 计算 $GOTO(I_0, T)$**

- **Step 1 提取:** 在 $I_0$ 中寻找圆点后是 $T$ 的项目。

	找到两个: $E \rightarrow \cdot T + E$ 和 $E \rightarrow \cdot T$。

- **Step 2 移位:** 将圆点**越过 $T$。**

	得到核心项目集: $J = \{ E \rightarrow T \cdot + E, \; E \rightarrow T \cdot \}$。

- **Step 3 求闭包:** 计算 $CLOSURE(J)$。

	观察 $J$ 中的项目：

	- $E \rightarrow T \cdot + E$：圆点后是终结符 `+`，不需要展开。

	- $E \rightarrow T \cdot$：圆点已经到达末尾，不需要展开。

		因此，$CLOSURE(J)$ **没有引入任何新项目。**

		**结果:** $GOTO(I_0, T) = \{ E \rightarrow T \cdot + E, \; E \rightarrow T \cdot \}$。

### 四、 考点实战与自测题 (Exam-Style Questions)

以下题目针对 GOTO 函数的运算流程进行考察，主要测试“移位后求闭包”的易错点。

**Question 1: Computation of GOTO with Complex Closure (带复杂闭包展开的 GOTO 计算)**

**文法 (Grammar):**

(1) $S \rightarrow A B$

(2) $A \rightarrow a A$

(3) $A \rightarrow b$

(4) $B \rightarrow c B$

(5) $B \rightarrow d$

**已知当前状态 (Current State) $I_x$:**

$S \rightarrow \cdot A B$

$A \rightarrow \cdot a A$

$A \rightarrow \cdot b$

**Question:** 请严格按照推导步骤，计算 $GOTO(I_x, A)$ 的完整项目集合。

*(请遮盖下方答案作答自检)*

**答案与解析:**

- **Step 1 提取:** 在 $I_x$ 中，寻找圆点后为 $A$ 的项目。

	仅有一个：$S \rightarrow \cdot A B$。

- **Step 2 移位:** 将圆点越过 $A$。

	得到核心项目集 $J = \{ S \rightarrow A \cdot B \}$。

- **Step 3 求闭包 (关键易错点):** 计算 $CLOSURE(J)$。

	检查集合 $J$ 中的项目 $S \rightarrow A \cdot B$。由于圆点后面紧跟着**非终结符 $B$**，必须根据文法展开 $B$ 的所有产生式，并将其初始项目加入集合中。

	文法中 $B$ 的产生式为 $B \rightarrow c B$ 和 $B \rightarrow d$。

	因此加入：$B \rightarrow \cdot c B$ 和 $B \rightarrow \cdot d$。

	此时圆点后分别为终结符 `c` 和 `d`，无需继续展开。

**最终结果:**

$$GOTO(I_x, A) = \begin{cases} S \rightarrow A \cdot B \\ B \rightarrow \cdot c B \\ B \rightarrow \cdot d \end{cases}$$

**Question 2: GOTO on Terminals vs. Non-terminals (关于终结符与非终结符的 GOTO 概念辨析)**

**Question:** 在构造 LR 分析表时，$GOTO(I, X)$ 计算出的新状态，会根据 $X$ 是终结符 (Terminal) 还是非终结符 (Non-terminal)，填入分析表的不同区域。请简述这两种情况在分析表 (ACTION 表与 GOTO 表) 中的具体填表映射规则。

*(请遮盖下方答案作答自检)*

**答案与解析:**

- **情况 1: $X$ 为终结符 (Terminal, 设为 $a$)。**

	如果 $GOTO(I_i, a) = I_j$，这代表一次**移进动作 (Shift)**。此时需要在分析表的 **ACTION 表** 的第 $i$ 行、列 $a$ 的单元格中填入 `sj` (即 Shift to state $j$)。

- **情况 2: $X$ 为非终结符 (Non-terminal, 设为 $A$)。**

	如果 $GOTO(I_i, A) = I_j$，这代表在发生归约 (Reduce) 后，状态机返回历史状态时应当进行的**状态跳转动作**。此时需要在分析表的 **GOTO 表** 的第 $i$ 行、列 $A$ 的单元格中直接填入状态编号 `j`。

# 移进,规约,接受到底怎么去判断的

在 LR 语法分析的**运行时 (Runtime)**，分析器(Parser) **究竟是如何“判断”该执行移进、归约还是接受的？**

其核心决策机制是一个**简单的函数映射。分析器在任何时刻的决策，仅取决于两个离散变量：**

1. **当前栈顶状态 (Current Top State, 设为 $s$)**
2. **当前向前看输入符号 (Lookahead Input Symbol, 设为 $a$)**

分析器通过**查询二维矩阵 `ACTION[s, a]` 的值来做出判断**。以下是这三种动作的具体判定条件、执行逻辑以及其背后的编译原理学依据。

### 一、 移进 (Shift) 的判定与执行

**1. 判定条件：**

当**查询 `ACTION[s, a]` 得到的结果是一个形如 `sN` (例如 `s2`, `s5`) 的值时**，分析器**判断应当执行“移进”。**

**2. 编译原理依据 (为什么在这个状态允许移进？):**

这说明**在构建 DFA 时，状态 $s$ 对应的项目集 (Item Set) 中，一定存在一个形如 $A \rightarrow \alpha \cdot a \beta$ 的项目。**

- **物理意义：** 分析器**当前正在识别产生式 $A$，并且它“期望”在输入流中看到的下一个符号恰好就是终结符 $a$**。既然**当前的输入符号真的是 $a$，匹配成功，因此允许将 $a$ 吞入。**

**3. 动作执行 (Action):**

1. 将**输入流中的符号 $a$ 从输入缓冲区移除（指针前移）。**
2. 将**符号 $a$ 压入栈中。**
3. 将**表项指示的新状态 $N$ 压入栈顶。**

### 二、 归约 (Reduce) 的判定与执行 (核心难点)

**1. 判定条件：**

当查询 `ACTION[s, a]` 得到的**结果是一个形如 `rK` (例如 `r2`, `r4`) 的值时，分析器判断应当执行“归约”。（其中 $K$ 是文法产生式的固定编号）。**

**2. 编译原理依据 (为什么在这个状态允许归约？):**

这说明状态 $s$ 的**项目集中，存在一个形如 $B \rightarrow \gamma \cdot$ 的归约项目 (Reduce Item)**，且**圆点已经到达了产生式的最末尾。**

- **物理意义：** 分析器在**栈中已经完整地识别出了产生式 $K$ 的右部 $\gamma$**。这个在**栈顶形成的、与产生式右部完全匹配的子串，在编译原理中被称为 句柄 (Handle)**。既然**句柄已经形成，分析器判断此时应当将这个局部结构“折叠”回它的左部非终结符 $B$**。

**3. 动作执行 (Action) [必须严格遵守的 4 步]:**

假设**按产生式 (K): $B \rightarrow X_1 X_2 \dots X_n$ 进行归约**，右部长度为 $n$：

1. **出栈 (Pop):** 从栈顶**弹出 $n$ 个状态和 $n$ 个符号（即把整个句柄抹去）。**
2. **暴露历史状态:** 弹出后，栈顶会**暴露出一个历史状态，记为 $s'$。**
3. **查 GOTO 表:** 查询 `GOTO[s', B]`，获**取一个新的状态 $N'$。**
4. **入栈 (Push):** 将**左部非终结符 $B$ 及其对应的新状态 $N'$ 压入栈顶**。**注意：此时输入流指针不移动，当前输入符号 $a$ 保留，用于下一次判断。**

### 三、 接受 (Accept) 的判定与执行

**1. 判定条件：**

当查询 `ACTION[s, a]` 得到的**结果是 `acc` (Accept) 时。这通常发生在输入符号 $a$ 为结束符 `$` 时。**

**2. 编译原理依据:**

此时**栈顶状态 $s$ 必定包含拓广文法的接收项目：$S' \rightarrow S \cdot$。**

- **物理意义：** **圆点走到了开始符号 $S$ 的后面**，并且**所有的输入串都已读取完毕（遇到 `$`）**。这意味着整个输入文件已经**成功地从底向上、完完整整地归约成了文法开始符号 $S$**。语法分析大功告成。

**3. 动作执行 (Action):**

停止分析器，报告语法分析成功。

*(补充：如果查表 `ACTION[s, a]` 得到的是空白，判断为**报错 (Error)**，意味着在当前状态下遇到了语法不允许的输入符号。)*

### 四、 考点实战与自测题 (Exam-Style Questions)

以下题目考查“归约判断”与其引发的栈操作的严格联动，这是填空题和过程追踪题的必考点。

**Question 1: Parsing Action Execution (分析动作的执行追踪)**

**已知条件 (Given):**

- 文法产生式 (2) **为：$E \rightarrow T * F$ (注：右部长度为 3)**
- **当前栈状态 (Stack): `$ 0 id 5 T 2 * 7 F 10`  *(注：栈顶状态为 10)***
- 剩余输入串 (Remaining Input): `+ id $`
- 分析表数据: `ACTION[10, +] = r2`, `GOTO[2, E] = 8`

**Question:**

分析器进行当前这一步的判断依据是什么？执行完这一步动作后，**新的栈状态**和**新的剩余输入串**分别是什么？请使用规范术语作答。

*(请遮挡下方答案进行自测)*

**答案与解析:**

- **判断依据:** 分析器查看当前栈顶状态为 `10`，当前向前看符号 (Lookahead symbol) 为 `+`。查询分析表得到 `ACTION[10, +] = r2`，因此判断执行 **按产生式 (2) 归约 (Reduce by production 2)**。栈顶形成了句柄 $T * F$。
- **执行过程:**
	1. 产生式 $E \rightarrow T * F$ 右部长度为 3。因此从栈顶弹出 3 组符号和状态：弹出 `F 10`，弹出 `* 7`，弹出 `T 2`。
	2. 此时栈中暴露出的栈顶状态为 `5`。
	3. 将产生式左部非终结符 $E$ 压栈。
	4. 查表获取新状态（题目给出 `GOTO[2, E] = 8` 是干扰项，实际暴露状态是 5。假设实际表中存在 `GOTO[5, E] = 9`，则新状态为 9。**在此题中，你需要敏锐地指出弹出后暴露的是状态 5，并查询 GOTO[5, E]**。假设 `GOTO[5, E] = 9`）。
- **最终结果:**
	- **新栈状态:** `$ 0 id 5 E 9`
	- **新剩余输入串:** `+ id $` (执行归约时不消耗输入符号)。

# 闭包编号怎么编号的,有没有顺序规定?

关于 LR 分析中项目集闭包（状态）的编号顺序，直接的结论是：**理论上没有任何严格的强制规定，但在算法实现和考试规范中，通常遵循“广度优先 (Breadth-First Search, BFS) 的发现顺序”。**

以下是详细的专业解释与实战建议：

### 一、 编号的本质与规则 (Nature and Rules of Numbering)

1. **初始状态的唯一性 (Uniqueness of Initial State):**
	- **规则:** 由**拓广文法的起始项目 $S' \rightarrow \cdot S$ 求闭包得到的第一个项目集**，**必须且永远被编号为 $I_0$ (或状态 0)**。这是分析的唯一入口。
2. **新状态的分配 (Allocation of New States):**
	- **规则:** 编号**仅仅是一个自增的标识符 (Identifier)。**当你计算 $GOTO(I_i, X)$ 时：
		- 如果得到的结果是一个**全新的、之前从未出现过**的项目集，你就按照当前未使用的最小自然数给它分配一个新的编号（例如 $I_1, I_2, I_3 \dots$）。
		- 如果得到的结果与之前已经存在的某个项目集 $I_k$ 内容**完全相同**，则不分配新编号，直接让状态转移指向已存在的 $I_k$。
3. **理论无关性 (Theoretical Independence):**
	- **LR 状态机本质上是一个确定的有限自动机 (DFA)，即一个有向图**。图的拓扑结构（节点之间的连接关系）才是核心。
	- 你把某个状态称为 $I_2$ 还是 $I_5$，仅仅是改变了节点的名字，它**绝对不会改变**状态机的识别逻辑。在 LR 分析表中，这仅仅表现为**两行数据的互换**，以及其它单元格中指向这两个状态的**数字的替换**。

### 二、 考试与手算时的推荐顺序 (Recommended Order for Manual Calculation)

虽然理论上随意编号不影响正确性，但为了在考场上**防止漏算状态**并且**方便检查**，强烈建议你采用以下系统化的**广度优先 (BFS)** 策略：

1. **按状态顺序计算:** 永远**先算完 $I_0$ 的所有后继状态**，再算 $I_1$ 的所有后继状态，依此类推。不要跳跃（例如不要算了 $I_0$ 到 $I_1$，马上顺着去算 $I_1$ 的后继）。
2. **按符号顺序计算:** 在处理某一个特定状态 $I_i$ 的 GOTO 时，**按照固定的文法符号顺序进行提取和移位。**
	- *推荐标准:* 先处理**非终结符 (Non-terminals)**（按字母表或产生式顺序），再处理**终结符 (Terminals)**（按字母表顺序）。

**举例说明 (Example):**

假设 $I_0$ 中圆点后有非终结符 $S, B$ 和终结符 $a, b$。

- 先算 $GOTO(I_0, S)$ **发现新状态**，命名为 $I_1$。

- 再算 $GOTO(I_0, B)$ 发现新状态，命名为 $I_2$。

- 再算 $GOTO(I_0, a)$ 发现新状态，命名为 $I_3$。

- 再算 $GOTO(I_0, b)$ 发现新状态，命名为 $I_4$。

	此时 $I_0$ 处理完毕。接下来严格按照顺序，从 $I_1$ 开始处理它的 GOTO，产生的新状态继续命名为 $I_5, I_6 \dots$。

### 三、 考点实战与自测题 (Exam-Style Questions)

**Question: Impact of State Renumbering (状态重编号的影响)**

**Question:** 假设在构建某个文法的 LR(0) 分析表时，学生甲得到的 DFA 中，状态 $I_3$ 仅包含移进项目并向后转移；状态 $I_4$ 是仅包含 $A \rightarrow b \cdot$ 的归约状态。

学生乙在计算时，由于处理符号的顺序不同，他将甲的 $I_3$ 编号为了 $I_4$，将甲的 $I_4$ 编号为了 $I_3$。其他状态编号完全一致。

请问：在学生乙最终写出的 LR(0) 分析表中，相比于学生甲的表，会发生哪些具体的物理位置和数值的变化？（列出至少两点核心变化）

*(请遮挡下方答案作答自检)*

**答案:**

1. **行的互换 (Row Swapping):** 学生乙的分析表中，第 3 行和第 4 行的整行数据，将与学生甲的第 4 行和第 3 行的数据完全对调。

2. **转移目标数值的替换 (Replacement of Target Values):** 在整个分析表（包括 ACTION 表和 GOTO 表）的所有其他行中：

	- 原本填有移进动作 `s3` 的单元格，将全部变为 `s4`。

	- 原本填有移进动作 `s4` 的单元格，将全部变为 `s3`。

	- 原本在 GOTO 表中填有数字 `3` 的单元格，将变为 `4`。

	- 原本在 GOTO 表中填有数字 `4` 的单元格，将变为 `3`。

		*(注：归约动作 `rN` 中的 `N` 是产生式编号，与状态编号无关，因此原表中的 `rK` 动作本身不发生任何数字变化，仅仅是随着行跟着移动了位置。)*

# 怎么标出接受状态和规约状态的

在 LR 语法分析中，判断一个状态是**接受状态 (Accepting State)** 还是**归约状态 (Reducing State)**，唯一的判定标准是：**检查该状态（项目集）中“圆点 `·`”的位置，以及产生式左部的非终结符是什么。**

以下是严格对应您提供的图片中 $I_1$、$I_4$、$I_5$ 状态的详细判定依据。

### 一、 接受状态 (Accepting State) 的判定规则

**1. 判定标准：**

当且仅当**一个项目集中包含了拓广文法的接收项目 (Accepting Item of Augmented Grammar) 时，该状态被称为接受状态。**

接收项目的**固定形式为：$S' \rightarrow S \cdot$ （其中 $S'$ 是人为引入的新开始符号，$S$ 是原开始符号）。****

**2. 结合图片解析 ($I_1$)：**

图片中：$GOTO(I_0, S) = CLOSURE(\{S' \rightarrow S \cdot\}) = I_1$ (接受状态)

- **分析：** 在**状态 $I_1$ 中，圆点 `·` 已经移动到了原开始符号 $S$ 的后面。**
- **物理意义：** 这意味着**分析器已经成功地将整个输入串自底向上归约成了完整的开始符号 $S$。**此时任务完成，因此 $I_1$ **被标记为接受状态**。在填表时，**会在 `ACTION[1, $]` 处填入 `acc`。**

### 二、 归约状态 (Reducing State) 的判定规则

**1. 判定标准：**

如果一个项目集中包含形如 **$A \rightarrow \alpha \cdot$** 的项目（注意：**$A$ 必须是普通非终结符，不能是拓广后的 $S'$**），即**圆点 `·` 已经到达了产生式右部的最末尾**，该项目被称为**归约项目 (Reduce Item)**。**包含归约项目的状态通常被称为归约状态。**

- *(注：在严格的 LR(0) 文法中，**如果一个状态是归约状态，那么该项目集只能包含这一个归约项目**，**不能有其他移进项目，否则会产生冲突**。)*

**2. 结合图片解析 ($I_4$ 与 $I_5$)：**

- **图片中的 $I_4$：** $GOTO(I_0, b) = CLOSURE(\{B \rightarrow b \cdot\}) = I_4$ (归约状态)
	- **分析：** 圆点 `·` 在终结符 $b$ 的后面，且是产生式最末尾。
	- **物理意义：** 分析器在栈顶找到了完整的句柄 `b`，可以将其“打包”替换为左部符号 $B$。
- **图片中的 $I_5$：** $GOTO(I_2, B) = CLOSURE(\{S \rightarrow BB \cdot\}) = I_5$ (归约状态)
	- **分析：** 圆点 `·` 在两个 $B$ 的后面，即产生式 $S \rightarrow BB$ 的最末尾。
	- **物理意义：** 分析器在栈顶找到了完整的句柄 `BB`，可以执行归约动作替换为 $S$。

### 三、 对比：移进 / 待约状态 (Shift / Pending State)

为了更全面地理解，我们对比一下图片中**没有**被标为归约或接受的 $I_2$ 和 $I_3$：

- **图片中的 $I_2$：** 包含 $S \rightarrow B \cdot B$ 和 $B \rightarrow \cdot aB$ 等。圆点 `·` **都在符号的前面或中间**，没有走到任何产生式的末尾。这意味着**句柄尚未完整形成，分析器必须继续读入或推导，执行移进 (Shift) 或状态转移**，所以它们不是归约状态。

### 四、 考点实战与自测题 (Exam-Style Questions)

**Question 1: Item Classification and Table Action (项目分类与动作映射)**

**已知:** 某一 LR 分析过程中的状态 $I_8$ 完整项目集如下：

$I_8 = \{ E \rightarrow T * F \cdot \}$ (已知此产生式在文法中编号为 (3))

**Question:**

1. 请问 $I_8$ 属于什么状态？（移进状态、归约状态、还是接受状态？）判断依据是什么？
2. 在构建 **LR(0)** 分析表时，针对状态 8，你应该在 ACTION 表的哪些列填入什么具体的动作值？

*(请遮盖下方答案作答自检)*

**答案:**

1. **状态分类:** $I_8$ 是**归约状态 (Reducing State)**。判断依据是项目 $E \rightarrow T * F \cdot$ 中的圆点位于产生式右部的最末尾，这是一个完整的归约项目，且左部 $E$ 不是拓广文法的新开始符号 $S'$。
2. **填表动作:** 在构建 LR(0) 分析表时，应当在 ACTION 表的第 8 行的**所有终结符列**（包括结束符 `$`）中，均填入 **`r3`** (表示按第 3 条产生式归约)。

**Question 2: Differentiating Accept and Reduce (接受与归约的辨析)**

**Question:** 假设文法的开始符号为 $S$。如果一个状态中仅包含项目 $S \rightarrow a B c \cdot$，该状态是接受状态吗？为什么？

*(请遮盖下方答案作答自检)*

**答案:**

**不是接受状态。** 它是一个**归约状态**。

**原因:** 只有包含 $S' \rightarrow S \cdot$（拓广文法的起始项目，且圆点在末尾）的状态才是唯一的接受状态。项目 $S \rightarrow a B c \cdot$ 只是原开始符号 $S$ 的一个普通产生式识别完成。当分析器利用该产生式执行完归约动作，将 $aBc$ 替换为 $S$ 并压入栈后，由于暴露出的栈底状态（必定是 $I_0$）与 $S$ 会触发 GOTO 转移，才能最终转移到包含 $S' \rightarrow S \cdot$ 的真正接受状态（即 $I_1$）。

# 为我详细地讲解一下这个SLR(1)表完整的构建流程,从头到尾每一步都需要详细地讲清楚,不能有任何省略的地方,我需要完整地理解这整个流程每一步,如果你一次讲不清楚你就分几次讲,不要压缩省略

构建 **SLR(1) (Simple LR(1))** 分析表是编译原理中极为重要的一环。它直接**建立在 LR(0) 的基础之上**，旨在**解决 LR(0) 过于“粗暴”的归约策略所导致的移进-归约冲突 (Shift-Reduce Conflict) 和 归约-归约冲突 (Reduce-Reduce Conflict)。**

为了让您彻底理解，我们将**引入一个在 LR(0) 下会产生冲突，但可以被 SLR(1) 成功解决的经典微型文法**，并以此贯穿这 5 个标准构建步骤。

**示例文法 (Grammar G):** (1) S→Ab (2) S→c (3) A→c

### 第一步：构造拓广文法 (Step 1: Construct the Augmented Grammar)

**与 LR(0) 完全相同，引入唯一接受状态**。

**拓广文法 G′:** (0) S′→S (1) S→Ab (2) S→c (3) A→c

### 第二步：计算非终结符的 FOLLOW 集 (Step 2: Compute FOLLOW Sets)

**这是 SLR(1) 与 LR(0) 的根本区别。** SLR(1) 的核心思想是：**利用非终结符的随前符号集 (Lookahead / FOLLOW Set) 来精确限制归约动作的发生时机。**

我们需**要计算所有非终结符（S′ 可以省略，主要算 S 和 A）的 FOLLOW 集**：

1. **计算 FOLLOW(S):**
	- 根据公理，**结束符 `$` 自动加入开始符号的 FOLLOW 集。因此 $ 加入 FOLLOW(S)。**
	- 检查产生式右部，**没有其他符号跟在 S 后面**。
	- **结果:** FOLLOW(S)={$}
2. **计算 FOLLOW(A):**
	- 遍历产生式，找到**包含 A 的产生式右部：(1) S→Ab。**
	- 在 A 后面**紧跟的符号是终结符 b**。因此，将 b 加入 FOLLOW(A)。
	- **结果:** FOLLOW(A)={b}

*(物理意义：在任何合法的语法树中**，非终结符 A 被归约出来之后，它的下一个输入符号绝对只可能是 `b`。如果是其他符号，说明语法错误**。)*

### 第三步：构造 LR(0) 项目集规范族 (Step 3: Construct Canonical LR(0) Collection)

这一步使用**与 LR(0) 完全相同的 CLOSURE 和 GOTO 函数进行计算**。

- **状态 I0:** S′→⋅S S→⋅Ab S→⋅c A→⋅c
- **状态 I1:** GOTO(I0,S) S′→S⋅  *(**接受**状态)*
- **状态 I2:** GOTO(I0,A) S→A⋅b
- **状态 I3 (核心冲突状态):** GOTO(I0,c) S→c⋅  *(**归约项目，对应产生式 2**)* A→c⋅  *(**归约项目，对应产生式 3**)*
- **状态 I4:** GOTO(I2,b) S→Ab⋅ *(归约项目，**对应产生式 1**)*

**冲突分析 (Conflict Analysis):** 在**状态 I3 中，存在两个归约项目**。如果是 LR(0) 分析表，它会要求**在第 3 行的所有终结符列既填入 `r2` 又填入 `r3`**，导致**归约-归约冲突 (Reduce-Reduce Conflict)**，LR(0) **构建失败。**

### 第四步：执行 SLR(1) 填表规则 (Step 4: SLR(1) Table Filling Rules)

这一步是 SLR(1) 的灵魂。**前三条规则（移进、跳转、接受）与 LR(0) 完全一致**，**唯独第 4 条规则（归约）被严格限制了。**

对于状态 Ii：

1. **移进 (Shift):** 若 **X→α⋅aβ∈Ii 且 GOTO(Ii,a)=Ij，则 `ACTION[i, a] = sj`。**
2. **跳转 (Goto):** 若 **GOTO(Ii,A)=Ij，则 `GOTO[i, A] = j`。**
3. **接受 (Accept):** 若 S′→S⋅∈Ii，则 `ACTION[i, $] = acc`。
4. **归约 (Reduce) 【SLR(1) 核心修正】:** 若**归约项目 A→α⋅∈Ii （设产生式编号为 k），则仅对属于 FOLLOW(A) 的终结符 a，在 `ACTION[i, a]` 填入 `rk`。**

**应用规则填表 (结合我们的示例):**

- **第 0, 1, 2, 4 行填表逻辑与之前相同。**
- **【关键】处理第 3 行 (I3):** I3 包含：S→c⋅ (编为 r2) 和 A→c⋅ (编为 r3)。
	- **看 S→c⋅：左部为 S。**我们在第二步算出 FOLLOW(S)={$}。因此，**只在 `$` 列填入 `r2`**。
	- **看 A→c⋅：左部为 A。**我们在第二步算出 FOLLOW(A)={b}。因此，**只在 `b` 列填入 `r3`**。

**最终构建的 SLR(1) 分析表：**

| 状态 (State) | ACTION: b | ACTION: c | ACTION: $ | GOTO: S | GOTO: A |
| ------------ | --------- | --------- | --------- | ------- | ------- |
| **0**        |           | s3        |           | 1       | 2       |
| **1**        |           |           | acc       |         |         |
| **2**        | s4        |           |           |         |         |
| **3**        | **r3**    |           | **r2**    |         |         |
| **4**        |           |           | r1        |         |         |

*结果表明，通过**引入 FOLLOW 集**，**原本在 LR(0) 中重叠在同一单元格的 `r2` 和 `r3` 被成功分离到了不同的列，冲突被完美化解。这就是 SLR(1) 的强大之处。***

### 五、 考点实战与自测题 (Exam-Style Questions)

以下题目针对 SLR(1) 归约动作填表的边界条件进行考查，是期末和考研的经典陷阱题。

**Question: Shift-Reduce Conflict Resolution in SLR(1) (SLR(1) 移进-归约冲突处理)** **已知 (Given):** 某文法的拓广文法中，计算得到状态 I6 的项目集如下： I6={E→E+⋅T,T→⋅id,T→⋅(E)} 另外有一个状态 I9: I9={E→E+T⋅,T→T∗⋅F,…}

假设非终结符 E 的 FOLLOW(E)={+,),$}。 在处理状态 I9 时，由于它包含归约项目 E→E+T⋅ (假设产生式编号为 r2)，同时它通过读入 `*` 转移到新状态 (假设为 s11)。

**Question:** 请判断在 SLR(1) 填表时，状态 I9 是否存在冲突？请根据 SLR(1) 的填表规则详细说明在 `ACTION[9, +]` 和 `ACTION[9, *]` 处应该填入什么动作？

*(请遮盖下方答案作答自检)*

**答案:** **不存在冲突。**

- **计算 `ACTION[9, +]` (处理归约):** 状态 I9 包含**归约项目 E→E+T**⋅。根据 SLR(1) 规则，需要检**查当前终结符 `+` 是否属于 FOLLOW(E)**。已知 **FOLLOW(E)={+,),$}，由于 `+` ∈FOLLOW(E)，因此在 `ACTION[9, +]` 填入归约动作 `r2`。**
- **计算 `ACTION[9, \*]` (处理移进与冲突检验):** 状态 I9 中**包含移进项目 T→T∗⋅F（圆点前是 `*`，实际在形成此状态前，必定是 T→T⋅∗F，读入 `*` 发生移进**）。因**此在 `ACTION[9, *]` 应当填入移进动作 `s11`**。 **冲突检验:** 此时**会不会需要在此处填入 `r2` 导致移进-归约冲突？**需要**检查终结符 `*` 是否在 FOLLOW(E) 中。由于 FOLLOW(E) 包含 `{+, ), $}`，不包含 `\*`。因此，在 `*` 列不需要填入 `r2`。冲突被完美避开。**

# s1,r3这些都是怎么编号的,为什么action和goto表填入的具体内容是不一样的

### 一、 `sN` 与 `rK` 编号系统的本质区别 (The Fundamental Difference in Numbering)

请你在脑海中建立一个绝对的认知：**`s` 后面的数字和 `r` 后面的数字，属于两个完全不相干的编号系统！**

#### 1. 移进动作 `sN` (Shift N) 中的 `N`

- **含义：目标状态编号 (Target State Number)。**
- **来源：** 它来**源于 DFA 的状态集编号（即项目集闭包 $I_0, I_1, I_2 \dots I_N$）。**
- **动作含义：** `sN` 指示分析器：**“将当前的输入符号（终结符）压入栈中，并且将状态机切换到状态 $N$（即将数字 $N$ 压入栈顶）。”**
- **计算对应：** 如果公式**计算出 $GOTO(I_i, a) = I_N$，那么填表时就是 `sN`。**

#### 2. 归约动作 `rK` (Reduce K) 中的 `K`

- **含义：产生式编号 (Production Rule Index)。**
- **来源：** 它来源于我们**最初给定的文法 $G$ 的产生式列表序号。**
- **动作含义：** `rK` 指示分析器：“此刻**栈顶已经形成了完整的句柄，请使用文法中的第 $K$ 条产生式进行归约折叠。”**
- **关键注意点：** 这里的**数字 $K$ 跟当前状态机有多少个状态毫无关系**。哪怕 DFA 有 100 个状态，只要**归约用的是第 3 条产生式，它就是 `r3`。**

**举例说明：**

假设文法：(1) $S \rightarrow Ab$, (2) $S \rightarrow c$, (3) $A \rightarrow c$。

如果 `ACTION[0, c] = s3`，这里的 `3` 指的是转移到**状态 $I_3$**。

如果 `ACTION[3, b] = r3`，这里**的 `3` 指的是使用产生式 (3) $A \rightarrow c$ 进行归约。**

### 二、 ACTION 表与 GOTO 表内容差异的底层逻辑 (Why the Content Differs)

分析表分为 ACTION 和 GOTO 两半，**不仅是因为列的符号类型不同（终结符 vs 非终结符）**，更是因为它们在**运行时的触发时机和承担的任务完全不同**。

#### 1. ACTION 表：处理当前输入与动作决策 (Handling Inputs & Action Decisions)

- **列标识：** **终结符 (Terminals) 和结束符 `$`.**
- **触发时机：** 只要**分析器需要决定下一步该干什么（读入一个真实存在的单词符号），它就去查 ACTION 表。**
- **填入内容类型 (`sN`, `rK`, `acc`)：**
	- 因为**面临真实的输入符号**，分析器**要么把它吞入（移进 Shift），产生状态转移；**
	- 要么发现**栈顶已经成型，暂时不吞入，而是把栈顶的内容合并（归约 Reduce）；**
	- 要么**分析结束（接受 Accept）**。
	- **结论：** ACTION 表**必须明确指出操作的类型 (s/r/acc) 以及操作的参数 (状态号或产生式号)。**

#### 2. GOTO 表：处理归约后的状态恢复 (State Recovery Post-Reduction)

- **列标识：** **非终结符 (Non-terminals).**
- **触发时机（极度重要）：** **GOTO 表绝不在读取输入符号时触发**！**它仅在归约动作 (Reduce) 发生时的后半段触发。**
- **填入内容类型 (纯数字 `N`)：**
	- 回想一下归约 `rK` (如 $A \rightarrow \beta$) 的过程：**弹出 $|\beta|$ 个元素 $\rightarrow$ 暴露出一个历史状态 $s'$ $\rightarrow$ 将左部符号 $A$ 压栈。**
	- 此时，状态机**必须确立一个新的栈顶状态。分析器会问：“我本来在状态 $s'$，现在凭空多出了一个非终结符 $A$，我该进入哪个新状态？”**
	- 查询 `GOTO[s', A]`。
	- **结论：** 在此阶段，动作的类型绝对且唯一是**“状态转移 (State Transition)”，不可能有移进、归约或接受。因此，为了表格的精简和数据结构的单一，GOTO 表中不需要带有任何字母前缀，直接填入纯数字的目标状态编号 `N` 即可。**

### 三、 考点实战与自测题 (Exam-Style Questions)

以下题目专门针对这两种编号的混淆点进行考察，这是期末考试填空题的常客。

**Question 1: Parsing Table Decoding (分析表解码辨析)**

**已知 (Given):**

给定文法 $G$:

(1) $E \rightarrow T * F$

(2) $E \rightarrow T$

(3) $T \rightarrow id$

在某个 LR 分析器运行过程中，**当前状态栈为 `$ 0 id 4`，输入缓冲区剩余 `* id $`。**

查表得到 `ACTION[4, *] = r3`。

**Question:**

请明确解释 `r3` 中数字 `3` 的准确含义。如果在执行完这一步 `r3` 动作后，暴露的历史状态是 0，且查表得知 `GOTO[0, T] = 2`，请问这个数字 `2` 代表什么含义？执行完这两个关联动作后，栈顶的状态数字是多少？

*(请遮盖下方答案作答自检)*

**答案:**

1. **`r3` 中 `3` 的含义：** 代表**文法的产生式编号。即指示分析器使用第 (3) 条产生式 $T \rightarrow id$ 进行归约操作。**
2. **`GOTO[0, T] = 2` 中 `2` 的含义：** 代表 **DFA 的目标状态编号。即指示分析器在历史状态 0 识别出非终结符 $T$ 后，应当转移并压入状态 $I_2$。**
3. **栈顶最终状态数字：** 是 **2**。（**归约时弹出 `id` 和 `4`，暴露出 `0`**，压入 `T`，然后**将查 GOTO 表得到的纯数字 `2` 压入栈顶。栈变为 `$ 0 T 2`）。**

![image-20260625160355638](./scut%E7%BC%96%E8%AF%91%E5%8E%9F%E7%90%86%E5%A4%8D%E4%B9%A02/image-20260625160355638-1782374636403-1.png)

### 1. 题目解析 (Question Analysis)

**题目大意：** 编译器 (Compiler) 中中间代码生成器 (Intermediate code generator) 的输入是 ( )。 **正确答案：** (B) An annotated syntaxtree (**注释语法树**)

这道题考察的是**编译器的逻辑阶段 (Phases of a Compiler)** 以及各个阶段之间传递的**数据结构 (Data Structures)**。编译器是一个流水线结构，前一个阶段的输出就是后一个阶段的输入。

### 2. 核心知识点讲解 (Core Concepts)

为了彻底弄懂这道题及其变体，我们需要梳理编译器前端 (Compiler Front-end) 的核心阶段及其对应的输入输出。

- **词法分析阶段 (Lexical Analysis / Scanning)**
	- **输入：** 源程序字符流 (Source program / Character stream)
	- **任务：** 扫描字符，识别出具有独立意义的词法单元。
	- **输出：** **词法单元 (Tokens)**。这就是选项 (D)。
- **语法分析阶段 (Syntax Analysis / Parsing)**
	- **输入：** 词法单元 (Tokens)
	- **任务：** **根据语言的语法规则 (Grammar rules)，检查 Tokens 的组合是否合法，并构建出表示程序层次结构的树状模型。**
	- **输出：** **语法树 (Syntax tree / Parse tree)**。这就是选项 (A)。此时的**树仅包含结构信息，不包含类型和语义信息。**
- **语义分析阶段 (Semantic Analysis)**
	- **输入：** 语法树 (Syntax tree)
	- **任务：** 检查**程序是否符合语义规则（如类型检查 Type checking、作用域解析 Scope resolution）。**在此过程中，语义分析器会**将类型信息、符号表 (Symbol table) 的引用等附加到语法树的节点上。**
	- **输出：** **注释语法树 (Annotated syntax tree)**。这就是选项 (B)。
- **中间代码生成阶段 (Intermediate Code Generation)**
	- **输入：** **注释语法树 (Annotated syntax tree)**（即语义分析的输出）。
	- **任务：** 将带有语义信息的语法树转换为一种独立于具体机器的低级表示形式（如三地址码 Three-address code）。
	- **输出：** **中间代码 (Intermediate code)**。这就是选项 (C)。

### 3. 选项详细剖析 (Option Breakdown)

- **(A) A syntaxtree (语法树)：** 这是语法分析器 (Syntax Analyzer) 的输出，同时是语义分析器 (Semantic Analyzer) 的输入。它缺乏生成代码所需的类型等语义信息，不能直接给中间代码生成器。
- **(B) An annotated syntaxtree (注释语法树)：** 包含了程序的结构信息以及经过验证的语义信息（如变量类型），是**中间代码生成器 (Intermediate code generator) 的标准输入。**
- **(C) An intermediate code (中间代码)：** 这是**中间代码生成器的输出**，而不是输入。它**是代码优化器 (Code Optimizer) 或目标代码生成器 (Target Code Generator) 的输入。**
- **(D) Tokens (词法单元)：** 这是词法分析器 (Lexical Analyzer) 的输出，语法分析器 (Syntax Analyzer) 的输入。处于编译的最早期阶段。

### 4. 考试风格练习题 (Exam-Style Practice)

基于上述知识点，以下是常考的变体题目及解答，用于自测：

**题目 1 (单选题 - 考查前驱阶段)：** The output of a lexical analyzer is ( ). (A) A syntax tree (B) A character stream (C) Tokens (D) Intermediate code

**【答案与解析】** **答案：** (C) **解析：** 词法分析器 (Lexical analyzer) 负责读取源程序的字符流 (Character stream)，并将其转换为词法单元序列 (Tokens)。(A) 是语法分析的输出；(B) 是词法分析的输入；(D) 是中间代码生成的输出。

**题目 2 (单选题 - 考查语义分析的作用)：** Type checking is normally done during ( ). (A) Lexical analysis (B) Syntax analysis (C) Semantic analysis (D) Code optimization

**【答案与解析】** **答案：** (C) **解析：** 类型检查 (Type checking) 是语义分析 (Semantic analysis) 阶段最核心的任务，旨在确保操作数和运算符在语义上是匹配的。词法分析只管拼写，语法分析只管结构，代码优化负责提升代码效率。

**题目 3 (简答题/填空题 - 考查阶段衔接)：** Please briefly describe the process of how "Tokens" are transformed into "Intermediate code" in the front-end of a compiler. (请简述在编译器前端中，"词法单元(Tokens)"是如何转化为"中间代码(Intermediate code)"的。)

**【答案与解析】** **答案：**

1. 语法分析器 (Syntax Analyzer) 接收 Tokens，根据上下文无关文法构建出语法树 (Syntax tree)。
2. 语义分析器 (Semantic Analyzer) 接收语法树进行类型检查等语义验证，并将属性信息绑定到节点上，生成注释语法树 (Annotated syntax tree)。
3. 中间代码生成器 (Intermediate Code Generator) 接收注释语法树，将其翻译成中间代码 (Intermediate code)。

# **Attribute Grammar (属性文法)** 以及相关的属性分类（**Synthesized/Inherited attributes**）和文法分类（**S-attributes/L-attributes**）

### 1. 语义分析基础知识点精讲

**1.1 Attribute Grammar (属性文法)**  **属性文法是上下文无关文法 (Context-Free Grammar, C.F.G.) 的扩展**。在属性文法中，**每个文法符号（终结符或非终结符）都有一组关联的 Attributes (属性)**，用于**存放数据（如类型、值、内存地址等）**。同时，**每个产生式 (Production) 都配有一组 Semantic Rules (语义规则)，用于指明如何计算这些属性的值。**  

- **作用**：在**语法分析的基础上，通过计算属性值来完成类型检查 (Type checking)** 或**中间代码生成 (Intermediate Code Generation) 等任务** 。  

**1.2 Attributes (属性) 的两大分类**

在**语法分析树 (Parse Tree) 中，属性值是通过节点之间的依赖关系计算出来的**。根据数据**流向，属性严格分为两类：**

- **Synthesized Attributes (综合属性)** ：节点 **N** 上的综合属性值**只能由其子节点 (Children) 的属性值或 N 自身的其他属性值计算得出。**数据流向自底向上 (Bottom-up)。  
- **Inherited Attributes (继承属性)** ：节点 **N** 上的继承属性值只能由其**父节点 (Parent)** 的属性值或其**兄弟节点 (Siblings)** 的属性值计算得出。数据**流向自顶向下 (Top-down) 或同级传递。**  

**1.3 Attribute Grammars (属性文法) 的两大分类**

根据**文法中包含的属性类型及计算规则的约束**，考试常考以下两种属性文法：

- **S-attributes (S属性文法)** ：如果**一个属性文法仅包含综合属性 (Synthesized attributes)，则称之为 S属性文法**。此类文法**完美契合自底向上的分析器（如 LR 分析），属性可在规约 (Reduction) 时一并计算**。  
- **L-attributes (L属性文法)** ：**允许包含综合属性，且如果包含继承属性 (Inherited attributes)**，则**继承属性的计算必须严格遵循从左到右 (Left-to-right) 的顺序。**具体而言，对于产生式 $A \to X_1 X_2 ... X_n$ ，其**中 $X_i$ 的继承属性只能依赖于其左侧兄弟节点（$X_1$ 到 $X_{i-1}$）的属性**，**以及父节点 $A$ 的继承属性**。**L属性文法常用于自顶向下的分析器（如 LL(1)）。**  

### 2. 考试风格实战演练 (Practice Questions)

**Question 1: Attribute Classification and Grammar Types (10 pts)**

Consider the following production and semantic rules:

Production: $T \to F T'$

Semantic Rules:

1. $T'.inh = F.val$
2. $T.val = T'.syn$

**Questions:**

1. Identify whether $T'.inh$ and $T.val$ are Synthesized Attributes or Inherited Attributes, and briefly explain why based on the parse tree structure. (指出 $T'.inh$ 和 $T.val$ 是综合属性还是继承属性，并基于语法树结构简述原因。)
2. Does this grammar fragment belong to S-attributes? Why? (该文法片段属于 S属性文法吗？为什么？)

**[Answer / 答案]**

1. **$T'.inh$**: Inherited Attribute (继承属性)。

	**Reason**: In the parse tree, $F$ is the left sibling of $T'$. The value of $T'.inh$ depends on $F.val$, which means the data flows from a sibling node. (在语法树中，$F$ 是 $T'$ 的左侧兄弟节点。$T'.inh$ 的值依赖于 $F.val$，数据从兄弟节点流入。)

	**$T.val$**: Synthesized Attribute (综合属性)。

	**Reason**: In the parse tree, $T'$ is a child node of $T$. The value of $T.val$ depends on $T'.syn$, which means the data flows from a child node to its parent. (在语法树中，$T'$ 是 $T$ 的子节点。$T.val$ 的值依赖于 $T'.syn$，数据从子节点流向父节点。)

2. **No (不属于)**。

	**Reason**: An S-attributed grammar can only contain synthesized attributes. Since this grammar contains an inherited attribute ($T'.inh$), it is not an S-attributed grammar. (S属性文法只能包含综合属性。因为该文法包含了继承属性 $T'.inh$，所以它不是S属性文法。注：它符合L属性文法，因为 $T'.inh$ 仅依赖于其左兄弟 $F$。)

	# **Dependency Graphs (依赖图)** 与 **Algorithms for attribute computation (属性计算算法)**。

	这部分内容在考试的大题中经常出现，通常要求**你在一棵语法分析树上展示属性是如何一步步计算出来的。**

	### 1. 知识点精讲：依赖图与属性计算

	**1.1 Dependency Graphs (依赖图)** 依赖图是一个**有向图 (Directed Graph)**，用于描述**语法分析树 (Parse Tree)** 中各个属性之间的数据依赖关系。它决定了属性计算的先后顺序。

	- **Nodes (节点)**：图中的**每一个节点代表语法树中某一个文法符号的具体属性（例如 E.val）。**
	- **Edges (边)**：如果**属性 b 的计算需要用到属性 c 的值（即语义规则中存在类似 b=f(c) 的计算），那么在依赖图中，就会存在一条从 c 指向 b 的有向边 (c→b)。**

	**1.2 Evaluation Order (计算顺序) 与 Topological Sort (拓扑排序)**

	- **拓扑排序**：依赖图的**任何一个有效的拓扑排序，都可以作为计算属性的有效顺序**。在拓扑排序中，如果**存在边 c→b，那么在排序序列中 c 必须出现在 b 之前。**
	- **循环依赖 (Circular Dependency)**：如果依赖图中存在环，则无法进行拓扑排序，这意味着该属性文法存在逻辑错误，属性无法被正常计算。

	**1.3 Algorithms for Attribute Computation (属性计算算法)** 考试中，属性的计算方法通常分为以下几种层次：

	1. **基于语法树的方法 (Parse-Tree Methods)**：
		- 步骤：**先构造完整的语法分析树 → 构建依赖图 → 进行拓扑排序 → 按照拓扑排序的顺序逐个计算属性值。**
		- 结果：形成一棵 **Annotated Syntax Tree (注释语法树)**，即树上的**每个节点都标注了最终计算得出的属性值。**
	2. **结合分析过程的动态计算 (On-the-fly Evaluation)**：
		- **不需要真正在内存中构造出完整的语法树，而是与 Parsing (语法分析) 阶段同步进行。**
		- **对于 S-attributes (S属性文法)**：由于只包含综合属性，可以在 **Bottom-Up (自底向上，如 LR 分析)** 的规约 (Reduction) 阶段，直接利**用分析栈 (Parsing stack) 计算并合并属性值。**
		- **对于 L-attributes (L属性文法)**：可以**在 Top-Down (自顶向下，如 LL(1) 或递归下降) 的推导过程中计算**。**继承属性在展开子节点前计算，综合属性在子节点匹配完成后计算。**

	### 2. 考试风格实战演练 (Practice Questions)

	这道题直接提炼自你提供的往年样卷大题，非常具有代表性。请仔细在草稿纸上画出树状结构后再核对答案。

	**Question 2: Dependency Graph & Attribute Computation (15 pts)** Consider the following attribute grammar, where `h` is the synthesized attribute of nonterminals `S` and `L`. (考虑以下属性文法，**其中 `h` 是非终结符 `S` 和 `L` 的综合属性**)：

	| Production (产生式) | Semantic Rules (语义规则) |
	| ------------------- | ------------------------- |
	| S→(L)               | S.h=L.h×2                 |
	| S→a                 | S.h=1                     |
	| L→L1,S              | L.h=L1.h+S.h              |
	| L→S                 | L.h=S.h                   |

	*(注：规则中 **L1 表示产生式右部的 L，用于和左部的 L 区分**)*

	**Tasks:**

	1. Draw **the dependency graph for the input string: `(a,a)` (画出句子 `(a,a)` 的依赖图)**
	2. What’s the final value of S.h for the string `(a,a)`? **(句子 `(a,a)` 根节点 S.h 的最终值是多少？)**

	**[Answer / 答案]**

	**1) Dependency Graph (依赖图构造逻辑与文字描述):** *(考试作答时，你**需要先画出语法分析树，然后在对应的节点旁边标上 `.h`，并画出单向箭头**。)*

	- **语法树结构 (Parse Tree)**: **根节点 S0 产生 → `(` , Ltop , `)`** 内部节点 **Ltop 产生 → Lleft** , `,` , **Sright 叶子节点 Lleft 产生 → Sleft 叶子节点 Sleft 产生 → `a` 叶子节点 Sright 产生 → `a`**
	- **依赖图的边 (Edges in Dependency Graph)**:
		- 因为 Sleft→a，根据规则 S.h=1，此时 Sleft.h 是定值 1 (无前置依赖)。
		- 因为 Sright→a，同理，Sright.h 是定值 1。
		- 因为 Lleft→Sleft，规则为 L.h=S.h。**添加有向边：** Sleft.h→Lleft.h
		- 因为 Ltop→Lleft,Sright，规则为 L.h=L1.h+S.h。**添加两条有向边：** Lleft.h→Ltop.h 并且 Sright.h→Ltop.h
		- 因为 S0→(Ltop)，规则为 S.h=L.h×2。**添加有向边：** Ltop.h→S0.h

	**2) Final Value Computation (属性值计算):** **按照拓扑排序（自底向上）**推导：

	- Sleft.h=1
	- Sright.h=1
	- Lleft.h=Sleft.h=1
	- Ltop.h=Lleft.h+Sright.h=1+1=2
	- 根节点 S0.h=Ltop.h×2=2×2=4

	**最终答案 (Final Value):** S.h=4

	### 第一步：构造语法分析树

	首先，根据文法推导句子 `(a, a)` 的语法树：

	plaintex

	```
	          S（根节点）
	         /|\
	        ( L )
	         /|\
	        L , S
	        |   |
	        S   a
	        |
	        a
	```

	**推导过程**：

	- 最外层：`S → (L)`
	- 括号内的 L：`L → L₁, S`
	- 左部的 L₁：`L → S`
	- 两个 S：`S → a`

	------

	### 第二步：标注各节点的属性值（自底向上计算）

	h 是**综合属性（Synthesized Attribute）**，从**子节点向父节点传递，按后序遍历顺序计算：**

	|     节点     |  产生式   |           属性计算           | h 值 |
	| :----------: | :-------: | :--------------------------: | :--: |
	|   左下角 S   |   S → a   |           S.h = 1            |  1   |
	| 左部 L（L₁） |   L → S   |        L.h = S.h = 1         |  1   |
	|   右下角 S   |   S → a   |           S.h = 1            |  1   |
	|  括号内的 L  | L → L₁, S | L.h = L₁.h + S.h = 1 + 1 = 2 |  2   |
	|   根节点 S   |  S → (L)  |  S.h = L.h × 2 = 2 × 2 = 4   |  4   |

	**带属性标注的语法树**：

	

	```
	          S.h = 4
	         / | \
	        (  L.h=2  )
	           / | \
	      L.h=1  ,  S.h=1
	        |        |
	      S.h=1      a
	        |
	        a
	```

	------

	### 第三步：构造依赖图（Dependency Graph）

	#### 依赖图节点

	语法树中每个非终结符的 h 属性对应依赖图中的一个节点，共 5 个属性节点：

	1. **左 S.h**（左下角 S 的 h 属性）
	2. **L₁.h**（左部 L 的 h 属性）
	3. **右 S.h**（右下角 S 的 h 属性）
	4. **L_outer.h**（括号内 L 的 h 属性）
	5. **根 S.h**（最外层 S 的 h 属性）

	#### 依赖边（有向边：被依赖 → 依赖）

	根据语义规则：

	1. `L₁.h = S.h` → 左 S.h → L₁.h
	2. `L_outer.h = L₁.h + S.h` → L₁.h → L_outer.h，且 右 S.h → L_outer.h
	3. `根 S.h = L_outer.h × 2` → L_outer.h → 根 S.h

	注意：`S.h = 1` 是常量赋值，不依赖其他属性，因此左 S.h 和右 S.h 没有入边（是源节点）。

	#### 依赖图（文字描

	

	```
	左 S.h = 1        右 S.h = 1
	    |                  |
	    ↓                  |
	  L₁.h = 1             |
	    |                  |
	    └──────┬───────────┘
	           ↓
	      L_outer.h = 2
	           |
	           ↓
	       根 S.h = 4
	```

	**拓扑排序顺序**（一种**合法的计算顺序**）：

	左 S.h → 右 S.h → L₁.h → L_outer.h → 根 S.h

	------

	### 最终答案

	**根节点 S.h 的最终值 = 4**

	------

	### 验证（完整计算链）

	

	```
	左 S.h = 1
	  ↓
	L₁.h = 左 S.h = 1          右 S.h = 1
	  ↓                            ↓
	  └─────────── + ─────────────┘
	               ↓
	         L_outer.h = 1 + 1 = 2
	               ↓
	         根 S.h = 2 × 2 = 4 ✓
	```

	这部分内容在考试中，通常**会结合一段包含嵌套作用域（如 C/C++ 的大括号 `{}`）的代码块**，要求你展示符号表的动态演变过程，或者指出其中的语义错误。

	### 1. 知识点精讲：符号表与语义检查

	**1.1 Chained Symbol Table (链式符号表)**

	在支持块结构 (Block-structured) 的语言中，变量的作用域是嵌套的。为了管理这些嵌套的作用域，编译器通常**使用链式符号表。**

	- **结构 (Structure)**：**每一个作用域（如一个函数或一个 `{}` 语句块）都有一个独立的符号表 (Symbol Table)**，用于**记录该作用域内声明的标识符（变量名、函数名等）及其属性（类型、内存偏移量等）。**
	- **基于指针的链接 (Pointer-based Chaining)**：内部作用域的符号表**会包含一个指向其直接外部作用域符号表的指针 (通常记为 `prev` 或 `next` 指针)。**这样就形成了一条**从最内层作用域指向全局作用域的链表**。

	**1.2 Scope Checking (作用域检查)**

	编译器利用链式符号表执行两项基本的作用域检查规则：

	1. **Declaration (声明检查)**：在当前作用域**声明一个变量 `x` 时，编译器仅在当前符号表中查找**。如果当前**表已有 `x`，则报“重复声明 (Multiple Declaration)”错误；否则，将 `x` 插入当前表。**
	2. **Resolution (引用消解)**：当代码中使用变量 `y` 时，编译器从**当前最内层的符号表开始**查找。如果找到，则使用该记录；如果没找到，则顺着指针向上访问外层符号表，直到全局作用域。如果在链条的最顶端依然未找到，则报“未声明 (Undeclared Identifier)”错误。这也就是**“就近原则 (Closest Nested Scope Rule)”的底层实现。**

	**1.3 Type Checking (类型检查)**

	确认了**变量合法存在后，编译器需要检查操作数类型是否匹配。**

	- 类型检查通常**通过语法分析树 (Parse Tree) 结合属性文法 (Attribute Grammar) 自底向上 (Bottom-up) 进行。**
	- 遇到标识符 (Identifier) 时，通过查符号表获取其类型，赋给节点的类型属性（如 `E.type`）。
	- 在执行运算（如 $E \to E_1 + E_2$）时，语义规则会检查 $E_1.type$ 和 $E_2.type$ **是否兼容（Compatible）。如果一个是 `int` 另一个是 `float`，编译器可能需要插入类型转换 (Type Coercion/Cast) 节点**，或者**在强类型语言中直接报错。**

	### 2. 考试风格实战演练 (Practice Questions)

	**Question 3: Chained Symbol Table & Scope Resolution (15 pts)**

	Consider the following C-like pseudo-code:

	C

	```
	int x;
	float y;
	void main() {
	    float x;
	    int z;
	    // Point A
	    x = y + z;
	}
	```

	**Tasks:**

	1. Draw or describe the structure of the chained symbol tables exactly at **Point A**. (画出或描述在 **Point A** 时，链式符号表的结构。)
	2. Explain how the compiler resolves the variables `x`, `y`, and `z` in the statement `x = y + z;` using the chained symbol table, and state their resolved types. (解释编译器如何使用链式符号表消解语句 `x = y + z;` 中的变量 `x`, `y`, `z`，并说明它们最终确定的类型。)

	**[Answer / 答案]**

	**1) Chained Symbol Table Structure (链式符号表结构):**

	*(考试时建议用方框代表符号表，用箭头代表指针)*

	- **Table 0 (Global Scope / 全局作用域):**
		- `x` $\to$ `type: int`
		- `y` $\to$ `type: float`
		- `main` $\to$ `type: function`
		- **`prev` pointer $\to$ `NULL` (无外层)**
	- **Table 1 (Local Scope of `main` / main函数局部作用域):**
		- `x` $\to$ `type: float`
		- `z` $\to$ `type: int`
		- **`prev` pointer $\to$ Points to Table 0 (指向全局符号表 Table 0)**

	*(注：在 Point A 时，当前激活的符号表是 Table 1)*

	**2) Scope Resolution and Type Checking (作用域消解与类型确定):**

	- **For `x`**: The compiler searches the current active table (Table 1) first. It finds `x` immediately.
		- **Resolved Type**: `float` (The global `x` is hidden/shadowed).
	- **For `y`**: The compiler searches Table 1 and does not find `y`. It follows the `prev` pointer to Table 0 (Global Scope) and finds `y`.
		- **Resolved Type**: `float`.
	- **For `z`**: The compiler searches Table 1 and finds `z` immediately.
		- **Resolved Type**: `int`.
	- *(Additional Type Check Note)*: For `y + z`, it is `float + int`. The compiler will safely coerce (隐式转换) `z` from `int` to `float` before addition. The result is `float`, which matches the resolved type of the left-hand side `x` (`float`). No type error.



# **Intermediate Code Generation (中间代码生成)** 。  

本次输出，我们将聚焦于 **Three-Address Code (TAC / 三地址码)** 的基本构造、**Short-circuit code (短路代码)** 的翻译逻辑，以及考试中最硬核的算法：**Backpatching (回填技术)** 。  

### 1. 知识点精讲：中间代码生成与控制流

**1.1 Three-Address Code, TAC (三地址码)** 三地址码是一种机器无关的中间表示形式。它的核心特征是：**每一条指令最多包含三个地址（操作数），并且最多执行一个运算符。**   

- **常见指令格式**：
	- **赋值：`x = y op z` 或 `x = op y` (一元运算) 或 `x = y` (直接赋值)**
	- **无条件跳转：`goto L`**
	- **条件跳转：`if x relop y goto L` (其中 `relop` 是关系运算符，如 `<`, `>`, `==`)**
- **考试要求**：遇到复杂的嵌套表达式（如 `x = a + b * c`），必**须引入临时变量（如 `t1`, `t2`）将其拆解为平坦的 TAC 序列 ： `t1 = b * c` `x = a + t1`**  

**1.2 Control Flow & Short-circuit Code (控制流与短路代码)** 在**翻译布尔表达式 (Boolean Expressions，如 `&&`, `||`, `!`) 时**，编译器**通常不计算出具体的 `true` 或 `false` 值，而是直接通过跳转指令 (goto) 来反映控制流逻辑。这被称为短路计算 。**  

- **对于 `B1 || B2`**：如果 `B1` 为真，则整个表达式为真，直接跳过 `B2` 的代码（跳向 `B.true`）；如果 `B1` 为假，则必须计算（执行）`B2`。
- **对于 `B1 && B2`**：如果 `B1` 为假，则整个表达式为假，直接跳向 `B.false`；如果 `B1` 为真，则必须计算 `B2`。
- **⚠️ 考试避坑点**：在**最终生成的 TAC 序列中，绝对不能出现 `&&` 或 `||` 运算符！所有的逻辑关系都必须完全转化为 `if ... goto` 和 `goto`。**

**1.3 Backpatching (回填技术)** **为什么需要回填？** 编译器**在自底向上 (Bottom-up) 一遍扫描生成代码时，经常会生成一条跳转指令（如 `goto ?`）**，但此**时目标的指令标号 (Label) 还没有生成（因为目标代码在后面）**。 **核心机制 (综合属性)**： 为了解决这个问题，**文法节点会维护几个包含了“未填写的指令地址（标号）”的列表（List）：**

- **`B.truelist`：包含所有在布尔表达式 `B` 为真时需要跳转的指令地址。**
- **`B.falselist`：包含所有在 `B` 为假时需要跳转的指令地址。**
- **`S.nextlist`：包含所有在语句 `S` 执行完毕后需要跳出的指令地址。**

**三个核心辅助函数 (Auxiliary Functions)**： 在**考卷上写 SDT (Syntax-Directed Translation) 规则时，必须准确使用这三个函数：**

1. **`makelist(i)`**：创建**一个只包含指令地址 `i` 的新列表。**
2. **`merge(p1, p2)`**：将**列表 `p1` 和 `p2` 合并，返回合并后的新列表。**
3. **`backpatch(p, i)`**：**核心动作**。将**目标标号（或地址） `i` 回填到列表 `p` 中所有未决的跳转指令中作为目标地址。**

### 2. 考试风格实战演练 (Practice Questions)

这部分在大题中往往要求手工把高级语言片段翻译成三地址码。请准备好草稿纸，体会短路求值和控制流跳转的逻辑。

**Question 4: Short-circuit TAC Generation (15 pts)** Translate the following statement into a sequence of Three-Address Code (TAC) using short-circuit evaluation. Ensure that logical operators (`&&`, `||`) do not appear in the final TAC. *(使用短路求值，将下列语句翻译为三地址码序列，确保 TAC 中不出现逻辑运算符)*

```
while (a < b && c > d) {
    x = y + z;
}
```

**[Answer / 答案]**

*(提示：在考试中，通常**使用 L1, L2 等 Label 标号来表示代码块的首地址)***

Plaintext

```
L1: if a < b goto L2      // B1.true 跳转到 B2 继续计算
    goto L_next           // B1.false 直接跳出循环 (&& 的短路特性)
L2: if c > d goto L3      // B2.true 跳转到循环体内部 (S1)
    goto L_next           // B2.false 跳出循环
L3: t1 = y + z            // 循环体 S1 开始，引入临时变量
    x = t1
    goto L1               // S1 结束，无条件跳转回 while 的条件判断处
L_next: ...               // 循环结束后的后续代码
```

*(注：第一行的 `if a < b goto L2` 和 `goto L_next` **可以优化合并为一句 `if False a < b goto L_next`**，如果你在试卷上写出优化版本或上述标准版本，均可得分。)*

**控制流语句的 SDT (Syntax-Directed Translation, 语法制导翻译) 与属性传递**。

这类大题通常会给你一个循环结构（如 `do-while` 或 `repeat-until`），并给出一段简短的语义规则 (Semantic Rules)。你需要手动推导整棵**抽象语法树 (AST)**，标出所有属性的流向，最终拼装出**三地址码 (TAC)**。

### 1. 知识点精讲：控制流 SDT 核心机制

在控制流的翻译中，我们强烈依赖**继承属性 (Inherited Attributes)** 来向下传递标签（Label），并利用**综合属性 (Synthesized Attributes)** 来向上收集生成的代码。

**1.1 核心属性定义 (Core Attributes)**

你需要牢记以下四个控制流中最关键的**继承属性 (Inherited Attributes)**：

- **`.begin`**：当前语句（循环体）开始执行的指令标号 (Label)。
- **`.next`**：当前语句执行完毕后，应该跳转到的下一条指令的标号。
- **`.true`**：对于布尔表达式 $E$，如果 $E$ 为真，控制流应该跳转到的标号。
- **`.false`**：如果 $E$ 为假，控制流应该跳转到的标号。

以及一个核心的**综合属性 (Synthesized Attribute)**：

- **`.code`**：当前节点（及其子节点）翻译出来的完整 TAC 字符串。

**1.2 辅助函数 (Auxiliary Function)**

- **`newlabel`**：每次调用都会生成一个新的、全局唯一的标号（如 `L1`, `L2`, `L3` ...）。

**1.3 控制流的拼接逻辑 (Concatenation Logic)**

以考卷中出现的 `do S while E` 为例，其物理执行顺序是：先无条件执行一次 $S$，然后判断表达式 $E$。如果 $E$ 为真，跳回 $S$ 的开头；如果 $E$ 为假，跳出循环。

因此，SDT 语义规则通常如下设定：

1. 首先，为循环的开头和结尾申请新标号：`do-stmt.begin = newlabel; do-stmt.next = newlabel;`
2. 接着，配置 $E$ 的跳转目标（这就是属性的向下传递）：
	- `E.true = do-stmt.begin` (为真，跳回循环开头)
	- `E.false = do-stmt.next` (为假，跳出循环)
3. 最后，自底向上拼接代码：
	- `do-stmt.code = Label do-stmt.begin || S.code || E.code` (注：`||` 表示字符串连接)

### 2. 考试风格实战演练 (Practice Questions)

这道题完全复刻自 2021 年样卷的压轴大题，请务必在草稿纸上亲手画一遍语法树并进行标注。

**Question 5: SDT Translation for `do-while` loop (20 pts)**

Given the source code: `do a=a+1 while a<b and a<c`

Based on the grammar `do-stmt -> do S while E` and the semantic rules provided in the lecture:

1. Draw the Abstract Syntax Tree (AST).
2. Calculate the inherited attributes `true`, `false`, `begin`, and `next` for the relevant nodes (assume the loop gets labels L1, L2, etc.).
3. Translate the annotated tree into Three-Address-Code (TAC) using short-circuit evaluation.

**[Answer / 答案]**

**1) Abstract Syntax Tree (抽象语法树结构):**

*(考试时请画出标准的树状图，此处使用文本缩进表示层级)*

- **`do-stmt`** (根节点)
	- `do` (关键字)
	- **`S`** (循环体): `a = a + 1`
	- `while` (关键字)
	- **`E`** (布尔表达式): `E1 and E2`
		- **`E1`**: `a < b`
		- `and`
		- **`E2`**: `a < c`

**2) Annotated Inherited Attributes (继承属性计算与标注):**

*(自顶向下推导)*

- 假设 `do-stmt.begin = L1` 且 `do-stmt.next = L2`。
- 根据 `do-while` 规则，向 `E` 传递属性：
	- `E.true = do-stmt.begin = L1`
	- `E.false = do-stmt.next = L2`
- **硬核考点：处理 `E1 and E2` 的短路规则：**
	- 如果 `E1` 为假，整个 `and` 为假，直接跳出：`E1.false = E.false = L2`。
	- 如果 `E1` 为真，还需要继续判断 `E2`。所以我们需要申请一个新标号（设为 `L3`）放在 `E2` 的开头：`E1.true = newlabel = L3`。
	- 对于 `E2`，它的真假决定了整个 `and` 的真假：`E2.true = E.true = L1`，`E2.false = E.false = L2`。

**3) Three-Address-Code Generation (最终三地址码生成):**

结合上述标注的跳转地址，自底向上生成 TAC：

**S.code:**

Plaintext

```
    t1 = a + 1
    a = t1
```

**E1.code:**

Plaintext

```
    if a < b goto L3
    goto L2
```

**E2.code (位于标号 L3 处):**

Plaintext

```
L3: if a < c goto L1
    goto L2
```

**do-stmt.code (完整拼装，并在开头打上 L1 标号):**

Plaintext

```
L1: t1 = a + 1
    a = t1
    if a < b goto L3
    goto L2
L3: if a < c goto L1
    goto L2
L2: ...
```

*(得分要点：无遗漏的标号声明、正确的引入临时变量 `t1`、严格执行短路求值并且不能出现逻辑运算符 `and`)*

**控制流语句的 SDT (Syntax-Directed Translation, 语法制导翻译) 与属性传递**。

这类大题通常会给**你一个循环结构（如 `do-while` 或 `repeat-until`），并给出一段简短的语义规则 (Semantic Rules)**。你需要**手动推导整棵抽象语法树 (AST)，标出所有属性的流向，最终拼装出三地址码 (TAC)。**

### 1. 知识点精讲：控制流 SDT 核心机制

在控制流的翻译中，我们**强烈依赖继承属性 (Inherited Attributes) 来向下传递标签（Label），并利用综合属性 (Synthesized Attributes) 来向上收集生成的代码。**

**1.1 核心属性定义 (Core Attributes)**

你需要牢记**以下四个控制流中最关键的继承属性 (Inherited Attributes)：**

- **`.begin`**：**当前语句（循环体）开始执行的指令标号 (Label)。**
- **`.next`**：当前**语句执行完毕后，应该跳转到的下一条指令的标号。**
- **`.true`**：**对于布尔表达式 $E$，如果 $E$ 为真，控制流应该跳转到的标号。**
- **`.false`**：**如果 $E$ 为假，控制流应该跳转到的标号。**

以及一个核心的**综合属性 (Synthesized Attribute)**：

- **`.code`**：**当前节点（及其子节点）翻译出来的完整 TAC 字符串。**

**1.2 辅助函数 (Auxiliary Function)**

- **`newlabel`**：**每次调用都会生成一个新的、全局唯一的标号（如 `L1`, `L2`, `L3` ...）。**

**1.3 控制流的拼接逻辑 (Concatenation Logic)**

以考卷中出现的 `do S while E` 为例，其物理执行顺序是：**先无条件执行一次 $S$，然后判断表达式 $E$。如果 $E$ 为真，跳回 $S$ 的开头；如果 $E$ 为假，跳出循环。**

因此，SDT 语义规则通常如下设定：

1. 首先，为循环**的开头和结尾申请新标号：`do-stmt.begin = newlabel; do-stmt.next = newlabel;`**
2. **接着，配置 $E$ 的跳转目标（这就是属性的向下传递）：**
	- **`E.true = do-stmt.begin` (为真，跳回循环开头)**
	- **`E.false = do-stmt.next` (为假，跳出循环)**
3. 最后，**自底向上拼接代码：**
	- `do-stmt.code = Label do-stmt.begin || S.code || E.code` (注**：`||` 表示字符串连接)**

### 2. 考试风格实战演练 (Practice Questions)

这道题完全复刻自 2021 年样卷的压轴大题，请务必在草稿纸上亲手画一遍语法树并进行标注。

**Question 5: SDT Translation for `do-while` loop (20 pts)**

Given the source code: `do a=a+1 while a<b and a<c`

Based on the grammar `do-stmt -> do S while E` and the semantic rules provided in the lecture:

1. Draw the Abstract Syntax Tree (AST).
2. Calculate the inherited attributes `true`, `false`, `begin`, and `next` for the relevant nodes (assume the loop gets labels L1, L2, etc.).
3. Translate the annotated tree into Three-Address-Code (TAC) using short-circuit evaluation.

**[Answer / 答案]**

**1) Abstract Syntax Tree (抽象语法树结构):**

*(考试时请画出标准的树状图，此处使用文本缩进表示层级)*

- **`do-stmt`** (**根节点**)
	- **`do` (关键字)**
	- **`S` (循环体): `a = a + 1`**
	- **`while` (关键字)**
	- **`E`** (布尔表达式): `E1 and E2`
		- **`E1`**: `a < b`
		- `and`
		- **`E2`**: `a < c`

**2) Annotated Inherited Attributes (继承属性计算与标注):**

***(自顶向下**推导)*

- 假设 `do-stmt.begin = L1` 且 `do-stmt.next = L2`。
- 根据 `do-while` 规则，向 `E` 传递属性：
	- `E.true = do-stmt.begin = L1`
	- `E.false = do-stmt.next = L2`
- **硬核考点：处理 `E1 and E2` 的短路规则：**
	- **如果 `E1` 为假，整个 `and` 为假**，**直接跳出：`E1.false = E.false = L2`。**
	- 如果 `E1` 为真，**还需要继续判断 `E2`。所以我们需要申请一个新标号（设为 `L3`）放在 `E2` 的开头：`E1.true = newlabel = L3`。**
	- 对于 `E2`，它**的真假决定了整个 `and` 的真假：`E2.true = E.true = L1`，`E2.false = E.false = L2`。**

**3) Three-Address-Code Generation (最终三地址码生成):**

结合上述标注的跳转地址，自底向上生成 TAC：

**S.code:**



```
    t1 = a + 1
    a = t1
```

**E1.code:**



```
    if a < b goto L3
    goto L2
```

**E2.code (位于标号 L3 处):**



```
L3: if a < c goto L1
    goto L2
```

**do-stmt.code (完整拼装，并在开头打上 L1 标号):**

```
L1: t1 = a + 1
    a = t1
    if a < b goto L3
    goto L2
L3: if a < c goto L1
    goto L2
L2: ...
```

*(得分要点：无遗漏的标号声明、正确的引入临时变量 `t1`、严格执行短路求值并且不能出现逻辑运算符 `and`)*



### 第一步：画出抽象语法树（AST）

#### 1.1 语法结构分析

源程序 `do a=a+1 while a<b and a<c` 是一个 **do-while 循环语句**，由三部分组成：

- **关键字 do**：标记循环开始
- **循环体 S**：赋值语句 `a = a + 1`
- **关键字 while**：标记条件开始
- **条件 E**：逻辑与表达式 `a < b and a < c`

#### 1.2 AST 结构

抽象语法树**只保留操作符和操作数**，省略括号、关键字等语法细节：

```
              do-while
              /      \
             =        and
            / \      /   \
           a   +    <     <
              / \  / \   / \
             a  1 a  b  a  c
```

**节点说明**：

- 根节点 `do-while`：**循环语句，两个子节点分别是循环体和循环条件**
- 左子树 `=`：赋值语句，左子节点是左值 `a`，右子节点是表达式 `a + 1`
- 右子树 `and`：**逻辑与运算，两个子节点分别是 `a < b` 和 `a < c`**
- 叶子节点 `a, b, c, 1`：**变量和常量**

------

### 第二步：计算继承属性（true, false, begin, next）

#### 2.1 四个继承属性的含义

|   属性    |                     含义                     |           作用           |
| :-------: | :------------------------------------------: | :----------------------: |
| **begin** |  **该语句 / 表达式的第一条三地址码的标号**   | **标记代码段的起始位置** |
| **next**  | 该语句**执行完后，下一条要执行的语句的标号** |   **标记代码段的出口**   |
| **true**  |       **布尔表达式为真时跳转到的标号**       |    **条件为真的出口**    |
| **false** |       **布尔表达式为假时跳转到的标号**       |    **条件为假的出口**    |

这四个都是**继承属性**，信息**从父节点自顶向下传递给子节点。**

------

#### 2.2 do-while 循环的控制流逻辑

do-while 循环的执行顺序：

1. **先执行循环体 S**
2. **再判断条件 E**
3. 若 E 为真 → 跳回循环体开头，继续循环
4. 若 E 为假 → 退出循环，执行后续语句

对应的**控制流结构：**

```
Lbegin:  循环体 S 的代码
Lcond:   条件 E 的代码
         if E为真 goto Lbegin
         goto Lnext    （E为假时退出）
Lnext:   循环后的语句
```

------

#### 2.3 自顶向下传递继承属性

我们假设**整个 do-while 循环**：

- 开始标号：**L1**
- 结束出口（next）：**Lnext**

##### 第 1 层：根节点 do-while

**整个 do-while 语句的属性：**

- `do.begin = L1`（**循环从 L1 开始**）
- `do.next = Lnext`（循环**结束后去 Lnext**）

##### 第 2 层：循环体 S 和条件 E

根据 do-while 的控制流：

**循环体 S 的属性**：

- `S.begin = L1`（**循环体从 L1 开始，即 do-while 的 begin**）
- `S.next = L2`（循环体**执行完后，去 L2 判断条件，L2 是条件 E 的 begin**）

**条件 E 的属性**：

- `E.begin = L2`（**条件判断从 L2 开始**）
- `E.true = L1`（条件**为真 → 跳回 L1 继续循环**）
- `E.false = Lnext`（条件**为假 → 退出循环，即 do-while 的 next**）

------

##### 第 3 层：逻辑与 E = E₁ and E₂

**条件 E** 是 `a < b and a < c`，即 E₁ and E₂，其中：

- E₁ = a < b
- E₂ = a < c

**短路求值（Short-Circuit Evaluation）规则**：

对于 `E = E₁ and E₂`：

1. 先算 E₁
2. **若 E₁ 为假 → 整个表达式为假，直接跳转到 E 的假出口（不用算 E₂）**
3. **若 E₁ 为真 → 继续算 E₂**
4. **E₂ 的真假就是整个表达式的真假**

对应属性传递：

- `E₁.begin = E.begin = L2`（E₁ **从 E 的 begin 开始**）
- `E₁.true = L3`（**E₁ 为真 → 去 L3 算 E₂，L3 是 E₂ 的 begin**）
- `E₁.false = E.false = Lnext`（**E₁ 为假 → 整个 E 为假，直接跳 E 的假出口**）
- `E₂.begin = L3`（E₂ 从 L3 开始）
- `E₂.true = E.true = L1`（E₂ **为真 → 整个 E 为真，跳 E 的真出口**）
- `E₂.false = E.false = Lnext`（E₂ **为假 → 整个 E 为假，跳 E 的假出口**）

------

##### 第 4 层：赋值语句和关系表达式

**赋值语句 S = a = a + 1**：

- **赋值语句没有 true/false（不是布尔表达式）**
- **只有 begin 和 next**
- `S.begin = L1`，`S.next = L2`（前面已给出）

**关系表达式 E₁ = a < b 和 E₂ = a < c**：

- 它们是**最底层的布尔表达式**
- **各自有 begin、true、false 属性**
- **属性值都已由父节点传递下来，不需要再往下传**

------

#### 2.4 完整的属性标注 AST

```
              do-while
           begin=L1, next=Lnext
              /            \
             =              and
          begin=L1       begin=L2
          next=L2        true=L1
                         false=Lnext
           / \           /       \
          a   +         <         <
             / \    begin=L2    begin=L3
            a  1    true=L3     true=L1
                   false=Lnext  false=Lnext
                     / \         / \
                    a  b        a  c
```

------

### 第三步：生成三地址码（TAC）

#### 3.1 生成顺序

**按照深度优先、从左到右的顺序遍历 AST，依次生成代码**。每个节点**生成的代码段的第一条指令标号就是该节点的 begin 属性。**

------

#### 3.2 逐步生成

##### (1) 循环体 S：赋值语句 `a = a + 1`

**从 L1 开始，生成赋值语句的三地址码：**

```
L1: t1 = a + 1
    a = t1
```

- **第一条指令标号 L1，对应 S.begin**
- 执行完后，**下一条就是条件判断，对应 S.next = L2**

------

##### (2) 条件 E：逻辑与 `a < b and a < c`

**从 L2 开始，按短路求值生成。**

**第一步：E₁ = a < b**

```
L2: if a < b goto L3
    goto Lnext
```

- 第一条指令标号 L2，对应 E₁.begin
- 条件为真跳 L3（E₁.true = L3，继续算 E₂）
- 条件为假跳 Lnext（E₁.false = Lnext，整个表达式为假）

**第二步：E₂ = a < c**



```
L3: if a < c goto L1
    goto Lnext
```

- 第一条指令标号 L3，对应 E₂.begin
- 条件为真跳 L1（E₂.true = L1，整个表达式为真，回到循环体）
- 条件为假跳 Lnext（E₂.false = Lnext，整个表达式为假）

------

#### 3.3 完整的三地址码

将所有代码段**按顺序拼接：**







```
L1:     t1 = a + 1       // 循环体开始：计算 a+1
        a = t1           // 赋值给 a
L2:     if a < b goto L3 // 条件判断开始：先判断 a<b
        goto Lnext       // a<b 为假 → 整个条件为假，退出循环
L3:     if a < c goto L1 // 再判断 a<c
        goto Lnext       // a<c 为假 → 整个条件为假，退出循环
Lnext:                   // 循环结束，后续语句从这里开始
```

# 为什么这些地方要把E都列出来,不会重复吗

### 为什么每个 E 都要有自己的 true/false？

你观察到的 "重复" 是对的 —— 在这道题里：

- E₁.false = E.false = Lnext
- E₂.true = E.true = L1
- E₂.false = E.false = Lnext

看起来很多值都是一样的，好像可以直接用父节点的，不用单独列。

但这是**模块化设计的必然结果**，不是真的冗余。

------

### 1. 每个表达式只关心自己的 "出口"

**每个布尔表达式 E 只需要知道两件事：**

- **我为真时，跳去哪里？** → E.true
- **我为假时，跳去哪里？** → E.false

至于这个 "去哪里" 是循环的开头、另一个表达式的开头、还是整个程序的结尾，**E 自己不关心**。

这就像函数的参数：每个子表达式像一个函数，true 和 false 是它的两个 "参数"，调用者（父节点）告诉它 "真了跳这、假了跳那"，它只管生成自己的代码。

------

### 2. 为什么看起来 "重复"

在 `a < b and a < c` 这个简单例子里：

- 外层 E 的假出口 = 内层 E₁ 的假出口 = 内层 E₂ 的假出口
- 外层 E 的真出口 = 内层 E₂ 的真出口

所以值刚好一样，显得重复。

但如果是更复杂的嵌套表达式，比如：

plaintext









```
(a < b and c < d) or (e < f and g < h)
```

每个子表达式的 true/false 就都不一样了：

- 左边 and 的真出口 = 整个 or 的真出口
- 左边 and 的假出口 = 右边 and 的开始
- 右边 and 的真出口 = 整个 or 的真出口
- 右边 and 的假出口 = 整个 or 的假出口
- ……

每个子表达式的出口都不同，就不会觉得重复了。

------

### 3. 核心思想：自顶向下传递，每个节点只管好自己

表格







|  层级  |    表达式     |    true 出口     |     false 出口      |            由谁决定            |
| :----: | :-----------: | :--------------: | :-----------------: | :----------------------------: |
| 最外层 | E = E₁ and E₂ |   L1（循环体）   |    Lnext（退出）    | ** 父节点（do-while）** 告诉 E |
|  左子  |  E₁ = a < b   | L3（E₂ 的开始）  | Lnext（E 的假出口） |         **E** 告诉 E₁          |
|  右子  |  E₂ = a < c   | L1（E 的真出口） | Lnext（E 的假出口） |         **E** 告诉 E₂          |

每个表达式的 true/false 都是**父节点传下来的参数**，不是自己决定的。

- 父节点传什么，子节点就用什么
- 子节点不关心这个值是 "循环开头" 还是 "另一个表达式开头"

这样设计的好处是：**每个表达式的翻译代码是通用的**，不管它嵌在什么上下文里，只要给它 true 和 false 两个值，它就能生成正确的三地址码。

# 那这里E的false和true到底由什么决定啊,你这赋值写的不是在E1和E2都重复出现了吗

### 一、E 的 true 和 false 到底由什么决定？

**由它的父节点决定，是从外面 "传进来" 的参数**，不是 E 自己决定的。

你可以把每个布尔表达式想象成一个**通用模块**，它**需要两个 "出口参数" 才能工作：**

- **真出口（true）：我算出来为真时，跳去哪里？**
- **假出口（false）：我算出来为假时，跳去哪里？**

这两个值是**调用者（父节点）告诉它的**，它自己不知道也不关心。

------

#### 具体到这道题，一层一层看：

**第 1 层：最外层的 E（整个条件 `a<b and a<c`）**

它的父节点是 **do-while 语句**。**do-while 告诉 E：**

- **你为真 → 跳回循环体开头 L1（继续循环）**
- **你为假 → 跳到 Lnext（退出循环）**

所以：

plaintext









```
E.true = L1      ← do-while 给的
E.false = Lnext  ← do-while 给的
```

------

**第 2 层：E₁（`a < b`）和 E₂（`a < c`）**

它们的父节点是 **E（and 表达式）**。E 作为父节点，根据 and 的短路逻辑，分别告诉两个子表达式：

告诉 E₁（第一个条件）：

- 你为真 → 别停，继续去算 E₂（跳 L3）
- 你为假 → 整个 and 都假了，直接去 E 的假出口（跳 Lnext）

所以：

plaintext









```
E₁.true = L3        ← E 给的（去算 E₂）
E₁.false = E.false  ← E 给的（直接传 E 的假出口）
```

告诉 E₂（第二个条件）：

- 你为真 → 整个 and 都真了，去 E 的真出口（跳 L1）
- 你为假 → 整个 and 都假了，去 E 的假出口（跳 Lnext）

所以：

plaintext









```
E₂.true = E.true    ← E 给的（直接传 E 的真出口）
E₂.false = E.false  ← E 给的（直接传 E 的假出口）
```

------

### 二、E₁ 和 E₂ 都有 true/false，不是重复吗？

**不是重复，是 "每个表达式都需要自己的一套出口"**。

虽然在这个简单例子里，有些值刚好一样（比如 E₁.false = E.false，E₂.true = E.true），但这是**父节点传下来的结果**，不是冗余。

#### 打个比方

就像一个公司三级结构：老板 → 经理 → 两个员工

- 老板告诉经理："项目成了去 A 会议室，败了来我办公室"
- 经理告诉员工甲："你做完了去乙那儿，做砸了直接去老板办公室"
- 经理告诉员工乙："你做完了去 A 会议室，做砸了去老板办公室"

两个员工都有 "成了去哪、败了去哪" 两个属性，但：

- **值是经理分配的**，不是员工自己决定的
- **两个员工的值可能相同，也可能不同**
- 不能因为值相同就说 "重复了，合并成一个吧"—— 它们是两个独立的人，各干各的活

------

#### 换到表达式里也是一样

表格







|   表达式    | true 是谁给的 | false 是谁给的 |              值刚好等于谁的              |
| :---------: | :-----------: | :------------: | :--------------------------------------: |
| E（最外层） |   do-while    |    do-while    |                    —                     |
|     E₁      |   E（and）    |    E（and）    |           false 刚好 = E.false           |
|     E₂      |   E（and）    |    E（and）    | true 刚好 = E.true，false 刚好 = E.false |

**每个布尔表达式都必须有自己的 true 和 false**，因为：

1. 它是一个独立的模块，不知道自己嵌在什么地方
2. 它只认 "我真了跳这、我假了跳那" 两个参数
3. 父节点给它什么值，它就用什么值
4. 嵌套越深，子表达式越多，每个表达式的出口都可能不一样

在简单的 `a<b and a<c` 里，你觉得 "重复"；但如果是 `(a<b and c>d) or (e==f and g!=h)` 这种嵌套的，每个子表达式的 true/false 就都不一样了，就不会觉得重复了。