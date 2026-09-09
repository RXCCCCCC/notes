# 第三章

## 第一部分：语言模型的演进路径 (Evolution of Language Models)

### 1. 统计语言模型与 N-gram (Statistical LM & N-gram)

**📍 指引：请看讲义 3.1.1 节第 (1) 部分《统计语言模型与 N-gram 的思想》。**

这一部分探讨的是“如何让计算机计算一句话出现的概率”。

- **核心逻辑 (Core Logic):** 讲义给出了 **Chain Rule of Probability** (概率的链式法则)。你会看到一个复杂的公式：

	$$P(S) = P(w_1) \cdot P(w_2|w_1) \cdots P(w_m|w_1, \dots, w_{m-1})$$

	**老师点评：** 这个公式在实际中“算不动”，因为前面的上下文太长了。于是引入了 **Markov Assumption (马尔可夫假设)**：即假设下一个词只跟前面最近的 $N-1$ 个词有关。

- **计算方式 (Calculation):** 讲义中提到了 **Maximum Likelihood Estimation (MLE, 最大似然估计)**。

	- **Bigram (N=2):** 下一个词只看前 **1** 个词。公式为：$P(w_i | w_{i-1}) = \frac{Count(w_{i-1}, w_i)}{Count(w_{i-1})}$。
	- **实战练习：** 讲义中举了 "datawhale agent learns" 的例子。通过在 **Corpus (语料库)** 中数数（Counting）来计算概率。

- **局限性 (Limitations):** * **Data Sparsity (数据稀疏性):** **只要有一个词对没见过，概率就变 0。**

	- **Poor Generalization (泛化能力差):** 模型**不理解语义。它不知道 "agent" 和 "robot" 其实很像。**

------

### 2. 神经网络语言模型与词嵌入 (Neural LM & Word Embedding)

**📍 指引：请看讲义 3.1.1 节第 (2) 部分《神经网络语言模型与词嵌入》。**

这是自然语言处理的“第一次革命”。

- **破局点 (The Breakthrough):** **放弃“数数**”，改用 **Continuous Vector (连续向量)**。
- **Word Embedding (词嵌入):** 讲义提到，每个词被**映射到高维空间的点**。
	- **Semantic Space (语义空间):** **意思相近的词，距离就近。**
	- **Cosine Similarity (余弦相似度):** 讲义给出了公式 $\cos(\theta) = \frac{\vec{a} \cdot \vec{b}}{|\vec{a}| |\vec{b}|}$。这是评估两个词“像不像”的标准英文术语。
- **词向量运算 (Vector Arithmetic):** * 讲义展示了那个著名的案例：$vector('King') - vector('Man') + vector('Woman') \approx vector('Queen')$。
	- **理解：** 这证明模型学到了 **Gender (性别)** 和 **Royalty (皇室)** 这些抽象特征。在 PyTorch 中，这**对应的就是 `nn.Embedding` 层。**

------

### 3. 循环神经网络 (RNN) 与长短时记忆 (LSTM)

**📍 指引：请看讲义 3.1.1 节第 (3) 部分《循环神经网络 (RNN) 与长短时记忆网络 (LSTM)》。**

这是为了解决“窗口大小固定”的问题。

- **RNN 的创新:** 引入了 **Hidden State (隐藏状态)**，它像一个“滚雪球”的记忆，**每一步都会结合当前输入和上一步的记忆。**
- **致命伤 (The Achilles' Heel):** * **Long-term Dependency (长期依赖问题):** RNN 很难记住太久之前的信息。
	- **Gradient Vanishing/Explosion (梯度消失/爆炸):** 讲义提到，由于反向传播时需要连乘（Chain Rule），误差传到前面就几乎消失了。
- **LSTM 的补救:** 讲义介绍了一种**带有 Gating Mechanism (门控机制) 的特殊 RNN**。
	- **Cell State (细胞状态):** 这是**一条信息“高速公路”，信息可以长距离无损传输**。
	- **Three Gates:**
		1. **Forget Gate (遗忘门):** **决定丢弃**哪些旧记忆。
		2. **Input Gate (输入门):** **决定存入**哪些新信息。
		3. **Output Gate (输出门):** **决定这一步对外**输出什么。

------

### 📖 第一部分自测 (Review Question)

为了确保你跟上了“老师”的节奏，请尝试回答以下问题：

**Question:** In the context of N-gram models, why is the **Markov Assumption** necessary? Also, explain how **LSTM** addresses the **Gradient Vanishing** problem found in standard **RNNs**.

(在 N-gram 模型的语境下，为什么**马尔可夫假设**是必要的？另外，解释 **LSTM** 是如何解决标准 **RNN** 中存在的**梯度消失**问题的。)

> **Answer (请遮住查看):**
>
> 1. **Markov Assumption:** Because calculating the absolute probability of a long sentence is computationally impossible due to the sheer variety of word combinations. The assumption simplifies this by assuming a word only depends on its immediate **context** (the previous $N-1$ words).
>
> 	(因为**直接计算长句子的绝对概率在计算上是不可能的**。该假设通过**设定一个词仅依赖其直接上下文**来简化计算。)
>
> 2. **LSTM & Gradient Vanishing:** LSTM introduces a **Cell State** (a memory line) and **Gating Mechanisms**. These gates allow the network to explicitly decide what to remember and what to forget, creating a "shortcut" for information and gradients to flow through long sequences without being repeatedly multiplied by small weights.
>
> 	(LSTM 引入了**细胞状态**和**门控机制**。这些门**允许网络显式决定记住什么和遗忘什么**，为**信息和梯度在长序列中流动创建了“捷径”**，**避免了因反复乘以微小权重而导致的衰减**。)

------

## 第二部分：现代大模型的基石 - Transformer (The Transformer Architecture)

这一部分讲义中关于 RNN 和 LSTM 的图示非常重要，建议你对着讲义中的 **图3.3** 仔细观察那个循环结构。

好的，请翻开讲义，我们要进入最激动人心的 **Transformer** 时代。这部分内容不仅是智能体的核心，也是你之后进行 **PyTorch** 编程和理解大模型论文的基础。

------

### 1. Encoder-Decoder 整体结构 (Encoder-Decoder Architecture)

**📍 指引：请看讲义 3.1.2 节第 (1) 部分《Encoder-Decoder 整体结构》，特别是图 3.4。**

在这一页，讲义向你展示了 Transformer 的“全景图”。

- **核心理念 (The Core Philosophy):** 以往的 RNN 像是在**串行 (Serial)** 读报纸，读到一个词才能读下一个；而 Transformer 像是在**并行 (Parallel)** 扫视整张报纸。

- **分工与协作 (Division of Labor):**

	- **Encoder (编码器):** 负责“输入理解”。它把原始词元（Tokens）转换成富有语义的向量。注意讲义代码中**的 `EncoderLayer`，它通过 `self_attn` 捕捉句子内部的联系。**
	- **Decoder (解码器):** 负责“预测输出”。它比 Encoder 多了一个 `cross_attn` (交叉注意力层)，这就像是在写作业时，一边写（参考已写出的前文），一边翻书（咨询 Encoder 提供的输入信息）。

- **代码范式 (Coding Pattern):** 讲义在这里给出的 PyTorch 骨架非常典型。

	- **模块化 (Modularity):** 你会看到**每一个 `Layer` 都继承自 `nn.Module`。**
	- **组合 (Composition):** 注意 `EncoderLayer` 里的 `norm1`, `norm2` (**Layer Normalization**) 和 `dropout`。这种“**残差连接 (Residual Connection)** + **层归一化 (LayerNorm)**”的结构（即代码中的 `x = self.norm1(x + self.dropout(attn_output))`）是训练深层网络而不崩坏的关键。

- ![Transformer Architecture, AI generated](./helloagent%E9%9A%8F%E8%AE%B01/licensed-image.jpeg)

	Getty Images

------

### 2. 自注意力机制 (Self-Attention)

**📍 指引：请看讲义 3.1.2 节第 (2) 部分《从自注意力到多头注意力》。**

这是 Transformer 的灵魂。讲义用了一个非常生动的比喻——“**开卷考试 (Open-book Exam)**”。

- **三大角色 (The Three Roles):** 为了实现这种搜索，每个词都会**生成三个向量**：
	1. **Query (Q, 查询):** “我**正在找什么**？”（**代表当前词主动去寻找信息**）。
	2. **Key (K, 键):** “我**有什么标签**？”（代表**其他词被查询的特征**）。
	3. **Value (V, 值):** “我包**含的具体信息是什么**？”（代表**词元携带的实际内容**）。
- **计算四部曲 (The Four Steps):**
	1. **Scoring (打分):** 用 **Q 与所有 K 进行 Dot-product (点积)。**这就是我们之前聊过的 `torch.matmul(Q, K.transpose(-2, -1))`。分值越高，表示**相关性（Relevance）越强。**
	2. **Scaling (缩放):** 讲义强调要除以$sqrt(d_k)$。这叫 **Scaled Dot-Product**，目的是防止点积结果过大导致 **Gradient Vanishing (梯度消失)**。
	3. **Normalizing (归一化):** 通过 **Softmax** 函数。这让所有分值变成 0 到 1 之间的概率分布（总和为 1），也就是 **Attention Weights (注意力权重)**。
	4. **Weighted Sum (加权求和):** 用**权重乘以对应的 V。**

**老师补充：** 最终得到的向量，就不再是孤立的词向量了，而是吸收了全句精华的“上下文向量”。

------

### 📖 第二部分自测 (Phase 2 Checkpoint)

既然你是软件工程专业的，尝试从算法设计和系统架构的角度回答以下问题：

**Question:** Why is **Parallel Computation** in Transformer superior to the **Sequential Processing** of RNNs in terms of hardware efficiency? Also, explain the functional difference between the **Self-Attention** in the Encoder and the **Cross-Attention** in the Decoder. (为什么 Transformer 的**并行计算**在硬件效率上优于 RNN 的**顺序处理**？另外，请解释 Encoder 中的**自注意力**与 Decoder 中的**交叉注意力**在功能上的区别。)

> **Answer (请遮住查看):**
>
> 1. **Efficiency:** RNNs require the hidden state of step t−1 to compute step t, which creates a **computational bottleneck** (sequential dependency) that prevents GPUs from utilizing their massive parallel cores. Transformer's Attention mechanism allows all tokens to be processed simultaneously, maximizing **GPU throughput**. (RNN **需要前一时刻的状态来计算当前时刻，**这种顺序依赖形成了计算瓶颈；而 Transformer 的注意力机制**允许所有词元同时处理**，**最大化了 GPU 的吞吐量**。)
> 2. **Self vs. Cross Attention:** >     * **Self-Attention** computes the relationships within the *same* sequence (e.g., how "it" refers back to "agent").
> 	- **Cross-Attention** computes relationships between *two different* sequences: it uses Queries from the Decoder to query Keys and Values from the Encoder's output, allowing the model to "focus" on relevant parts of the input while generating the output. (自注意力计算**同一序列内部的关系**；交叉注意力计算**两个不同序列间的关系**：它**用解码器的 Query 去查询编码器输出的 Key 和 Value**，让模型在**生成输出时能“关注”输入中的相关部分**。)

------

讲义接下来会涉及到 **Multi-Head Attention (多头注意力)**，这可以理解为“多个人从不同角度（维度）同时看同一张卷子”。

## 第三部分

攻克 Transformer 中负责“深层特征提取”和“非线性变换”的两个核心组件。这部分内容决定了模型不仅能“看到”联系，还能“看懂”复杂的逻辑。

------

### 1. 多头注意力机制 (Multi-Head Attention, MHA)

**📍 指引：请看讲义 3.1.2 (2) 《从自注意力到多头注意力》中关于“多头”的描述。**

如果说自注意力是让你“盯着全句看”，那么**多头注意力**就是让你“**带上几副不同的眼镜同时看**”。

- **为什么要“多头”？ (The Motivation):**

	单纯的**一层注意力（Single-Head）可能会被某一个强烈的信号掩盖**。比如在句子 "The animal didn't cross the street because it was too tired" 中：

	- **头 A** 可能专注于 **Syntactic (语法)** 关系：发现 "it" 是主语。
	- **头 B** 可能专注于 **Semantic (语义)** 关系：发现 "it" 指代 "animal"。

- **计算逻辑 (The Mechanism):**

	1. **Split (拆分):** 讲义代码中虽然是占位符，但逻辑是**将高维的 $Q, K, V$ 拆分成 $h$ 个低维的子空间。**
	2. **Parallel Attention (并行注意):** 每个“头”**独立进行上一节讲过的“点积打分”。**
	3. **Concatenate (拼接):** 将**所有头的输出拼在一起**。
	4. **Linear Projection (线性投影):** 最后**通过一个权重矩阵 $W^O$ 进行融合**，把分散的注意力结果合成一个最终向量。

------

### 2. 位置前馈网络 (Position-Wise FeedForward Network, FFN)

**📍 指引：请看讲义 3.1.2 (4) 《位置前馈网络》。**

在 `EncoderLayer` 的代码里，你会看到**执行完 `self_attn` 后，紧接着就是一个 `feed_forward`。**

- **它的角色 (The Role):**

	如果说 Attention 负责的是 **Communication (信息交互)**（让**词之间打招呼**），那么 FFN 负责的就是 **Computation (信息加工)**（让**每个词自己消化吸收**）。

- **结构特点 (Structure):**

	1. **Position-Wise (逐位置):** 它是对序列中的每个词**独立且并行**地**应用相同的操作。**
	2. **Two-Layer Linear (两层线性变换):** 通常**由两个全连接层（Fully Connected Layers）组成**，中间夹一个 **Activation Function (激活函数)**，如 ReLU 或 GELU。

	- **数学形式:** $FFN(x) = \max(0, xW_1 + b_1)W_2 + b_2$。

- **为什么需要它？ (The Necessity):**

	Attention 本质上是**词向量的加权线性组合 (Linear Combination)**。如果**没有 FFN 这种非线性变换，模型就只是一个巨大的线性回归**，**无法处理极其复杂的逻辑模式**。

- ![Transformer Feed-Forward Network, AI generated](./helloagent%E9%9A%8F%E8%AE%B01/licensed-image-1774354489078-3.jpeg)

------

### 📖 第三部分自测 (Phase 3 Checkpoint)

作为软件工程专业的学生，理解算法的“表现力”至关重要，请尝试分析以下问题：

**Question:** In the Transformer architecture, what would be the consequence of removing the **Position-Wise FeedForward Network**? Also, how does **Multi-Head Attention** improve the model's **Representation Subspaces** (表达子空间)?

(在 Transformer 架构中，如果移除**位置前馈网络**会有什么后果？此外，**多头注意力**是如何改善模型的**表达子空间**的？)

> **Answer (请遮住查看):**
>
> 1. **Consequence of removing FFN:** The model would lose its **Non-linear Transformation** capability. Since Attention is a weighted linear sum, a stack of pure Attention layers remains a linear model, which lacks the **Model Capacity** to fit complex datasets. The FFN provides the necessary non-linearity to process features extracted by Attention.
>
> 	(后果：模型将失去**非线性变换**能力。由于注意力机制是加权线性求和，**纯注意力层的堆叠仍然是一个线性模型**，缺乏拟合复杂数据集的**模型容量**。FFN 提供了处理注意力提取特征所必需的非线性。)
>
> 2. **Multi-Head & Representation:** Single-head attention forces the model to focus on one single "averaged" relationship. **Multi-Head Attention** allows the model to jointly attend to information from different **Representation Subspaces** at different positions, capturing multiple types of dependencies (e.g., long-range vs. short-range, or grammatical vs. factual) simultaneously.
>
> 	(多头与表达：**单头注意力迫使模型关注一个“平均”关系**。**多头注意力**允许模型**同时关注来自不同位置、不同表达子空间的信息**，同时**捕捉多种类型的依赖关系**，如长程与短程、或语法与事实关系。)

## QA

## 这段代码的K.transpose(-2, -1)是干什么用的

```py
attn_scores = torch.matmul(Q, K.transpose(-2, -1)) / math.sqrt(self.d_k)
```

在 PyTorch 代码中，`K.transpose(-2, -1)` 的核心作用是进行 **Matrix Transposition** (矩阵转置)，以便满足矩阵乘法的维度匹配要求。

------

### 1. 维度对齐 (Dimension Alignment) 📐

在深度学习中，Query ($Q$) 和 Key ($K$) 的**张量维度通常是：**

```
[Batch_Size, Num_Heads, Seq_Len, d_k]
```

- **Seq_Len (序列长度):** 句子中**有多少个词（Token）**。
- **d_k (特征维度):** 每个**词被表示向量的长度。**

为了**计算 $Q$ 和 $K$ 之间的相似度（即注意力分数）**，我们需要**执行点积**。根据线性代数规则，两个矩阵 $A$ 和 $B$ 相乘，若 $A$ 的维度是 $(m \times n)$，则 $B$ 的维度必须是 $(n \times p)$。

- $Q$ 的**最后两个维度是 `(Seq_Len, d_k)`。**
- $K$ 的**最后两个维度也是 `(Seq_Len, d_k)`。**

直接相乘是不行的。通过 `K.transpose(-2, -1)`，我们将 **$K$ 的最后两个维度交换**，变成了 `(d_k, Seq_Len)`。

### 2. 语义理解 (Semantic Meaning) 🧠

这一步的数学表达式是：

$$\text{Attention}(Q, K, V) = \text{softmax}\left(\frac{QK^T}{\sqrt{d_k}}\right)V$$

- **$QK^T$ 的结果：** 得到的**矩阵维度是 `(Seq_Len, Seq_Len)`。**
- **含义：** 矩阵中的每一个元素 $(i, j)$ **代表了第 $i$ 个词对第 $j$ 个词的 Attention Score (注意力得分)。**这反映了序列中**各个词之间的相互关系。**

------

### 3. 代码拆解 (Code Breakdown) 💻

- **`.transpose(-2, -1)`**: 表示交换倒数第二维（Sequence Length）和倒数第一维（Head Dimension）。
- **`torch.matmul(Q, ...)`**: 执行矩阵乘法。
- **`/ math.sqrt(self.d_k)`**: 这是 **Scaled Dot-Product** (缩放点积)，防止梯度消失或爆炸。

------

### 📖 进阶自测 (Review Question)

**Question:** In the context of the Transformer architecture, why do we use **Scaled Dot-Product** (dividing by $\sqrt{d_k}$) instead of just a regular dot product? What would happen to the **Softmax** function if the scores were too large?

(在 Transformer 架构中，为什么我们使用**缩放点积**（除以 $\sqrt{d_k}$）而不是普通的点积？如果分数过大，**Softmax** 函数会发生什么？)

> **Answer (请遮住查看):**
>
> 1. **Reason:** 当 $d_k$ 很大时，点积的**结果量级会变得很大。**
> 2. **Effect on Softmax:** **极大的数值会导致 Softmax 进入 Saturated Region (饱和区)，**此时梯度（Gradient）变得极小，导致模型难以训练（即 **Vanishing Gradient** 问题）。除以 $\sqrt{d_k}$ 可以**将数值推回梯度较大的区域。**

