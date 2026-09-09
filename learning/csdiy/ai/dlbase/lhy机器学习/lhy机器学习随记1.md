# TIPS:

- Using an exclamation mark (!) **starts a new shell**, does the operations, and  then **kills that shell**
- percentage (%) affects the process associated  with the notebook, and it is called a magic command.  Use % instead of ! for cd (change directory) command 

# Colab and Kaggle Tutorial

## Colab 

### TIPS:

- 需要先save a copy in drive
- 上傳到Colab的檔案並不是永久存在的，檔案會在session斷線後消失。

### File Manipulation

Mounting Google Drive（掛載 Google 雲端硬碟）指的是將 **Google Drive 連接到一個運行環境（如本地電腦、雲端虛擬機或 Google Colab）**，讓其**像本地磁碟一樣使用**。這樣就可以**直接**讀取、寫入、修改 Google Drive 上的文件和資料夾。

執行後，會跳出授權連結，點擊後登入 Google 帳號，複製驗證碼並貼回 Colab。 掛載成功後，Google Drive 會出現在 [/content/drive](https://colab.research.google.com/drive/1m3UfFC7IFO3H_-N9kpfMGyl8QXMp2lSM#) 目錄下。

![image-20260301182530627](./lhy%E6%9C%BA%E5%99%A8%E5%AD%A6%E4%B9%A0%E9%9A%8F%E8%AE%B01/image-20260301182530627.png)

# **Pytorch Tutorial**

## QA

![image-20260304194601496](./lhy%E6%9C%BA%E5%99%A8%E5%AD%A6%E4%B9%A0%E9%9A%8F%E8%AE%B01/image-20260304194601496.png)

### z.backward()有什么用,为什么是写x.grad而非z.grad?

PyTorch 这个框架最强大的“魔法”——**自动求导 (Autograd)** 机制。

我们结合你刚才学的**前向传播**，来把这几行代码彻底弄懂：

### 1. `z.backward()` 有什么用？（按下“反向传播”的按钮）

还记得我们刚聊过的**前向传播**吗？代码里的第 1 步和第 2 步，就是前向传播：你给了一个输入 $x$，经过**平方和求和的运算，最终得到了一个结果 $z$。**

- **`z.backward()` 的作用就是发号施令：** “嘿，PyTorch，我的**前向计算做完了（拿到了结果 $z$）**。现在请你顺着刚才的计算路径**反向走一遍**，帮我**算出所有参与计算的变量的导数（梯度）！**”
- 在这个命令执行之前，你的电脑里**只有计算结果**，没有导数。按下这个按钮，微积分引擎才开始疯狂运转。

### 2. 为什么是查 `x.grad` 而非 `z.grad`？（谁才是我们要调整的人？）

这跟我们机器学习的**根本目的**有关。

- **我们要的是“灵敏度”：** 在训练神经网络时，$z$ 通常代表我们的**最终误差（Loss）**，而 $x$ 代表我们的**模型参数（权重）**。
- 我们关心的问题是：**“如果我稍微调整一下参数 $x$，最终的误差 $z$ 会发生怎样的变化？”** 数学上这就是求偏导 $\frac{\partial z}{\partial x}$。
- **PyTorch 的贴心设计：** **当 `z.backward()` 运行完后，它会把算出来的导数 $\frac{\partial z}{\partial x}$ 巧妙地塞进自变量 $x$ 的口袋里。**这个口袋就叫 `.grad`。所以，`x.grad` 读出来的意思是：**“最终结果 $z$ 针对我 ($x$) 的导数是多少”**。

**为什么不是查 `z.grad`？**

因为 $z$ 是最终的那个结果本身。探讨**“结果对结果自己的导数” $\frac{\partial z}{\partial z}$ 永远是 1**，这没有任何意义。我们**想要改变的是源头（$x$），所以我们要看源头的梯度（`x.grad`）。**

你看图里右下角的公式，因为 $z = \sum x^2$，所以导数是 $2x$。代码输出的 `x.grad` 里的数字，正好就是原来 $x$ 矩阵里数字的两倍，完美对应！

## 为什么z的形式是长这样的?这样子z的结果不是一个标量吗?

![image-20260304194830895](./lhy%E6%9C%BA%E5%99%A8%E5%AD%A6%E4%B9%A0%E9%9A%8F%E8%AE%B01/image-20260304194830895.png)

你的观察力太敏锐了！**没错，$z$ 的结果绝对是一个标量（Scalar，即一个单独的数字）！**

这不仅仅是这个例子的巧合，而是深度学习中一个**铁打的定律**：**在调用 `.backward()` 时，最终的目标变量（通常是 Loss）必须是一个标量。**

我们来详细拆解一下，为什么这个公式长这样，以及为什么必须是标量：

### 1. 为什么公式长这样？

图片里的公式 $z = \sum_i \sum_j x_{i,j}^2$ 其实是一个为了演示求导过程而构造的**“玩具公式”**。

- 代码 `x.pow(2)` 把矩阵 $x$ 里的**每一个元素都自己平方**。
- 代码 `.sum()` 把**所有平方后的数字加在一起**，融合成**唯一的一个总和** $z$。
- 如果在真实的机器学习场景里，你可以把这个过程想象成**均方误差 (MSE, Mean Squared Error)** 的简化版：把所有的预测误差平方一下，然后全部加起来，得到一个总的误差分数。

### 2. 为什么 $z$ 必须是一个标量？（为什么不输出矩阵？）

想象一下，如果 $z$ 不是一个数字，而是一个包含了 10 个数字的向量（比如代表 10 个不同科目的考试成绩）。

- **如果你向机器下令：** “请你调整平时的学习时间（更新参数 $x$），让结果 $z$ 变小！”
- **机器会崩溃的：** “老板，你是想让数学成绩变好一点，还是让英语成绩变好一点？如果我多睡一小时，数学提高了 2 分，但英语下降了 3 分，这算好还是坏？我到底该听谁的？”

**为了让机器有一个明确的“唯一目标”可以努力**，我们必须**把所有的表现浓缩成一个最终评分（标量）。**

- 这个评分就是 **Loss（损失值）**。
- 只有当 $z$ 是一个唯一的数字时，机器**才能明确地计算出：矩阵 $x$ 里的每一个元素，到底是该变大一点，还是该变小一点，才能让最终的 $z$ 降到最低。**

### 3. 数学视角的补充

如果在 PyTorch 中，你想对一个**非标量**（比如**向量或矩阵**）调用 `.backward()`，程序会**直接报错**。如果你真的非要**对矩阵求导**，数学上得到的**结果会是一个“雅可比矩阵（Jacobian）”（也就是高维的导数张量）**，计算量极大，而且通常不是我们在梯度下降中想要的。

------

**总结一下：**

**把矩阵 $x$ 经过一番折腾（平方、求和）变成一个标量 $z$**，就是为了**给反向传播提供一个唯一的“靶心”**。**所有的参数 $x$ 都会根据这个唯一的靶心来调整自己的方向。**



# HW1

TIPS:

- heuristics (e.g. classifying the questions based on their lengths, determining if the question need a search or not, reconfirm the answer before returning it to the user......)

## QA

### 解释代码

```py
from typing import List
from googlesearch import search as _search # 导入Google搜索库，重命名为_search避免与下方自定义的search函数命名冲突
from bs4 import BeautifulSoup # 用于解析HTML并提取网页中的纯文本
from charset_normalizer import detect # 用于检测字节流的字符编码格式
import asyncio # Python的原生异步I/O库，用于实现非阻塞的并发网络请求
from requests_html import AsyncHTMLSession # 提供支持异步请求的HTML会话环境
import urllib3
urllib3.disable_warnings() # 禁用urllib3的警告信息（主要是为了屏蔽下方请求中 verify=False 触发的不安全请求警告）

async def worker(s: AsyncHTMLSession, url: str):
    """
    异步工作函数：负责向单个URL发送请求并安全地获取其HTML文本。
    """
    try:
        # 步骤1：先发送一个HEAD请求（只请求响应头，不下载网页内容主体）。
        # verify=False表示不验证SSL证书，timeout=10设置10秒超时防止卡死。
        header_response = await asyncio.wait_for(s.head(url, verify=False), timeout=10)
        
        # 检查响应头中的'Content-Type'字段，判断目标URL是否为HTML页面。
        # 如果不是（例如直接指向PDF、图片或压缩包的链接），则返回None，避免下载无用的大文件。
        if 'text/html' not in header_response.headers.get('Content-Type', ''):
            return None
            
        # 步骤2：确认是HTML页面后，正式发送GET请求获取网页主体内容，同样设置10秒超时。
        r = await asyncio.wait_for(s.get(url, verify=False), timeout=10)
        
        # 返回请求到的HTML文本内容
        return r.text
    except:
        # 捕获执行过程中的所有异常（如超时、网络断开、DNS解析失败等）。
        # 遇到错误时不中断程序，而是直接返回None。
        return None

async def get_htmls(urls):
    """
    并发调度函数：接收URL列表，并发执行获取所有URL的网页源码。
    """
    # 初始化一个异步HTML会话实例
    session = AsyncHTMLSession()
    
    # 为传入的每个URL创建一个worker协程任务（此处使用了生成器表达式）
    tasks = (worker(session, url) for url in urls)
    
    # 使用asyncio.gather并发执行所有构建好的任务，等待它们全部完成，并返回包含所有结果的列表
    return await asyncio.gather(*tasks)

async def search(keyword: str, n_results: int=3) -> List[str]:
    '''
    核心主函数：搜索指定关键词，并提取排名靠前网页的纯文本内容。

    警告注释翻译：如果在短时间内搜索次数过多，可能会遭遇HTTP 429（请求过多）错误。这是无法避免的，如果想一次尝试搜索更多结果需要自行承担风险。
    Google并未明确公布速率限制，因此除了更换IP或等待Google解禁（我们也不知道惩罚持续时间）之外，没有太多应对办法。
    '''
    # 截取搜索关键词的前100个字符，防止过长的查询参数导致Google搜索报错
    keyword = keyword[:100]
    
    # 步骤1：调用同步的_search函数在Google获取搜索结果URL。
    # 为了防止获取到的URL中有死链或非HTML格式，这里冗余获取了目标数量两倍 (n_results * 2) 的链接。
    # lang="zh"指定搜索中文内容，unique=True确保返回的URL不重复。
    results = list(_search(keyword, n_results * 2, lang="zh", unique=True))
    
    # 步骤2：调用并发函数，批量且异步地获取这些URL的HTML源代码。
    results = await get_htmls(results)
    
    # 步骤3：清洗数据，过滤掉列表中由于请求失败或非HTML页面而返回None的无效结果。
    results = [x for x in results if x is not None]
    
    # 步骤4：将有效的HTML文本转化为BeautifulSoup对象，构建文档树，方便后续解析。
    results = [BeautifulSoup(x, 'html.parser') for x in results]
    
    # 步骤5：深度清洗与提取文本。这是一个列表推导式，包含两个操作：
    # 条件判断: detect(x.encode()).get('encoding') == 'utf-8' 
    #   -> 将解析对象重新编码并检测其编码格式，严格剔除掉非UTF-8编码的页面（防止后续出现乱码）。
    # 文本处理: ''.join(x.get_text().split()) 
    #   -> x.get_text() 剥离HTML标签，提取纯文本。
    #   -> .split() 将文本按空白字符分割成列表。
    #   -> ''.join() 将列表重新拼接。这套组合拳的作用是：彻底清除提取文本中的所有空格、换行符和制表符。
    results = [''.join(x.get_text().split()) for x in results if detect(x.encode()).get('encoding') == 'utf-8']
    
    # 步骤6：由于前面获取了双倍结果以备过滤，这里将清洗干净的结果截取前 n_results 个并返回。
    return results[:n_results]
```

### 代码里的await关键词有什么作用,async关键词又有什么作用

在 Python 中，`async` 和 `await` 是用来实现**异步编程（Asynchronous Programming）**的核心关键字。它们的主要目的是在**处理耗时操作（如网络请求、数据库查询等 I/O 密集型任务）时，不阻塞主程序的运行**，从而大幅**提高程序的并发处理能力**。

为了更直观地理解，你可以先看看同步与异步执行的区别：

以下是这两个关键字的具体作用：

### `async` 的作用

- **定义协程（Coroutine）：** 当你在**函数定义 `def` 前面加上 `async` 时**（即 `async def`），你就把这个普通函数**变成了一个异步函数，官方称之为“协程”。**
- **改变调用机制：** 普通函数一经调用就会立刻执行代码并返回结果。**但 `async` 函数被调用时不会立即执行内部代码，**而是**返回一个“协程对象”**。要让内部代码**真正跑起来，必须配合 `await` 调用**，或者将其**放入事件循环（Event Loop）中调度。**

### `await` 的作用

- **交出控制权（非阻塞等待）：** **`await` 必须且只能写在 `async` 函数的内部**。它的核心台词是：**“我现在要等这个耗时的操作完成。在干等的这段时间里，我把系统的控制权交出去，**让程序先去干别的事情。”
- **获取异步结果：** 它像**一个标记点**。当那个耗时的操作（比如下载网页）终于**在后台完成后，程序会带着下载好的数据回到 `await` 所在的这行代码，把数据赋值给变量，然后继续往下执行。**

### 结合你提供的代码来理解

在你刚才那段爬虫代码中，网络请求是最耗时的部分。这两个关键字的应用完美体现了异步的优势：

1. **如果不使用它们（同步模式）：** 假设你要搜索并获取 6 个网页的代码。程序**会去请求第 1 个网页，死等网络响应；拿到结果后，再去请求第 2 个网页，继续死等。**时间是**线性累加**的。
2. **使用了 `async` / `await`（异步模式）：** 在 `get_htmls` 调度函数中，程序**通过并发任务向 6 个网页同时发起请求**。当 `worker` 内部执行到 `await asyncio.wait_for(s.get(url...))` 时，它告诉系统：“第 1 个网页还在建立连接，别在这傻等，快去发第 2 个网页的请求。” 这样，**所有网页的网络等待时间发生了重叠**，**整体**耗时基本**只取决于响应最慢的那个网页。**

### 为我精简解释一下以下代码的实现逻辑:

 

```py
   # 3. 整合搜索结果作为上下文，并进行简单的长度截断以符合 context window
    context = "\n".join(search_results)
    if len(context) > 12000: # 预留空间给系统提示和问题
        context = context[:12000]
```

核心目的是**将获取到的信息打包，并限制其大小以适配模型的输入上限**。

具体可以精简为两步：

1. **拼接整合（合并）：** `"\n".join(search_results)` 将一个包含多条搜索结果的**列表（List）**，**用换行符连成一整个长字符串**。这就相当于把散落的参考资料整理成了一篇长文档。
2. **截断保护（防溢出）：** 就像在跑深度学习模型时，输入序列（比如 Transformer 的 Sequence Length）都有**严格的维度或长度上限**。这里的 `if` 语句检查拼接后的长文本**是否超过了 12000 个字符。如果超过，就利用 Python 的切片功能 `[:12000]` 强行截断**，丢弃后面的内容。

**为什么要这么做？** 大语言模型有一个固定的“上下文窗口”（Context Window）。如果搜索结果太长，直接塞给模型不仅可能导致报错，还会把系统设定（System Prompt）和你真正想问的问题挤掉。设定 12000 的阈值，就是为了给这些更关键的指令预留出安全空间。

------

目前这种“硬截断”（直接切断字符）可能会导致最后一句话被砍断一半。

# HW

## QA

### 为什么有两个return语句

![image-20260307130323739](./lhy%E6%9C%BA%E5%99%A8%E5%AD%A6%E4%B9%A0%E9%9A%8F%E8%AE%B01/image-20260307130323739.png)

在 `plan_and_code_query` 方法中之所以有两个 `return` 语句，是为了处理**重试逻辑**和**失败回调**：

1. **第一个 `return nl_text, code` (在循环内)**： 这是「成功路径」。代码会尝试最多 3 次 (`retries=3`)。如果在某次尝试中，LLM 生成的内容能够成功被 `extract_code` 提取出有效的 Python 代码，函数就会立即返回解析出的方案计划和代码，不再继续后面的循环。
2. **第二个 `return "", completion_text` (在循环外)**： 这是「**兜底**/失败路径」。如果循环运行了 3 次都没有成功提取到代码块（例如模型没有按格式输出），代码会跳出循环。此时，它会返回一个空字符串作为计划，并将原始的全文回复 `completion_text` 返回。这可以防止程序因为无法提取代码而直接崩溃，同时也方便调试查看模型到底输出了什么。

这种结构在处理不稳定的 LLM 输出时非常常见，确保了即使在解析失败的情况下，函数依然有一个明确的返回格式。



# lecture 3

## QA

### unembeded,unembeding,embeded和embeding这些专有名词在transformer里面分别意味着什么

在标准英文文献和代码中，这些词通常带有双写字母 `d`，即 **embedding / embedded** 和 **unembedding / unembedded**。

在 Transformer 模型（以及大多数现代 NLP 模型）中，这四个词描述的是**数据在进入模型和离开模型时的状态转换过程**。你可以把它们理解为模型的“入口翻译官”和“出口翻译官”。

为了让你能精简地理解，我们把它们成对来拆解：

### 1. 模型的“入口”：Embedding 与 Embedded

在 Transformer 处理数据之前，我们需要把人类的语言（离散的词语**或 Token**）转换成神经网络能看懂的**数学向量（连续的浮点数）**。

- **Embedding（动名词/名词 - 嵌入）：**
	- **含义**：指的是**将离散标记映射为连续向量的“动作”或“层”（Embedding Layer）**。
	- **通俗理解**：你可以把它**当成一本“查词典”的动作**。**输入一个 Token ID**（比如单词 "apple" 对应的数字编码 `852`），**Embedding 层负责在一个巨大的权重矩阵中查找到第 852 行，取出那一行的稠密向量**（比如一个 768 维的浮点数数组）。
	- **代码对应**：PyTorch 中的 `nn.Embedding(vocab_size, hidden_size)`。
- **Embedded（形容词/过去分词 - 已嵌入的）：**
	- **含义**：描述的是**数据经过 Embedding 层之后的状态**。
	- **通俗理解**：当 "apple" 这个词**被转换成了那串 768 维的向量后，我们就可以说这个特征是 "embedded"**（已经被嵌入到高维连续空间中）了。Transformer 内部的**注意力机制（Self-Attention）全都是在处理这些 embedded 的向量。**

### 2. 模型的“出口”：Unembedding 与 Unembedded

当 Transformer 经过**多层复杂的注意力计算，最终输出了一堆高度抽象的隐藏状态向量（Hidden States）**时，我们需要把它们**重新翻译回人类能看懂的语言。**

- **Unembedding（动名词/名词 - 解嵌 / 反嵌入）：**
	- **含义**：指的是**将高维连续的隐藏状态向量，重新映射回离散词表空间的“动作”或“层”**。
	- **通俗理解**：这是 Embedding 的逆过程。它**通常是一个线性分类层（Linear Layer）**。它把 Transformer 输出的 768 维向量，**转换成一个长度等于词表大小（比如 50,000）的向量。**
	- **代码对应**：通常是模型**最后的一层 `nn.Linear(hidden_size, vocab_size)`。**
- **Unembedded（形容词/过去分词 - 已解嵌的）：**
	- **含义**：描述的是**数据经过 Unembedding 层之后的状态**。
	- **通俗理解**：经过线性层映射后，我们得到的是**长度为 50,000 的 Logits（未归一化的分数）**。此时的**数据状态就是 unembedded**。随后，我们**对这个 unembedded 的结果进行 Softmax 操作，就能得出预测下一个词的概率分布。**

------

### 精简总结对比表

| **阶段**          | **英文词汇**    | **词性**    | **作用与含义**                         | **数据维度变化**             |
| ----------------- | --------------- | ----------- | -------------------------------------- | ---------------------------- |
| **入口 (输入端)** | **Embedding**   | 名词/动作   | 将 **Token ID 查表转化为向量**的层     | `[词表大小] -> [隐藏层维度]` |
|                   | **Embedded**    | 形容词/状态 | 已经被转化为向量的数据本身             | 数据目前是 `[隐藏层维度]`    |
| **出口 (输出端)** | **Unembedding** | 名词/动作   | 将最终向量映射回词表分数的层           | `[隐藏层维度] -> [词表大小]` |
|                   | **Unembedded**  | 形容词/状态 | 已经转**换为词表分数（Logits）的数据** | 数据目前是 `[词表大小]`      |

在很多经典的 Transformer 模型（比如 GPT 系列）中，为了节省显存和加速训练，**Embedding 层的权重矩阵和 Unembedding 层的权重矩阵是完全共享（转置使用）的**，这在工程上被称为**“权重绑定”（Weight Tying）。**

