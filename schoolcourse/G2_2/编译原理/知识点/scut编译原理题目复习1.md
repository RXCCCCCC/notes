# ![image-20260625161815144](./scut%E7%BC%96%E8%AF%91%E5%8E%9F%E7%90%86%E9%A2%98%E7%9B%AE%E5%A4%8D%E4%B9%A01/image-20260625161815144.png)

### 1. 题目解析 (Question Analysis)

**题目大意：** 以下哪一项**不是**属性计算 (attribute computation) 中“属性 (attributes)”的例子？ **正确答案：** (B) The size of a symbol table (符号表的大小)

这道题考察的是**语法制导翻译 (Syntax-Directed Translation, SDT)** 和**属性文法 (Attribute Grammars)** 中关于“属性”的严格定义及其应用范围。

### 2. 核心知识点讲解 (Core Concepts)

要准确判断，需要明确编译器前端在进行语义分析和中间代码生成时所依赖的核心机制：**语法制导翻译 (Syntax-Directed Translation)**。

**什么是属性 (Attributes)？** 在属性文法中，**属性 (Attributes) 是指与文法符号 (Grammar symbols) —— 即语法树的节点（包括终结符 Terminals 和非终结符 Non-terminals）—— 相关联的特定值或性质**。属性被**用来在语法树的节点之间传递信息，以完成语义检查或代码生成。**

属性分为两大类：

1. **综合属性 (Synthesized attributes):** 节点上的**该属性值由其子节点 (children nodes) 的属性值计算而来。信息在语法树中自底向上 (bottom-up) 传递。**
2. **继承属性 (Inherited attributes):** 节点上**的该属性值由其父节点 (parent node) 或兄弟节点 (sibling nodes) 的属性值计算而来。信息在语法树中自顶向下 (top-down) 或横向传递。**

**选项详细剖析 (Option Breakdown)：**

- **(A) The value of an expression (表达式的值):** 这是一个经典的**综合属性 (Synthesized attribute)**。例如，在计算表达式 `3 + 4` 时，父节点（加法表达式节点）的属性值（7）是由其两个子节点（操作数 3 和 4）的属性值相加得来的。它直接绑定在表达式相关的文法符号上（如 `E.val`）。
- **(C) The data type of a variable (变量的数据类型):** 这是语义分析（类型检查 Type checking）中最常用的属性。变量的**数据类型通常作为声明语句中标识符节点的属性（如 `T.type`），用于在后续表达式中进行类型验证**。它**可以是综合属性也可以是继承属性。**
- **(D) The object code of a procedure (过程的目标代码):** 在**通过语法制导翻译直接生成目标代码或中间代码时**，**代码片段可以作为字符串属性绑定在非终结符上（如 `S.code`）**，通过**拼接子节点的代码属性来生成父节点的代码。**
- **(B) The size of a symbol table (符号表的大小):** **符号表 (Symbol table)** 是**编译器维护的用于记录源程序中各种标识符（变量名、函数名等）及其相关信息（如类型、作用域、内存分配）的全局或局部数据结构 (Data structure)**。符号表的“大小”是这个数据结构的总体运行状态或容量特征，它**并不绑定在某一个特定的文法符号或语法树节点上参与语法规则的推导计算。因此，它不是属性文法中的“属性”。**

### 3. 考试风格练习题 (Exam-Style Practice)

基于上述知识点，以下是相关的考题变体及解答，帮助你巩固“属性”及“语法制导翻译”的概念。

**题目 1 (单选题 - 考查属性的分类方向)：** In an attribute grammar, if the value of an attribute for a node in the parse tree is determined solely by the attributes of its children, this attribute is called a(n) ( ). (A) Inherited attribute (B) Synthesized attribute (C) Lexical attribute (D) Global attribute

**【答案与解析】** **答案：** (B) **解析：** **综合属性 (Synthesized attribute)** 的定义就是其值仅依赖于子节点 (children) 的属性值。**继承属性 (Inherited attribute)** 依赖于父节点 (parent) 或兄弟节点 (siblings)。

**题目 2 (单选题 - 考查属性的实际应用场景)：** Which of the following is typically implemented using **inherited attributes** in syntax-directed translation? (A) Evaluating the final numerical result of a postfix expression. (B) Generating intermediate code for an arithmetic expression. (C) Passing the type information from a declaration down to the identifiers in a list. (D) Counting the total number of nodes in a syntax tree.

**【答案与解析】** **答案：** (C) **解析：** (C) 将声明（如 `int a, b;`）中的类型信息（`int`）沿着语法树向下 (down) 传递给标识符列表（`a` 和 `b`），这是典型的自顶向下传递，属于继承属性 (Inherited attributes) 的应用场景。(A) 和 (B) 都是自底向上计算结果，属于综合属性。

![image-20260625163158081](./scut%E7%BC%96%E8%AF%91%E5%8E%9F%E7%90%86%E9%A2%98%E7%9B%AE%E5%A4%8D%E4%B9%A01/image-20260625163158081.png)

![image-20260625163204596](./scut%E7%BC%96%E8%AF%91%E5%8E%9F%E7%90%86%E9%A2%98%E7%9B%AE%E5%A4%8D%E4%B9%A01/image-20260625163204596.png)

### 1. 题目解析 (Question Analysis)

**题目大意：** Which of the following is NOT correct about parsing stack? (以下关于分析栈的说法哪一项是不正确的？)

**正确答案：** (D) Both the top-down parsing stack and bottom-up parsing stack contain states. (自顶向下分析栈和自底向上分析栈都包含状态。)

这道题考察的是语法分析 (Syntax Analysis) 阶段中，两种主要分析方法——自顶向下分析 (Top-down parsing) 和自底向上分析 (Bottom-up parsing) 在核心数据结构即分析栈 (Parsing stack) 上的本质差异。

### 2. 核心知识点讲解 (Core Concepts)

在**语法分析 (Syntax Analysis)** 阶段，无论是哪种分析器，**都需要一个栈 (Stack) 来记录分析进度和历史。但两者的内部结构截然不同：**

**1. 自顶向下分析栈 (Top-down parsing stack)**

- **代表算法：** **LL(1) 预测分析 (Predictive parsing)。**
- **栈内元素：** 仅包含**文法符号 (Grammar symbols)**，这其中**包括了非终结符 (Nonterminals) 和终结符/词法单元 (Terminals/Tokens)**，以及**位于栈底的结束标记 '$' (End marker '$')。**
- **运行机制：** 它**不需要状态 (States)**。它的**核心操作是：如果栈顶是非终结符 (Nonterminal)**，则**根据向前看词法单元 (Lookahead token) 查表并将其展开 (Expand) 为产生式的右部**；如果**栈顶是终结符 (Terminal)，则与输入串进行匹配 (Match) 并弹出。**

**2. 自底向上分析栈 (Bottom-up parsing stack)**

- **代表算法：** **LR 分析家族 (LR parsing family, 包括 SLR, LALR, LR(1))。**
- **栈内元素：** 包含**状态 (States)** 和**文法符号 (Grammar symbols)**。在标准的 LR 分析器中，**栈内的元素是交替出现的，即 (状态, 符号, 状态, 符号...)**。符号**同样包含终结符 (Terminals) 和非终结符 (Nonterminals)**，**栈底也有结束标记 '$' (End marker '$') 以及初始状态 (Initial state)。**
- **运行机制：** 它强依赖于**状态 (States)**。它**使用栈顶的当前状态 (Current state) 和输入串的向前看词法单元 (Lookahead token) 查表**，决定是**进行移进 (Shift)（将终结符和新状态压栈）还是归约 (Reduce)（将句柄弹出，并将非终结符和新状态压栈）。**

**选项详细剖析 (Option Breakdown)：**

- **(A) Both contain tokens (两者都包含词法单元)：正确。** **自顶向下分析栈 (Top-down parsing stack) 在展开产生式时**，**会将右部的词法单元压栈等待匹配**；**自底向上分析栈 (Bottom-up parsing stack) 在执行移进 (Shift) 时会将词法单元压栈。**
- **(B) Both contain the end marker '$' (两者都包含结束标记 '$')：正确。** 两者都使用 '$' 来**标识输入串的结束以及栈底边界。**
- **(C) Both contain nonterminals (两者都包含非终结符)：正确。** 自顶向下分析栈 (Top-down parsing stack) **初始和展开时都含有待处理的非终结符**；**自底向上分析栈 (Bottom-up parsing stack) 在执行归约 (Reduce) 操作后，会将归约得到的非终结符压入栈中。**
- **(D) Both contain states (两者都包含状态)：错误。** 这是两者的核心区别。**只有自底向上分析栈 (Bottom-up parsing stack) 包含表示 DFA 进度的状态 (States)**，而**自顶向下分析栈 (Top-down parsing stack) 纯粹由文法符号构成，没有状态的概念。**

### 3. 考试风格练习题 (Exam-Style Practice)

根据考点深度，以下是两道检验栈操作和结构理解的题目：

**题目 1 (单选题 - 考查自底向上分析栈的细节操作)：**

In an LR parsing (LR分析) process, when a shift (移进) action is performed, what is exactly pushed onto the parsing stack (分析栈)?

(A) Only a state (仅压入一个状态)

(B) Only a terminal (仅压入一个终结符)

(C) A terminal and a state (压入一个终结符和一个状态)

(D) A nonterminal and a state (压入一个非终结符和一个状态)

**【答案与解析】**

**答案：** 中文作答：**(C) 压入一个终结符和一个状态。**

**解析：** 在 LR 分析 (LR parsing) 中**，移进 (Shift) 操作的定义是从输入流中读取当前的词法单元/终结符 (Token/Terminal)**，将**其压入栈中，随后根据动作表 (Action table) 查到的目标状态 (Target state)，将该新状态也一并压入栈顶。**

**题目 2 (判断题 - 考查自顶向下分析栈的初始化)：**

Before parsing begins in an LL(1) predictive parser (LL(1)预测分析器), the parsing stack (分析栈) is initialized by pushing the start symbol (起始符号) and then the end marker '$' (结束标记 '$').

(True / False)

**【答案与解析】**

**答案：** 中文作答：错误 (False)。

**解析：** 初始化的压栈顺序说反了。因为**栈是后进先出 (LIFO) 的数据结构，为了让起始符号 (Start symbol) 处于栈顶被优先处理，必须先将结束标记 '$' (End marker '$') 压入作为栈底，然后再将起始符号 (Start symbol) 压入**。**栈顶应该是起始符号，栈底是 '$'。**

![image-20260625163946306](./scut%E7%BC%96%E8%AF%91%E5%8E%9F%E7%90%86%E9%A2%98%E7%9B%AE%E5%A4%8D%E4%B9%A01/image-20260625163946306.png)

### 1. 题目解析 (Question Analysis)

**题目大意：** 语句 `x = a or b and not c` 的三地址码 (Three-address code) 是（ ）。 **正确答案：** (D)

这道题主要考察两个核心知识点：**三地址码的严格定义 (Definition of Three-Address Code)** 以及**布尔表达式的运算符优先级 (Operator Precedence in Boolean Expressions)**。

### 2. 核心知识点讲解 (Core Concepts)

**1. 三地址码 (Three-Address Code, TAC)** 三地址码是编译器中使用的一种重要的**中间表示 (Intermediate Representation, IR)**。它的**核心原则是：每条指令最多包含三个地址（通常是两个操作数和一个目标地址），且每条指令右侧最多只能有一个运算符 (At most one operator on the right side)。 常见的 TAC 指令形式包括：**

- **二元运算 (Binary operation):** `x = y op z`
- **一元运算 (Unary operation):** `x = op y`
- **赋值/复制 (Copy):** `x = y`

**2. 运算符优先级 (Operator Precedence)** 在**没有括号显式改变结合顺序的情况下，编译器在进行语法分析和中间代码生成时，会遵循标准的布尔逻辑运算符优先级，从高到低依次为：**

1. **`not` (非)** - 一元运算符，**优先级最高。**
2. **`and` (与)** - 二元运算符，**优先级次之。**
3. **`or` (或)** - 二元运算符，**优先级最低。**

**3. 代码生成过程 (Code Generation Process)** 编译器会**将复杂的表达式分解为一系列引入了临时变量 (Temporary variables, 如 t1, t2...) 的简单指令**。基于上述优先级，表达式 `a or b and not c` 的**求值顺序**为：

- **Step 1:** 计算 `not c`。生成一元运算指令：`t1 = not c`
- **Step 2:** 计算 `b and (not c)`，即 `b and t1`。生成二元运算指令：`t2 = b and t1`
- **Step 3:** 计算 `a or (b and not c)`，即 `a or t2`。生成二元运算指令：`t3 = a or t2`
- **Step 4:** 赋值给 `x`。生成**复制指令：`x = t3`**

### 3. 选项详细剖析 (Option Breakdown)

- **(A)** `t1 = a or b and not c`: 错误。这一行代码包含了三个运算符 (`or`, `and`, `not`)，严重**违反了三地址码“右侧最多只有一个运算符”的定义。**
- **(B)** 错误。它首先计算了 `t1 = a or b`，这**颠倒了 `and` 和 `or` 的优先级。**在标准语法中，`and` 的优先级高于 `or`，应该先计算 `b and not c`。
- **(C)** `t1 = b and not c`: 错误。这一行包含了两个运算符 (`and`, `not`)，同样违反了三地址码每条指令只能有一个运算符的限制规则。
- **(D)** 正确。严格遵循了 `not` -> `and` -> `or` 的优先级，并且每条临时指令都只包含一个运算符，完全符合三地址码的规范。

### 4. 考试风格练习题 (Exam-Style Practice)

**题目 1 (单选题 - 考查算术与一元混合优先级及TAC生成)：** What is the correct three-address code (三地址码) for the arithmetic assignment `x = a + b * -c` ? (Note: '-' is the unary minus operator here). (A) t1 = a + b t2 = t1 * -c x = t2 (B) t1 = -c t2 = b * t1 t3 = a + t2 x = t3 (C) t1 = b * -c t2 = a + t1 x = t2 (D) t1 = -c t2 = a + b t3 = t2 * t1 x = t3

**【答案与解析】** **答案：** (B) **解析：** 算术运算符的优先级为：一元负号 (Unary minus) > 乘除 (`*`, `/`) > 加减 (`+`, `-`)。

1. 先计算一元负号：`t1 = -c`
2. 再计算乘法：`t2 = b * t1`
3. 最后计算加法：`t3 = a + t2`
4. 赋值：`x = t3` (C) 选项中的 `t1 = b * -c` 包含了两个运算符（乘号和一元负号），不符合 TAC 定义。

**题目 2 (判断题 - 考查三地址码的合法性边界)：** In a standard compiler intermediate representation, the instruction `x = y[i]` is considered a valid single three-address code instruction. (在标准的编译器中间表示中，指令 `x = y[i]` 被视为一条合法的单句三地址码指令。) (True / False)

**【答案与解析】** **答案：** True (正确) **解析：** 数组访问 (Array access) 如 `x = y[i]` 或 `y[i] = x` 在三地址码中是特殊允许的形式，被视为基本的一步操作。它包含三个地址：数组基址 `y`，偏移量 `i`，以及目标 `x`。因此它是合法的。但如果写成 `x = y[i] + 1` 则不合法，必须拆分为 `t1 = y[i]` 和 `x = t1 + 1`。

![image-20260625164314475](./scut%E7%BC%96%E8%AF%91%E5%8E%9F%E7%90%86%E9%A2%98%E7%9B%AE%E5%A4%8D%E4%B9%A01/image-20260625164314475.png)

### 1. 题目解析 (Question Analysis)

**题目大意：** 假设 Alphabet (字母表) $\Sigma = \{a, b\}$。表示 ALL possible strings ending with 'ab' (所有以 'ab' 结尾的可能字符串) 的 Regular expression (正则表达式) 是 ( )。

**正确答案：** (B) $(a|b)^*ab$

这道题考察的是**词法分析 (Lexical Analysis)** 中的核心基础：如何**使用 Regular expressions (正则表达式) 来精确描述特定的 Pattern (模式) 或 Formal language (形式语言)。**

### 2. 核心知识点讲解 (Core Concepts)

要构建正确的 Regular expression (正则表达式)，我们需要掌握其基本的三个操作符 (Operators) 以及如何组合它们来表达“任意”的概念。

- **Alphabet (字母表 $\Sigma$):** 题目**给定了 $\Sigma = \{a, b\}$，这意味着我们所有的 Strings (字符串) 只能由字符 'a' 和 'b' 组成。**
- **Union / Alternation (并集 / 选择 `|`):** `a|b` 表示**“要么是 a，要么是 b”。它匹配单个字符。**
- **Kleene Star / Closure (星号 / 闭包 `\*`):** 表示**前面的元素可以重复 0 次或多次**。
- **Concatenation (连接):** 字符**直接相邻写在一起，表示按顺序匹配**。例如 `ab` 表示先匹配 a，紧接着匹配 b。

**构建目标表达式 (Constructing the Target Expression):**

题目要求的是“**所有**以 'ab' 结尾的可能字符串”。我们可以将其拆分为两部分：

1. **前半部分 (Prefix):** 它可以是**任何**由 'a' 和 'b' 组成的字符串，甚至可以是 Empty string (空串 $\epsilon$)。在正则表达式中，**“任何由 a 和 b 组成的字符串”的标准写法是 $(a|b)^\*$。**

2. **后半部分 (Suffix):** 它必须严格是 **`ab`**。

	将这两部分 Concatenate (连接) 起来，就得到了完美描述该语言的表达式：**$(a|b)^\*ab$**。

### 3. 选项详细剖析 (Option Breakdown)

通过寻找 Counter-examples (反例，即应该被匹配但被拒绝的字符串，或者不该被匹配却被接受的字符串)，我们可以轻松排除错误选项：

- **(A) $(ab)^\*$:**
	- **含义：** **只能是 `ab` 连续重复 0 次或多次（如 `ab`, `abab`, `ababab`）。**
	- **反例：** 字符串 `aab` 是以 'ab' 结尾的，符合题目要求，但该表达式**无法匹配**它，因为它限制了前缀必须也是严格的 `ab` 循环。
- **(B) $(a|b)^\*ab$:**
	- **含义：** 任意长度、任意 'a' 和 'b' 的组合，最后跟上一个 `ab`。**完全正确**。
- **(C) $(ab)^\*ab$:**
	- **含义：** 类似于 (A)，**前面是 `ab` 的重复，最后再加一个 `ab`（如 `ab`, `abab`, `ababab`）。**
	- **反例：** 同样无法匹配诸如 `bbab` 或 `aab` 这样合法的字符串。
- **(D) $(a^\*|b^\*)ab$:**
	- **含义：** **前半部分要么*全部*是 'a'，要么*全部*是 'b'。**
	- **反例：** 字符串 `abaab` 是以 'ab' 结尾的合法字符串，但该表达式**无法匹配**它，因为 `aba` 既不是纯 'a' 串，也不是纯 'b' 串。

### 4. 考试风格练习题 (Exam-Style Practice)

为了确保你完全掌握了 Regular expressions (正则表达式) 的构造逻辑，请尝试以下两道变体题：

**题目 1 (单选题 - 考查前缀与后缀约束)：**

Suppose $\Sigma = \{a, b\}$. Which of the following regular expressions (下列哪个正则表达式) represents ALL strings starting with 'a' and ending with 'b' (表示所有以 'a' 开头且以 'b' 结尾的字符串)?

(A) $a(a|b)^*b$

(B) $(a|b)^*ab(a|b)^*$

(C) $a(ab)^*b$

(D) $a^*b^*$

**【答案与解析】**

**答案：** (A)

**解析：** “以 'a' 开头”用 `a` 放在最前面；“以 'b' 结尾”用 `b` 放在最后面；“中间可以是任何字符串”用 $(a|b)^*$ 填补。连接起来即为 $a(a|b)^*b$。(B) 表示包含子串 'ab' 的所有字符串；(C) 中间只能是 'ab' 的交替；(D) 表示**任意数量的 'a' 后跟任意数量的 'b'。**

**题目 2 (单选题 - 考查包含特定子串)：**

Suppose $\Sigma = \{0, 1\}$. A regular expression for ALL possible strings containing the substring '00' (所有包含子串 '00' 的可能字符串) is ( ).

(A) $(0|1)^*00$

(B) $00(0|1)^*$

(C) $(0|1)^*00(0|1)^*$

(D) $(0^*1^*)*00$

**【答案与解析】**

**答案：** (C)

**解析：** “包含子串 '00'”**意味着 '00' 可以出现在字符串的任何位置（开头、中间、结尾）**。因此，它的前面可以是任意字符串 $(0|1)^*$，它的后面也可以是任意字符串 $(0|1)^*$。(A) 只匹配以 '00' 结尾的；(B) 只匹配以 '00' 开头的。

# 为我再详细地给出并讲解一下可能考试会考到的各种正则表达式的写法,包括如何进行匹配的,匹配的个数(必须标明可以为0还是至少为1)等等

在编译原理的词法分析 (Lexical Analysis) 考试中，正则表达式 (Regular Expressions, RE) 用于精确定义各种词法单元 (Tokens) 的模式。理解匹配次数 (Matching Quantities) 的边界是做对题目的关键。

这里我们使用 $r$ 代表一个基础正则表达式。

- **Kleene 闭包 (Kleene Closure) `$r^\*$`**
	- **匹配个数：** **可以为 0 次**，或 1 次，或多次连续重复。
	- **数学定义：** $r^* = \epsilon\ |\ r\ |\ rr\ |\ rrr\ |\ ...$ （其中 $\epsilon$ 表示空串 Empty String）。
	- **考试考点：** 只要**看到 `*`，就必须立刻意识到该模式可以完全不出现**。例如，正则表达式 `a*` 可以匹配空串 $\epsilon$。
- **正闭包 (Positive Closure) `$r^+$`**
	- **匹配个数：** **至少为 1 次**，或**多次连续重复**。**不能为 0 次**。
	- **数学定义：** $r^+ = r\ |\ rr\ |\ rrr\ |\ ... = rr^*$
	- **考试考点：** 用于**定义必须存在的序列**。例如，**一个合法的纯数字序列不能是空串，必须用 `[0-9]+` 而不是 `[0-9]*`。**
- **可选/问号量词 (Optional / Question Mark) `$r?$`**
	- **匹配个数：** **只能为 0 次 或 1 次**。
	- **数学定义：** $r? = \epsilon\ |\ r$
	- **考试考点：** 常用于表示可选的前缀或后缀，如数字的正负号 `(+|-)?`，或者浮点数的小数部分。

### 2. 考试高频模式模板 (High-Frequency Exam Templates)

考试中通常会要求你为特定的编程语言结构编写正则表达式。以下是标准答案级别的写法：

#### 2.1 标识符 (Identifiers)

**定义：** 以**字母或下划线开头**，后跟零个或多个字母、数字或下划线。

- **基础字符集 (Character sets)：**

	- `letter` $\rightarrow$ `[a-zA-Z_]`
	- `digit` $\rightarrow$ `[0-9]`

- **正则表达式 (Regular Expression)：**

	**`letter (letter | digit)\*`**

- **解析 (Explanation)：** 第一个 `letter` **确保了“至少为1个字符”且“不能以数字开头”。后面的 `*` 表示后续可以跟 0个或多个 字母或数字。**

#### 2.2 无符号整数 (Unsigned Integers)

**定义：** 由一个或多个数字组成的序列。

- **正则表达式 (Regular Expression)：**

	**`digit+`** (或者写为 `digit digit*`)

- **解析 (Explanation)：** 使用 `+` 确保匹配个数**至少为 1**。空串不能被识别为整数。

#### 2.3 浮点数 (Floating-Point Numbers)

**定义：** 包含整数部分、小数点、小数部分，以及可选的指数部分（如科学计数法 `1.23E-4`）。

- **正则表达式 (Regular Expression)：**

	**`digit+ \. digit+ (E (+|-)? digit+)?`**

- **解析 (Explanation)：**

	- `digit+ \. digit+`：匹配基础小数（如 `3.14`）。小数点前和后都**至少为1**个数字。
	- `(+|-)?`：指数的符号，匹配 **0次或1次**（可选）。
	- `(E ...)?`：整个**指数部分作为一个组，使用 `?` 表示它可以出现 0次或1次（可选）。**

#### 2.4 特定模式字符串限制 (Specific Pattern Constraints)

假设 $\Sigma = \{a, b\}$。

- **偶数长度的字符串 (Strings of even length)：**

	**`((a|b)(a|b))\*`**

	- **解析：** 将**两个字符 `(a|b)(a|b)` 作为一个整体原子，对其应用 `*`。可以匹配 0 次（长度0也是偶数）**，2次，4次等。

- **包含偶数个 'a' 的字符串 (Strings with an even number of 'a's)：**

	**`b\* (a b\* a b\*)\*`**

	- **解析：** `b*` (任意数量的 b，可以是0) 可以出现在任何地方。每一组 `(a b* a b*)` 严格包含两个 'a'。外层的 `*` 保证了 'a' 的总个数是 2 的倍数（0, 2, 4...）。

### 3. 考试风格练习题 (Exam-Style Practice)

**题目 1 (简答题 - 考查 `\*` 与 `+` 的等价转换)：**

In regular expressions, the positive closure (正闭包) operator `+` can be defined using the Kleene closure (Kleene闭包) operator `*`. Please write the equivalent expression for $(a|b)^+$ using only the `*` operator and concatenation (连接符). (请仅使用 `*` 操作符和连接符写出 $(a|b)^+$ 的等价表达式。)

**【答案与解析】**

**答案：** `(a|b)(a|b)*`

**解析：** 考查核心定义 $r^+ = rr^*$。$r^+$ 表示至少出现 1 次，因此先强行写出一个 $(a|b)$，后面再跟上 $(a|b)^*$（出现 0 次或多次），两者连接 (Concatenation) 的总效果就是至少出现 1 次。

**题目 2 (单选题 - 考查匹配边界和量词应用)：**

Which of the following regular expressions generates the language consisting of all strings over $\Sigma = \{0, 1\}$ that do **NOT** contain two consecutive 1s (不包含两个连续的 1)?

(A) $(0|10)^*$

(B) $(0|10)^* (1|\epsilon)$

(C) $(0^* 1 0^*)^*$

(D) $0^* (10)^*$

**【答案与解析】**

**答案：** (B)

**解析：**

这道题需要拆解分析：要避免出现 `11`，意味着每一个 `1` 后面都必须紧跟一个 `0`，或者该 `1` 就是整个字符串的结尾。

- (A) `(0|10)*`：确保了每次出现 `1` 都与 `0` 绑定为 `10`，完美避开了 `11`。但是，它**无法匹配以 `1` 结尾的合法字符串**（如 `0101` 可以匹配，但 `01011` 非法，而合法的 `0101` 后缀再加一个 `1` 变成 `0101` -> `01011`... 等等，稍微举例：合法的 `001` 无法被 (A) 匹配，因为 (A) 结尾必须是 0）。
- (B) `(0|10)* (1|\epsilon)`：在 (A) 的基础上，在整个表达式的最后增加了一个可选的 `1` (使用 $1|\epsilon$ 等价于 $1?$)。这样既保证了内部没有 `11`，也允许了字符串以单个 `1` 结尾。是正确答案。
- (C) `(0* 1 0*)*`：会产生两个组相连的情况，例如匹配出 `... 0* 1 0* 0* 1 0* ...`，如果中间的 `0*` 取 0 次，就会出现连续的 `11`。
- (D) 只能生成 0...0101010... 这种严格交替的模式，无法生成 `00100` 这样的合法串。

**题目 3 (构造题 - 考查词法分析器的实际应用)：**

Construct a regular expression for a Hexadecimal integer literal (十六进制整数常量) in C language. (构造C语言中十六进制整数常量的正则表达式。)

*Rules:* It must start with `0x` or `0X`, followed by at least one hexadecimal digit (`0-9`, `a-f`, `A-F`).

**【答案与解析】**

**答案：** `0 (x|X) (0-9 | a-f | A-F)+`

**解析：**

1. **前缀 (Prefix):** 必须精确匹配 `0x` 或 `0X`。可以直接写为 `0(x|X)`。注意这里的 `0` 是强制要求的 1 次，不可省略。
2. **主体 (Body):** 要求“至少一个 (at least one)”十六进制数字，因此必须使用正闭包 **`+`**，而绝不能使用 `*`（因为 `0x` 不是一个合法的十六进制数值）。字符集为 `(0-9 | a-f | A-F)`。

# 小括号和中括号在RE里面到底有什么区别

在正则表达式 (Regular Expressions, RE) 以及编译原理的词法分析 (Lexical Analysis) 中，小括号 `()` 和中括号 `[]` 具有完全不同的语义。混淆它们是考试中最常见的失分点。

以下是它们最准确的术语定义和核心区别：

### 1. 术语与定义 (Terminology and Definitions)

- **小括号 `()`：分组 / 子表达式 (Grouping / Subexpression)**
	- **作用：** 将括号内的多个字符或子表达式**绑定为一个不可分割的原子整体 (Atomic unit)。**
	- **匹配单位：** 匹配括号内定义的**完整字符序列 (Exact sequence)**。
- **中括号 `[]`：字符集 / 字符类 (Character Class / Character Set)**
	- **作用：** 定义**一个备选字符的集合。**
	- **匹配单位：** **无论括号里写了多少个字符，它在输入串中只匹配且仅匹配一个字符 (Exactly one character position)，该字符必须是集合中的任意一个。**

### 2. 核心区别对比 (Core Differences)

#### 区别一：匹配长度与逻辑 (Matching Length and Logic)

- **`(abc)`**：严格匹配完整的字符串 `"abc"`。长度固定为 3。
- **`[abc]`**：**等价于 `(a|b|c)`。它匹配 `"a"`，或者 `"b"`，或者 `"c"`。长度固定为 1**。

#### 区别二：与量词结合时的作用域 (Scope with Quantifiers)

当后面紧跟量词（如 `*`, `+`, `?`）时，它们的作用域完全不同：

- **`(ab)\*`**：星号作用于**整个整体 `"ab"`。**
	- **合法匹配：** $\epsilon$ (空串), `"ab"`, `"abab"`, `"ababab"`
	- **非法匹配：** `"a"`, `"ba"`, `"aab"`
- **`[ab]\*`**：星号作**用于字符类，即“从集合 {a, b} 中取出一个字符”这个动作重复 0 次或多次。**
	- **等价写法：** `(a|b)*`
	- **合法匹配：** $\epsilon$, `"a"`, `"b"`, `"aa"`, `"ab"`, `"ba"`, `"bb"`, `"aba"` 等任意由 a 和 b 组成的字符串。

#### 区别三：内部元字符的转义与语义 (Semantics of Internal Metacharacters)

- **在 `()` 内部**：特殊符号**保留其作为正则表达式操作符的语义。例如，`|` 表示逻辑或 (Alternation)。`(a|b)` 匹配 `"a"` 或 `"b"`。**
- **在 `[]` 内部**：大多数**特殊的元字符会失去其原有语义，退化为普通字符 (Lose special meaning)**，除了**连字符 `-` (表示范围) 和脱字符 `^` (在开头表示取反 Negation)。**
	- **`[a|b]`**：**不再是逻辑或**。它**匹配的是字符 `"a"`，或者字符 `"|"`，或者字符 `"b"`。**
	- **`[a-z]`**：匹配从 a **到** z 的任意**一个**小写字母。
	- **`[^0-9]`**：匹配任意**一个非**数字字符。

### 3. 考试等价转换速记 (Equivalence Mapping for Exams)

在考试的选择题或化简题中，经常需要你进行**等价替换：**

1. `[abc]` $\equiv$ `(a|b|c)`
2. `[0-9]` $\equiv$ `(0|1|2|3|4|5|6|7|8|9)`
3. `(a|b)*` $\equiv$ `[ab]*`
4. `(a|b)+` $\equiv$ `[ab]+`

### 4. 考试风格练习题 (Exam-Style Practice)

**题目 1 (单选题 - 考查匹配逻辑的区别)：**

Consider the two regular expressions $R_1 = (01)^+$ and $R_2 = [01]^+$. Which of the following strings is matched by $R_2$ but **NOT** matched by $R_1$? (考虑正则表达式 $R_1 = (01)^+$ 和 $R_2 = [01]^+$。以下哪个字符串能被 $R_2$ 匹配但**不能**被 $R_1$ 匹配？)

(A) 01

(B) 0101

(C) 010101

(D) 1010

**【答案与解析】**

**答案：** (D)

**解析：** * $R_1 = (01)^+$ 是分组的正闭包，必须是完整的 `"01"` 序列重复 1 次或多次。即：`01`, `0101`, `010101` 等。(A), (B), (C) 都符合 $R_1$。

- $R_2 = [01]^+$ 是字符类的正闭包，等价于 `(0|1)+`，表示由 0 和 1 组成的任意非空字符串（至少一个字符），如 `0`, `1`, `00`, `11`, `1010` 等。
- 选项 (D) `"1010"` 完全由 0 和 1 组成，符合 $R_2$，但它不是以 `"01"` 作为原子单位重复的，因此不符合 $R_1$。

**题目 2 (判断题 - 考查中括号内的元字符退化)：**

In lexical analysis, the regular expression `[+|-|*|/]` can correctly match the division operator `/`. (在词法分析中，正则表达式 `[+|-|*|/]` 可以正确匹配除号 `/`。)

(True / False)

**【答案与解析】**

**答案：** True (正确)

**解析：** 虽然这是一种糟糕且冗余的写法，但在中括号 `[]` 内部，竖线 `|` 失去了其“逻辑或”的特殊含义，退化成了普通的竖线字符。因此，这个字符集实际上包含了 5 个字符：`+`, `|`, `-`, `*`, `/`。输入串中的除号 `/` 确实包含在这个集合中，所以匹配成功。（注：标准的写法应该是 `[+\-*/]`，注意减号如果在中间需要被转义 `\-`，或者放在最前面/最后面，因为 `-` 在中括号内有表示范围的特殊语义）。

![image-20260625165212397](./scut%E7%BC%96%E8%AF%91%E5%8E%9F%E7%90%86%E9%A2%98%E7%9B%AE%E5%A4%8D%E4%B9%A01/image-20260625165212397.png)

### 1. 题目解析 (Question Analysis)

**题目大意：** 以下哪个正则表达式 (Regular expression) 与给定的非确定性有限自动机 (NFA, Nondeterministic Finite Automaton) 等价？

**正确答案：** (D) `ab*a`

这道题考察的是**词法分析 (Lexical Analysis)** 中的核心能力：**自动机模型与正则表达式的等价性分析 (Equivalence between Automata and Regular Expressions)**。特别地，它考察了如何通过追踪状态转移来还原带有 $\epsilon$-转移 (Epsilon transitions) 的 NFA 的语言特性。

### 2. 核心知识点讲解 (Core Concepts)

要解开这种图转表达式的题目，我们需要将 **NFA 拆解为三个逻辑部分 (Logical parts)：前缀 (Prefix)、中间循环 (Middle loop / Closure) 和后缀 (Suffix)。**

**1. 识别必经路径 (Identifying Mandatory Paths)**

- **起始部分 (Start):** 从**初始状态 (Start state) `0` 出发，唯一的路径是通过输入字符 `a` 到达状态 `1`。这意味着所有被接受的字符串必须以 `a` 开头。**
- **终止部分 (End):** 唯一的**接受状态 (Accepting state) 是 `5`。到达 `5` 的唯一路径是从状态 `4` 输入字符 `a`。**这意味着**所有被接受的字符串必须以 `a` 结尾**。
- **推论：** 我们的**目标正则表达式必定形如 `a(中间部分)a`。**此时，选项 (A) `a*ba` 可以直接被排除，因为它不以单个 `a` 强制开头，且以 `a` 结尾的保证也是错误的。

**2. 破解 $\epsilon$-闭包结构 (Cracking the $\epsilon$-Closure Structure)**

状态 1, 2, 3, 4 构成了编译器**使用 Thompson 构造法 (Thompson's construction) 生成 Kleene 闭包 (`*`) 的标准 NFA 结构。**我们来详细追踪它的所有可能路径：

- **路径 1 (零次重复，Bypass):** 从 `1` 直接走上方的 $\epsilon$-转移到达 `4`。这不需要消耗任何输入字符。**这意味着中间部分可以为空 ($\epsilon$)。这对应了量词 `*` 中的 0 次。**
- **路径 2 (一次匹配):** 从 `1` $\xrightarrow{\epsilon}$ `2` $\xrightarrow{b}$ `3` $\xrightarrow{\epsilon}$ `4`。这消耗了一个字符 `b`。
- **路径 3 (多次匹配):** 从 `1` 走到 `3` 后，不走向 `4`，而是走下方的回路 `3` $\xrightarrow{\epsilon}$ `2`，再次经过 `2` $\xrightarrow{b}$ `3` 消耗一个 `b`。这个**回环可以无限次重复。**
- **结论：** 状态 1 到 4 完美地表示了字符 `b` 可以出现 0 次、1 次或多次，这在正则表达式中精确定义为 **`b\*`**。

综合以上三点，该 NFA 接受的语言是：以 `a` 开头，中间跟着任意数量的 `b`，并以 `a` 结尾。即表达式 **`ab\*a`**。

### 3. 选项详细剖析 (Option Breakdown)

通过寻找**最短接受串 (Shortest accepted string)** 和**反例测试 (Counter-example testing)** 可以快速排除错误选项。

首先，观察 NFA，从 0 到 5 的最短路径是 0 $\xrightarrow{a}$ 1 $\xrightarrow{\epsilon}$ 4 $\xrightarrow{a}$ 5。因此，该 NFA 能接受的最短字符串是 **`aa`**。

- **(A) `a\*ba`**: 这个表达式生成的最短字符串是 `ba` (当 `a*` 取 0 次时)。NFA 显然无法接受 `ba`（因为它强制以 `a` 开头）。
- **(B) `(ab)\*a`**: 这个表达式生成的最短字符串是 `a` (当 `(ab)*` 取 0 次时)。且它可以生成 `ababa`，但无法生成连续的 `b`。而 NFA 中 `3` $\xrightarrow{\epsilon}$ `2` 的回环明确允许连续的 `b` (例如 `abba`)。
- **(C) `a(ba)\*`**: 这个表达式生成的最短字符串也是 `a` (当 `(ba)*` 取 0 次时)。而 NFA 的最短接受串是 `aa`，不匹配。
- **(D) `ab\*a`**: 最短串 `aa` (b取0次)，可生成连续b如 `abba`。与 NFA 完美吻合。

### 4. 考试风格练习题 (Exam-Style Practice)

在考试中，除了正向推导（NFA 找 RE），反向理解自动机的构造规则（特别是 Thompson 构造法引入的 $\epsilon$ 边）也是高频考点。

**题目 1 (单选题 - 考查 NFA 的路径追踪与状态化简)：**

Consider an NFA with states $\{q_0, q_1, q_2\}$ where $q_0$ is the start state and $q_2$ is the accepting state. The transitions are:

$q_0 \xrightarrow{a} q_0$

$q_0 \xrightarrow{\epsilon} q_1$

$q_1 \xrightarrow{b} q_2$

$q_2 \xrightarrow{b} q_2$

Which regular expression represents the language accepted by this NFA? (哪个正则表达式表示此 NFA 接受的语言？)

(A) $a^* b^*$

(B) $a^+ b^+$

(C) $a^* b^+$

(D) $a b^*$

**【答案与解析】**

**答案：** (C)

**解析：** 1. 在起始状态 $q_0$ 有针对 `a` 的自环，由于有 $\epsilon$ 边跳出，所以 `a` 可以重复 0 次或多次，即 $a^*$。

2. 必须经过 $\epsilon$ 边到 $q_1$，然后必须走一次 $q_1 \xrightarrow{b} q_2$ 消耗一个 `b` 才能到达接受状态 $q_2$。所以至少需要一个 `b`。
3. 在 $q_2$ 处有针对 `b` 的自环，因此后续可以跟任意多个 `b`。

综合：至少一个 `b`，即 $b^+$（或写成 $bb^*$）。因此完整表达式为 $a^* b^+$。

**题目 2 (单选题 - 考查 Thompson 构造法的标准形态)：**

When converting the regular expression `a|b` (alternation) to an NFA using standard Thompson's construction (使用标准 Thompson 构造法将正则 `a|b` 转化为 NFA), how many new $\epsilon$-transitions (新的 $\epsilon$-转移) are explicitly introduced to connect the sub-NFAs?

(A) 1

(B) 2

(C) 4

(D) 0

**【答案与解析】**

**答案：** (C)

**解析：** 这是一个极其经典的编译原理理论题。根据 Thompson 构造法处理并集 `|` 的规则：为了组合用于匹配 `a` 的 NFA 和匹配 `b` 的 NFA，我们会新建一个全局的开始状态和一个全局的接受状态。

- 从新的开始状态，会有 **2 条** $\epsilon$-边分别连向 `a` NFA 和 `b` NFA 的旧开始状态。

- 从 `a` NFA 和 `b` NFA 的旧接受状态，会分别有 **2 条** $\epsilon$-边连向新的全局接受状态。

	因此总共引入了 2 + 2 = 4 条 $\epsilon$-转移。

![image-20260625180121073](./scut%E7%BC%96%E8%AF%91%E5%8E%9F%E7%90%86%E9%A2%98%E7%9B%AE%E5%A4%8D%E4%B9%A01/image-20260625180121073.png)

### 1. 题目解析 (Question Analysis)

**题目大意：** Which of the following is LL(1) grammar? (以下哪一项是 LL(1) 文法？) **正确答案：** (C) $G[A]: A \rightarrow cAa\ |\ \epsilon$

这道题是编译原理语法分析 (Syntax Analysis) 阶段的绝对重点，直接考察判断一个文法是否属于 **LL(1)** 的**核心数学条件：FIRST 集 (FIRST sets) 和 FOLLOW 集 (FOLLOW sets) 的交集运算。**

### 2. 核心知识点讲解 (Core Concepts)

要判断一个文法是否是 LL(1) 文法，必须检查它的所有产生式是否满足**无冲突预测 (Conflict-free prediction)** 的条件。

假设非终结符 $A$ 有两个候选产生式：$A \rightarrow \alpha\ |\ \beta$ **LL(1) 文法必须严格满足以下两个条件**：

1. **条件 1：首符号集不相交 (Disjoint FIRST sets)**
	- **规则：** $First(\alpha) \cap First(\beta) = \emptyset$
	- **解释：** 预测分析器在看到下一个输入符号 (Lookahead token) 时，**必须能唯一决定选择哪个产生式**。如果 **$\alpha$ 和 $\beta$ 能推导出的首个终结符有重叠，分析器就会产生冲突 (Conflict)。**
2. **条件 2：空串推导的后跟符不相交 (Disjoint FIRST and FOLLOW sets for $\epsilon$-productions)**
	- **规则：** **如果 $\beta \Rightarrow^* \epsilon$ (即 $\beta$ 可以推导出空串 $\epsilon$)**，那么必须满足 $First(\alpha) \cap Follow(A) = \emptyset$。
	- **解释：** 如果**分析器面临选择，且当前输入符号属于 $Follow(A)$ (即紧跟在 $A$ 后面的合法符号)**，它**应该安全地选择推导为空串 $\epsilon$ 的那个产生式 ($\beta$)**。但**如果 $\alpha$ 的开头也能推导出这个输入符号，分析器又会陷入迷茫**：到底是**选择 $\alpha$ 吃掉这个符号，还是选择 $\epsilon$ 让 $A$ 后面的符号去吃掉它？**这就产生了冲突。

### 3. 选项详细剖析 (Option Breakdown)

我们用上述两条规则来逐一验证：

- **(A) $G[A]: A \rightarrow aAa\ |\ \epsilon$**
	- 设 $\alpha = aAa$, $\beta = \epsilon$。
	- $First(aAa) = \{a\}$。
	- 因为**包含了 $\epsilon$-产生式 ($\beta = \epsilon$)，我们需要计算 $Follow(A)$。**
		- **$A$ 是起始符号 (Start symbol)，所以结束标记 $\$\in Follow(A)$。**
		- 在产生式 $A \rightarrow aAa$ 中，**非终结符 $A$ 的右侧紧跟的是终结符 $a$。所以 $a \in Follow(A)$。**
		- 因此，**$Follow(A) = \{a, \$\}$**。
	- **判定：** $First(aAa) \cap Follow(A) = \{a\} \cap \{a, \$\} = \{a\} \neq \emptyset$。**交集不为空，违反了条件 2。不是 LL(1) 文法。**
- **(B) $G[A]: A \rightarrow bAa\ |\ b$**
	- 设 $\alpha = bAa$, $\beta = b$。
	- $First(bAa) = \{b\}$。
	- $First(b) = \{b\}$。
	- **判定：** $First(bAa) \cap First(b) = \{b\} \cap \{b\} = \{b\} \neq \emptyset$。交集不为空，违反了条件 1。这**实际上需要进行提取左公因子 (Left factoring) 才能转化为 LL(1)**。**不是 LL(1) 文法。**
- **(C) $G[A]: A \rightarrow cAa\ |\ \epsilon$**
	- 设 $\alpha = cAa$, $\beta = \epsilon$。
	- $First(cAa) = \{c\}$。
	- 计算 $Follow(A)$：
		- $A$ 是起始符号，$\$\in Follow(A)$。
		- 在产生式 $A \rightarrow cAa$ 中，$A$ 右侧紧跟的是终结符 $a$。所以 $a \in Follow(A)$。
		- 因此，**$Follow(A) = \{a, \$\}$**。
	- **判定：** $First(cAa) \cap Follow(A) = \{c\} \cap \{a, \$\} = \emptyset$。交集为空！完全满足 LL(1) 的条件 2。**是 LL(1) 文法。**

### 4. 考试风格练习题 (Exam-Style Practice)

**题目 1 (单选题 - 考查 LL(1) 的致命缺陷)：** Which of the following grammar properties makes it IMPOSSIBLE for a grammar to be an LL(1) grammar directly? (以下哪种文法特性使得一个文法绝不可能是 LL(1) 文法？) (A) Having right recursion (存在右递归) (B) Having left recursion (存在左递归) (C) Containing $\epsilon$-productions (包含 $\epsilon$-产生式) (D) Having multiple non-terminals (具有多个非终结符)

**【答案与解析】****答案：** (B) **解析：** 左递归 (Left recursion) 形如 $A \rightarrow A\alpha\ |\ \beta$。由于第一个产生式的开头依然是 $A$，它会导致自顶向下分析器 (Top-down parser) 陷入无限循环。从数学条件看，$First(A\alpha)$ 必然包含 $First(\beta)$，导致交集绝对不为空，永远违反 LL(1) 条件 1。必须先消除左递归 (Eliminate left recursion) 才能进行 LL(1) 分析。(A) 右递归正是 LL(1) 所偏好的形式。(C) $\epsilon$-产生式只要满足 FOLLOW 集条件就是合法的。

**题目 2 (单选题 - 考查 FOLLOW 集的计算规则)：** Given the grammar (给定文法):$S \rightarrow ABc$$A \rightarrow a\ |\ \epsilon$$B \rightarrow b$ What is the $Follow(A)$ set? ($Follow(A)$ 集是什么？) (A) $\{b\}$ (B) $\{c\}$ (C) $\{a, b\}$ (D) $\{b, c\}$

**【答案与解析】****答案：** (A) **解析：** 找 $Follow(A)$ 就是看在所有产生式右部，谁紧紧跟在 $A$ 的后面。 在产生式 $S \rightarrow ABc$ 中，$A$ 后面紧跟的是非终结符 $B$。 因此，把 $B$ 的开头集合，即 $First(B)$ 加入到 $Follow(A)$ 中。$First(B) = \{b\}$。并且 $B$ 不能推导出空串 $\epsilon$，所以停止寻找。 最终 $Follow(A) = \{b\}$。

# 怎么确定要求哪些$First$集合和$Follow$集合的啊

确定需要求哪些 $First$ 集和 $Follow$ 集，**完全取决于题目的最终要求**。通常分为两种题型：**判断 LL(1) 文法** 和 **构造预测分析表 (Predictive Parsing Table)**。

以下是严格的判断依据和执行流程：

### 1. 核心判断准则 (Core Criteria)

#### 必须求的 $First$ 集合 (Mandatory FIRST sets)

无论是什么题型，你都**必须求：**

1. **所有非终结符 (All Non-terminals) 的 $First$ 集：** 这是基础。因为**产生式右部往往包含非终结符，你不求出它们，就无法进行后续计算。**
2. **所有存在分支候选的产生式右部 (All candidate Right-Hand Sides) 的 $First$ 集：** 如**果非终结符 $A$ 有多个推导 $A \rightarrow \alpha_1\ |\ \alpha_2\ |\ ...$，你必须求出 $First(\alpha_1)$, $First(\alpha_2)$ 等，用于检查它们是否相交（LL(1) 的条件 1）。**

#### 选择性求的 $Follow$ 集合 (Selective FOLLOW sets)

这是节省时间的关键。你**是否需要求 $Follow(A)$**，**取决于非终结符 $A$ 能否推导出空串 $\epsilon$ (Nullable non-terminals)。**

1. **对于只判断 LL(1) 属性的题目：**
	- **绝对需要求：** 只要**文法中存在 $\epsilon$-产生式 (如 $A \rightarrow \epsilon$)**，或者**某个非终结符能间接推导出空串** (如 $A \rightarrow BC$，且 $B \Rightarrow^* \epsilon, C \Rightarrow^* \epsilon$)，你**就必须且仅需要求出该非终结符 $A$ 的 $Follow$** 集。用于检查 $First(\alpha)$ 和 $Follow(A)$ 是否相交（LL(1) 的条件 2）。
	- **不需要求：** 对于那些**绝对不可能推导出 $\epsilon$ 的非终结符，它们的 $Follow$ 集在验证 LL(1) 条件时是不需要的。**
2. **对于要求画出完整预测分析表 (Parsing Table) 的题目：**
	- **全都要求：** 为了稳妥和填表规则的完整性（特别是填入 `sync` 错误恢复标志或者将 $\epsilon$ 填入对应列时），标准流程要求你计算出**所有非终结符**的 $Follow$ 集。

### 2. 考试实战做题流程 (Exam Workflow)

在考场上，面对一道给定的文法题，请严格按照以下顺序执行：

- **Step 1: 扫描空串 (Scan for $\epsilon$)**

	一眼扫过所有产生式，**圈出所有直接含有 $\epsilon$ 的非终结符。再观察是否有间接能推导出 $\epsilon$ 的非终结符。将这些标记为 Nullable (可空的)。**

- **Step 2: 求解 $First$ 集 (Compute FIRST sets)**

	**从下往上（从不依赖其他非终结符的产生式开始），求出所有非终结符的 $First$ 集。**

- **Step 3: 按需求解 $Follow$ 集 (Compute FOLLOW sets on demand)**

	如果题目只问**“是否是 LL(1)”**，你**只去求 Step 1 中标记为 Nullable 的非终结符的 $Follow$ 集**。从**起始符号 (Start symbol) 开始，利用求好的 $First$ 集自顶向下寻找后跟符。如果题目要求画表，则全部求出。**

- **Step 4: 冲突检测 / 填表 (Conflict Check / Table Construction)**

	利用求出的集合，进行交集验证或填写二维表。

### 3. 考试风格练习题 (Exam-Style Practice)

为了巩固这个“按需计算”的思路，请看以下题目：

**题目 1 (单选题 - 考查按需计算 FOLLOW 集的敏锐度)：**

Given the following grammar (给定以下文法):

$S \rightarrow a A b$

$A \rightarrow c\ |\ \epsilon$

$B \rightarrow d B\ |\ d$

To strictly determine whether this grammar is an LL(1) grammar using the LL(1) definition conditions, for which non-terminal(s) MUST you compute the FOLLOW set? (为了严格**利用 LL(1) 定义条件判断该文法是否为 LL(1) 文法**，**你必须求哪个（些）非终结符的 FOLLOW 集？)**

(A) Only $S$ (仅 $S$)

(B) Only $A$ (仅 $A$)

(C) $S$ and $A$ ($S$ 和 $A$)

(D) $S$, $A$, and $B$ ($S$、$A$ 和 $B$)

**【答案与解析】**

**答案：** (B)

**解析：** 1. 扫描文法发现，**只有非终结符 $A$ 包含 $\epsilon$-产生式 ($A \rightarrow \epsilon$)。$S$ 和 $B$ 都不可能推导出空串。**

2. 根据 LL(1) 条件判断法则，只有**当存在 $A \rightarrow \epsilon$ 时，才需要验证 $First(c) \cap Follow(A) = \emptyset$。**
3. 对于 $B$，它的两个候选右部是 $dB$ 和 $d$，$First(dB)=\{d\}$，$First(d)=\{d\}$，**交集不为空，此时已经可以直接判定它不是 LL(1)（发生了左公因子冲突）**，全程根本不需要用到 $Follow(B)$。因此为了验证 LL(1) 条件，仅需计算 $Follow(A)$。

**题目 2 (简答题 - 考查完整分析流程)：**

Given the grammar $G[E]$:

$E \rightarrow T E'$

$E' \rightarrow + T E'\ |\ \epsilon$

$T \rightarrow F T'$

$T' \rightarrow * F T'\ |\ \epsilon$

$F \rightarrow ( E )\ |\ id$

Please state the specific FIRST and FOLLOW sets you need to compute to construct the full LL(1) Parsing Table. (请说明为了构造完整的 LL(1) 预测分析表，你需要计算哪些具体的 FIRST 和 FOLLOW 集。)

**【答案与解析】**

**答案：**

为了构造完整的预测分析表 (Parsing Table)：

1. **需要计算的 FIRST 集：** 必须**计算所有非终结符 $E, E', T, T', F$ 的 $First$ 集。**同时需要**计算所有产生式右部的 $First$ 集（如 $First(+TE')$, $First(*FT')$ 等）以确定表项的填入列。**
2. **需要计算的 FOLLOW 集：** **必须计算所有非终结符 $E, E', T, T', F$ 的 $Follow$ 集。虽然只有 $E'$ 和 $T'$ 能推导为空串 $\epsilon$，但填表规则规定，当产生式右部推导为空时，需要将对应的产生式填入 $Follow$ 集合指示的终结符列中；**此外，构建健壮的错误恢复机制 (Error recovery) 也需要依赖所有非终结符的 $Follow$ 集来填入 `sync` 标记。

![image-20260625181220216](./scut%E7%BC%96%E8%AF%91%E5%8E%9F%E7%90%86%E9%A2%98%E7%9B%AE%E5%A4%8D%E4%B9%A01/image-20260625181220216.png)

![image-20260625181225956](./scut%E7%BC%96%E8%AF%91%E5%8E%9F%E7%90%86%E9%A2%98%E7%9B%AE%E5%A4%8D%E4%B9%A01/image-20260625181225956.png)

### 1. 题目解析 (Question Analysis)

**题目大意：** 给定一个上下文无关文法 (CFG) G[S]:S→S+S∣S∗S∣a。在输入字符串 a∗a+a 的**最左推导 (leftmost derivation)** 过程中，以下哪一项可能是**一个句型 (sentential form)？ 正确答案： (A) S∗S+S**

这道题是语法分析中极其核心的考点，专门考察你对**推导顺序 (Derivation order)** 严格性的理解，尤其是最左推导的特征限制。

### 2. 核心知识点讲解 (Core Concepts)

要秒杀这类题目，你需要深刻理解推导的机制以及一个“黄金法则”。

**1. 推导 (Derivation) 与句型 (Sentential Form)**

- **推导 (Derivation)：** 是**将文法中的非终结符 (Non-terminal，如本题的 S) 按照产生式规则不断替换为右部的过程，直到全部变成终结符 (Terminal，如本题的 a,+,∗ )。**
- **句型 (Sentential Form)：** 在**从起始符号推导到最终字符串的任何一个中间步骤产生的字符串，都称为句型。**它可能包含非终结符，也**可能只包含终结符。**

**2. 最左推导 (Leftmost Derivation)**

- **定义：** 在推导的每一步中，**总是强制选择最左边的非终结符 (the leftmost non-terminal)** 进行替换。
- **黄金法则 (The Golden Rule for Leftmost Derivation)：** 在**一个最左推导的句型中，位于最左非终结符左侧的所有符号，必须全部是终结符 (Terminals)。**如果你看到**某个非终结符的右边已经被替换成了终结符，而它自己还没被替换，那这就绝对不是最左推导！**

### 3. 选项详细剖析 (Option Breakdown)

给定目标字符串：a∗a+a。由于**文法存在二义性 (Ambiguity)，该字符串有两棵不同的语法树，对应两种最左推导路径。**

我们利用“黄金法则”直接排除错误选项：

- **(C) S∗a+S**：
	- **诊断：** 句型中最左边的非终结符是第一个 S。但是，在它右边的第二个 S 竟然已经变成了终结符 a。
	- **结论：** 这意味着推导时跳过了最左边的 S，先去替换了右边的 S。严重违反最左推导原则，排除。
- **(D) S∗S+a**：
	- **诊断：** 同样，最左边还有未展开的 S，但最右边的 S 已经被替换成了 a。
	- **结论：** 违反最左推导原则，排除。
- **(B) a∗S+a**：
	- **诊断：** 乍一看，最左边的 S 已经被替换成 a 了，似乎合法？仔细看，目前的**最左非终结符**是中间的这个 S。但是在它的右边，竟然出现了一个已经被推导出来的终结符 a（即 +a 部分）。这意味着上一步是 a∗S+S⇒a∗S+a。这分明是去替换了最右边的 S！
	- **结论：** 这是一个**混合推导或最右推导的句型**，绝不是最左推导，排除。
- **(A) S∗S+S**：
	- **诊断：** 没有任何非终结符在它左侧的非终结符之前被展开。它完全符合规范。
	- **推导路径还原 (Tracing the path)：** 我们来推导一下如何得到它以及最终的 a∗a+a：
		1. S⇒S+S  (运用规则 S→S+S)
		2. ⇒S∗S+S (将最左边的 S 运用规则 S→S∗S，**这里正是选项 A**)
		3. ⇒a∗S+S (将最左边的 S 替换为 a)
		4. ⇒a∗a+S (将最左边的 S 替换为 a)
		5. ⇒a∗a+a (将最左边的 S 替换为 a，推导完成)

### 4. 考试风格练习题 (Exam-Style Practice)

为了确保你能在考试中灵活应对，这里准备了两道变体题：

**题目 1 (单选题 - 考查最右推导 / Rightmost Derivation)：** Given the same CFG G[S]:S→S+S∣S∗S∣a, which of the following may be a sentential form during the **rightmost** derivations for the input string a+a∗a? (给定相同的文法，以下哪一项可能是在输入字符串 a+a∗a 的**最右推导**过程中的句型？) (A) S+S∗S (B) S+a∗S (C) a+S∗S (D) a+a∗S

**【答案与解析】** **答案：** (A) **解析：** 最右推导 (Rightmost derivation) 也被称为规范推导 (Canonical derivation)，规则与最左推导完全相反：**每一步必须替换最右边的非终结符**。因此，最右非终结符的**右侧**必须全是终结符。

- (B) 中间的 S 被替换成了 a，但最右边还有 S 没替换。违法。
- (C) 最左边的 S 被替换成了 a，但右边还有 S 没替换。违法。
- (D) 左边的两个 S 都变成了 a，最右边的 S 居然还在。违法。
- (A) S+S∗S 是合法的初始展开步骤（例如从 S⇒S+S 出发，替换右边的 S 得到 S+S∗S）。

**题目 2 (判断题 - 考查文法的二义性 / Ambiguity)：** If a string generated by a CFG has more than one valid leftmost derivation, the grammar is strictly defined as ambiguous. (如果**由一个 CFG 生成的某个字符串拥有超过一种有效的最左推导，那么该文法被严格定义为是二义性的。**) (True / False)

**【答案与解析】** **答案：** True (正确) **解析：** 这是**二义性文法 (Ambiguous grammar) 的经典数学定义**。**一棵语法树 (Parse tree) 和一个最左推导 (Leftmost derivation) 是一一对应的**。如果一个句子有**两个不同的最左推导，就意味着它有两棵不同的语法树，这正是“二义性”的本质。**本题中的文法 S→S+S∣S∗S∣a 就是一个经典的二义性文法，因为它**没有规定加法和乘法的优先级 (Precedence) 与结合性 (Associativity)。**

![image-20260625181906644](./scut%E7%BC%96%E8%AF%91%E5%8E%9F%E7%90%86%E9%A2%98%E7%9B%AE%E5%A4%8D%E4%B9%A01/image-20260625181906644.png)

### 1. 题目解析 (Question Analysis)

**题目大意：** 考虑上下文无关文法 (CFG) G[S]:S→SaS∣SbS∣ϵ。以下说法正确的是（ ）。 **正确答案：** (A) G[S] is ambiguous. (G[S] 是二义性的。)

这道题考察的是**文法二义性 (Ambiguity)** 的严格数学定义及其证明方法，同时也涉及到文法分类 (Chomsky Hierarchy) 和 LL(1) 文法的基本限制条件。

### 2. 核心知识点讲解 (Core Concepts)

**1. 上下文无关文法 (Context-Free Grammar, CFG)**

- **定义：** 乔姆斯基体系 (Chomsky Hierarchy) 中的 2 型文法。其核心特征是：**所有产生式的左部必须是单一的非终结符 (a single non-terminal)。**本题中，产生式左部只有单一的 S，因此**它绝对是一个合法的 CFG。**

**2. 二义性 (Ambiguity)**

- **严格定义：** 如果**一个文法能够为同一个合法的句子 (Sentence) 生成两棵或两棵以上不同的语法树 (Parse trees)**，或者**等价地说，存在两个或以上不同的最左推导 (Leftmost derivations)，那么该文法就是二义性文法 (Ambiguous grammar)。**
- **证明方法：** 考场上证明二义性的唯一有效手段是**举反例 (Counter-example)**。只需要找到一个具体的字符串，画出两棵不同的树即可。

**3. 左递归 (Left Recursion) 与 LL(1) 的互斥性**

- 形如 A→Aα 的推导称为**直接左递归 (Direct left recursion)**。本题中 S→SaS 和 S→SbS 都是典型的左递归。
- **金科玉律：** **包含左递归的文法绝对不可能是 LL(1) 文法**，因为它**会导致自顶向下分析器陷入死循环（First 集合存在严重冲突）**。同时，**二义性文法也绝对不可能是 LL(1)、LR(1) 等任何确定性分析文法。**

### 3. 选项详细剖析 (Option Breakdown)

- **(D) G[S] is not a context-free grammar:** 错误。如前所述，产生式左部均为单一非终结符 S，符合 CFG 定义。
- **(C) G[S] is LL(1) grammar:** 错误。文法包含明显的左递归（S 直接推导出以 S 开头的串），并且是二义性的。LL(1) 要求必须无歧义且无左递归。
- **(A) G[S] is ambiguous (正确) / (B) G[S] is unambiguous (错误):**
	- **证明过程 (Proof)：** 我们选取字符串 `aba` 来寻找最左推导。
	- **推导路径 1 (从 SaS 开始)：** S⇒SaS⇒ϵaS⇒ϵaSbS⇒ϵaϵbS⇒ϵaϵbϵ=aba （对应的语法树根节点是 S→SaS）
	- **推导路径 2 (从 SbS 开始)：** S⇒SbS⇒SaSbS⇒ϵaSbS⇒ϵaϵbS⇒ϵaϵbϵ=aba （对应的语法树根节点是 S→SbS）
	- **结论：** 同一个字符串 `aba` 拥有两个完全不同的最左推导（即两棵结构不同的语法树），因此该文法是二义性的。

### 4. 考试风格练习题 (Exam-Style Practice)

**题目 1 (单选题 - 考查二义性与分析方法的关系)：** If a Context-Free Grammar (CFG) is formally proven to be ambiguous, which of the following statements is ALWAYS true? (如果一个上下文无关文法被正式证明为是二义性的，以下哪个陈述**总是**正确的？) (A) It can be parsed by an LL(1) parser. (它可以被 LL(1) 分析器解析。) (B) It can be parsed by an LR(1) parser. (它可以被 LR(1) 分析器解析。) (C) It cannot be parsed by any deterministic top-down or bottom-up parser. (它不能被任何确定性的自顶向下或自底向上分析器解析。) (D) The ambiguity can always be eliminated by left factoring. (二义性总是可以通过提取左公因子来消除。)

**【答案与解析】** **答案：** (C) **解析：** 这是一个极其重要的理论考点。**任何确定性 (Deterministic) 的语法分析器（包括所有的 LL 和 LR 家族）在每一步都必须做出唯一的、无冲突的决定。**二义性文法的**本质就是存在多条合法的解析路径，这必然会导致分析器在状态转移时产生冲突（如 LL 的多重入口，LR 的移进-归约冲突或归约-归约冲突）。**因此**二义性文法不能用于任何确定性分析器。**(D) 是错误的，**有些二义性（如固有二义性 Inherent ambiguity）是无法消除的**；**提取左公因子解决的是 First 集冲突，并不一定能解决二义性。**

**题目 2 (判断题 - 考查经典二义性结构识别)：** The classical "dangling-else" grammar: Stmt→if Expr then Stmt∣if Expr then Stmt else Stmt∣other is unambiguous because the compiler always matches the 'else' with the closest unmatched 'then'. (**经典的“悬空-else”文法是无二义性的**，因为**编译器总是将 'else' 与最近的未匹配的 'then' 进行匹配。**) (True / False)

**【答案与解析】** **答案：** False (错误) **解析：** 这句话的**前半句和后半句混淆了“文法理论”和“编译器实现”**。在**纯粹的文法数学定义**上，**“悬空-else”文法绝对是二义性 (Ambiguous) 的，因为形如 `if E1 then if E2 then S1 else S2` 的句子可以画出两棵合法的语法树（`else` 既可以归属外层 `if` 也可以归属内层 `if`）**。“将 else 与**最近的 then 匹配”是编译器在编写分析表时人为强加的消解规则 (Disambiguation rule)**，它并**没有改变该文法本身是二义性文法的事实。**

# 到底什么叫做文法,为我解释一下

在编译原理和形式语言理论中，**文法 (Grammar)** 不是我们日常生活中所说的“语法习惯”，而是一个**严格的数学模型 (Strict mathematical model)**。

简单来说，**文法是一组精确的数学规则 (A set of precise mathematical rules)**，用于描述如何利用有限的符号**生成一个形式语言 (Formal language) 中所有合法的句子 (Sentences)。**

为了在考试中准确作答，你需要掌握文法的**形式化定义 (Formal definition)**。

### 1. 文法的四元组定义 (The 4-Tuple Definition)

在考试中，文法 G **通常被严格定义为一个四元组 (4-tuple)：**

**G=(VT,VN,P,S)**

这四个字母代表了构成文法的四大核心要素：

- **VT - 终结符集合 (Set of Terminals):**
	- **定义：** 语言中**不可再细分的基本符号，是最终出现在合法句子中的实体字符。**
	- **举例：** 关键字 (`if`, `while`)、标识符 (`id`)、常数 (`123`)、运算符 (`+`, `*`)、标点符号等。在文法题目中，通常用小写字母 (a, b, c) 或具体符号表示。
- **VN - 非终结符集合 (Set of Non-terminals):**
	- **定义：** 代表语法类别或语法结构的变量符号。它们可以被进一步展开或替换。
	- **举例：** 表达式 (`Expr`)、语句 (`Stmt`)、项 (`Term`)。在文法题目中，通常用大写字母 (A, B, S) 表示。
	- **注意：** 终结符和非终结符的交集必须为空 (VT∩VN=∅)，它们的并集构成了文法的**字母表 (Vocabulary/Alphabet)**。
- **P - 产生式集合 (Set of Productions / Rewrite rules):**
	- **定义：** **定义非终结符如何被替换为其他符号序列的核心规则。**
	- **形式：** 形式为 α→β。读作 “α 产生 β” 或 “α 定义为 β”。
		- **左部 (Left-Hand Side, LHS) α：** 必须**包含至少一个非终结符。**
		- **右部 (Right-Hand Side, RHS) β：** 是**由终结符和非终结符混合组成的任意符号串（也可以是空串 ϵ）。**
- **S - 起始符号 (Start Symbol):**
	- **定义：** **一个特殊的非终结符 (S∈VN)**。**所有的推导 (Derivation) 都必须从这个符号开始。它代表了该文法所能识别的最大的语法结构（通常代表整个“程序”）。**

### 2. 文法的两个核心视角 (Two Core Perspectives)

编译器利用文法做两件事情：

1. **生成视角 (Generation - 自顶向下):** 从**起始符号 S 开始，反复利用产生式 P 将非终结符替换为右部**，直到**得到一个只包含终结符的符号串。这个过程称为推导 (Derivation)**。文法**能推导出的所有终结符串的集合，就是该文法生成的语言 (Language)，记为 L(G)。**
2. **识别/分析视角 (Recognition/Parsing - 自底向上):** 给定**一个由终结符组成的输入串（如你的源代码）**，编译器**尝试通过产生式 P 逆向推导（即归约 Reduction）**，看是否**能将其合法地折叠回起始符号 S**。如果**能，说明代码语法正确；如果不能，则报语法错误。**

### 3. 乔姆斯基文法体系 (Chomsky Hierarchy)

在考试中，你必须清楚文法被分为四个层级，限制越来越严格：

- **0型文法 (Type-0):** **无限制文法。对应图灵机。**
- **1型文法 (Type-1):** **上下文有关文法 (Context-Sensitive Grammar, CSG)。**
- **2型文法 (Type-2):** **上下文无关文法 (Context-Free Grammar, CFG)。**
	- **特点：** 产生式**左部必须是一个单一的非终结符 (A→β)。**
	- **考点：** 它**是语法分析 (Syntax Analysis) 的基础。LL(1), LR(1) 等分析算法处理的都是 CFG。**
- **3型文法 (Type-3):** **正则文法 (Regular Grammar)。**
	- **特点：** 产生式**形式被严格限制为 A→aB 或 A→a（右线性）**，**或者 A→Ba 或 A→a（左线性）。**
	- **考点：** 它**是词法分析 (Lexical Analysis) 的基础，与正则表达式 (Regular Expression) 和有限自动机 (DFA/NFA) 完全等价。**

### 4. 考试风格练习题 (Exam-Style Practice)

**题目 1 (单选题 - 考查文法四要素的区分)：** In the formal definition of a grammar G=(VT,VN,P,S), which of the following best describes the Start Symbol S? (在文法 G=(VT,VN,P,S) 的形式化定义中，以下哪一项最准确地描述了起始符号 S？) (A) It is a terminal symbol that indicates the end of a string. (它是一个指示字符串结束的终结符。) (B) It is a distinguished non-terminal symbol from which all derivations begin. (它是一个特殊的非终结符，所有的推导都从它开始。) (C) It is a production rule that generates the empty string ϵ. (它是一个生成空串 ϵ 的产生式。) (D) It is a function that maps non-terminals to terminals. (它是一个将非终结符映射到终结符的函数。)

**【答案与解析】** **答案：** (B) **解析：** 严格按照四元组定义：S 属于非终结符集合 VN (S∈VN)，并且它是整个文法推导的入口起点。选项 (A) 描述的是结束标记 `$` 或 `#`；(C) 描述的是 ϵ-产生式。

**题目 2 (判断题 - 考查终结符与非终结符的本质)：** In a Context-Free Grammar (CFG), a terminal symbol (终结符) can appear on the left-hand side (左部) of a production rule. (在上下文无关文法中，终结符可以出现在产生式的左部。) (True / False)

**【答案与解析】** **答案：** False (错误) **解析：** 2型文法（上下文无关文法 CFG）的严格数学定义要求，**产生式 α→β 的左部 α 必须是单个非终结符 (A single non-terminal)**。终结符是原子的、不可再分的，它绝对不能出现在产生式的左侧去“产生”别的东西。

![image-20260625182847318](./scut%E7%BC%96%E8%AF%91%E5%8E%9F%E7%90%86%E9%A2%98%E7%9B%AE%E5%A4%8D%E4%B9%A01/image-20260625182847318.png)

### 1. 题目解析 (Question Analysis)

**题目大意：** 给定一个**无二义性文法 (unambiguous grammar)，关于推导 (derivation) 和语法树 (parse tree)**，以下哪一项是**不正确 (NOT CORRECT)** 的？ **正确答案：** (C) The derivation is unique for a given string. (对于给定的字符串，推导是唯一的。)

这道题考察的是**文法二义性的本质 (Essence of Grammar Ambiguity)**，以及**语法树 (Parse Tree)** 与**推导序列 (Derivation sequence)** 之间的严格对应关系。

### 2. 核心知识点讲解 (Core Concepts)

**1. 无二义性文法的定义 (Definition of Unambiguous Grammar)** 如果一个文法是无二义性的 (Unambiguous)，这意味着对于该文法生成的语言中的任何一个合法句子（字符串），**只存在唯一的一棵语法树 (Parse tree)**。

**2. 语法树与推导的对应关系 (Mapping between Parse Tree and Derivation)**

- **语法树 (Parse tree)** 是**推导过程的图形化展示**。它**忽略了非终结符被替换的先后顺序**，**只记录了“哪个非终结符被替换成了什么”。**
- **最左推导 (Leftmost derivation)** 和 **最右推导 (Rightmost derivation)** 是两种采用了**极其严格顺序**的推导策略。
- **核心定理：** 一棵**特定**的语法树，**唯一对应 (uniquely corresponds to)** 一个最左推导，也**唯一对应**一个最右推导。**既然无二义性文法保证了语法树是唯一的，那么它也必然保证最左推导和最右推导是唯一的。**

**3. 一般推导的非唯一性 (Non-uniqueness of General Derivation)** 为什么 (C) 是错误的？因为“**一般推导 (General derivation)”没有规定替换的顺序**。 **举个极简的证明例子 (Proof by example)：** 假设有一个极简的无二义性文法： S→AB A→a B→b 我们要推导出字符串 ab。

- **最左推导 (Leftmost):** S⇒AB⇒aB⇒ab （唯一）
- **最右推导 (Rightmost):** S⇒AB⇒Ab⇒ab （唯一） 可以看到，**为了得到完全相同的目标字符串 ab，并且构建出完全相同的一棵语法树（根节点 S，左子节点 A→a，右子节点 B→b）**，我们**依然可以有两条不同的推导路径（先替换 A 还是先替换 B）**。 因此，对于给定的字符串，**一般的“推导 (derivation)”绝不是唯一的，只有“最左”或“最右”推导才是唯一的。**

### 3. 选项详细剖析 (Option Breakdown)

- **(A) The leftmost derivation is unique for a given string (最左推导是唯一的):** 正确的表述。因为文法无二义性 → 语法树唯一 → 最左推导必然唯一。
- **(B) The rightmost derivation is unique for a given string (最右推导是唯一的):** 正确的表述。同上，语法树唯一 → 最右推导必然唯一。
- **(D) The parse tree is unique for a given string (语法树是唯一的):** 正确的表述。这就是无二义性文法 (Unambiguous grammar) 的官方数学定义。
- **(C) The derivation is unique for a given string (推导是唯一的):** **错误的表述 (符合题意)**。即使文法没有二义性，由于**推导过程中展开非终结符的先后顺序可以随意组合（如先左后右，或先右后左，或交替进行），所以总推导序列通常有多个。**

### 4. 考试风格练习题 (Exam-Style Practice)

**题目 1 (单选题 - 考查等价概念映射)：** For any context-free grammar, which of the following mapping relations is ALWAYS one-to-one (一 一对应)? (对于任何上下文无关文法，以下哪种映射关系**总是**一一对应的？) (A) A string and a parse tree (一个字符串和一棵语法树) (B) A rightmost derivation and a leftmost derivation (一个最右推导和一个最左推导) (C) A parse tree and a leftmost derivation (一棵语法树和一个最左推导) (D) A string and a leftmost derivation (一个字符串和一个最左推导)

**【答案与解析】** **答案：** (C) **解析：** (A) 错误。如果文法是**二义性的，一个字符串可以对应多棵语法树**。 (D) 错误。同理**，二义性文法下，一个字符串可以对应多个最左推导。** (B) 错误。推导序列本身是两个不同的集合，谈不上一一映射。 (C) 正确。无论文法是否有二义性，**一棵特定的语法树**其结构是固定的，当你严格按照“从左到右”的顺序去遍历展开这棵树时，你只能写出**唯一的一个**最左推导序列。这两者在数学上是绝对的一一对应等价物。

**题目 2 (判断题 - 考查二义性定义的逆向思维)：** If a string w generated by a grammar G has two different derivations, then grammar G must be ambiguous. (如果由文法 G 生成的字符串 w 拥有两种不同的推导，那么文法 G 必定是二义性的。) (True / False)

**【答案与解析】** **答案：** False (错误) **解析：** 这正是你刚刚做过的真题 (10) 的核心陷阱。拥有“两种不同的推导 (two different derivations)”并不代表它是二义性的，因为可能一个是推导顺序先展开左边，另一个推导顺序先展开右边，但它们最终画出来的是同一棵语法树。 只有当它拥有**两个不同的最左推导 (two different leftmost derivations)** 或 **两棵不同的语法树 (two different parse trees)** 时，才能判定该文法是二义性的。定义中的限定词（leftmost / parse tree）极其严格，不可省略。



![image-20260625183206717](./scut%E7%BC%96%E8%AF%91%E5%8E%9F%E7%90%86%E9%A2%98%E7%9B%AE%E5%A4%8D%E4%B9%A01/image-20260625183206717.png)

### 1. 题目解析 (Question Analysis)

这是一道极其经典的自动机综合大题，涵盖了词法分析 (Lexical Analysis) 中从非确定性有限自动机 (NFA) 到确定性有限自动机 (DFA)，再到最小化 DFA 的完整流水线。

### 2. 核心知识点讲解与解题 (Core Concepts & Solutions)

#### 1) 求解正则表达式 (Regular Expression for NFA)

**【解题思路】**

观察状态图，寻找从起始状态 `S` 到接受状态 `D` 的必经路径和循环：

1. **必经前缀：** 必须从 `S` 输入 `1` 到达 `A`。
2. **$\epsilon$-转移的自由移动：** 到达 `A` 后，可以**通过 $\epsilon$ 边无条件**直接到达 `B`，再无条件到达 `C`。这意味着 `A`、`B`、`C` 在**不消耗输入字符的情况下是互通的（形成了 $\epsilon$-闭包）。**
3. **循环部分：** 在状态 `B` 有一个针对 `0` 和 `1` 的自环，表示可以接受任意数量的 `0` 和 `1` 的组合，对应正则表达式的 `(0|1)*`。
4. **必经后缀：** 必须从 `C` 输入 `1` 才能到达最终的接受状态 `D`。

**【最终答案】**

该 NFA 对应的正则表达式为：

**`1(0|1)\*1`**

#### 2) 子集构造法 (Subset Construction to Convert NFA to DFA)

**【核心概念】**

子集构造法的**核心在于消除不确定性。由于 NFA 在某一状态面对同一输入可以有多个去向（包含 $\epsilon$ 边），我们需要将“NFA 的状态集合”打包成“DFA 的单一状态”。**

关键操作是**求解 $\epsilon$-闭包 ($\epsilon$-closure)：即从某状态出发，仅通过 $\epsilon$ 边能到达的所有状态的集合。**

**【解题步骤】**

- **初始状态 $I_0$：** $I_0 = \epsilon\text{-closure}(S) = \{S\}$
- **计算 $I_0$ 的转移：**
	- 输入 `0`：$\text{move}(I_0, 0) = \emptyset$ **(空集，通常记为死状态或省略**)
	- 输入 `1`：$\text{move}(I_0, 1) = \{A\}$ $\rightarrow$ $\epsilon\text{-closure}(\{A\}) = \{A, B, C\}$。我们**将其命名为新状态 $I_1$。**
- **计算 $I_1 = \{A, B, C\}$ 的转移：**
	- 输入 `0`：**只有状态 `B` 接收 `0` 到达 `B`。$\epsilon\text{-closure}(\{B\}) = \{B, C\}$。命名为新状态 $I_2$。**
	- 输入 `1`：状态 `B` 接收 `1` 到达 `B`，状态 `C` 接收 `1` 到达 `D`。集合为 $\{B, D\}$。$\epsilon\text{-closure}(\{B, D\}) = \{B, C, D\}$。**命名为新状态 $I_3$。**
- **计算 $I_2 = \{B, C\}$ 的转移：**
	- 输入 `0`：$\text{move}(\{B, C\}, 0) = \{B\}$ $\rightarrow$ $\epsilon\text{-closure}(\{B\}) = \{B, C\}$。这**正是 $I_2$。**
	- 输入 `1`：$\text{move}(\{B, C\}, 1) = \{B, D\}$ $\rightarrow$ $\epsilon\text{-closure}(\{B, D\}) = \{B, C, D\}$。这**正是 $I_3$。**
- **计算 $I_3 = \{B, C, D\}$ 的转移：** (注意：因为**包含了 NFA 的接受状态 D，所以 $I_3$ 是 DFA 的接受状态**)
	- 输入 `0`：只**有 `B` 接收 `0`。结果同上，为 $I_2$。**
	- 输入 `1`：只有 `B` 接收 `1` (D没有出边)。$\text{move}(\{B, C, D\}, 1) = \{B\}$ $\rightarrow$ $\epsilon\text{-closure}(\{B\}) = \{B, C\}$。等等，这里需要仔细检查！
	- *勘误：* 在计算 $I_3$ 输入 `1` 时，$\text{move}(\{B, C, D\}, 1)$。`B` 输入 `1` 到 `B`，`C` 输入 `1` 到 `D`。所以**集合是 $\{B, D\}$。$\epsilon\text{-closure}(\{B, D\}) = \{B, C, D\}$。所以结果是 $I_3$。**

**【最终答案：状态转移表 (Transition Table)】**

| **DFA State**          | **NFA States** | **Input 0** | **Input 1** |
| ---------------------- | -------------- | ----------- | ----------- |
| **$I_0$** **(Start)**  | $\{S\}$        | $\emptyset$ | $I_1$       |
| **$I_1$**              | $\{A, B, C\}$  | $I_2$       | $I_3$       |
| **$I_2$**              | $\{B, C\}$     | $I_2$       | $I_3$       |
| **$I_3$** **(Accept)** | $\{B, C, D\}$  | $I_2$       | $I_3$       |

*(注：在画转移图时，**根据此表连线即可，$I_3$ 画双圈代表接受状态**。)*

#### 3) 状态最小化 (State Minimization Algorithm)

**【核心概念】**

**使用 划分法 (Hopcroft's Algorithm)**。思想是：如果两个状态在面对所有可能的输入时，都**跳转到相同的状态组，那么这两个状态就是等价的 (Equivalent states)，可以合并。**

**【解题步骤】**

1. **初始划分 $\Pi_0$：** 将**状态分为接受状态组和非接受状态组。**
	- 非接受组：$P_1 = \{I_0, I_1, I_2\}$
	- 接受组：$P_2 = \{I_3\}$
	- $\Pi_0 = \{ \{I_0, I_1, I_2\}, \{I_3\} \}$
2. **尝试细分 $P_1 = \{I_0, I_1, I_2\}$：** 考察**它们输入 `1` 时的去向。**
	- **$I_0 \xrightarrow{1} I_1$ (留在 $P_1$ 内部)**
	- **$I_1 \xrightarrow{1} I_3$ (去往 $P_2$)**
	- **$I_2 \xrightarrow{1} I_3$ (去往 $P_2$)**
	- 显然**，$I_0$ 的行为与 $I_1, I_2$ 不同（$I_0$ 无法一步到达接受组）。因此 $I_0$ 必须被剥离出来。**
	- 得到新划分：$\Pi_1 = \{ \{I_0\}, \{I_1, I_2\}, \{I_3\} \}$
3. **尝试细分 $\{I_1, I_2\}$：** 考察它们输入 `0` 和 `1` 时的去向。
	- 输入 `0`：$I_1 \xrightarrow{0} I_2$, $I_2 \xrightarrow{0} I_2$。它们都去往同一组 $\{I_1, I_2\}$。
	- 输入 `1`：$I_1 \xrightarrow{1} I_3$, $I_2 \xrightarrow{1} I_3$。它们都去往同一组 $\{I_3\}$。
	- 结论：$I_1$ 和 $I_2$ 是**不可区分的 (Indistinguishable)**，它们**是等价状态。**
4. **最终划分：** $\Pi_{final} = \{ \{I_0\}, \{I_1, I_2\}, \{I_3\} \}$。无法继续细分。

**【最终答案】**

- **Equivalent states (等价状态):** **$I_1$ 和 $I_2$ 是等价的** (可**合并为一个状态，记为 $I_{12}$)**。
- **Minimum-state DFA graph (最小化转移图描述):**
	- **状态节点：$I_0$ (Start), $I_{12}$, $I_3$ (Accept)。**
	- 转移规则：
		- $I_0 \xrightarrow{1} I_{12}$
		- $I_{12} \xrightarrow{0} I_{12}$
		- $I_{12} \xrightarrow{1} I_3$
		- $I_3 \xrightarrow{0} I_{12}$
		- $I_3 \xrightarrow{1} I_3$

### 3. 考试风格练习题 (Exam-Style Practice)

为了巩固子集构造和状态最小化的敏锐度，请尝试解答以下变体问题：

**题目 1 (简答题 - 考查 $\epsilon$-闭包的深度理解)：**

In subset construction, if state $q_x$ has an $\epsilon$-transition to $q_y$, and $q_y$ has an $\epsilon$-transition to $q_z$, does $\epsilon\text{-closure}(\{q_x\})$ include $q_z$? Briefly explain why. (在子集构造中，如果状态 $q_x$ 有一条 $\epsilon$ 边指向 $q_y$，且 $q_y$ 有一条 $\epsilon$ 边指向 $q_z$，那么 $\epsilon\text{-closure}(\{q_x\})$ 是否包含 $q_z$？请简述原因。)

**【答案与解析】**

**答案：** 是的，包含 $q_z$。

**解析：** $\epsilon$-闭包的数学定义是自反且传递的 (reflexive and transitive)。它代表了从给定状态出发，**通过任意次（包括 0 次）连续的 $\epsilon$-转移**所能到达的所有状态的集合。由于 $q_x \xrightarrow{\epsilon} q_y \xrightarrow{\epsilon} q_z$ 形成了一条纯 $\epsilon$ 路径，状态机可以在不读取任何输入字符的情况下从 $q_x$ 瞬间抵达 $q_z$，因此 $q_z$ 必定属于该闭包。

**题目 2 (单选题 - 考查最小化算法的终止条件)：**

During the DFA state minimization process (Hopcroft's algorithm), the algorithm terminates when: (在 DFA 状态最小化过程中，算法在何时终止？)

(A) All states are reduced to a single state. (所有状态被合并为一个单一状态。)

(B) No further splitting of any partition sets is possible. (任何划分集合都无法被进一步拆分。)

(C) The number of states equals the number of accepting states. (状态数等于接受状态数。)

(D) All non-accepting states are merged into one state. (所有非接受状态被合并为一个状态。)

**【答案与解析】**

**答案：** (B)

**解析：** 划分法的核心是不断寻找“可区分”的证据。如果在某一轮循环中，检查了所有集合针对所有可能输入的转移情况后，发现没有任何一个集合的内部状态出现了去向分歧（即它们面对同样的输入都去往同一个集合），这意味着当前划分就是最简等价类，算法立刻终止。(A)、(C)、(D) 描述的都是极端的、视具体语言而定的偶然情况，绝不是算法的通用终止条件。

![image-20260625185532229](./scut%E7%BC%96%E8%AF%91%E5%8E%9F%E7%90%86%E9%A2%98%E7%9B%AE%E5%A4%8D%E4%B9%A01/image-20260625185532229.png)

![image-20260625185539072](./scut%E7%BC%96%E8%AF%91%E5%8E%9F%E7%90%86%E9%A2%98%E7%9B%AE%E5%A4%8D%E4%B9%A01/image-20260625185539072.png)

完整覆盖了**文法设计 (Grammar Design)**、**文法等价变换 (Grammar Transformation)** 以及 **LL(1) 分析条件验证 (LL(1) Verification)**。

*(注：原图中的文法 $B \rightarrow B \text{ or } B \mid B \text{ and } B \mid \text{not } B$ 是不完整的，因为**缺少推导到终结符的基础情况。**为了使文法完整且能够进行后续的 LL(1) 验证，这里我们在重写时合理地**引入标识符 `id` 和括号 `( B )` 作为基础操作数**。)*

### 1. 第一问：消除二义性 (Rewrite the grammar to eliminate ambiguities)

**【解题核心逻辑】**

要消除包含运算符的表达式文法的二义性，必须严格遵循两个维度的规则：

1. **优先级 (Precedence)：** **优先级越低的运算符，在语法树中越靠近根节点（即越早被推导，放在越外层的非终结符规则中）；优先级越高的运算符，越靠近叶子节点**。
	- 本题优先级：`not` > `and` > `or`。因此 **`or` 在最顶层，`not` 在最底层。**
2. **结合性 (Associativity)：** * **左结合 (Left associative)：** 必须使用**左递归 (Left recursion)**。例如 $A \rightarrow A \text{ op } B$。
	- **右结合 (Right associative)：** 必须使用**右递归 (Right recursion)**。例如 $A \rightarrow B \text{ op } A$。
	- 本题**中 `and` 和 `or` 都是左结合，所以必须写成左递归形式。`not` 是一元前缀运算符，通常视为右结合。**

**【重写后的无二义性文法】**

我们**引入分层非终结符：$B$ (代表 or 层), $T$ (代表 and 层), $F$ (代表 not/基础层)。**

$$\begin{aligned} B &\rightarrow B \text{ or } T \mid T \\ T &\rightarrow T \text{ and } F \mid F \\ F &\rightarrow \text{not } F \mid ( B ) \mid \text{id} \end{aligned}$$

### 2. 第二问：消除左公因子和左递归 (Eliminate left factor and left recursion)

**【解题核心逻辑】**

第一问构造的文法为了满足“左结合性”，引入了直接左递归 (Immediate left recursion)，如 $B \rightarrow B \text{ or } T$。这种形式会导致自顶向下的预测分析器陷入死循环，必须消除。

- **消除直接左递归的通用公式：**

	对于 $A \rightarrow A\alpha \mid \beta$ （其中 $\beta$ 不以 $A$ 开头），等价转化为：

	$A \rightarrow \beta A'$

	$A' \rightarrow \alpha A' \mid \epsilon$

- **检查左公因子 (Left factoring)：** 当前文法**不存在多个右部具有相同前缀的情况，因此不需要提取左公因子。**

**【消除左递归后的文法】**

应用上述公式，我们**将 $B$ 和 $T$ 的左递归消除，引入新的非终结符 $B'$ 和 $T'$：**

$$\begin{aligned} B &\rightarrow T B' \\ B' &\rightarrow \text{or } T B' \mid \epsilon \\ T &\rightarrow F T' \\ T' &\rightarrow \text{and } F T' \mid \epsilon \\ F &\rightarrow \text{not } F \mid ( B ) \mid \text{id} \end{aligned}$$

### 3. 第三问：验证 LL(1) 条件是否满足 (Verify whether the LL(1) condition is satisfied)

**【解题核心逻辑】**

要严格验证 LL(1) 条件，必须求出**各非终结符的 FIRST 集 和必要的 FOLLOW 集**，然后检查所有**拥有多个候选式的产生式是否发生冲突。**

**步骤 3.1：计算 FIRST 集**

$$First(F) = \{\text{not}, (, \text{id}\}$$

$$First(T) = First(F) = \{\text{not}, (, \text{id}\}$$

$$First(B) = First(T) = \{\text{not}, (, \text{id}\}$$

$$First(B') = \{\text{or}, \epsilon\}$$

$$First(T') = \{\text{and}, \epsilon\}$$

**步骤 3.2：计算 FOLLOW 集**

*(注意：**只有当非终结符能推导出 $\epsilon$ 时，才必须计算其 FOLLOW 集用于冲突检测**。**这里 $B'$ 和 $T'$ 是 nullable 的**。)*

- 设 $B$ 是起始符号，则结束符 $\$$ 加入 $Follow(B)$。且在 $F \rightarrow ( B )$ 中，$) \in Follow(B)$。所以：

	$$Follow(B) = \{\$, )\}$$

- $**B'$ 出现在 $B \rightarrow T B'$ 尾部**，所以 $Follow(B') = Follow(B) = \{\$, )\}$。

- $T$ 出现在 $B \rightarrow T B'$ 和 $B' \rightarrow \text{or } T B'$ 中。其后面跟着 $B'$，所以 $First(B') \setminus \{\epsilon\} \subseteq Follow(T)$。且当 $B' \Rightarrow \epsilon$ 时，$Follow(B')$ 也要加入。

	$$Follow(T) = \{\text{or}\} \cup Follow(B') = \{\text{or}, \$, )\}$$

- **$T'$ 出现在 $T \rightarrow F T'$ 尾部**，所以 $Follow(T') = Follow(T) = \{\text{or}, \$, )\}$。

**步骤 3.3：LL(1) 冲突检测 (Intersection Check)**

我们需要**检查具有多个右部的产生式：**

1. **对于 $B' \rightarrow \text{or } T B' \mid \epsilon$ :**

	$First(\text{or } T B') = \{\text{or}\}$

	$Follow(B') = \{\$, )\}$

	交集：$\{\text{or}\} \cap \{\$, )\} = \emptyset$。**无冲突。**

2. **对于 $T' \rightarrow \text{and } F T' \mid \epsilon$ :**

	$First(\text{and } F T') = \{\text{and}\}$

	$Follow(T') = \{\text{or}, \$, )\}$

	交集：$\{\text{and}\} \cap \{\text{or}, \$, )\} = \emptyset$。**无冲突。**

3. **对于 $F \rightarrow \text{not } F \mid ( B ) \mid \text{id}$ :**

	$First(\text{not } F) = \{\text{not}\}$

	$First(( B )) = \{(\}$

	$First(\text{id}) = \{\text{id}\}$

	这三个集合互不相交。**无冲突。**

**【最终结论】**

因为所有具有多个候选式的非终结符都严格满足 LL(1) 判断条件（FIRST 集不相交，且含有 $\epsilon$ 产生式时 FIRST 集与 FOLLOW 集也不相交），**该文法完全满足 LL(1) 条件 (The LL(1) condition is satisfied)。**

### 4. 考试风格练习题 (Exam-Style Practice)

**题目 1 (单选题 - 考查结合性与递归方向的理论关系)：**

When constructing a context-free grammar to parse an expression, to ensure that the subtraction operator `-` is strictly **left-associative** (左结合), which of the following production rule structures MUST be used? (在构造用于解析表达式的上下文无关文法时，为了确保减号运算符 `-` 严格保持**左结合**，必须使用以下哪种产生式结构？)

(A) $E \rightarrow T - E$

(B) $E \rightarrow E - T$

(C) $E \rightarrow T \mid - E$

(D) $E \rightarrow T - T$

**【答案与解析】**

**答案：** (B)

**解析：** 左结合 (Left-associative) 的本质是相同的运算符连续出现时（如 `a - b - c`），左边的操作优先进行，即 `(a - b) - c`。在语法树上，左边的计算需要位于树的更深层。使**用左递归 (Left recursion)（即产生式右部的第一个符号是左部的非终结符本身，如 $E \rightarrow E - T$）能够使语法树向左侧不断向下延伸，从而完美实现左结合**。(A) 属于右递归，会导致右结合。

**题目 2 (简答题 - 考查消除左递归的通用公式应用)：**

Given the grammar rule with immediate left recursion: $A \rightarrow A x y \mid A z \mid b \mid c$. Please rewrite it to eliminate the left recursion. (给定包含直接左递归的文法规则：$A \rightarrow A x y \mid A z \mid b \mid c$。请**重写它以消除左递归**。)

**【答案与解析】**

**答案：**

$$A \rightarrow b A' \mid c A'$$

$$A' \rightarrow x y A' \mid z A' \mid \epsilon$$

**解析：** 将产生式分为两组**：以 $A$ 开头的（$\alpha_1 = x y$, $\alpha_2 = z$）和不以 $A$ 开头的（$\beta_1 = b$, $\beta_2 = c$）**。根据通用公式，原非终结符 $A$ 的右部现在由所有的 $\beta_i$ 后接新非终结符 $A'$ 组成；新非终结符 $A'$ 的右部由所有的 $\alpha_i$ 后接 $A'$ 组成，**并增加一个空串 $\epsilon$。**

# 第一问为什么第一行还可单独推出T,第三行为何还推出(B)和id

### 1. 回答“为什么第一行还可以单独推出 $T$” ($B \rightarrow B \text{ or } T \mid T$)

这涉及到表达式文法分层设计的一个核心原则：**级联推导 (Cascading Derivation)**，目的是**为了处理没有该层运算符的情况。**

**【详细解释】**

我们在设计文法时，将表达式分为不同的优先级层：$B$ 层处理 `or`，$T$ 层处理 `and`，**$F$ 层处理 `not` 和基础项。**

考虑这样一个输入串：`x and y`。

这个串里**根本没有 `or` 运算符，但它是一个完全合法的表达式。**

如果我们的**规则只有 $B \rightarrow B \text{ or } T$，那么我们永远无法推导出不包含 `or` 的表达式！因为每一次推导都必须带出一个 `or`。**

加上 $\mid T$ 后，**它提供了一条“降维/穿透”的逃生路线**。

当我们需要**推导 `x and y` 时，最顶层的 $B$ 就会选择 $\mid T$ 这条分支：**

$B \Rightarrow T \Rightarrow T \text{ and } F \Rightarrow F \text{ and } F \dots$

**总结：** $B \rightarrow T$ 表示**“一个由 `or` 连接的表达式，也可以仅仅是一个没有 `or` 的 `and` 表达式”**。同理，$T \rightarrow F$ 表示“一个 `and` 表达式，也可以仅仅是一个基础项”。这保证了**文法能向下覆盖所有高优先级的简单表达式**。

### 2. 回答“第三行为何还推出 `(B)` 和 `id`” ($F \rightarrow \text{not } F \mid ( B ) \mid \text{id}$)

原题目给出的文法 $B \rightarrow B \text{ or } B \mid B \text{ and } B \mid \text{not } B$ 是一个**理论上不完整**的缩略文法。

**【详细解释】**

1. **为什么需要 `id` (基础标识符)？**

	在原规则中，所有的推导结果依然是非终结符 $B$。例如：

	$B \Rightarrow B \text{ or } B \Rightarrow (B \text{ and } B) \text{ or } (\text{not } B) \dots$

	如果不提供一个能够推导为**纯终结符**（如变量名 $x, y, z$ 或常量 $true, false$）的出口，这个推导过程将**永远无法停止**。

	因此，在实际的编译器实现和严谨的考试中，必须在优先级最高的底层（这里是 $F$ 层）加入**基础操作数，通常用 `id` (identifier) 或 `value` 表示。**

2. **为什么需要 `(B)` (括号表达式)？**

	括号的作用是**打破默认的优先级规则**。

	优先级规则是：`not` > `and` > `or`。

	如果用户想表达：先计算 `or`，再对结果进行 `not` 运算呢？例如 `not (x or y)`。

	如果**没有括号，文法只能推导 `not x or y`，这会被默认解析为 `(not x) or y`。**

	在**底层 $F$ 中加入 `( B )`，允许表达式递归地回到最顶层 $B$，从而赋予括号内表达式绝对的最高计算优先级。**这是所有现代编程语言表达式文法的标准做法。

**补充说明：**

在我的解答中，明确标注了“*(注：原图中的文法...是不完整的...这里我们在重写时合理地引入标识符 `id` 和括号 `( B )` 作为基础操作数。)*”。如果你在考试中遇到这种未给全基础符号的简略文法，你有两种选择：

- **更严谨的做法：** 像我一样，**自己补充 `id`（有时老师会认为这是你理解深刻的表现）。**

- **最保守的做法：** 严格按照原题给出的终结符，假设最底层的 `not` 运算后不再继续推导。此时文法写为：

	$B \rightarrow B \text{ or } T \mid T$

	$T \rightarrow T \text{ and } F \mid F$

	$F \rightarrow \text{not } F \mid \text{not } B$ (这种写法很别扭且存在循环依赖)

	所以，强烈建议采用补充 `id` 和 `( B )` 的标准教科书写法，这是业界通用的规范模式。

![image-20260625192524489](./scut%E7%BC%96%E8%AF%91%E5%8E%9F%E7%90%86%E9%A2%98%E7%9B%AE%E5%A4%8D%E4%B9%A01/image-20260625192524489.png)

**题目大意：** 请根据**给定的 LL(1) 分析表 (LL(1) analysis table)** 写出**对应的上下文无关文法 (CFG grammar)**，并对输入字符串 `"num-num/"` 的分析过程进行推导 (make inferences)。

这道题考察了 LL(1) 预测分析器 (Predictive Parser) 的**两个核心逆向与正向操作：**

1. **从分析表还原文法：** 分析表中的**每一个非空条目 (Non-empty entry) M[A,a]=A→α 都直接对应了一条文法产生式。**
2. **表驱动的语法分析过程 (Table-driven parsing process)：** 使用**显式的分析栈 (Parsing stack) 和输入缓冲区 (Input buffer)**，**结合分析表一步步模拟匹配和推导，直到成功接受或发现语法错误。**

### 2. 第一部分：提取上下文无关文法 (Part 1: Extracting the CFG)

**【解题逻辑】** 我们只**需要遍历分析表中的每一行（非终结符 Non-terminals）和每一列（终结符 Terminals）**。如果**单元格中有产生式，就将其提取出来。将左部相同的产生式用 `|` 合并即可。**

根据给定的表格，**还原出的完整 CFG 如下**：

$$\begin{aligned}
E &\rightarrow T E' \\
E' &\rightarrow - T E' \mid \epsilon \\
T &\rightarrow F T' \\
T' &\rightarrow / F T' \mid \epsilon \\
F &\rightarrow \text{num} \mid ( E )
\end{aligned}$$

*(注：表格中手写的 T→FT′ 分别填在 `num` 和 `(` 列，完全符合正确的 LL(1) 构造逻辑。)*

### 3. 第二部分：输入串的分析推导过程 (Part 2: Tracing the Input String)

**【解题逻辑】** 我们要**利用分析栈 (Stack) 来模拟 LL(1) 分析器的运行**。

- **初始状态：** **栈底压入结束标记 `$`，然后压入起始符号 `E`。**输入**缓冲区放入目标串，并在末尾加上 `$`。**
- **动作规则：**
	1. 如果栈顶是**非终结符 (Non-terminal)**，去**查分析表 M[栈顶,当前输入符号]**。如果表项**有产生式，则弹出 (Pop) 栈顶非终结符，并将产生式右部逆序压栈 (Push in reverse order)**。如果表项为空，则报错。
	2. 如果栈顶是**终结符 (Terminal)**，将其与当前输入符号对比。如果相同，则**匹配 (Match)**，同时**弹出栈顶符号并将输入指针后移一位**。如果不相同，则报错。
	3. 如果**栈顶和当前输入均为 `$`，则分析成功 (Accept)。**

目标输入串：`num - num /`

| 步骤 (Step) | 分析栈 (Stack)  | 剩余输入串 (Input)      | 动作 / 查表推导 (Action / Inference)            |
| ----------- | --------------- | ----------------------- | ----------------------------------------------- |
| 1           | `$` E           | `num` `-` `num` `/` `$` | **查表 M[E,num]，输出 E→TE′**                   |
| 2           | `$` E′ T        | `num` `-` `num` `/` `$` | 查表 M[T,num]，输出 T→FT′                       |
| 3           | `$` E′ T′ F     | `num` `-` `num` `/` `$` | 查表 M[F,num]，输出 F→num                       |
| 4           | `$` E′ T′ `num` | `num` `-` `num` `/` `$` | **匹配 (Match)** `num`                          |
| 5           | `$` E′ T′       | `-` `num` `/` `$`       | 查表 M[T′,−]，输出 T′→ϵ                         |
| 6           | `$` E′          | `-` `num` `/` `$`       | 查表 M[E′,−]，输出 E′→−TE′                      |
| 7           | `$` E′ T `-`    | `-` `num` `/` `$`       | **匹配 (Match)** `-`                            |
| 8           | `$` E′ T        | `num` `/` `$`           | 查表 M[T,num]，输出 T→FT′                       |
| 9           | `$` E′ T′ F     | `num` `/` `$`           | 查表 M[F,num]，输出 F→num                       |
| 10          | `$` E′ T′ `num` | `num` `/` `$`           | **匹配 (Match)** `num`                          |
| 11          | `$` E′ T′       | `/` `$`                 | 查表 M[T′,/]，输出 T′→/FT′                      |
| 12          | `$` E′ T′ F `/` | `/` `$`                 | **匹配 (Match)** `/`                            |
| 13          | `$` E′ T′ F     | `$`                     | 查表 M[F,$]。**表格对应项为空 (Blank entry)！** |

**【最终推导结论 (Final Inference)】** 在步骤 13 中，**分析栈栈顶为非终结符 F，而向前看符号 (lookahead) 为输入结束符 `$`。**查 LL(1) 分析表**发现 M[F,$] 为空**。 **结论：** 语法分析器**拒绝 (Reject) 该输入串，并报告语法错误 (Syntax Error)**。这符合逻辑，因为 `num - num /` 结尾多了一个除号，缺少右操作数，是一个非法的算术表达式。

### 4. 考试风格练习题 (Exam-Style Practice)

**题目 1 (简答题 - 考查错误恢复机制)：** In the LL(1) parsing process above, the parser encountered an error at M[F,$]. If the compiler implements a **Panic-Mode Error Recovery (恐慌模式错误恢复)** strategy, how would it typically handle this specific error state? (在上文的 LL(1) 分析过程中，分析器在 M[F,$] 处遇到了错误。如果编译器实现了**恐慌模式错误恢复**策略，它通常会如何处理这个具体的错误状态？)

**【答案与解析】** **答案：** 在恐慌模式下，分析器**会不断丢弃输入流中的符号（即指针后移），直到遇到属于当前栈顶非终结符 F 的同步词法单元 (Synchronizing tokens) 为止。**通常，**同步词法单元被定义为 Follow(F) 集合中的元素。** 然而，在此特定情况下，**输入流已经到达了末尾符号 `$`，且 `$` 也在 Follow(F) 集合中**。因此，**分析器不会丢弃任何输入符号，而是直接将栈顶的 F 弹出 (Pop F)**，试图让分析栈下面的符号继续与 `$` 匹配，以期完成剩余的分析并报出具体的“缺少操作数”的警告。

**题目 2 (单选题 - 考查 LL(1) 表的性质)：** If a cell in an LL(1) parsing table contains multiple production rules (e.g., M[A,a]=A→α∣β), what does this indicate about the grammar? (如果一个 LL(1) 分析表中的某个单元格包含多条产生式规则，这说明该文法具有什么性质？) (A) The grammar is unambiguous. (文法是无二义性的。) (B) The grammar has left recursion. (文法存在左递归。) (C) The grammar is NOT an LL(1) grammar. (该文法不是 LL(1) 文法。) (D) The grammar requires an LR parser. (该文法需要 LR 分析器。)

**【答案与解析】** **答案：** (C) **解析：** LL(1) 文法的核心定义和严格要求就是：**构造出的预测分析表中，任何一个单元格 (Entry) 最多只能包含一条产生式 (At most one production rule)。**如果**一个格子出现了多条产生式，说明分析器在此刻面临着无法消除的冲突 (Conflict)**，即无法唯一确定接下来该用哪条规则推导。只要有冲突，它在数学上就不属于 LL(1) 文法。**(B) 是导致冲突的原因之一，但并非唯一原因（如左公因子也会导致冲突），因此 (C) 是最准确的结论。**

![image-20260625192516715](./scut%E7%BC%96%E8%AF%91%E5%8E%9F%E7%90%86%E9%A2%98%E7%9B%AE%E5%A4%8D%E4%B9%A01/image-20260625192516715-1782386721704-3.png)

### 1. 题目解析 (Question Analysis)

**题目大意：** 给定文法和输入字符串，**求字符串 `"@##()"` 的最右推导 (Rightmost derivation)**，并**找出右句型 (Right sentential form) `"@M#()"` 的句柄 (Handle) 和活前缀 (Viable prefixes)。**

**正确答案速览：**

1. **最右推导 (Rightmost Derivation):** L⇒@M(N⇒@M()⇒@M#()⇒@##()
2. **句柄 (Handle):** **`M#`** (**对应产生式 M→M#**)
3. **活前缀 (Viable Prefixes):** **ϵ (空串), `@`, `@M`, `@M#`**

这道题考察的是**自底向上语法分析 (Bottom-up Syntax Analysis)** 中的**三个绝对核心概念：最右推导、句柄和活前缀。它们是理解移进-归约分析器 (Shift-Reduce Parser) 工作原理的基础。**

### 2. 核心知识点讲解 (Core Concepts)

在自底向上的语法分析（如 LR 分析）中，编译器试图将输入字符串“反向”折叠回起始符号。这个**折叠的过程，恰好是最右推导的逆过程 (Reverse of Rightmost Derivation)。**

- **最右推导 (Rightmost Derivation / Canonical Derivation):**
	- 在推导的每一步，**总是强制选择最右边的非终结符 (the rightmost non-terminal)** 进行替换。
- **句型与右句型 (Sentential Form & Right Sentential Form):**
	- **推导过程中的任何中间字符串都是句型**。**由最右推导产生的句型**，被特称为**右句型**。
- **句柄 (Handle):**
	- **定义：** 句柄是**右句型中与某个产生式右部匹配的子串，并且把它归约（替换）为该产生式左部的非终结符后，代表了最右推导逆过程的上一步。**
	- **大白话解释：** 在**自底向上分析的当前步骤中，最先被“揪出来”并打包归约的那个子串**，就是**句柄**。一棵**语法树中最左边的一棵只有叶子节点的子树，其叶子连起来就是句柄。**
- **活前缀 (Viable Prefix):**
	- **定义：** 右句型的**一个前缀，该前缀不超过句柄的右端点。**
	- **大白话解释：** 在移进-归约分析器中，**分析栈 (Parsing stack) 里面能合法存在的字符串就是活前缀**。只要**栈里的内容加上剩余的输入有可能组合成一个合法的程序，当前栈里的内容就是“活”的。**

### 3. 详细解题步骤 (Step-by-Step Solution)

**步骤 1：求解最右推导 (Rightmost Derivation)** 目标字符串：`@##()` 文法：L→@M(N∣M→#∣M→M#∣N→) 我们**从起始符号 L 开始**，每次只替换最右边的非终结符：

1. L⇒@M(N （此时最右边的非终结符是 N）
2. ⇒@M() （用 N→) 替换 N。此时最右边且唯一的非终结符是 M）
3. ⇒@M#() （我们需要两个 `#`，所以先用 M→M# 展开）
4. ⇒@##() （最后用 M→# 将剩下的 M 替换掉）

**完整的最右推导序列：**

L⇒@M(N⇒@M()⇒@M#()⇒@##()

**步骤 2：找出右句型 `@M#()` 的句柄 (Find the Handle)** 观察我们**刚才写出的最右推导序列，寻找 `@M#()` 的位置：**

...⇒@M()⇒@M#()⇒@##()

在自底向上分析（归约）时，过程是反过来的： **`@M#()` 的下一步操作（即反向推导的上一环）是将其还原为 `@M()`**。 在这个过程中，**子串 `M#` 被归约成了非终结符 `M`（使用的是产生式 M→M#）**。 因此，**右句型 `@M#()` 的句柄是 `M#`。**

**步骤 3：找出活前缀 (Find Viable Prefixes)** 根据定义，活前缀是右句型的前缀，且**不能越过句柄的右侧**。

- **当前右句型：`@M#()`**
- 当前句柄：`M#` 句柄的右端点在 `#` 字符处。因此，**该右句型从开头直到 `#` 的所有前缀都是活前缀：**

1. ϵ (空串)
2. `@`
3. `@M`
4. `@M#`

*(注：在一些严格的考试中，需要包含空串 ϵ。**分析栈初始为空时，栈内即为 ϵ**。)*

### 4. 考试风格练习题 (Exam-Style Practice)

**题目 1 (简答题 - 考查在长句型中定位句柄)：** Given the grammar (给定文法): S→aABe A→Abc∣b B→d Consider the right sentential form (考虑右句型): `aAbcde` What is the handle of this right sentential form? (这个右句型的句柄是什么？)

**【答案与解析】** **答案：** `Abc` **解析：** 我们**写出产生该字符串的最右推导： S⇒aABe⇒aAde (替换了最右的 B) ⇒aAbcde (替换了最右的 A)**。 在这个序列中，`aAbcde` 的**上一步是 `aAde`。在这个倒退过程中，子串 `Abc` 被归约成了非终结符 `A`。所以 `Abc` 就是当前“最应该被打包”的句柄**。千万**不要错误地认为是 `b`，因为这里的 `b` 已经和 `Ac` 结合成了更高级的结构。**

**题目 2 (判断题 - 考查活前缀的本质意义)：** In a shift-reduce parser, any viable prefix of a right sentential form can correctly appear on the parsing stack. (在移进-归约分析器中，**右句型的任何活前缀都可以合法地出现在分析栈上**。) (True / False)

**【答案与解析】** **答案：** True (正确) **解析：** 这是活前缀的核心理论意义。活前缀的定义保证了分析器在读取输入并将其压入栈（移进）或进行打包（归约）的过程中，只要栈里的内容是活前缀，分析器就没有走入死胡同，仍然有希望最终归约为起始符号 S。如果栈里的内容不是活前缀了，说明已经发生了语法错误。

![image-20260625192505392](./scut%E7%BC%96%E8%AF%91%E5%8E%9F%E7%90%86%E9%A2%98%E7%9B%AE%E5%A4%8D%E4%B9%A01/image-20260625192505392.png)

### 1. 题目解析 (Question Analysis)

**题目大意：** 给定一个表达式文法，要求：

1. 构造它的 **LR(0) 自动机 (LR(0) automaton)。**
2. 判断在语法分析过程中，**执行移进 (Shift) 或归约 (Reduction) 动作时是否存在冲突 (Conflicts)。**

这道题考察的是**自底向上语法分析 (Bottom-up Parsing)** 的核心：如何通过计算项目集规范族 (Canonical Collection of Item Sets) 来构建 LR(0) 状态机，以及如何识别 LR(0) 分析表的固有缺陷——移进-归约冲突。

### 2. 详细解题步骤 (Detailed Solution)

#### 第一步：构造增广文法 (Augmented Grammar)

为了明确定义分析的接受状态，我们**引入新的起始符号 $R'$：**

(0) $R' \rightarrow R$

(1) $R \rightarrow R < S$

(2) $R \rightarrow S$

(3) $S \rightarrow S == P$

(4) $S \rightarrow P$

(5) $P \rightarrow (R)$

(6) $P \rightarrow num$

#### 第二步：计算 LR(0) 项目集规范族 (Construct LR(0) Automaton States)

我们需要**利用闭包函数 (Closure) 和转移函数 (Goto) 推导所有的状态 (States)**。

- **状态 $I_0$ (初始状态):**

	**从 $R' \rightarrow \cdot R$ 开始求闭包：**

	$R' \rightarrow \cdot R$

	$R \rightarrow \cdot R < S$

	$R \rightarrow \cdot S$

	$S \rightarrow \cdot S == P$

	$S \rightarrow \cdot P$

	$P \rightarrow \cdot (R)$

	$P \rightarrow \cdot num$

- **状态 $I_1$ = Goto($I_0$, $R$):**

	$R' \rightarrow R \cdot$

	$R \rightarrow R \cdot < S$

- **状态 $I_2$ = Goto($I_0$, $S$):**

	$R \rightarrow S \cdot$

	$S \rightarrow S \cdot == P$

- **状态 $I_3$ = Goto($I_0$, $P$):**

	$S \rightarrow P \cdot$

- **状态 $I_4$ = Goto($I_0$, `(`):**

	$P \rightarrow ( \cdot R )$

	*(求闭包展开 $R$)*

	$R \rightarrow \cdot R < S$

	$R \rightarrow \cdot S$

	$S \rightarrow \cdot S == P$

	$S \rightarrow \cdot P$

	$P \rightarrow \cdot (R)$

	$P \rightarrow \cdot num$

- **状态 $I_5$ = Goto($I_0$, `num`):**

	$P \rightarrow num \cdot$

- **状态 $I_6$ = Goto($I_1$, `<`):**

	$R \rightarrow R < \cdot S$

	*(**求闭包展开 $S$**)*

	$S \rightarrow \cdot S == P$

	$S \rightarrow \cdot P$

	$P \rightarrow \cdot (R)$

	$P \rightarrow \cdot num$

- **状态 $I_7$ = Goto($I_2$, `==`):**

	$S \rightarrow S == \cdot P$

	*(求闭包展开 $P$)*

	$P \rightarrow \cdot (R)$

	$P \rightarrow \cdot num$

- **状态 $I_8$ = Goto($I_4$, $R$):**

	$P \rightarrow ( R \cdot )$

	$R \rightarrow R \cdot < S$

- **状态 $I_9$ = Goto($I_6$, $S$):**

	$R \rightarrow R < S \cdot$

	$S \rightarrow S \cdot == P$

- **状态 $I_{10}$ = Goto($I_7$, $P$):**

	$S \rightarrow S == P \cdot$

- **状态 $I_{11}$ = Goto($I_8$, `)`):**

	$P \rightarrow (R) \cdot$

*(注：其他转移如 Goto($I_4$, $S$) 去往 $I_2$，Goto($I_6$, $P$) 去往 $I_3$，Goto($I_7$, `(`) 去往 $I_4$ 等**均复用已有状态**，自动机构造完毕。)*

#### 第三步：冲突分析 (Conflict Analysis)

LR(0) 分析的核心原则是：**如果一个状态中存在一个归约项目（即圆点 `.` 在最后的项目）**，那么**不论遇到什么输入字符，分析器都会进行归约**。因此，我们要检查是否存在以下冲突：

1. **移进-归约冲突 (Shift-Reduce Conflict):** 状态中**同时存在移进项目和归约项目。**
2. **归约-归约冲突 (Reduce-Reduce Conflict):** 状态中**同时存在多个不同的归约项目。**

检查我们构造出的状态集：

- **观察状态 $I_2$:**

	**包含归约项目：$R \rightarrow S \cdot$**

	**包含移进项目：$S \rightarrow S \cdot == P$ (等待移进 `==`)**

	**结论：存在移进-归约冲突 (Shift-Reduce Conflict)。** 分析器**不知道是该把 $S$ 归约为 $R$，还是应该把 `==` 移进栈中。**

- **观察状态 $I_9$:**

	包含归约项目：$R \rightarrow R < S \cdot$

	包含移进项目：$S \rightarrow S \cdot == P$ (等待移进 `==`)

	**结论：存在移进-归约冲突 (Shift-Reduce Conflict)。**

- **观察状态 $I_1$:**

	包含**归约**项目：$R' \rightarrow R \cdot$ (**接受状态**)

	包含移进项目：$R \rightarrow R \cdot < S$

	*(注：在严格的理论 LR(0) 定义中，这也是一个移进-归约冲突。但在实践中，![image-20260625213125881](./scut%E7%BC%96%E8%AF%91%E5%8E%9F%E7%90%86%E9%A2%98%E7%9B%AE%E5%A4%8D%E4%B9%A01/image-20260625213125881.png)，所以主要关注 $I_2$ 和 $I_9$ 即可。)*

**最终答题结论：**

Yes, there are Shift/Reduce conflicts in this grammar when using an LR(0) parser, specifically in states $I_2$ and $I_9$. (是的，在使用 LR(0) 分析器时存在移进-归约冲突，具体发生在状态 $I_2$ 和 $I_9$ 中。)

### 3. 考试风格练习题 (Exam-Style Practice)

**题目 1 (单选题 - 考查 LR(0) 缺陷的解决思路)：**

We identified a Shift-Reduce conflict in state $I_2$ $\{ R \rightarrow S \cdot, S \rightarrow S \cdot == P \}$ for the LR(0) automaton. If we upgrade our parser to an **SLR(1)** parser, how does it attempt to resolve this specific conflict? (我们在 LR(0) 自动机的状态 $I_2$ 中发现了**移进-归约冲突**。如果我们将分析器升级为 **SLR(1)** 分析器，它是如何尝试解决这个特定冲突的？)

(A) It looks at the FIRST set of $S$ to decide. (它通过查看 $S$ 的 FIRST 集来决定。)

(B) It performs reduction $R \rightarrow S$ ONLY IF the next lookahead token is in the FOLLOW set of $R$. (只有当下一个向前看符号属于 $R$ 的 FOLLOW 集时，它才执行归约 $R \rightarrow S$。)

(C) It merges state $I_2$ with another state. (它将状态 $I_2$ 与另一个状态合并。)

(D) SLR(1) cannot resolve Shift-Reduce conflicts; only LALR(1) can. (SLR(1) 无法解决移进-归约冲突；只有 LALR(1) 可以。)

**【答案与解析】**

**答案：** (B)

**解析：** 这正是 SLR(1) (Simple LR) 发明的原因。LR(0) 过于莽撞，**只要看到归约项目就想归约**。SLR(1) **增加了一层理智：它会求出左部非终结符（这里是 $R$）的 FOLLOW 集合。**在状态 $I_2$ 中，**当且仅当下一个输入的符号在 $Follow(R)$ 中时，它才允许进行归约**；如果**下一个输入符号是 `==`，它就执行移进。这就成功消除了冲突。**

**题目 2 (简答题 - 考查归约-归约冲突的识别)：**

Suppose a state in an LR(0) automaton contains the following items:

$A \rightarrow \alpha \cdot$

$B \rightarrow \alpha \cdot$

$C \rightarrow \cdot \gamma$

What type of conflict(s) exist in this state? (假设 LR(0) 自动机中的某个状态包含上述项目，该状态存在什么类型的冲突？)

**【答案与解析】**

**答案：** 该状态存在 **Reduce-Reduce conflict (归约-归约冲突)**。

**解析：** $A \rightarrow \alpha \cdot$ 和 $B \rightarrow \alpha \cdot$ 都是圆点在最后的归约项目。当栈顶出现了 $\alpha$ 时，LR(0) 分析器无法判断究竟是应该把它打包（归约）成 $A$ 还是打包成 $B$。*(注意：此状态没有移进-归约冲突，因为 $C \rightarrow \cdot \gamma$ 前面没有符号被处理，它属于别的路径的起点，当前并不会移进 `\gamma` 的首字符来与 $\alpha$ 竞争)*。

![image-20260625193202115](./scut%E7%BC%96%E8%AF%91%E5%8E%9F%E7%90%86%E9%A2%98%E7%9B%AE%E5%A4%8D%E4%B9%A01/image-20260625193202115.png)

![image-20260625193223082](./scut%E7%BC%96%E8%AF%91%E5%8E%9F%E7%90%86%E9%A2%98%E7%9B%AE%E5%A4%8D%E4%B9%A01/image-20260625193223082.png)

这道题目是编译原理中非常核心的**语法制导翻译 (Syntax-Directed Translation, SDT)** 和**中间代码生成 (Intermediate Code Generation)** 的综合大题。它考察了你如何**通过属性文法 (Attribute Grammar) 将高级语言的控制流 (Control Flow) 转化为底层机器容易理解的三地址码 (Three-Address Code, TAC)。**

在正式解题前，我们需要先明确一个**非常关键的细节**：原卷打印的输入字符串 `if x + 1 && y + 2 then z = x + 3` 在**语法上是非法的**（因为 `x + 1` 无法推导为布尔表达式 B）。但是，**图片上的手写笔记已经对此进行了修正**。 根据手写修正，我们将要处理的正确输入字符串为： **`if x + 1 == y + 3 && y + 3 == z + 1 then z = x + 3`**

下面我将为你详细拆解这道题的三个部分，并讲解背后的考点。

### 1. 第一问：绘制语法树 (Draw the parse tree)

**【解题逻辑】** **语法树 (Parse Tree)** 是**严格按照给定的上下文无关文法推导出来的层次结构**。我们需要将**修正后的字符串通过文法规则自顶向下地拆解。**

- **根节点： S→if B then S1**
- **布尔表达式：** 包含了 `&&` 操作，使用 B→B1 && B2。
- **关系表达式： 左右两侧分别使用 B→E1==E2。**
- **算术表达式： 使用 E→id+num。**
- **赋值语句：** `then` 后面的部分使用 S→id=E。

**【标准答案：语法树】**

Plaintext

```
                        S
                      / | \
                    /   |   \
                  /     |     \
               if       B       then       S (S1)
                      / | \               / | \
                    /   |   \           id(=z) =  E
                  /     &&    \                   |
               B(B1)          B(B2)             id(=x) + num(=3)
               / | \          / | \
             /   |   \      /   |   \
           /     ==    \  /     ==    \
         E               E              E
         |               |              |
 id(=x) + num(=1)  id(=y) + num(=3)  id(=z) + num(=1)
```

### 2. 第二问：基于继承属性的依赖图 (Dependency graph for inherited attributes)

**【解题核心知识点】**

- **继承属性 (Inherited Attributes)：** 信息在语法树中**自顶向下 (Top-down)** 或横向传递。在本题中，`true` (真出口标签)、`false` (假出口标签) 和 `next` (下一条语句标签) 都是继承属性。它们由父节点（如 `if` 语句）决定，并传递给子节点（布尔表达式），告诉子节点：“如果你计算为真，跳到哪里；如果为假，跳到哪里”。
- **短路求值 (Short-circuit Evaluation)：** 对于 `B1 && B2`，**如果 `B1` 为假，整个表达式直接为假，不需要计算 `B2`（所以 `B1.false = B.false`）。如果 `B1` 为真，必须继续计算 `B2`（所以 `B1.true = newlabel()`，指向 `B2` 的开头）。**

**【解题步骤与答案】** 为了直观，我们**假设函数 `newlabel()` 依次生成标签 `L1`, `L2`, `L3`。 根据语义规则 (Semantic Rules)，我们从根节点开始向下推导属性值：**

1. **对于根节点 S：**
	- `S.next = L1` (**通过 `newlabel()` 生成，代表整个 if 语句结束后的去向**)
	- `B.true = L2` (通过 `newlabel()` 生成，代表条件为真时，进入 `then` 块的标签)
	- **`B.false = S.next = L1` (条件为假时，直接跳到 if 语句结束)**
2. **对于节点 B→B1 && B2：** 此时**已知父节点传下来的 `B.true = L2`, `B.false = L1`。**
	- `B1.true = L3` (通过 `newlabel()` 生成，**B1 为真时，跳去执行 B2**)
	- `B2.true = B.true = L2` (B2 为真时，代表整个 && 为真，跳去 then 块)
	- **`B1.false = B.false = L1` (B1 为假，直接跳到 if 结束)**
	- **`B2.false = B.false = L1` (B2 为假，直接跳到 if 结束)**

**【标注后的注释语法树 (Annotated Parse Tree)】** *(在考场上，你需要画出语法树，并用箭头标注这些属性的流向。这里用文本形式清晰列出每个节点的继承属性)*

- **Node S:** `S.next = L1`
	- **Node B (Root of condition):** `B.true = L2`, `B.false = L1`
		- **Node B1 (`x+1 == y+3`):** `B1.true = L3`, `B1.false = L1`
		- **Node B2 (`y+3 == z+1`):** `B2.true = L2`, `B2.false = L1`
	- **Node S1 (`z = x+3`):** (虽然此节点无继承属性，但它的代码会被放置在 `label L2` 之后)

### 3. 第三问：翻译为三地址码 (Translate into Three-Address Code)

**【解题核心知识点】**

- **综合属性 (Synthesized Attributes)：** 信息**自底向上 (Bottom-up)** 传递。**`code` (生成的代码) 和 `name` (临时变量名) 都是综合属性**。**子节点生成好代码后，拼接起来交给父节点。**
- 根据题目约定：`||` 表示换行拼接，`++` 表示空格拼接。

**【自底向上的翻译过程 (Bottom-up Translation)】** **假设 `newTemp()` 依次生成临时变量 `t1`, `t2`, `t3`...**

1. **处理底层的算术表达式 E：**

	- E1 (`x+1`): `E1.name = t1`, `E1.code = "t1 = x + 1"`
	- E2 (`y+3`): `E2.name = t2`, `E2.code = "t2 = y + 3"`
	- E3 (`y+3`): `E3.name = t3`, `E3.code = "t3 = y + 3"`
	- E4 (`z+1`): `E4.name = t4`, `E4.code = "t4 = z + 1"`
	- E5 (`x+3`): `E5.name = t5`, `E5.code = "t5 = x + 3"`

2. **处理关系表达式 B (生成控制流跳转)：** 根据**规则 `B.code = E1.code || E2.code || 'if' E1.name '==' E2.name 'goto' B.true || 'goto' B.false`**

	- **B1 代码:**

		Plaintext

		```
		t1 = x + 1
		t2 = y + 3
		if t1 == t2 goto L3
		goto L1
		```

	- **B2 代码:**

		Plaintext

		```
		t3 = y + 3
		t4 = z + 1
		if t3 == t4 goto L2
		goto L1
		```

3. **处理赋值语句 S1：** 根据规则 `S.code = E.code || id.strval '=' E.name`

	- **S1 代码:**

		Plaintext

		```
		t5 = x + 3
		z = t5
		```

4. **顶级组装 (Top-level Assembly)：**

	- **B 的完整代码 (B→B1 && B2)： 规则：`B.code = B1.code || 'label' B1.true || B2.code`**
	- **S 的完整代码 (S→if B then S1)： 规则 (含手写修正)：`S.code = B.code || 'label' B.true || S1.code || 'label' S.next`**

**【标准答案：三地址码 (Three Address Code)】** 将上述步骤按严格的拼接规则组合，得到最终的 TAC 序列：

Plaintext

```
t1 = x + 1
t2 = y + 3
if t1 == t2 goto L3
goto L1
label L3
t3 = y + 3
t4 = z + 1
if t3 == t4 goto L2
goto L1
label L2
t5 = x + 3
z = t5
label L1
```

*(注意：考试时一定要保证临时变量 `t1, t2...` 和标签 `L1, L2...` 的分配逻辑与你第二问中声明的一致，并且体现出清晰的换行。)*

### 4. 考试风格练习题 (Exam-Style Practice)

为了确保你真正掌握了 SDT 在控制流翻译中的核心机制，请尝试根据上述**完全相同的语义规则**，完成以下小题：

**题目 (简答题 - 考查短路求值与标签分配的对应)：** Translate the following simplified string into Three-Address Code using the semantic rules provided in the original question (使用原题中提供的语义规则，将以下简化字符串翻译为三地址码): `if a + 1 == b + 2 then c = c + 1`

*(提示：这个字符串没有 `&&` 运算符，因此直接使用 S→if B then S1 和 B→E1==E2 规则)*

**【答案与解析】** **答案：**

Plaintext

```
t1 = a + 1
t2 = b + 2
if t1 == t2 goto L2
goto L1
label L2
t3 = c + 1
c = t3
label L1
```

**解析：**

1. 根节点 S **分配标签**：`S.next = L1`, `B.true = L2`, 且 `B.false = S.next = L1`。
2. 表达式 E1 为 `a+1`，生成 `t1 = a + 1`。
3. 表达式 E2 为 `b+2`，生成 `t2 = b + 2`。
4. 布尔表达式 B (`t1 == t2`)：如果为真跳到 `B.true` (即 `L2`)，为假跳到 `B.false` (即 `L1`)。
5. **拼接出 `if t1 == t2 goto L2` 和 `goto L1`。**
6. **放置 `label L2` 作为 `then` 块的入口**。
7. 计算 S1 (`c = c + 1`)：生成 `t3 = c + 1` 和 `c = t3`。
8. **最后放置 `label L1` 作为整个 if 语句的结束标志。**

不求select集是不是也能做,只求follow和first,你为什么要求select集,这是干什么的