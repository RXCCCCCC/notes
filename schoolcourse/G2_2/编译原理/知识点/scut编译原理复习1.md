### 第一章：编译器概述（Introduction）

|                 英文                 |        中文         |                         说明                          |
| :----------------------------------: | :-----------------: | :---------------------------------------------------: |
|             **Compiler**             |       编译器        |            输入源程序，输出目标程序的程序             |
|           **Interpreter**            |       解释器        |             边翻译边执行，不生成目标代码              |
|   **Source program / Source code**   |   源程序 / 源代码   |                高级语言编写的输入程序                 |
|   **Target program / Target code**   | 目标程序 / 目标代码 |               编译器输出的低级语言程序                |
|         **Source language**          |       源语言        |                    输入的高级语言                     |
|         **Target language**          |      目标语言       |           输出的低级语言（如机器码、汇编）            |
|      **Phases of compilation**       |      编译阶段       |                 编译器的各个工作阶段                  |
|         **Lexical analysis**         |    **词法**分析     |            **第一阶段，字符流 → Token 流**            |
|    **Syntax analysis / Parsing**     |    **语法**分析     |              第二阶段，Token 流 → 语法树              |
|        **Semantic analysis**         |    **语义**分析     |             第三阶段，语法树 → 注释语法树             |
|   **Intermediate code generation**   |    中间代码生成     |                第四阶段，生成中间表示                 |
|  **Intermediate code optimization**  |    中间代码优化     |                第五阶段，优化中间代码                 |
|      **Target code generation**      |    目标代码生成     |                第六阶段，生成目标代码                 |
|     **Target code optimization**     |    目标代码优化     |                第七阶段，优化目标代码                 |
|            **Front end**             |        前端         | 与机器无关的阶段（词法 + 语法 + 语义 + 中间代码生成） |
|             **Back end**             |        后端         |        与机器相关的阶段（优化 + 目标代码生成）        |
| **Intermediate representation (IR)** |      中间表示       |             前后端之间的桥梁，如三地址码              |
|           **Symbol table**           |       符号表        |             存储标识符及其属性的数据结构              |
|          **Error handler**           |     错误处理器      |                 处理各阶段错误的模块                  |
|           **Preprocessor**           |    **预处理器**     |                编译前处理宏、头文件等                 |
|            **Assembler**             |     **汇编器**      |                将汇编代码翻译成机器码                 |
|       **Linker / Link editor**       |       链接器        |            将多个目标文件链接成可执行文件             |
|              **Loader**              |       加载器        |               将可执行文件载入内存执行                |
|     **Relocatable machine code**     |   可重定位机器码    |                地址可重定位的目标代码                 |
| **Machine-independent optimization** |   与机器无关优化    |                在中间代码上进行的优化                 |
|  **Machine-dependent optimization**  |   与机器相关优化    |                在目标代码上进行的优化                 |

------

### 第二章：词法分析（Lexical Analysis）

表格







|                    英文                     |          中文           |                   说明                   |
| :-----------------------------------------: | :---------------------: | :--------------------------------------: |
|       **Lexical analyzer / Scanner**        | **词法分析器 / 扫描器** |            执行词法分析的程序            |
|                  **Token**                  |   **记号 / 词法单元**   |       词法分析的输出，如 <id, "x">       |
|                 **Pattern**                 |          模式           |             Token 的描述规则             |
|                 **Lexeme**                  |        **词素**         |       源程序中匹配模式的具体字符串       |
|                **Attribute**                |        **属性**         | Token 的附加信息（如标识符名字、常量值） |
|         **Regular expression (RE)**         |     **正则表达式**      |         描述正则语言的形式化工具         |
|            **Regular language**             |        正则语言         |    正则表达式 / 有限自动机描述的语言     |
|                **Alphabet**                 |       **字母表**        |              符号的有限集合              |
|                 **String**                  |           串            |          字母表中符号的有限序列          |
|            **Empty string (ε)**             |          空串           |              长度为 0 的串               |
|                  **Union**                  |           并            |           正则运算之一，r \| s           |
|              **Concatenation**              |          连接           |             正则运算之一，rs             |
|      **Kleene closure / Star closure**      |  **克林闭包 / 星闭包**  |             正则运算之一，r*             |
|            **Positive closure**             |       **正闭包**        |                 r⁺ = rr*                 |
|               **Precedence**                |         优先级          |             闭包 > 连接 > 并             |
|          **Algebraic properties**           |      **代数性质**       |           正则表达式的运算定律           |
|            **Finite automaton**             |     **有限自动机**      |           识别正则语言的自动机           |
|  **Deterministic finite automaton (DFA)**   |     确定有限自动机      |      每个状态对每个输入只有一个转移      |
| **Nondeterministic finite automaton (NFA)** |    非确定有限自动机     |          允许 ε 转移和多个转移           |
|                  **State**                  |          状态           |               自动机的节点               |
|       **Start state / Initial state**       | **开始状态 / 初始状态** |             自动机的起始状态             |
|       **Accept state / Final state**        |   **接受状态 / 终态**   |              识别成功的状态              |
|           **Transition function**           |      **转移函数**       |        δ: 状态 × 输入 → 状态集合         |
|        **State transition diagram**         |     **状态转移图**      |             自动机的图形表示             |
|              **ε-transition**               |       **ε 转移**        |            不读输入符号的转移            |
|         **Thompson's construction**         |   **Thompson 构造法**   |         正则表达式 → NFA 的算法          |
|           **Subset construction**           |       子集构造法        |             NFA → DFA 的算法             |
|                **ε-closure**                |         ε 闭包          |    经 0 条或多条 ε 边能到达的所有状态    |
|                  **Move**                   |          转移           |   **从状态集合经一条输入边到达的状态**   |
|            **DFA minimization**             |     DFA **最小化**      |          减少 DFA 状态数的过程           |
|          **Hopcroft's algorithm**           |      Hopcroft 算法      |            DFA 最小化算法之一            |
|         **Table-filling algorithm**         |         填表法          |            DFA 最小化算法之一            |
|            **Equivalent states**            |        等价状态         |        对任意输入串行为相同的状态        |
|         **Distinguishable states**          |     **可区分**状态      |        存在输入串使行为不同的状态        |
|                **Partition**                |          划分           |              状态集合的分组              |
|      **Maximal munch / Longest match**      |    **最长匹配原则**     |        **多个模式匹配时选最长的**        |
|               **Lex / Flex**                |     Lex / Flex 工具     |            词法分析器生成工具            |

------

### 第三章：上下文无关文法（Context-Free Grammar）

表格







|                      英文                       |          中文           |               说明               |
| :---------------------------------------------: | :---------------------: | :------------------------------: |
|         **Context-free grammar (CFG)**          |     上下文无关文法      |   描述上下文无关语言的形式文法   |
|      **Nonterminal / Non-terminal symbol**      |      **非终结符**       |     语法变量，用大写字母表示     |
|         **Terminal / Terminal symbol**          |         终结符          |  语言的基本符号，用小写字母表示  |
|        **Production / Production rule**         | **产生式 / 产生式规则** |     **A → α 形式的重写规则**     |
|                **Start symbol**                 |        开始符号         |        文法的初始非终结符        |
|           **BNF (Backus-Naur Form)**            |       巴科斯范式        |          文法的表示方法          |
|                 **Derivation**                  |        **推导**         |   **用产生式重写符号串的过程**   |
|              **Direct derivation**              |      **直接推导**       |         **一步推导，⇒**          |
|        **Derives in zero or more steps**        |     0 步或多步推导      |                ⇒*                |
|        **Derives in one or more steps**         |     1 步或多步推导      |                ⇒⁺                |
|               **Sentential form**               |        **句型**         |   **从开始符号推导出的任意串**   |
|                  **Sentence**                   |          句子           |       全由终结符组成的句型       |
|            **Language of a grammar**            |     **文法的语言**      |     **所有句子的集合 L (G)**     |
|             **Leftmost derivation**             |      **最左推导**       |   **每步替换最左边的非终结符**   |
| **Rightmost derivation / Canonical derivation** | **最右推导 / 规范推导** |   **每步替换最右边的非终结符**   |
|   **Parse tree / Parsing tree / Syntax tree**   | **语法分析树 / 语法树** |          推导的图形表示          |
|         **Abstract syntax tree (AST)**          |     **抽象语法树**      | **压缩的语法树，只保留语义信息** |
|              **Yield / Frontier**               |     产出 / 叶子序列     |     语法树叶子从左到右的排列     |
|                **Interior node**                |        内部节点         |        语法树中非叶子节点        |
|                  **Leaf node**                  |        叶子节点         |        语法树的终结符节点        |
|              **Ambiguous grammar**              |       二义性文法        |   存在句子对应多棵语法树的文法   |
|                  **Ambiguity**                  |         二义性          |      同一句子有多种语法结构      |
|                 **Precedence**                  |         优先级          |        运算符的运算优先级        |
|                **Associativity**                |         结合性          |         左结合 / 右结合          |
|                **Dangling else**                |        悬空 else        |     if-else 语句的二义性问题     |
|               **Left recursion**                |         左递归          |    产生式左部出现在右部最左边    |
|            **Direct left recursion**            |     **直接**左递归      |           A → Aα 形式            |
|           **Indirect left recursion**           |     **间接左递归**      |    **经多步推导后出现左递归**    |
|               **Left factoring**                |     **左因子提取**      |    **消除公共前缀的文法改写**    |
|                **Common prefix**                |      **公共前缀**       |    **多个产生式右部开头相同**    |
|            **Context-free language**            |   上下文无关**语言**    |          CFG 描述的语言          |

------

### 第四章：自顶向下语法分析（Top-Down Parsing）

表格







|                英文                 |        中文        |                             说明                             |
| :---------------------------------: | :----------------: | :----------------------------------------------------------: |
|        **Top-down parsing**         |  自顶向下语法分析  |                   从开始符号出发构造语法树                   |
|        **Bottom-up parsing**        |  自底向上语法分析  |                  从输入串出发归约到开始符号                  |
|    **Recursive-descent parsing**    |  **递归下降分析**  |               **每个非终结符对应一个递归过程**               |
|       **Predictive parsing**        |    **预测分析**    |                   **无回溯的自顶向下分析**                   |
|          **Backtracking**           |        回溯        |                     选错产生式时回退重试                     |
|          **LL(1) grammar**          |    LL (1) 文法     | 第一个 L：从左到右扫描；第二个 L：最左推导；1：向前看 1 个符号 |
|            **FIRST set**            |      FIRST 集      |                     **串首终结符**的集合                     |
|           **FOLLOW set**            |     FOLLOW 集      |               **非终结符后面可能出现的终结符**               |
|    **Predictive parsing table**     |   **预测分析表**   |                   **M [A, a] 对应产生式**                    |
|          **Parsing stack**          |     **分析栈**     |                   **存放待展开的文法符号**                   |
|          **Input buffer**           |     输入缓冲区     |                      存放待分析的输入串                      |
|          **Configuration**          |        格局        |                分析过程的瞬时描述（栈，输入）                |
|              **Shift**              |        移进        |                栈顶**与输入匹配，弹出并前进**                |
|             **Reduce**              |        归约        |                   用**产生式左部替换右部**                   |
|             **Accept**              |        接受        |                           分析成功                           |
|              **Error**              |        出错        |                           分析失败                           |
| **Nonrecursive predictive parsing** | **非递归预测分析** |                     **表驱动的预测分析**                     |
|      **Table-driven parsing**       |   **表驱动分析**   |                   **用分析表控制分析过程**                   |
|            **Lookahead**            |     **向前看**     |                  **预测时查看的输入符号数**                  |

------

### 第五章：自底向上语法分析（Bottom-Up Parsing）

表格







|                   英文                    |         中文          |                     说明                      |
| :---------------------------------------: | :-------------------: | :-------------------------------------------: |
|           **Bottom-up parsing**           |   自底向上语法分析    |          从输入串自下而上构造语法树           |
|                **Reduce**                 |         归约          |           将句柄替换为对应非终结符            |
|                **Handle**                 |       **句柄**        |           **右句型中可归约的子串**            |
|         **Right sentential form**         |        右句型         |             最右推导过程中的句型              |
|             **Viable prefix**             |       可行前缀        |         右句型的前缀，不超过句柄右端          |
|              **LR parsing**               |      **LR 分析**      |   **L：从左到右扫描；R：构造最右推导的逆**    |
|             **LR(0) parsing**             |    **LR (0) 分析**    |         **向前看 0 个符号的 LR 分析**         |
|            **SLR(1) parsing**             |   **SLR (1) 分析**    |    **简单 LR 分析，用 FOLLOW 集解决冲突**     |
|     **LR(1) parsing / Canonical LR**      | LR (1) 分析 / 规范 LR |         向前看 1 个符号的规范 LR 分析         |
|            **LALR(1) parsing**            |     LALR (1) 分析     |         向前看 LR 分析，合并同心项集          |
|              **LR(0) item**               |      LR (0) 项目      |          产生式右部加点表示分析进度           |
|               **Item set**                |        项目集         |              若干项目组成的集合               |
|     **Canonical collection of items**     |     项目集规范族      |               所有项目集的集合                |
|                **Closure**                |         闭包          |            展开项目集中的待约项目             |
|             **GOTO function**             |       GOTO 函数       |          项目集经过文法符号后的转移           |
|           **Augmented grammar**           |       增广文法        |             增加新开始符号的文法              |
|              **Shift item**               |     **移进项目**      |           **点后面是终结符的项目**            |
|              **Reduce item**              |     **归约项目**      |         **点在产生式右部最后的项目**          |
|              **Accept item**              |     **接受项目**      |            **开始符号的归约项目**             |
|              **Kernel item**              |     **核心项目**      |       **初始项目或点不在最左端的项目**        |
|            **Nonkernel item**             |    **非核心项目**     |          **点在最左端的非初始项目**           |
| **Shift-reduce conflict (S/R conflict)**  |    移进 - 归约冲突    |           同一状态既有移进又有归约            |
| **Reduce-reduce conflict (R/R conflict)** |    归约 - 归约冲突    |              同一状态有多个归约               |
|             **ACTION table**              |       ACTION 表       | 终结符对应的动作（移进 / 归约 / 接受 / 出错） |
|              **GOTO table**               |      **GOTO 表**      |          **非终结符对应的状态转移**           |
|             **Parsing stack**             |      **分析栈**       |         **存放状态（隐含文法符号）**          |
|                 **Shift**                 |         移进          |             读入符号，压入新状态              |
|                **Reduce**                 |         归约          |      弹出句柄对应状态，压入归约后的状态       |
|            **Handle pruning**             |     **句柄裁剪**      |              **归约时裁剪句柄**               |
|          **Conflict resolution**          |     **冲突消解**      |            **解决分析表中的冲突**             |

------

### 第六章：语义分析（Semantic Analysis）

表格







|                英文                |            中文             |                      说明                      |
| :--------------------------------: | :-------------------------: | :--------------------------------------------: |
|       **Semantic analysis**        |          语义分析           |            检查程序语义正确性的阶段            |
|       **Attribute grammar**        |          属性文法           |             带属性和语义规则的文法             |
|           **Attribute**            |            属性             |             文法符号关联的语义信息             |
|     **Synthesized attribute**      |        **综合属性**         |      **由子节点属性计算得到（自底向上）**      |
|      **Inherited attribute**       |        **继承属性**         | **由父节点和兄弟节点属性计算得到（自顶向下）** |
|      **S-attributed grammar**      |         S 属性文法          |            只使用综合属性的属性文法            |
|      **L-attributed grammar**      |         L 属性文法          |     继承属性只依赖左边符号和父节点继承属性     |
|         **Semantic rule**          |          语义规则           |               属性之间的计算规则               |
|        **Dependency graph**        |           依赖图            |            属性之间依赖关系的有向图            |
|        **Topological sort**        |          拓扑排序           |              依赖图的合法计算顺序              |
|      **Annotated parse tree**      |       **注释语法树**        |            **标注了属性值的语法树**            |
|     **Parse tree annotation**      |       **语法树标注**        |            **在语法树上标注属性值**            |
|          **Symbol table**          |           符号表            |            存储标识符信息的数据结构            |
|             **Scope**              |           作用域            |              标识符可见的程序范围              |
|         **Scope checking**         |         作用域检查          |          验证标识符使用是否在作用域内          |
|          **Nested scope**          |       **嵌套作用域**        |              **作用域的嵌套结构**              |
|      **Chained symbol table**      |       **链式符号表**        |     **用链表 / 指针实现的多作用域符号表**      |
|             **Insert**             |            插入             |            符号表操作：添加新标识符            |
|             **Lookup**             |            查找             |             符号表操作：查找标识符             |
|     **Undeclared identifier**      |        未声明标识符         |               使用了未声明的变量               |
|         **Redeclaration**          |          重复声明           |         同一作用域内重复声明同名标识符         |
|         **Type checking**          |          类型检查           |             验证操作数类型是否兼容             |
|          **Type system**           |          类型系统           |                 类型规则的集合                 |
|      **Static type checking**      |        静态类型检查         |              编译时进行的类型检查              |
|     **Dynamic type checking**      |        动态类型检查         |              运行时进行的类型检查              |
|           **Type error**           |          类型错误           |                类型不匹配等错误                |
|         **Semantic error**         |          语义错误           |            语法正确但语义错误的情况            |
|  **Type conversion / Type cast**   |          类型转换           |            一种类型转换为另一种类型            |
| **Implicit conversion / Coercion** | **隐式类型转换 / 强制转换** |          **编译器自动进行的类型转换**          |
|      **Explicit conversion**       |      **显式类型转换**       |          **程序员手动指定的类型转换**          |
|            **Widening**            |          拓宽转换           |              不丢失信息的类型转换              |
|           **Narrowing**            |          窄化转换           |             可能丢失信息的类型转换             |

------

### 第七章：中间代码生成（Intermediate Code Generation）

表格







|                 英文                  |       中文       |                 说明                 |
| :-----------------------------------: | :--------------: | :----------------------------------: |
|   **Intermediate code generation**    |   中间代码生成   |          生成中间表示的阶段          |
| **Intermediate representation (IR)**  |     中间表示     |    介于源语言和目标语言之间的表示    |
|  **Three-address code (TAC / 3AC)**   |     三地址码     |    每条指令最多三个地址的中间代码    |
|     **Temporary variable / Temp**     |     临时变量     |     编译器生成的中间结果存储变量     |
|              **Address**              |       地址       |        三地址码中的操作数位置        |
|            **Quadruples**             |    **四元式**    |  **(op, arg1, arg2, result) 表示**   |
|              **Triples**              |    **三元式**    |      **用语句编号表示结果位置**      |
|         **Indirect triples**          |    间接三元式    |         增加指针数组的三元式         |
|       **Assignment statement**        |     赋值语句     |           赋值类语句的翻译           |
|        **Boolean expression**         |    布尔表达式    |          计算真假值的表达式          |
|           **Control flow**            |      控制流      |          程序的执行流程控制          |
|     **Flow-of-control statement**     |    控制流语句    |         if、while 等控制语句         |
|     **Short-circuit evaluation**      |   **短路求值**   |     **布尔表达式的提前求值终止**     |
|           **Backpatching**            |   **回填技术**   |   **跳转地址未知时先记录，后回填**   |
|             **Truelist**              |    真出口链表    |      跳转到真出口的语句地址链表      |
|             **Falselist**             |    假出口链表    |      跳转到假出口的语句地址链表      |
|             **Makelist**              |     构造链表     |        创建只含一个地址的链表        |
|               **Merge**               |     合并链表     |           合并两个地址链表           |
|             **Backpatch**             |       回填       | 将链表中所有语句的跳转地址设为指定值 |
|           **If statement**            |     if 语句      |             条件分支语句             |
|          **While statement**          |    while 语句    |          先判断后执行的循环          |
|        **Do-while statement**         |  do-while 语句   |          先执行后判断的循环          |
|           **For statement**           |     for 语句     |             计数循环语句             |
|            **Declaration**            |       声明       |            变量声明的翻译            |
|              **Offset**               |      偏移量      |         变量相对基地址的偏移         |
|               **Width**               |       宽度       |           类型占用的字节数           |
|          **Array reference**          |     数组引用     |            数组元素的访问            |
|           **Base address**            |      基地址      |            数组的起始地址            |
|          **Row-major order**          |      行优先      |           二维数组按行存储           |
|        **Column-major order**         |      列优先      |           二维数组按列存储           |
| **Syntax-directed translation (SDT)** | **语法制导翻译** |     **语法分析同时执行语义动作**     |
|          **Semantic action**          |     语义动作     |        嵌入产生式中的翻译动作        |
| **Syntax-directed definition (SDD)**  | **语法制导定义** |       **属性文法的另一种说法**       |
|    **Abstract syntax tree (AST)**     |    抽象语法树    |            中间表示的一种            |
|   **Directed acyclic graph (DAG)**    |    有向无环图    |         表示表达式的优化形式         |
|         **Postfix notation**          |     后缀表示     |             逆波兰表示法             |
|          **Procedure call**           |     过程调用     |            函数调用的翻译            |
|             **Parameter**             |       参数       |            函数调用的参数            |
|         **Return statement**          |     返回语句     |           函数返回值的翻译           |

# ch3

![image-20260624155346787](./scut%E7%BC%96%E8%AF%91%E5%8E%9F%E7%90%86%E5%A4%8D%E4%B9%A01/image-20260624155346787.png)

词法分析（Lexical Analysis）阶段中，**非确定性有限自动机（NFA）转换为确定性有限自动机（DFA）** 的核心收尾工作，即**子集构造法（Subset Construction Algorithm）的最后两步：构建DFA的状态转换表（State Transition Table）以及绘制对应的DFA状态转换图（State Transition Diagram）。**

以下是针对该内容的专业知识点讲解及考点剖析。

### 核心知识点讲解

**1. 子集构造法与状态集合重命名 (Subset Construction & Renaming Sets of States)**

- **概念 (Concept):** 子集构造法的核心思想**是将 NFA 中的一组状态（通常是通过 $\epsilon$-闭包 计算得出的状态集合）映射为 DFA 中的一个单一状态。**
- **重命名 (Renaming):** 在计算出**所有的 NFA 状态集合后，为了简化表示并构建最终的 DFA，需要对这些集合进行重命名**（例如将集合 $\{0, 1, 7, 2, 4\}$ **重命名为 $T_0$ 或直接命名为状态 $0$**）。材料中的表格即展示了这一过程：$T_0$ 到 $T_4$ **分别代表了五个不同的 NFA 状态集合。**

**2. DFA 状态转换表 (DFA State Transition Table)**

表格详细**定义了 DFA 的转移函数（Transition Function, $\delta$）：**

- **$I$ 列:** 当前 **DFA 状态（Current State），对应一个 NFA 状态集合。**
- **$I_a, I_b$ 列:** 分别表示**当前状态接收到输入字符（Input Symbol） `a` 或 `b` 时，转移到的目标 NFA 状态集合及其对应的 DFA 状态别名**。例如**，$T_0$ 接收 `a` 到达集合 $\{8, 3, 6, 7, 1, 2, 4\}$，该集合被命名为 $T_1$。**
- **最右侧列 (0/1 指示位):** 表示该状态是否为**接受状态 / 终态 (Accepting State / Final State)**。
	- **判定规则:** 如果**一个 DFA 状态所代表的 NFA 状态集合中，包含了原 NFA 的接受状态，那么该 DFA 状态就是接受状态。**
	- 在此表中，$T_4$（对应集合 $\{10, 5, 6, 7, 1, 2, 4\}$）的指示位为 `1`，说明原 NFA 的接受状态（推测为状态 `10`）包含在该集合内，因此 $T_4$ 是唯一的接受状态。

**3. DFA 状态转换图 (DFA State Transition Diagram)**

这是表格的**可视化等价物，考试中通常要求根据表格绘制此图：**

- **节点 (Nodes):** 圆圈**代表 DFA 的状态。图中将 $T_0 \dots T_4$ 进一步简写为 $0 \dots 4$。**
- **有向边 (Directed Edges):** 带有**字符标签的箭头代表状态转换**。例如，从节点 `0` 引出一条标有 `a` 的边指向节点 `1`，这严格对应表格中的 $\delta(T_0, a) = T_1$。
- **起始状态 (Start/Initial State):** 节点 `0` 左侧有一个无起点的单向箭头，标明这是 **DFA 的入口。**
- **接受状态 (Accepting State):** 节点 `4` 是**双层圆圈（Double Circle），严格对应表格中最右侧标为 `1` 的状态。**

### 考查深度与解题思路 (Exam Focus & Strategy)

在编译原理考试中，这部分内容的常见考法和易错点如下：

1. **完整执行子集构造法:** 题目通常会给出一个带有 $\epsilon$-转移的 NFA，要求你**从头计算 $\epsilon$-closure 和 move 函数，填出类似材料中的表格。**
	- *思路:* 必须确保**没有遗漏任何状态的闭包，特别是初始状态的 $\epsilon$-closure。**
2. **准确标记终态:** 很多学生在画图时忘记标记**双层圆圈**，或者标错了终态。
	- *思路:* 检查每个生成的集合，只要**集合中“混入”了 NFA 的终态**，**这个集合在 DFA 中就必须画成双圆圈。**
3. **状态合并与化简 (DFA Minimization):** 生成此图后，可能会**进一步要求使用划分法 (Hopcroft's Algorithm) 对这个 DFA 进行最小化。**

### 实战演练 (Exam-Style Practice)

**Question 1 (Theory & Core Concept)**

**Problem Statement:**

During the lexical analysis phase, a compiler uses the Subset Construction algorithm to convert an NFA into a DFA. **Suppose the original NFA has a set of states $Q_{NFA}$ and its designated final state is $q_f = 9$.** After applying the algorithm, we obtained three DFA states (which are subsets of $Q_{NFA}$):

$S_0 = \{0, 1, 2, 4\}$

$S_1 = \{2, 3, 5, 8\}$

$S_2 = \{4, 5, 7, 9\}$

1. Which of the above DFA states should be marked as an Accepting State (Final State), and why?
2. If the transition function gives $\text{move}(S_0, 'x') = \{3, 5\}$ and the $\epsilon\text{-closure}(\{3, 5\}) = \{2, 3, 5, 8\}$, what is the destination state of the transition $\delta(S_0, 'x')$ in the resulting DFA?

**Answer:**

1. **状态 $S_2$ 应当被标记为接受状态（终态）。**

	*原因：* 根据子集构造法的规则，**如果一个 DFA 状态（即 NFA 状态的子集）包含了原 NFA 的接受状态，那么该 DFA 状态就是接受状态**。已知**原 NFA 的终态为 $q_f = 9$，在 $S_0, S_1, S_2$ 中，只有 $S_2$ 包含了状态 $9$。**

2. **目标状态为 $S_1$。**

	*原因：* DFA 的**状态转移定义为 $\delta(S, a) = \epsilon\text{-closure}(\text{move}(S, a))$**。根据题意计算**得出结果集合为 $\{2, 3, 5, 8\}$，该集合在 DFA 的状态集合中被命名为 $S_1$。**

### 可视化逻辑结构 (Visualization of the Graph Structure)

为了确保理解无误，以下是材料中 DFA 图的纯文本结构化梳理，你可以用它来校验自己画图时是否漏边：

Plaintext

```
[DFA Graph Structure]
Start State: 0
Accepting State(s): 4 (Double Circle)

Transitions (Edges):
State 0: 
  --('a')--> State 1
  --('b')--> State 2
State 1: 
  --('a')--> State 1 (Self-loop)
  --('b')--> State 3
State 2: 
  --('a')--> State 1
  --('b')--> State 2 (Self-loop)
State 3: 
  --('a')--> State 1
  --('b')--> State 4
State 4: 
  --('a')--> State 1
  --('b')--> State 2
```

这是一个非常典型且关键的概念问题。这里的数字分属于两个完全不同的**状态空间 (State Spaces)**：**原 NFA 的状态**与**变换后的 DFA 状态**。

### 核心知识点解析

1. **集合内的数字（如 5, 6, 7, 8, 9, 10）：**

	它们代表的是**原非确定性有限自动机 (Original NFA)** 的状态标号。**在执行子集构造法 (Subset Construction) 的过程中，原 NFA 的多个状态会因为 $\epsilon$-闭包 ($\epsilon\text{-closure}$) 或字符转移映射（$\text{move}$ 函数）被组合在一起，构成一个状态子集 (Subset of States)。**从表格第一列可以看出，原 NFA 实际上至少包含了 0 到 10 这么多状态。

2. **图中的数字（0, 1, 2, 3, 4）：**

	它们代表的是**新生成的确定性有限自动机 (DFA)** 的状态标号。**为了简化状态转换表的表示并方便绘制状态转换图，算法对生成的 NFA 状态子集进行了重命名 (Renaming)**，建立了从“子集”到“新状态标号”的映射关系：

	- 表格中的 $T_0$ 被重命名为图中的**状态 `0`，它等价于 NFA 状态集合 $\{0, 1, 7, 2, 4\}$。**
	- 表格中的 $T_1$ 被重命名为图中的**状态 `1`，它等价于 NFA 状态集合 $\{8, 3, 6, 7, 1, 2, 4\}$。**
	- 依此类推，$T_2 \rightarrow 2$，$T_3 \rightarrow 3$，$T_4 \rightarrow 4$。

**结论：** 状态 5, 6, 7, 8 并没有消失，它们**作为原 NFA 的内部状态**，**已经被合并并封装在了 DFA 的状态 `1`、`2`、`3`、`4` 的内部**。图中的 0 到 4 是**宏观的 DFA 状态，而集合内的数字是微观的 NFA 状态。**

### 实战演练 (Exam-Style Practice)

**Question 1 (Concept Verification)**

**Problem Statement:**

In the process of converting an NFA to a DFA using subset construction, a student argues that "if the original NFA has 11 states (numbered 0 to 10), the resulting DFA must also have exactly 11 states, and their state labels must correspond one-to-one." Is this argument correct? Explain the relationship between NFA states and DFA states using professional terminology.

**Answer:**

**该说法错误。**

*原因：* 1. 在子集构造法中，DFA 的每一个状态本质上都是原 NFA 状态空间的一个**状态子集 (Subset of States)**，二者**并不是一一对应的关系。**

2. DFA **状态的数目仅取决于从初始状态出发、通过输入符号驱动可达的唯一状态子集 (Unique Subsets) 的数量。**
3. 由于采用了**重命名 (Renaming)** 技术，DFA 会使用一整套全新的、连续的标号（如 0, 1, 2...）来代表这些幂集空间中的子集。因此，DFA 的状态数通常会少于原 NFA 的状态数。

**Question 2 (Application & Table Reading)**

**Problem Statement:**

Based on the provided state transition table, when the DFA is currently at state `1` (which corresponds to $T_1$) and receives an input symbol `b`, what is the corresponding set of NFA states it transfers to, and what is the renamed label of this destination state in the final DFA graph?

**Answer:**

根据状态转换表（State Transition Table）的第三行可知：

1. 当 **DFA 处于状态 `1`（即 $T_1$，其包含的 NFA 状态集合为 $\{8, 3, 6, 7, 1, 2, 4\}$）时，接收输入字符 `b` 转移到的原 NFA 状态集合为 $\{9, 5, 6, 7, 1, 2, 4\}$。**
2. 该集合在表中的 $I_b$ 列对应的重命名别名为 **$T_3$**，因此在最终的 DFA 状态转换图中，其目标状态（Destination State）的标号为 **`3`**。

![image-20260624164901805](./scut%E7%BC%96%E8%AF%91%E5%8E%9F%E7%90%86%E5%A4%8D%E4%B9%A01/image-20260624164901805.png)

**DFA 最小化 (DFA Minimization)**。其使用的标准算法是**等价类划分法 (Hopcroft's Algorithm)**。

材料标题虽写着“六状态”，但图示与集合中实际包含了 $S, A, B, C, D, E, F$ 共 7 个状态。我们将严格按照图中给定的 7 个状态及转移关系，为你详细拆解这三个步骤的底层逻辑和推导过程。

### DFA 最小化算法核心步骤解析

**Step 1: 初始划分 (Initial Partition)**

- **专业术语:** 将状态集划分为**接受状态集 (Accepting States)** 和**非接受状态集 (Non-accepting States)**。
- **具体实现:** 观察原 DFA 图，双层圆圈代表接受状态（终态），单层圆圈代表非接受状态。
	- 非接受状态组 (设为 $G_1$): $\{S, A, B\}$
	- 接受状态组 (设为 $G_2$): $\{C, D, E, F\}$
- 此时的初始等价类划分 $\Pi_0 = \{\{S, A, B\}, \{C, D, E, F\}\}$。

**Step 2: 细分等价类 (Refining the Partition)**

- **专业术语:** 检查**同一等价类中的状态，在接收相同输入符号 (Input Symbol) 时，其状态转移 (State Transition) 是否指向不同的等价类。**如果是，则说明这些状态是**可区分的 (Distinguishable)**，必须**将它们拆分**。
- **具体实现 (核心考点):**
	1. **检查 $G_2 = \{C, D, E, F\}$:**
		- 输入 `a`: $\delta(C,a)=C$, $\delta(D,a)=F$, $\delta(E,a)=F$, $\delta(F,a)=C$。目标状态 $\{C,F\}$ 全部属于当前的 $G_2$。
		- 输入 `b`: $\delta(C,b)=E$, $\delta(D,b)=D$, $\delta(E,b)=C$, $\delta(F,b)=D$。目标状态 $\{C,D,E\}$ 全部属于当前的 $G_2$。
		- **结论:** 对于所有输入，$C, D, E, F$ 的转移目标**都在同一个等价类 $G_2$ 内部，因此它们彼此等价 (Equivalent)，不可再拆分。**
	2. **检查 $G_1 = \{S, A, B\}$:**
		- **先检查输入 `a`:**
			- $\delta(S, a) = A$ (属于 $G_1$)
			- $\delta(B, a) = A$ (属于 $G_1$)
			- $\delta(A, a) = C$ (属于 **$G_2$**)
		- **结论 1:** **状态 $A$ 接收 `a` 后跳出了 $G_1$ 到达了 $G_2$，而 $S, B$ 留在 $G_1$ 内。因此 $A$ 与 $S, B$ 是可区分的。$G_1$ 拆分为 $\{S, B\}$ 和 $\{A\}$。**
		- **再检查新集合 $\{S, B\}$ 的输入 `b`:**
			- **$\delta(S, b) = B$ (属于当前集合 $\{S, B\}$ 的等价类)**
			- **$\delta(B, b) = D$ (属于 $G_2$)**
		- **结论 2: 状态 $B$ 接收 `b` 后跳到了 $G_2$，而 $S$ 没有。因此 $S$ 和 $B$ 也是可区分的，必须拆分。**
- **最终划分:** $\Pi_{final} = \{\{S\}, \{A\}, \{B\}, \{C, D, E, F\}\}$。这与材料中第二步的结论完全一致。

**Step 3: 构建最小化 DFA (Constructing the Minimized DFA)**

- **专业术语:** 从**每个等价类中选取一个代表元 (Representative) 作为新 DFA 的状态**。更新原有的状态转移函数，使其指向这些代表元。
- **具体实现:**
	- 独立状态 $\{S\}, \{A\}, \{B\}$ 无法合并，保留原名。
	- 集合 $\{C, D, E, F\}$ 作为一个整体，**选取 $D$ 作为代表元（选 $C, E, F$ 中的任何一个都可以，材料指定了 $D$）。因为原集合是接受状态，所以合并后的 $D$ 必须是接受状态 (Double Circle)。**
	- **修正转移关系:** 原 DFA 中**所有指向 $C, D, E, F$ 的边，现在全部指向代表元 $D$。**例如，原图中 $A \xrightarrow{a} C$，现在变为 $A \xrightarrow{a} D$。**原图中 $C, D, E, F$ 内部的互相转移，现在全部变成了 $D$ 自身的自环 (Self-loop)（即 $D \xrightarrow{a, b} D$）。**

### 实战演练 (Exam-Style Practice)

以下题目主要考察你对划分法判定条件的严谨性理解。

**Question 1 (Core Mechanism)**

**Problem Statement:**

In a DFA, there is a non-accepting state set $Q_{test} = \{q_1, q_2, q_3\}$ within the current partition $\Pi$. The alphabet is $\Sigma = \{0, 1\}$. The transition function $\delta$ is given as follows:

- $\delta(q_1, 0) = q_4$, $\delta(q_1, 1) = q_5$

- $\delta(q_2, 0) = q_4$, $\delta(q_2, 1) = q_6$

- $\delta(q_3, 0) = q_7$, $\delta(q_3, 1) = q_5$

	Assume that in the current partition $\Pi$, states $q_4$ and $q_7$ belong to the **same** equivalence class $P_A$, while $q_5$ and $q_6$ belong to **different** equivalence classes ($q_5 \in P_B$, $q_6 \in P_C$).

	How should the set $Q_{test} = \{q_1, q_2, q_3\}$ be split in the next iteration of Hopcroft's Algorithm? Detail the exact resulting subsets and explain your reasoning.

**Answer:**

**拆分结果 (Resulting Subsets):** 将被拆分为三个独立的集合：$\{q_1\}$, $\{q_2\}$, $\{q_3\}$。

**原因推导 (Reasoning):**

1. 检查输入 `0`: 所有状态 $q_1, q_2, q_3$ 接收 `0` 后分别到达 $q_4, q_4, q_7$。已知 $q_4$ 和 $q_7$ 均属于同一个等价类 $P_A$。因此，仅从输入 `0` 来看，它们不可区分。
2. 检查输入 `1`:
	- $\delta(q_1, 1) = q_5 \in P_B$
	- $\delta(q_2, 1) = q_6 \in P_C$
	- $\delta(q_3, 1) = q_5 \in P_B$
3. 因为 $q_1$ 和 $q_3$ 在输入 `1` 时都到达了等价类 $P_B$，而 $q_2$ 到达了不同的等价类 $P_C$，所以 $q_2$ 必须被分离出来。此时集合变为 $\{q_1, q_3\}$ 和 $\{q_2\}$。
4. *注意易错点：* 题目并没有说明 $q_1$ 和 $q_3$ 在之前或当前的完整状态中是否还有其他维度的差异，仅就当前给出的条件，它们暂时等价。**但是**，在严谨的考题中，如果发现本应等价的，答案却是完全拆分，通常是因为忽略了前提。
	- *修正推导 (Self-Correction in answering):* 实际上，对于 $\{q_1, q_3\}$，接收 `0` 分别到 $q_4$ 和 $q_7$，虽然都在 $P_A$，但接收 `1` 都到了 $q_5$。它们在当前条件下**无法被拆分**。
	- **正确最终答案:** 拆分为 **$\{q_1, q_3\}$** 和 **$\{q_2\}$**。($q_2$ 因输入 `1` 的目标等价类不同而被单独剥离)。

**Question 2 (Conceptual Understanding)**

**Problem Statement:**

During DFA minimization, a group of accepting states $\{q_8, q_9\}$ is found to be indistinguishable and is merged into a single state $M$ in the final minimized DFA. In the original DFA, there was a transition $\delta(q_8, 'x') = q_9$. What will this specific transition look like in the final minimized DFA? Use professional terminology to describe it.

**Answer:**

在最终最小化的 DFA 中，该转移将表现为状态 $M$ 上的一个**自环 (Self-loop)**，即 $\delta(M, 'x') = M$。

**原因：** 因为 $q_8$ 和 $q_9$ 被判定为等价 (Equivalent) 并合并为代表元 $M$，原 DFA 中所有在等价类内部的状态转移 (Internal transitions within the equivalence class)，在合并后都会演变成目标指向自身的转移。

![image-20260624180153734](./scut%E7%BC%96%E8%AF%91%E5%8E%9F%E7%90%86%E5%A4%8D%E4%B9%A01/image-20260624180153734.png)

在形式语言（Formal Languages）与**上下文无关文法（Context-Free Grammar, CFG）**中，**`eps` 是希腊字母 epsilon ($\epsilon$) 的常用纯文本简写形式，代表空串（Empty String），即长度为 $0$ 的字符串。**

### 详细原理解析

在将正则表达式（Regular Expression）翻译为纯 CFG 的过程中，**`eps`（$\epsilon$）扮演着处理“$0$ 次出现”的核心角色：**

1. **克林闭包（Kleene Star, `\*`）的语义还原**：

	正则表达式中的 `*` 表示前面的**字符或分组可以出现 $0$ 次或多次（$0$ or more times）**。CFG **本身没有 `*` 操作符，只能通过递归产生式（Recursive Productions）来模拟重复。**

2. **终结推导与零次出现**：

	以图片中的 $a^*$ 为例，其被**转换为左递归（Left Recursion）文法 $A \rightarrow Aa \mid \epsilon$。**

	- 如果我们需要**匹配字符串 `aaa`，推导过程为：**$A \Rightarrow Aa \Rightarrow Aaa \Rightarrow Aaaa \Rightarrow \epsilon aaa \Rightarrow aaa$。

	- 如果我们**需要匹配 $0$ 个 `a`（即空串），推导过程直接为：$A \Rightarrow \epsilon$。**

		在这里，$\epsilon$（`eps`）的作用就是**作为递归的终止条件（Base Case / Termination Condition），并在需要时不产生任何实际的终结符（Terminal Symbol）。**

### 辅助练习题（Exam-Style Practice）

根据该知识点的考查深度，为你补充两道典型的考试风格题目，辅助巩固“正则到 CFG 转换”以及“空串处理”的解题思路。

#### 【练习题 1：基础正则结构转换】

**考查知识点**：**克林闭包（Kleene Star `*`）、正闭包（Positive Closure `+`，即 $1$ 次或多次）**以及**选择（Alternation `|`）的纯 CFG 等价转换**。

**题目（Question）**：

将**以下正则表达式转换为等价的上下文无关文法（CFG），要求只包含非终结符（Non-terminals）、终结符（Terminals）和空串（$\epsilon$）：**

$r = (a \mid b)^+ c^*$

**解答与思路（Answer & Solution）**：

- **Step 1: 拆分模块**。该正则**由两部分拼接（Concatenation）而成：前缀 $X = (a \mid b)^+$，后缀 $Y = c^*$。**

	因此**起始符号产生式为：$S \rightarrow XY$**

- **Step 2: 处理正闭包 $^+$（$1$ 次或多次）**。$X$ 需要**至少产生一个 $a$ 或 $b$。不需要引入 $\epsilon$ 来代表 $0$ 次。**

	使用**右递归（或左递归）表达：$X \rightarrow aX \mid bX \mid a \mid b$ （表示至少有一个 $a$ 或 $b$，之后可以继续接 $X$ 或直接结束）。**

- **Step 3: 处理克林闭包 $^\*$（$0$ 次或多次）**。$Y$ 可以**产生 $0$ 个或多个 $c$，必须引入 $\epsilon$。**

	使用**左递归表达：$Y \rightarrow Yc \mid \epsilon$ （与你图片中的处理方式一致）**。

**最终标准答案**：

$S \rightarrow XY$

$X \rightarrow aX \mid bX \mid a \mid b$

$Y \rightarrow Yc \mid \epsilon$

#### 【练习题 2：消除空产生式（$\epsilon$-Productions）】

**考查知识点**：在语法分析（Syntax Analysis）前期的文法化简中，识别并消除 $\epsilon$ 产生式。

**题目（Question）**：

给定以下 CFG，其中包含空串产生式（`eps` / $\epsilon$）。请构造一个不包含 $\epsilon$-产生式的等价文法（假设目标语言本身不包含空串）。

$S \rightarrow ABa$

$A \rightarrow BB \mid \epsilon$

$B \rightarrow b \mid \epsilon$

**解答与思路（Answer & Solution）**：

遇到这类题目，核心思路是找出所有“可推导出空串的非终结符（Nullable Non-terminals）”，然后将其在右部（Right-hand side）的出现进行全组合展开。

- **Step 1: 寻找 Nullable 符号**。

	显然 $B \rightarrow \epsilon$，所以 $B$ 是 Nullable 的。

	因为 $A \rightarrow BB$，且 $B$ 可以是 $\epsilon$，所以 $A \Rightarrow \epsilon \epsilon \Rightarrow \epsilon$，因此 $A$ 也是 Nullable 的。

- **Step 2: 改写产生式，枚举 Nullable 符号存在与缺失的情况**。

	- 对于 $S \rightarrow ABa$：

		$A$ 和 $B$ 都可以为空。其组合有：$A$ 存在 $B$ 存在 ($ABa$)、$A$ 空 $B$ 存在 ($Ba$)、$A$ 存在 $B$ 空 ($Aa$)、$A$ 和 $B$ 都空 ($a$)。

		更新后：$S \rightarrow ABa \mid Ba \mid Aa \mid a$

	- 对于 $A \rightarrow BB$：

		组合有：两个 $B$ 都在 ($BB$)、左 $B$ 空 ($B$)、右 $B$ 空 ($B$)。由于去除 $\epsilon$-产生式，不能两个都空（否则产生 $\epsilon$）。

		更新后：$A \rightarrow BB \mid B$

	- 对于 $B \rightarrow b \mid \epsilon$：

		直接删掉 $\epsilon$。

		更新后：$B \rightarrow b$

**最终标准答案**：

$S \rightarrow ABa \mid Ba \mid Aa \mid a$

$A \rightarrow BB \mid B$

$B \rightarrow b$

# 为什么模块是这么拆分的啊

正则表达式（Regular Expression）转换为上下文无关文法（CFG）时，**模块的拆分并非随意为之，而是严格基于正则表达式的运算符优先级（Operator Precedence）以及抽象语法树（Abstract Syntax Tree, AST）的自顶向下（Top-down）解析**逻辑。

以下是最直接的原理剖析：

### 1. 正则表达式的运算符优先级

在**正则表达式中，**标准运算符的**优先级从高到低依次为：**

1. **括号（Parentheses）**：`()`
2. **闭包运算（Closure Operations）**：**如 `*`（克林闭包，Kleene Star）、`+`（正闭包，Positive Closure）**
3. **连接运算（Concatenation）**：符号之间的**隐式拼接（例如 `ab` 表示 `a` 接着 `b`）**
4. **选择运算（Alternation / Union）**：`|`

### 2. 针对 $r = (a \mid b)^+ c^*$ 的解析逻辑

在面对一个复杂的正则表达式时，我们**需要采用分治法（Divide and Conquer），从优先级最低（即最外层）的运算符开始“切开”表达式。**

对于 $r = (a \mid b)^+ c^*$：

- 尽管**里面有括号 `()`、选择 `|`、闭包 `+` 和 `*`，但从整体上看，它是由两块内容拼接（连接，Concatenation）在一起的。**
- 这个**隐含的“连接”操作，是统领整个表达式的最外层级。**

因此，**拆分的第一步必须是处理连接运算（Concatenation）。**

### 3. 正则到 CFG 的标准转换规则映射

编译原理中，**每种正则运算符都有固定的 CFG 转换模板**。我们按照**语法树的层级将正则拆解为不同的非终结符（Non-terminals）：**

- **连接运算拆解**：

	规则：如果 $r = r_1 r_2$，则**创建产生式 $S \rightarrow N_1 N_2$（其中 $N_1$ 负责生成 $r_1$，$N_2$ 负责生成 $r_2$）。**

	**应用**：我们**将 $r_1 = (a \mid b)^+$ 赋予非终结符 $X$，将 $r_2 = c^*$ 赋予非终结符 $Y$**。所以**第一步写出 $S \rightarrow XY$。**

- **进入子模块（处理闭包）**：

	**拆分出 $X$ 和 $Y$ 后，它们各自成为独立的子问题。**

	对于 $X = (a \mid b)^+$，此时最**外层的运算符是正闭包 `+`，所以接下来用递归产生式处理它。**

	对于 $Y = c^*$，最**外层是克林闭包 `*`，同样用递归产生式处理。**

### 总结

我们将 $(a \mid b)^+ c^*$ 拆分为 $X$ 和 $Y$，是因为**在构造等价的 CFG 时，必须逆向剥离正则表达式的运算符**。因为**“连接（Concatenation）”是该表达式中最晚执行、层级最高的运算**，所以我们**在语法层面必须最先将它拆分成两个独立的非终结符模块**。这种**拆分方式保证了文法推导的严谨性，不会出现结合律或优先级混乱的错误。**

# 为什么这里会带括号啊

![image-20260624185226463](./scut%E7%BC%96%E8%AF%91%E5%8E%9F%E7%90%86%E5%A4%8D%E4%B9%A01/image-20260624185226463.png)

这里的括号**并不是**目标输入串 `i-i*i` 的一部分。图片下方标注的 `(i-i)*i` 与 `i-(i*i)` 是作者为了辅助说明**这两棵推导树（Parse Trees）所蕴含的隐式计算优先级（Implicit Evaluation Precedence）而添加的语义等价（Semantic Equivalence）表达。**

### 详细原理解析

在语法分析（Syntax Analysis）与抽象语法树（AST）的构建中，一个核心原则是：**树的层级越深（越靠近叶子节点），其代表的运算就越早被执行。**

1. **左侧推导树（对应 `(i-i)\*i`）：**
	- **根节点是 `*`（乘法）。**
	- **乘法的左子节点是一个完整的减法树 `E - E`。**
	- 根据**自底向上的计算逻辑，必须先算出左子树 `i-i` 的值，再将其结果与右侧的 `i` 相乘。**
	- 为了**在纯文本中表达这种“强行先算减法”的逻辑结构，我们借用数学中的括号来标注它的等价语义，即 `(i-i)*i`。这是不符合常规数学法则的，因此图片里标注为“错误”。**
2. **右侧推导树（对应 `i-(i\*i)`）：**
	- **根节点是 `-`（减法）。**
	- 减法的**右子节点是一个完整的乘法树 `E * E`。**
	- 同理**，必须先算出右子树 `i*i` 的值，再用左侧的 `i` 减去它。**
	- 这**等价于数学上的 `i-(i*i)`，**符合我们常识中的“先乘除后加减”，因此标注为“正确”。

**结论：** 文法 $E \rightarrow E-E \mid E*E \mid (E) \mid i$ 是**二义的（Ambiguous）**，因为**它没有在产生式中严格规定算符的优先级（Precedence）和结合性（Associativity）**。面对同一个无括号的输入串 `i-i*i`，它**既能生成先算减法的树，也能生成先算乘法的树，导致语义歧义。**

### 辅助练习题（Exam-Style Practice）

二义性（Ambiguity）及其消除是考试的高频重点。以下提供两道递进式练习题。

#### 【练习题 1：证明文法的二义性（Proving Ambiguity）】

**考查深度**：基础。要求掌握二义性的严格定义。

**题目（Question）**：

给定文法 $G$:

$S \rightarrow S+S \mid a$

请通过为句子 `a+a+a` 构造两棵不同的语法推导树（Parse Trees）或写出两个不同的最左推导（Leftmost Derivations），来证明该文法是二义的（Ambiguous）。

**解答与思路（Answer & Solution）**：

*定义：如果一个文法能为一个句子生成两棵及以上的推导树（即存在两个及以上不同的最左推导），则该文法是二义的。*

**答案**：

- **最左推导 1（生成左结合树）：**

	$S \Rightarrow S+S \Rightarrow S+S+S \Rightarrow a+S+S \Rightarrow a+a+S \Rightarrow a+a+a$

	*(对应树的结构等价于 `(a+a)+a`)*

- **最左推导 2（生成右结合树）：**

	$S \Rightarrow S+S \Rightarrow a+S \Rightarrow a+S+S \Rightarrow a+a+S \Rightarrow a+a+a$

	*(对应树的结构等价于 `a+(a+a)`)*

	由于**同一个句子 `a+a+a` 存在两个不同的最左推导**，故该文法是二义的。

#### 【练习题 2：消除二义性（Disambiguating Grammars）】

**考查深度**：进阶。要求掌握如何通过**引入新的非终结符（Non-terminals）来分层（Stratification），从而固化优先级与结合性。**

**题目（Question）**：

给定二义文法：

$E \rightarrow E+E \mid E*E \mid i$

请对其进行重写，构造一个等价的无二义性文法（Unambiguous Grammar）。要求：

1. 乘法 `*` 的优先级高于加法 `+`。
2. `+` 和 `*` 均满足左结合（Left-associative）。

**解答与思路（Answer & Solution）**：

- **Step 1: 分层级（处理优先级）。** 优先级**越低的算符，所在的非终结符层级越高（越靠近根节点）**。加法优先级低，**由 $E$ (Expression) 处理；乘法优先级高，引入 $T$ (Term) 处理；基础操作数引入 $F$ (Factor) 处理。**
- **Step 2: 确定递归方向（处理结合性）。** **左结合必须使用左递归（Left Recursion），即非终结符在产生式右部的最左侧出现。**例如 $E \rightarrow E+T$ 而**不是 $E \rightarrow T+E$。**

**答案**：

重写后的无二义性文法如下：

$E \rightarrow E+T \mid T$

$T \rightarrow T*F \mid F$

$F \rightarrow i$

# 为什么这样改过之后就不能产生右边的树了

![image-20260624185734193](./scut%E7%BC%96%E8%AF%91%E5%8E%9F%E7%90%86%E5%A4%8D%E4%B9%A01/image-20260624185734193.png)

这样改写后不能产生右边的推导树，其核心根本原因在于：新文法通过引入**非终结符层级（Stratification of Non-terminals）**，**切断了从高优先级算符向低优先级算符的自由推导路径。**

### 详细原理解析

要**产生右边的树，意味着在语法树（Parse Tree）的顶层（根节点附近）优先展开乘法 `*`**。我们用新文法来尝试一下，看看为什么会失败：

1. **尝试构造右树**：若要**在最顶层使用乘法，我们必须调用产生式 $T \rightarrow T*T$（因为新文法中只有 $T$ 能产生 `*`）。**

2. **产生矛盾**：若根结构是 $T * T$，为了匹配目标串 `i-i*i`，左边的 $T$ **必须要能够推导出 `i-i`，右边的 $T$ 推导出 `i`。**

3. **层级阻断（Hierarchy Blocking）**：我们检查从 $T$ 出发的推导路径：$T \Rightarrow F \Rightarrow i$ 或 $T \Rightarrow F \Rightarrow (E)$。

	你会发现，$T$ **根本没有能力**在没有括号保护的情况下凭空推导出一个带减号 `-` 的表达式。如果**想让 $T$ 生成包含 `-` 的表达式，唯一合法的路径是经过 $F \rightarrow (E)$，但这强制要求输入串中必须存在真实的物理括号（即 `(i-i)*i`）。**

4. **唯一合法解**：由于**输入串 `i-i*i` 没有括号，`-` 只能由最外层的 $E$ 生成**。因此，推**导必须从 $E \rightarrow E-E$ 开始（如左树所示），这就唯一确立了 `-` 在树的顶层，`*` 在树的底层，强制实现了“先乘后减”的优先级（Precedence），从而彻底消除了二义性（Ambiguity）。**

### 辅助练习题（Exam-Style Practice）

根据历年考试中对“文法二义性消除”的考查深度（极高频大题），为你补充两道辅助做题思路的实战考题。

#### 【练习题 1：优先级与结合性的综合考察】

**考查深度**：识别图片中改良文法的细微缺陷，并写出绝对标准的形式。

**背景**：图片中改写的文法 $E \rightarrow E-E \mid T$ 虽然解决了 `-` 和 `*` 的优先级问题，但对于形如 `i-i-i` 的串，由于产生式是 $E-E$，依然存在结合性（Associativity）的二义性（既可以左结合也可以右结合）。

**题目（Question）**：

请写出绝对无二义性的（Unambiguous）标准 CFG，要求：

1. 包含减法 `-` 和乘法 `*`。
2. 乘法优先级高于减法。
3. 减法和乘法**均满足左结合（Left-associative）**。
4. 包含括号 `()` 和操作数 `i`。

**解答与思路（Answer & Solution）**：

- **Step 1: 处理优先级（Precedence）**：用层级 $E$ 管 `-`，$T$ 管 `*`，$F$ 管原子。
- **Step 2: 处理结合性（Associativity）**：要**实现左结合，产生式必须是严格的左递归（Left Recursion）**。即**右部中最左边的非终结符与左部相同，其余部分降级。因此不能写 $E \rightarrow E-E$，必须写 $E \rightarrow E-T$。**

**最终标准答案**：

$E \rightarrow E - T \mid T$

$T \rightarrow T * F \mid F$

$F \rightarrow (E) \mid i$

#### 【练习题 2：引入右结合运算符】

**考查深度**：高阶拓展。在标准运算体系中加入右结合算符。

**题目（Question）**：

在练习题 1 的标准文法基础上，引入一个新的运算符：指数运算 `^`（Exponentiation）。

要求：`^` 的优先级**最高（仅次于括号），且必须是右结合（Right-associative）（即 `i^i^i` 必须解析为 `i^(i^i)`）。写出新的文法。**

**解答与思路（Answer & Solution）**：

- **Step 1: 确定新层级**：`^` **优先级在 `*` 之上，在原子 $F$ 之下，所以需要在 $T$ 和 $F$ 之间插入一个新层级 $P$ (Power)。**
- **Step 2: 处理右结合**：**右结合要求文法必须是右递归（Right Recursion）。即产生式右部最右侧的非终结符与左部保持一致。应写为** $P \rightarrow F \text{ \textasciicircum } P$。

**最终标准答案**：

$E \rightarrow E - T \mid T$  (处理 `-`，左结合，优先级最低)

$T \rightarrow T * P \mid P$  (处理 `*`，左结合，优先级中等)

$P \rightarrow F \text{ \textasciicircum } P \mid F$  (处理 `^`，**右递归实现右结合**，优先级高)

$F \rightarrow (E) \mid i$  (原子层，优先级最高)

# 必须是左递归和右递归到底是什么意思,为我详细地解释一下,与最左推导和最右推导什么区别

在形式语言与自动机理论（Formal Languages and Automata Theory）及编译原理（Compiler Design）中，**“递归”（Recursion）与“推导”（Derivation）是两个处于完全不同维度的概念。简单来说，左/右递归是文法产生式本身的静态结构特征，而最左/最右推导是用文法生成字符串时的动态执行顺序。**

以下是最专业、精简的术语级解析。

### 一、 静态文法特征：左递归与右递归 (Left and Right Recursion)

递归描述的是**上下文无关文法（CFG）中产生式（Productions）的结构定义**。它决定了语法分析树（Parse Tree）的生长方向以及运算符的**结合性（Associativity）**。

#### 1. 左递归 (Left Recursion)

- **定义**：在产生式中，**非终结符（Non-terminal）推导出的右部的最左侧符号就是它自身。**
- **形式化表示**：$A \rightarrow A\alpha \mid \beta$（其中 $\alpha$ 和 $\beta$ **为任意文法符号串**，且 $\beta$ 不以 $A$ 开头）。
- **工程意义**：**用于实现左结合（Left-associative）。例如加法和减法，`1-2-3` 会被解析为 `(1-2)-3`。**
- **语法树特征**：语法树**向左下方极度倾斜生长。**

#### 2. 右递归 (Right Recursion)

- **定义**：在产生式中，非终结符推导出的**右部的最右侧符号就是它自身。**
- **形式化表示**：$A \rightarrow \alpha A \mid \beta$。
- **工程意义**：用于实现**右结合（Right-associative）**。例如指数运算或编程语言中的赋值语句，`a = b = 1` 会被解析为 `a = (b = 1)`。
- **语法树特征**：语法树向右下方极度倾斜生长。

### 二、 动态生成过程：最左推导与最右推导 (Leftmost and Rightmost Derivation)

推导描述的是**从起始符号（Start Symbol）生成目标句型（Sentential Form）的替换步骤**。由于**一个句型中可能同时包含多个非终结符，我们需要规定替换的优先级。**

#### 1. 最左推导 (Leftmost Derivation)

- **定义**：在推导过程的每一步骤中，**总是选择当前句型中最左边的非终结符进行替换（展开）。**
- **工程意义**：自顶向下语法分析（Top-Down Parsing，如 LL(1) 分析法）所采用的推导策略。

#### 2. 最右推导 (Rightmost Derivation)

- **定义**：在推导过程的每一步骤中，总是选择当前句型中**最右边的非终结符**进行替换。
- **别名**：在编译原理中，**最右推导也被严格称为规范推导（Canonical Derivation）**。
- **工程意义**：其逆过程（规范归约）是**自底向上语法分析（Bottom-Up Parsing，如 LR 分析法）的基础。**

### 三、 核心区别与防混淆点总结

| **比较维度**   | **左/右递归 (Left/Right Recursion)**                         | **最左/最右推导 (Leftmost/Rightmost Derivation)** |
| -------------- | ------------------------------------------------------------ | ------------------------------------------------- |
| **研究对象**   | **文法规则 (Grammar Rules)**的形态                           | **动作执行 (Derivation Process)**的顺序           |
| **主要影响**   | 决定结合性（左结合/右结合）                                  | 决定分析算法（LL/LR 分析法）                      |
| **互相独立性** | **正交关系：** 无论文法是左递归还是右递归，你都可以对它使用最左推导或最右推导。即使是右递归文法，你依然可以采用最左推导去验证句型。 |                                                   |

### 辅助练习题（Exam-Style Practice）

根据知识点考查深度（高频必考），为你整理两道综合题目，用于巩固文法递归形态与推导顺序的解题规范。

#### 【练习题 1：递归识别与推导执行】

**考查深度**：基础。验证对静态结构判断与动态推导执行分离的理解。

**题目（Question）**：

给定上下文无关文法 $G$:

$S \rightarrow aSb \mid A$

$A \rightarrow Ac \mid d$

1. 指出文法 $G$ 中包含的递归类型（左递归或右递归），并说明原因。
2. 针对输入串 `adcb`，分别写出**其最左推导（Leftmost Derivation）和最右推导（Rightmost Derivation）。**

**解答与思路（Answer & Solution）**：

1. **递归类型**：文法中包含**左递归**。因为**存在产生式 $A \rightarrow Ac$，非终结符 $A$ 位于产生式右部的最左侧，符合左递归形式定义。**

2. **推导过程**：

	- **最左推导**（每次替换最左侧的非终结符）：

		$S \Rightarrow aSb \Rightarrow aAb \Rightarrow aAcb \Rightarrow adcb$

	- **最右推导**（每次替换最右侧的非终结符）：

		$S \Rightarrow aSb \Rightarrow aAb \Rightarrow aAcb \Rightarrow adcb$

		*(注：虽然此处推导结果碰巧看似相同，但推导时盯准“最左”或“最右”非终结符进行展开的逻辑不能乱)*。

#### 【练习题 2：消除直接左递归（Eliminating Direct Left Recursion）】

**考查深度**：极高频大题核心步骤**。在自顶向下（最左推导）的语法分析中，左递归会导致分析器陷入死循环，必须将其转换为等价的右递归形式。**

**题目（Question）**：

给定产生式规范形式 $A \rightarrow A\alpha \mid \beta$，请写出**消除直接左递归后的通用标准文法转换结果，并应用此通用规则，消除下列文法的直接左递归：**

$E \rightarrow E+T \mid E-T \mid T$

**解答与思路（Answer & Solution）**：

- **通用规则提取**：遇到 $A \rightarrow A\alpha \mid \beta$，引入**新的非终结符 $A'$，将其改写为右递归形式：**

	$A \rightarrow \beta A'$

	$A' \rightarrow \alpha A' \mid \epsilon$

- **应用规则**：

	对于 $E \rightarrow E+T \mid E-T \mid T$

	识别部分：**左部 $A$ 为 $E$；不含 $E$ 抬头的 $\beta$ 只有 $T$；$\alpha$ 有两个，分别是 $+T$ 和 $-T$。**

	代入公式，**引入 $E'$，进行等价改写。**

**最终标准答案**：

通用标准文法转换**结果为：$A \rightarrow \beta A'$ 以及 $A' \rightarrow \alpha A' \mid \epsilon$**

**消除该文法直接左递归后的结果为：**

**$E \rightarrow TE'$**

**$E' \rightarrow +TE' \mid -TE' \mid \epsilon$**

# 产生式（Productions）到底是什么意思,怎么用的

**在形式语言（Formal Languages）与上下文无关文法（Context-Free Grammar, CFG）**中，**产生式（Production / Production Rule）\**是定义该语言语法的\**核心重写规则（Rewriting Rule）**。它精确规定了文法中的符号如何**被合法地替换和展开。**

简单来说，**产生式就是编译器判断“什么样的语法结构是合法的”唯一数学依据。**

### 一、 产生式的标准结构

一个标准的 CFG 产生式通常表现为以下数学形式：

$A \rightarrow \alpha$

1. **左部（Left-hand side, LHS）**：即符号 $A$。在 CFG 中，左部必须是**单个非终结符（Non-terminal Symbol）**。它代表一个抽象的语法范畴（例如：“表达式”、“语句”、“变量定义”）。
2. **箭头（Arrow, $\rightarrow$ 或 $::=$）**：读作**“推导出（derives）”或“定义为”。**
3. **右部（Right-hand side, RHS）**：即序列 $\alpha$。它是**包含终结符（Terminals，即具体的字符或单词）、非终结符或空串（$\epsilon$）的任意有穷序列**。它表示左部的非终结符可以被替换成什么样的具体结构。

*(注：如果有多个左部相同的产生式，如 $A \rightarrow \alpha_1$、$A \rightarrow \alpha_2$，通常使用选择符 `|` 简写为 $A \rightarrow \alpha_1 \mid \alpha_2$)*。

### 二、 产生式是怎么用的？

产生式在编译原理中的核心用法分为**正向**和**逆向**两个过程，分别对应语言的“生成”与“解析”。

#### 1. 正向使用：推导（Derivation）—— 证明句子合法性

**操作规范**：从起始符号（Start Symbol）开始，在当前句型**中选择一个非终结符**，查找以它为左部的产生式，然后**用该产生式的右部替换这个非终结符。重复此过程，直到句型中全部变为终结符。**

- **目的**：推导用于理论证明，证明某个特定的字符串确实属于该文法所定义的语言（即该字符串能被推导出来）。

**实例展示**：

已知产生式：

1: $S \rightarrow aSb$

2: $S \rightarrow \epsilon$

要生成字符串 `aabb`，推导过程（使用符号 $\Rightarrow$ 表示推导动作）为：

$S \Rightarrow aSb$ （应用产生式 1）

$\Rightarrow aaSbb$ （再次应用产生式 1 替换中间的 $S$）

$\Rightarrow aa\epsilon bb \Rightarrow aabb$ （应用产生式 2 消除 $S$）

#### 2. 逆向使用：归约（Reduction）—— 语法分析器（Parser）的实际工作

**操作规范**：给定**一个由终结符组成的输入串，在串中寻找与某个产生式右部完全匹配的子串（称为句柄，Handle）**，将其**逆向替换（折叠）\**为该产生式的\**左部**非终结符。重复此过程，直到整个串被缩减为一个起始符号。

- **目的**：这是**自底向上语法分析（如 LR 分析法）的核心机制。如果能成功归约回起始符号，说明代码语法正确，同时也就隐式地建立起了语法树。**

### 辅助练习题（Exam-Style Practice）

理解产生式的正逆向使用是语法分析（Syntax Analysis）的基础考点。以下为你补充两道辅助巩固的题目。

#### 【练习题 1：产生式推导的应用】

**考查知识点**：熟练应用产生式进行最左推导（Leftmost Derivation），并区分“产生式定义”与“推导步骤”。

**题目（Question）**：

给定描述布尔表达式的文法 $G$:

$B \rightarrow B \text{ or } B \mid B \text{ and } B \mid \text{not } B \mid (B) \mid \text{true} \mid \text{false}$

请给出句子 `not (true or false)` 的最左推导过程，并标明每一步所使用的产生式。

**解答与思路（Answer & Solution）**：

*最左推导要求每一步都必须替换当前句型中最左边的非终结符 $B$。*

**答案**：

1. $B \Rightarrow \text{not } B$  （应用 $B \rightarrow \text{not } B$）
2. $\Rightarrow \text{not } (B)$  （应用 $B \rightarrow (B)$ 替换当前唯一的 $B$）
3. $\Rightarrow \text{not } (B \text{ or } B)$  （应用 $B \rightarrow B \text{ or } B$ 替换括号内的 $B$）
4. $\Rightarrow \text{not } (\text{true or } B)$  （应用 $B \rightarrow \text{true}$ 替换最左侧的 $B$）
5. $\Rightarrow \text{not } (\text{true or false})$  （应用 $B \rightarrow \text{false}$ 替换最后的 $B$）

#### 【练习题 2：通过归约识别错误（Reverse Application）】

**考查知识点**：理解归约（Reduction）的思想，识别非法输入。

**题目（Question）**：

给定文法 $G$:

$S \rightarrow (S) \mid a$

现有一个输入串 `((a)`。请尝试对其进行归约操作。说明该输入串是否合法，并指出在归约过程中哪里出现了问题。

**解答与思路（Answer & Solution）**：

*归约过程是**从左到右扫描，寻找可以匹配产生式右部的结构。***

**答案**：

- **归约步骤尝试**：扫描串 `((a)`，发现**子串 `a` 匹配产生式右部。将其归约为 $S$。**
- **当前句型变为**：`((S)`。
- **判定与问题剖析**：此时句型为 `((S)`。检查**现有的产生式，右部没有任何结构能匹配 `((S)` 或其子串（产生式要求 $S$ 必须被成对的括号包围，即 `(S)`）**。因此，**归约过程卡死（Stuck），无法继续归约到起始符号 $S$。**
- **结论**：该**输入串非法。这正是语法分析器报出 Syntax Error 的底层数学原因**——**找不到合法的产生式右部来进行归约。**

# 但是怎么选取哪个非终结符进行替换呢,而且怎么知道用哪条产生式,不会一直递归下去吗

这是一个极其敏锐的本质问题。你所指出的困惑，正是理论上的上下文无关文法（CFG）**与工程中真实的**语法分析器（Parser）之间的核心鸿沟：**非确定性（Nondeterminism）与确定性（Determinism）的矛盾。**

在真实考试和编译器设计中，解决这个问题的核心机制如下：

### 一、 怎么选取哪个非终结符进行替换？（替换位置的确定）

在工程实现中，**分析器绝对不会随机选取非终结符进行替换**，而是**强制固定推导方向**：

1. **自顶向下分析（Top-Down Parsing）**：强制采用**最左推导（Leftmost Derivation）**。如果在**当前句型中有多个非终结符，永远只扫描并替换最左边的那一个。**
2. **自底向上分析（Bottom-Up Parsing）**：强制采用**最右推导的逆过程（规范归约，Canonical Reduction）**。

通过**严格规定推导策略，编译器直接消除了“该替换谁”的二义性。**

### 二、 怎么知道用哪条产生式？（产生式的确定）

如果一个非终结符有多个产生式（如 $A \rightarrow \alpha \mid \beta$），分析器**通过向前看（Lookahead）机制和分析表（Parsing Table）来做出唯一决策。**

以**最经典的 LL(1) 分析法 为例：**

- **第一个 L**：从左到右扫描输入串。
- **第二个 L**：生成最左推导。
- **(1)**：**向前看 $1$ 个输入符号（Lookahead of 1 symbol）**。

**核心决策依据：FIRST 集与 FOLLOW 集**

编译器在预处理阶段，**会为文法计算出两个集合：**

1. **`FIRST` 集（首符号集）**：**产生式右部推导出的字符串的第一个终结符的集合。**
	- *决策逻辑*：如果**当前要展开非终结符 $A$，且面临的输入字符是 $a$，分析器会检查 $A$ 的哪条产生式的 `FIRST` 集中包含 $a$。**包**含 $a$ 的那条就是唯一正确的产生式。**
2. **`FOLLOW` 集（后继符号集）**：在**所有句型中，紧跟在非终结符 $A$ 后面的终结符集合。**
	- *决策逻辑*：**如果 $a$ 不在任何产生式的 `FIRST` 集中，但 $A$ 有一条推导出空串的产生式 $A \rightarrow \epsilon$，分析器会检查 $a$ 是否在 $A$ 的 `FOLLOW` 集中**。如果**在，则果断选择 $A \rightarrow \epsilon$。**

通过**查构建好的 LL(1) 预测分析表，分析器只需做到 $O(1)$ 的查表操作，即可精准知道选哪条产生式**。如果表中**某个格子里有两个以上的产生式，说明该文法不是 LL(1) 文法，存在冲突（Conflict），需要修改文法。**

### 三、 为什么不会一直递归下去？（死循环的规避）

1. **静态消除直接左递归**：如前所述，**自顶向下分析器如果遇到左递归（$A \rightarrow A\alpha$），确实会无限展开导致死循环（Stack Overflow）**。因此，在**将文法交给分析器之前，必须静态消除所有左递归（Eliminating Left Recursion），将其转化为右递归。**
2. **输入符号的消耗（Consuming Input）**：正常的产生式**展开必然伴随着终结符的生成。每生成一个终结符，分析器就将其与输入串中的字符进行匹配并消耗（Consume）掉**。由于输入串的长度是有限的，只要**文法没有无意义的循环推导（如 $A \Rightarrow B, B \Rightarrow A$），推导必定会随着输入串的耗尽而终止。**

### 辅助练习题（Exam-Style Practice）

“如何选择产生式”在考试中具体体现为对 `FIRST` 集的计算与 LL(1) 冲突的判断。这是语法分析（Syntax Analysis）部分最核心的必考计算题。

#### 【练习题 1：FIRST 集计算与产生式选择】

**考查深度**：极高频必考。掌握基于 FIRST 集选择产生式的计算逻辑。

**题目（Question）**：

已知以下文法 $G$：

$S \rightarrow aAB \mid bA$

$A \rightarrow c \mid \epsilon$

$B \rightarrow d$

1. 请分别计算**非终结符 $S$ 两个产生式右部的 `FIRST` 集：$\text{FIRST}(aAB)$ 和 $\text{FIRST}(bA)$。**
2. 假设在**自顶向下分析中，当前最左侧的非终结符是 $S$，向前看（Lookahead）的输入字符是 `b`，分析器应该选择哪条产生式？**

**解答与思路（Answer & Solution）**：

- **Step 1: 计算 FIRST 集**。

	$\text{FIRST}(aAB) = \{a\}$ （因为**右部以终结符 $a$ 开头**）

	$\text{FIRST}(bA) = \{b\}$ （因为**右部以终结符 $b$ 开头**）

- **Step 2: 匹配 Lookahead 符号**。

	**当前面临的输入符号是 `b`。由于 `b` 存在于 $\text{FIRST}(bA)$ 中，且不在 $\text{FIRST}(aAB)$ 中。**

- **结论**：分析器**应当毫无歧义地选择产生式 $S \rightarrow bA$。**

#### 【练习题 2：LL(1) 冲突判定（提取左公因子 / Left Factoring）】

**考查深度**：进阶大题考点。识别分析器无法决策的情况，并对文法进行等价转换。

**题目（Question）**：

给定文法（表示 `if-else` 语句的抽象）：

$S \rightarrow iEtS \mid iEtSeS \mid a$

（其中 $i, t, e, a$ 为终结符，$E, S$ 为非终结符）。

1. 当面临输入符号 $i$ 时，基于当前文法，分析器能否唯一确定使用哪条产生式？为什么？
2. 请使用提取左公因子（Left Factoring）技术重写该文法，使其满足 LL(1) 的确定性要求。

**解答与思路（Answer & Solution）**：

1. **不能唯一确定**。因为前两条产生式 $S \rightarrow iEtS$ 和 $S \rightarrow iEtSeS$ 的 **`FIRST` 集都包含 $i$（$\text{FIRST}(iEtS) \cap \text{FIRST}(iEtSeS) = \{i\} \neq \emptyset$）**。这意味着**分析器看了一个字符 $i$ 后，根本不知道该选哪条分支，产生了FIRST-FIRST 冲突（Conflict）。**
2. **提取左公因子**：为了延迟决策，直到我们看到足够的信息，我们**将它们共同的前缀（公共部分 $iEtS$）提取出来，用一个新的非终结符 $S'$ 来处理差异部分（即 $\epsilon$ 和 $eS$）。**

**最终标准答案**：

**消除冲突后的重写文法为：**

**$S \rightarrow iEtSS' \mid a$**

**$S' \rightarrow eS \mid \epsilon$**

***(此时，分析器遇到 $i$ 会毫无悬念地走 $S \rightarrow iEtSS'$，推迟到解析 $S'$ 时再决定是否有 `else` 分支)*。**

# 这里的follow(S)是怎么求出来的

![image-20260624215656567](./scut%E7%BC%96%E8%AF%91%E5%8E%9F%E7%90%86%E5%A4%8D%E4%B9%A01/image-20260624215656567.png)

计算 $\text{FOLLOW}(S)$ 需要**严格遵循语法分析中 FOLLOW 集（后继符号集，Follow Sets）的三条核心构造规则。**在这个特定的文法中，**由于 $S$ 和 $A$ 的互相递归依赖，它们的 FOLLOW 集计算是交织在一起的，形成了一个推导闭环。**

以下是最严密的计算推导过程：

**已知文法（Grammar）**：

1. $S \rightarrow aA$
2. $S \rightarrow d$
3. $A \rightarrow bAS$
4. $A \rightarrow \epsilon$

**前置计算：求 $\text{FIRST}(S)$**

**要计算 FOLLOW，必须先明确相关非终结符的 FIRST 集（首符号集，First Sets）。**

观察 $S$ 的产生式：$S \rightarrow aA$ 第一个符号是终结符 $a$；$S \rightarrow d$ 第一个符号是终结符 $d$。

因此，$\text{FIRST}(S) = \{a, d\}$。

**正式计算 $\text{FOLLOW}(S)$ 与 $\text{FOLLOW}(A)$**

根据 **FOLLOW 集的三条规则进行同步迭代推导：**

1. **规则 1（起步标记）**：由于 **$S$ 是文法的起始符号（Start Symbol），首先将输入结束标记 $\$$ 加入 $\text{FOLLOW}(S)$ 中。**

	*当前状态*：$\text{FOLLOW}(S) = \{\$\}$

2. **规则 2（内部传递）**：若**存在产生式 $X \rightarrow \alpha Y \beta$，则将 $\text{FIRST}(\beta)$ 中除 $\epsilon$ 外的所有元素加入 $\text{FOLLOW}(Y)$。**

	*应用*：观察产生式 $A \rightarrow bAS$。在**非终结符 $A$ 后面紧跟着非终结符 $S$。**

	**根据规则，必须将 $\text{FIRST}(S)$ 加入到 $\text{FOLLOW}(A)$ 中。**

	*推导动作*：将 $\{a, d\}$ 加入 $\text{FOLLOW}(A)$。

	*当前状态*：$\text{FOLLOW}(A) = \{a, d\}$

3. **规则 3（尾部继承）**：若**存在产生式 $X \rightarrow \alpha Y$（即 $Y$ 在最右端），或者** $X \rightarrow \alpha Y \beta$ 且 $\beta \Rightarrow^* \epsilon$**（即 $\beta$ 能推导出空串），**则**将 $\text{FOLLOW}(X)$ 的所有元素加入 $\text{FOLLOW}(Y)$ 中。**

	*应用 A（向右继承）*：观察产生式 $S \rightarrow aA$。**$A$ 位于产生式最右端。这意味着任何能够合法跟在 $S$ 后面的符号，也必然能跟在 $A$ 后面。**

	*推导动作*：**得出关系 $\text{FOLLOW}(S) \subseteq \text{FOLLOW}(A)$。将 $S$ 现有的 $\$$ 传给 $A$。**

	*当前状态*：$\text{FOLLOW}(A) = \{\$, a, d\}$

	*应用 B（反向传递）*：观察产生式 $A \rightarrow bAS$。**$S$ 位于产生式最右端。这意味着任何能够合法跟在 $A$ 后面的符号，也必然能跟在这个 $S$ 后面。**

	*推导动作*：得出关系 $\text{FOLLOW}(A) \subseteq \text{FOLLOW}(S)$。**将 $A$ 现有的全部元素传回给 $S$。**

	*当前状态*：$\text{FOLLOW}(S) = \{\$, a, d\}$

**最终结论**：

由于 $\text{FOLLOW}(S) \subseteq \text{FOLLOW}(A)$ 且 $\text{FOLLOW}(A) \subseteq \text{FOLLOW}(S)$，**两者的集合已经完全相等并达到稳定状态（固定点，Fixed Point），不再有新符号可以加入。**

故最终计算结果为：$\text{FOLLOW}(S) = \{\$, a, d\}$。

### 辅助练习题（Exam-Style Practice）

考研或期末考试中，**最容易失分的地方就是$\epsilon$（空串）引发的穿透（Epsilon Penetration）以及非终结符之间的循环依赖（Cyclic Dependencies）**。以下补充两道针对性练习。

#### 【练习题 1：$\epsilon$-穿透引发的 FOLLOW 扩展】

**考查知识点**：当**右侧相邻非终结符可以推导出空串时（Nullable），需要穿透它，继续向后寻找 FIRST 集或继承左部的 FOLLOW 集。**

**题目（Question）**：

给定文法 $G$:

$S \rightarrow ABc$

$A \rightarrow a \mid \epsilon$

$B \rightarrow b \mid \epsilon$

请分别计算非终结符 $A$ 和 $B$ 的 FOLLOW 集：$\text{FOLLOW}(A)$ 与 $\text{FOLLOW}(B)$。

**解答与思路（Answer & Solution）**：

- **计算 $\text{FOLLOW}(B)$**：

	在 $S \rightarrow ABc$ 中，$B$ 后面跟着终结符 $c$。

	**由于 $c$ 的 FIRST 集就是它自己，直接根据规则 2，$\text{FOLLOW}(B) = \{c\}$。**

- **计算 $\text{FOLLOW}(A)$**：

	在 $S \rightarrow ABc$ 中，$A$ 后面跟着 $B$。所以**首先把 $\text{FIRST}(B)$ 中非空的元素加入，$\text{FIRST}(B) = \{b, \epsilon\}$，提取出 $b$ 加入 $\text{FOLLOW}(A)$。**

	**关键点**：由于 **$B \rightarrow \epsilon$（即 $B$ 是 Nullable 的），$B$ 可以“隐身”。如果 $B$ 是空串，那么跟在 $A$ 后面的符号就变成了 $c$。**

	因此，还必须把 $\text{FIRST}(c)$ 加入到 $\text{FOLLOW}(A)$ 中。

	最终 $\text{FOLLOW}(A) = \{b, c\}$。

#### 【练习题 2：集合循环依赖与传递】

**考查知识点**：与你截图中的逻辑高度类似，重点测试对“规则 3 尾部继承”的反复迭代应用，直到集合不再增大为止。

**题目（Question）**：

给定描述指针解引用与赋值的文法 $G$:

$S \rightarrow L = R \mid R$

$L \rightarrow * R \mid id$

$R \rightarrow L$

（已知：$\text{FIRST}(S) = \text{FIRST}(L) = \text{FIRST}(R) = \{*, id\}$）

请计算 $\text{FOLLOW}(L)$ 与 $\text{FOLLOW}(R)$。

**解答与思路（Answer & Solution）**：

首先由起步规则：$\text{FOLLOW}(S) = \{\$\}$。

接着寻找所有产生式右侧中出现 $L$ 和 $R$ 的位置：

1. 从 $S \rightarrow L = R$ 观察 $L$**：$L$ 后面紧跟终结符 $=$。故将 $=$ 加入 $\text{FOLLOW}(L)$。**
2. 从 $S \rightarrow L = R$ 观察 $R$：**$R$ 在最右端。得到关系 $\text{FOLLOW}(S) \subseteq \text{FOLLOW}(R)$。故将 $\$$ 加入 $\text{FOLLOW}(R)$。**
3. 从 $S \rightarrow R$ 观察 $R$：$R$ 在最右端。**得到关系 $\text{FOLLOW}(S) \subseteq \text{FOLLOW}(R)$（无新元素加入）。**
4. 从 $L \rightarrow * R$ 观察 $R$：$R$ 在最右端。得到关系 $\text{FOLLOW}(L) \subseteq \text{FOLLOW}(R)$。此时 $\text{FOLLOW}(L)$ 中有 $=$，必须传给 $R$。此时 $\text{FOLLOW}(R) = \{\$, =\}$。
5. 从 $R \rightarrow L$ 观察 $L$：$L$ 在最右端。得到关系 $\text{FOLLOW}(R) \subseteq \text{FOLLOW}(L)$。此时 $\text{FOLLOW}(R)$ 有 $\$$ 和 $=$，全部传回给 $L$。此时 $\text{FOLLOW}(L) = \{\$, =\}$。

**最终结果**：

$\text{FOLLOW}(L) = \{\$, =\}$

$\text{FOLLOW}(R) = \{\$, =\}$

# 这里选择是确定的是什么意思,为我详细解释一下

它指的是：**分析器（Parser）在面对同一个非终结符的多个可选产生式时，仅仅通过向前看一个输入字符（Lookahead），就能百分之百明确地决定该用哪一条产生式展开，绝不犹豫，也绝对不需要猜测和回溯（Backtracking）。**

### 详细原理解析

在你的截图中，分析器**目前正在处理非终结符 $A$**。此时 **$A$ 面临一个“岔路口”，它有两条路可以走：**

1. $A \rightarrow bAS$
2. $A \rightarrow \epsilon$ （即**让 $A$ 隐身/变成空串**）

分析器如何做出选择？它**依靠的是“路标集（Director Sets）”的互斥性。**

- **走第一条路的条件（看 FIRST 集）**：

	如果用 $A \rightarrow bAS$ 展开，那么**这串结构推导出的第一个字符必定是 $b$（即 $\text{FIRST}(bAS) = \{b\}$）。**

	*结论*：**只要分析器看到当前输入的字符是 $b$，它就应该选第一条路。**

- **走第二条路的条件（看 FOLLOW 集）**：

	如果用 $A \rightarrow \epsilon$ 展开，$A$ 就**直接消失了。此时，原本跟在 $A$ 后面的合法字符就会暴露出来**，**直接与当前的输入字符进行匹配。哪些字符能合法地跟在 $A$ 后面？这正是 $\text{FOLLOW}(A)$ 的定义。由之前的计算得出 $\text{FOLLOW}(A) = \{\$, a, d\}$。**

	*结论*：只要**分析器看到当前输入的字符是 $\$$、$a$ 或 $d$，它就知道 $A$ 必须在这里“让路/消失”，因此坚决选择 $A \rightarrow \epsilon$。**

**为什么说“选择是确定的”？**

关键就在于这张截图里的公式：$\text{FIRST}(bAS) \cap \text{FOLLOW}(A) = \emptyset$。

即：$\{b\} \cap \{\$, a, d\} = \emptyset$（**空集**）。

**指引这两条路的路标没有任何重合部分**。如果**当前输入字符是 $b$，分析器肯定不会走空串产生式；如果是 $a$，肯定不会走 $bAS$**。**因为条件互斥，所以选择是绝对确定的，这就是该文法能够成为 LL(1) 文法的充要条件。**

### 辅助练习题（Exam-Style Practice）

在考试中，验证“选择是否确定”通常表现为让你判断一个文法是否为 LL(1) 文法，或者让你找出文法中的冲突（Conflicts）。以下为你补充两道对比强烈的练习题。

#### 【练习题 1：识别“选择不确定”的冲突（FIRST-FOLLOW Conflict）】

**考查深度**：深刻理解交集不为空时造成的灾难。

**题目（Question）**：

给定文法 $G$:

$S \rightarrow Aa$

$A \rightarrow a \mid \epsilon$

请通过计算判断，当分析器处理非终结符 $A$ 并且向前看的输入符号为 $a$ 时，选择是否确定？并说明原因。

**解答与思路（Answer & Solution）**：

- **Step 1: 计算相关的集合**。

	$A$ 的**第一条产生式 $A \rightarrow a$ 的 $\text{FIRST}$ 集：$\text{FIRST}(a) = \{a\}$。**

	**由于 $A$ 有空产生式 $A \rightarrow \epsilon$，我们需要计算 $\text{FOLLOW}(A)$。在产生式 $S \rightarrow Aa$ 中，$a$ 紧跟在 $A$ 后面，因此 $\text{FOLLOW}(A) = \{a\}$。**

- **Step 2: 检查交集**。

	$\text{FIRST}(a) \cap \text{FOLLOW}(A) = \{a\} \cap \{a\} = \{a\} \neq \emptyset$。交集不为空！

- **结论与解释**：

	**选择是不确定的。** 此时发生了严重的 **FIRST-FOLLOW 冲突**。当分析器处于 $A$ 且看到输入字符 $a$ 时，它完全处于懵圈状态：

	它既可以把这个 $a$ 解释为 $A \rightarrow a$ 产生的字符，也可以让 $A$ 变成空串 $A \rightarrow \epsilon$，从而让输入字符 $a$ 去匹配 $S \rightarrow Aa$ 尾部的那个 $a$。由于存在歧义，该文法不是 LL(1) 文法。

#### 【练习题 2：标准 LL(1) 条件证明】

**考查深度**：掌握用数学语言规范作答“选择确定性”的考试题型。

**题目（Question）**：

给定文法规则（用于匹配可选的 `else` 分支片段）：

$C \rightarrow \text{else } S \mid \epsilon$

假设在整个大文法中，已经算出 $\text{FOLLOW}(C) = \{\text{end}, \text{while}\}$。

请证明该产生式的选择是确定的。

**解答与思路（Answer & Solution）**：

*要证明包含 $\epsilon$-产生式的非终结符的选择是确定的，必须严格证明其非空产生式的 FIRST 集与该非终结符的 FOLLOW 集不相交。*

**标准作答（Standard Answer）**：

1. 对于产生式 $C \rightarrow \text{else } S$，其 $\text{FIRST}$ 集为：$\text{FIRST}(\text{else } S) = \{\text{else}\}$。
2. 已知 $\text{FOLLOW}(C) = \{\text{end}, \text{while}\}$。
3. 验证两者的交集：$\text{FIRST}(\text{else } S) \cap \text{FOLLOW}(C) = \{\text{else}\} \cap \{\text{end}, \text{while}\} = \emptyset$。
4. 因为两集合的交集为空集，所以在任何给定的 Lookahead 符号下，最多只有一条产生式满足匹配条件，**因此选择是确定的，不存在 FIRST-FOLLOW 冲突。**

# 为什么前面一个是算FIRST集后面一个是算FOLLOW集

在自顶向下语法分析（Top-Down Parsing）中，决定使用哪条产生式的理论依据，在编译原理中被称为**预测集（Predict Set）\**或\**选择集（SELECT Set）**。

之所以前面看 $\text{FIRST}$ 集，后面看 $\text{FOLLOW}$ 集，是因为**这两条产生式在推导（Derivation）和匹配（Matching）输入流时的物理意义完全不同。**以下是严谨的术语原理解析：

### 核心定理：SELECT 集的定义

**对于任意产生式 $A \rightarrow \alpha$，分析器（Parser）选择该产生式的条件是：当前向前看符号（Lookahead Symbol）属于 $\text{SELECT}(A \rightarrow \alpha)$。**

**$\text{SELECT}$ 集的构造规则分为两种互斥的情况：**

#### 1. 当右部 $\alpha$ 不能推导出空串时：看 FIRST 集

**应用场景**：对应你的产生式 $A \rightarrow bAS$。

- **解析**：如果分析器决定用 $A \rightarrow bAS$ 来替换句型（Sentential Form）中的 $A$，那么展开后，**句型在这一位置的前导符号立刻变成了 $bAS$ 所能产生的首个字符。**
- **匹配逻辑**：为了**保证推导不失败，输入流中当前的字符必须能够与 $bAS$ 产生的第一个字符匹配。因此，我们只需要计算 $\text{FIRST}(bAS)$。**
- **结论**：$\text{SELECT}(A \rightarrow bAS) = \text{FIRST}(bAS)$。

#### 2. 当右部 $\alpha$ 能够推导出空串（$\epsilon$）时：看 FOLLOW 集

**应用场景**：对应你的产生式 $A \rightarrow \epsilon$。

- **解析**：如果分析器**决定用 $A \rightarrow \epsilon$ 来替换 $A$，意味着 $A$ 在当前的语法树分支上被直接抹除（Erased）。**
- **匹配逻辑**：$A$ 一旦消失，$A$ 内部不再产生任何终结符去吃掉（Consume）当前的输入字符。此时，输入流中当前的字符必须与**语法树中紧挨在 $A$ 右侧的那个字符**进行匹配。
- **寻址逻辑**：在整个文法的所有可能句型中，哪些字符有资格紧挨在 $A$ 的右侧？这正是 $\text{FOLLOW}(A)$ 的严格数学定义。
- **结论**：$\text{SELECT}(A \rightarrow \epsilon) = \text{FOLLOW}(A)$。

### 总结

- 算 **$\text{FIRST}$** 集，是**因为产生式有实体内容，我们在看“这条路本身以什么字符开头”。**
- 算 **$\text{FOLLOW}$** 集，是**因为产生式是空串，我们在看“如果这条路不存在，后面紧跟着的字符是什么”。**

### 辅助练习题（Exam-Style Practice）

在期末考试中，通常不会直接让你口述这个原理，而是通过让你计算 SELECT 集（或预测分析表）来隐式考查你对该原理的掌握。

#### 【练习题：计算 SELECT 集并构造分析表】

**考查知识点**：精准应用 SELECT 集的定义规则，区分何时用 FIRST，何时用 FOLLOW。

**题目（Question）**：

给定文法 $G$:

$E \rightarrow TE'$

$E' \rightarrow +TE' \mid \epsilon$

已知：

$\text{FIRST}(TE') = \{id, (\}$

$\text{FIRST}(+TE') = \{+\}$

$\text{FOLLOW}(E') = \{ \$, ) \}$

请计算产生式 $E' \rightarrow +TE'$ 和 $E' \rightarrow \epsilon$ 的 $\text{SELECT}$ 集。并说明如果当前非终结符是 $E'$，且 Lookahead 符号是 `)`，分析器应该执行什么动作。

**解答与思路（Answer & Solution）**：

- **Step 1: 计算非空产生式的 SELECT 集**。

	对于 $E' \rightarrow +TE'$，**右部不能推导出空串。**

	根据定理，直接取其右部的 FIRST 集：

	$\text{SELECT}(E' \rightarrow +TE') = \text{FIRST}(+TE') = \{+\}$

- **Step 2: 计算空产生式的 SELECT 集**。

	对于 $E' \rightarrow \epsilon$，**右部推导出空串。**

	根据定理，取其左部非终结符的 FOLLOW 集：

	$\text{SELECT}(E' \rightarrow \epsilon) = \text{FOLLOW}(E') = \{ \$, ) \}$

- **Step 3: 根据 Lookahead 符号进行决策**。

	当前非终结符为 $E'$，**Lookahead 符号为 `)`。**

	检查 `)` 属于**哪个 SELECT 集：`)` 存在于 $\text{SELECT}(E' \rightarrow \epsilon)$ 中，且不在 $\text{SELECT}(E' \rightarrow +TE')$ 中。**

- **结论**：

	分析器应当确定性地选择产生式 $E' \rightarrow \epsilon$ 进行推导（即让 $E'$ 归为空串，**暴露出后面的结构去匹配 `)`**）。

# 为我详细地解释一下这页内容,我没有看明白

![image-20260624224427254](./scut%E7%BC%96%E8%AF%91%E5%8E%9F%E7%90%86%E5%A4%8D%E4%B9%A01/image-20260624224427254.png)

这页幻灯片讲解的是语法分析（Syntax Analysis）中一个极为重要的基础预处理步骤：**求解可空非终结符集合（Nullable Non-terminals Set）**。

在计算 `FIRST` 集和 `FOLLOW` 集之前，编译器必须**先知道文法中哪些非终结符能够推导出空串（$\epsilon$）。**

### 详细原理解析

#### 1. 核心定义 (Definition)

**可空（Nullable）**：如果**一个非终结符 $A$ 经过零步或多步推导，最终能够生成空串 $\epsilon$（即形式化表示为 $A \Rightarrow^* \epsilon$），那么 $A$ 就是可空非终结符。**

#### 2. 求解算法：不动点迭代 (Fixed-Point Iteration)

图片中给出的是**一个典型的自底向上的迭代算法。假设我们用集合 $S_{null}$ 来存放所有找出来的可空非终结符。**

- **Step 1（初始化边界条件）**：

	直接扫描所有产生式。如果**存在形如 $A \rightarrow \epsilon$ 的直接产生式，就把左部 $A$ 扔进 $S_{null}$ 集合中。**

	*逻辑：**能直接生成空串的，必然是可空的**。*

- **Step 2（依赖传递与推导）**：

	再次**扫描所有产生式，检查形如 $A \rightarrow X_1 X_2 \cdots X_n$ 的规则**。

	**判断条件**：如果**产生式右部的每一个符号（$X_1$ 到 $X_n$）全部都已经存在于 $S_{null}$ 集合中，这就意味着右部所有的符号都可以同时“隐身”变成空串**。因此，左部的 $A$ 也可以变成空串。此时，将 $A$ 加入 $S_{null}$ 集合。

	*(注：如果右部包含哪怕一个终结符，比如 $A \rightarrow Bc$，因为终结符 $c$ 永远不可能变成空串，所以 $A$ 绝对不可能通过这条规则变成可空。)*

- **Step 3（终止条件）**：

	由于 Step 2 可能会发现新的可空符号，**而新符号的加入又可能触发其他符号变成可空。所以必须反复执行 Step 2**，直到某一遍扫描结束后，$S_{null}$ 集合**不再增加任何新元素**为止。

#### 3. 课件例题逐趟追踪 (Trace)

已知文法：

$S \rightarrow AB \mid bC$

$A \rightarrow b \mid \epsilon$

$B \rightarrow aD \mid \epsilon$

$C \rightarrow AD \mid b$

$D \rightarrow aS \mid c$

- **初始（Step 1）**：找到 $A \rightarrow \epsilon$ 和 $B \rightarrow \epsilon$。

	当前集合：$S_{null} = \{A, B\}$

- **第 1 趟迭代（Step 2）**：扫描其他产生式。

	检查 $S \rightarrow AB$：因为 $A \in S_{null}$ 且 $B \in S_{null}$（右部全部可空），所以 $S$ 也可空。将 $S$ 加入集合。

	检查 $C \rightarrow AD$：$A$ 可空，但 $D$ 暂时不可空，所以 $C$ 暂时不能加入。

	当前集合：$S_{null} = \{A, B, S\}$

- **第 2 趟迭代（Step 2）**：再次扫描。

	发现没有新的产生式右部能满足“全部都在 $\{A, B, S\}$ 中”的条件了。集合不再变化，算法停止。

- **最终结果**：可空集合 = $\{A, B, S\}$。

### 辅助练习题（Exam-Style Practice）

求解 Nullable 集合是推导 FIRST 集的绝对前置条件，考查形式通常是直接给出文法让你判断，或者在计算大题的第一步隐式考查。

#### 【练习题 1：基础迭代求解】

**考查知识点**：严格执行三步迭代算法，尤其是多层传递的逻辑。

**题目（Question）**：

给定以下上下文无关文法（CFG），请写出求解可空非终结符集合（Nullable Set）的完整迭代过程与最终结果。

$S \rightarrow ABC \mid a$

$A \rightarrow BC \mid \epsilon$

$B \rightarrow C \mid b$

$C \rightarrow \epsilon \mid c$

**解答与思路（Answer & Solution）**：

- **初始化（Step 1）**：寻找直接生成 $\epsilon$ 的产生式。

	发现 $A \rightarrow \epsilon$ 和 $C \rightarrow \epsilon$。

	$S_{null} = \{A, C\}$

- **第 1 趟（Step 2）**：

	检查 $B \rightarrow C$：右部唯一的符号 $C$ 已经在集合中，故 $B$ 可空，加入 $B$。

	当前 $S_{null} = \{A, B, C\}$

- **第 2 趟（Step 2）**：

	检查 $S \rightarrow ABC$：右部符号 $A, B, C$ 此时**全部**都在集合中了，故 $S$ 可空，加入 $S$。

	当前 $S_{null} = \{S, A, B, C\}$

- **第 3 趟（Step 2）**：

	集合已包含所有非终结符，不可能再扩大，迭代停止。

**最终答案**：

Nullable Set = $\{S, A, B, C\}$

#### 【练习题 2：终结符阻断陷阱（Terminal Blocking）】

**考查知识点**：识别产生式右部中的终结符对可空性的绝对阻断作用。

**题目（Question）**：

给定以下文法，求其可空非终结符集合：

$S \rightarrow ABc \mid D$

$A \rightarrow \epsilon \mid a$

$B \rightarrow A \mid b$

$C \rightarrow AB$

$D \rightarrow d$

**解答与思路（Answer & Solution）**：

- **初始化（Step 1）**：

	仅有 $A \rightarrow \epsilon$。

	$S_{null} = \{A\}$

- **第 1 趟（Step 2）**：

	检查 $B \rightarrow A$：$A \in S_{null}$，加入 $B$。

	当前 $S_{null} = \{A, B\}$

- **第 2 趟（Step 2）**：

	检查 $C \rightarrow AB$：$A, B$ 均在 $S_{null}$ 中，加入 $C$。

	检查 $S \rightarrow ABc$：虽然 $A, B$ 可空，但右部存在**终结符** $c$。终结符永远不可空，所以这条产生式无法让 $S$ 变为空。

	检查 $S \rightarrow D$：$D$ 不在集合中。

	当前 $S_{null} = \{A, B, C\}$

- **第 3 趟（Step 2）**：

	扫描发现无新非终结符可加入。

**最终答案**：

Nullable Set = $\{A, B, C\}$

# 为什么可以推出这个包含关系

在直接左递归（Direct Left Recursion）产生式 $A \rightarrow A\alpha \mid \beta$ 中，能够推导出 $\text{FIRST}(A\alpha) \subseteq \text{FIRST}(\beta)$（实际上**在绝大多数标准文法中二者是完全相等的**），其核心严密逻辑在于**非终结符（Non-terminal）的终结推导（Terminal Derivation）必然依赖于非递归分支**。

以下是最严格的理论推导过程：

### 详细原理解析

1. **推导的终止条件（Termination Condition）**：

	产生式 $A \rightarrow A\alpha$ 是一个无限展开的递归环（$A \Rightarrow A\alpha \Rightarrow A\alpha\alpha \Rightarrow \dots$）。

	如果语法分析器要让这个展开过程最终停止，并生成具体的实实在在的字符，处于最左侧的那个 $A$ **必须在某一步强制结束递归**。

	它结束递归的唯一合法方式，**就是选择使用另一个不以 $A$ 开头的出口产生式，即 $A \rightarrow \beta$。**

2. **句型（Sentential Form）的必然结构**：

	综合上述推导过程，由非终结符 $A$ 生成的所有合法字符串，其最终的物理形态必然是：$\beta \alpha^n$ （其中 $n \ge 0$）。

	这意味着，无论 $A$ 怎么推导，它生成的任何一串符号，**绝对是以 $\beta$ 作为最前面的前缀（Prefix）**。

3. **FIRST 集的数学等价**：

	既然所有**由 $A$ 派生出的字符串都以 $\beta$ 打头，那么 $A$ 的首符号集自然就完全等于 $\beta$ 的首符号集。**

	得出关键推论：$\text{FIRST}(A) = \text{FIRST}(\beta)$。

4. **代回左递归分支**：

	现在我们来看**引起争议的左递归产生式右部：$A\alpha$。**

	**它的 $\text{FIRST}$ 集取决于它的第一个符号 $A$。**

	即：$\text{FIRST}(A\alpha) = \text{FIRST}(A)$。

	代入上一步的推论，立刻得到：$\text{FIRST}(A\alpha) = \text{FIRST}(\beta)$。

**结论（对 LL(1) 的破坏性）：**

在自顶向下解析的 LL(1) 文法（LL(1) Grammar）中，无冲突的绝对准则是：**同一个非终结符的多个候选分支，其 FIRST 集必须互不相交（即交集为空）。**

但在左递归中，分支 1（$A\alpha$）和分支 2（$\beta$）的 **FIRST 集存在包含或相等关系：**

$\text{FIRST}(A\alpha) \cap \text{FIRST}(\beta) = \text{FIRST}(\beta) \neq \emptyset$。

**只要有直接左递归，这两个分支必然发生 FIRST-FIRST 冲突（Conflict），分析器只要看到属于 $\beta$ 的开头字符，就永远无法确定到底是该走递归分支还是出口分支。因此，左递归文法绝对不可能是 LL(1) 文法。**

### 辅助练习题（Exam-Style Practice）

“证明左递归非 LL(1)”以及“识别间接左递归的隐蔽冲突”是期末考试中常考的证明与计算题。

#### 【练习题 1：具体文法的 FIRST 集冲突验证】

**考查深度**：基础。将上述纯理论符号推导应用于具体的运算文法中，强化直观理解。

**题目（Question）**：

给定直接左递归文法：

$E \rightarrow E+T \mid T$

已知 $\text{FIRST}(T) = \{ id \}$。

请分别求出 $E$ 的两个产生式右部（$E+T$ 和 $T$）的 $\text{SELECT}$（或 $\text{FIRST}$）集，并以此证明该文法不是 LL(1) 文法。

**解答与思路（Answer & Solution）**：

- **Step 1: 寻找出口，确定 FIRST(E)**。

	$E$ 必须通过 $E \rightarrow T$ 才能终止推导。故所有 $E$ 生成的串必以 $T$ 开头。

	$\text{FIRST}(E) = \text{FIRST}(T) = \{ id \}$。

- **Step 2: 计算各分支右部的 FIRST 集**。

	- 分支 1 ($E+T$)：由于第一个符号是 $E$，所以 $\text{FIRST}(E+T) = \text{FIRST}(E) = \{ id \}$。
	- 分支 2 ($T$)：$\text{FIRST}(T) = \{ id \}$。

- **Step 3: 检查冲突**。

	$\text{FIRST}(E+T) \cap \text{FIRST}(T) = \{ id \} \cap \{ id \} = \{ id \} \neq \emptyset$。

- **结论**：存在严重的 FIRST-FIRST 冲突。当分析器面临输入符号 $id$ 时，无法确定是该展开为 $E+T$ 还是直接展开为 $T$。因此非 LL(1) 文法。

#### 【练习题 2：间接左递归（Indirect Left Recursion）的冲突暴露】

**考查深度**：进阶大题。间接左递归的破坏力同样源自 FIRST 集的相交，这要求你在不消除左递归的情况下，敏锐识别出潜藏的包含关系。

**题目（Question）**：

给定文法 $G$：

$S \rightarrow Aa \mid b$

$A \rightarrow Sc \mid \epsilon$

这是一个典型的间接左递归文法（$S \Rightarrow Aa \Rightarrow Sca$）。

请计算产生式 $S \rightarrow Aa$ 与 $S \rightarrow b$ 右部的 $\text{FIRST}$ 集，并说明在解析 $S$ 时发生的具体冲突。

**解答与思路（Answer & Solution）**：

- **Step 1: 计算 FIRST(S)**。

	$S$ 可以直接推导出 $b$。如果走 $S \rightarrow Aa$，因为 $A$ 可以是 $\epsilon$，所以可以暴露出 $a$；如果 $A$ 走 $Sc$，首字母与 $S$ 相同。

	推导可知，$\text{FIRST}(S)$ 包含基础字符 $\{ a, b \}$。

- **Step 2: 计算分支 1 FIRST(Aa) 的集合**。

	$\text{FIRST}(Aa)$ 取决于 $A$。

	当 $A \rightarrow Sc$ 时，串变为 $Sca$，首字母集为 $\text{FIRST}(S)$，即包含 $\{ a, b \}$。

	当 $A \rightarrow \epsilon$ 时，串变为 $a$，首字母为 $a$。

	综合得出：$\text{FIRST}(Aa) = \{ a, b \}$。

- **Step 3: 计算分支 2 FIRST(b) 的集合**。

	$\text{FIRST}(b) = \{ b \}$。

- **Step 4: 判定冲突**。

	$\text{FIRST}(Aa) \cap \text{FIRST}(b) = \{ a, b \} \cap \{ b \} = \{ b \} \neq \emptyset$。

	此处完美展现了 $\text{FIRST}(b) \subseteq \text{FIRST}(Aa)$ 的包含关系。

- **结论**：当向前看符号（Lookahead）为 $b$ 时，分析器无法判断是直接采用 $S \rightarrow b$，还是绕远路采用 $S \rightarrow Aa \Rightarrow Sca \Rightarrow bca$。发生 FIRST-FIRST 冲突，该文法非 LL(1)。

# 这里消除左递归具体是怎么做的,为我详细地讲解一下

![image-20260624225855606](./scut%E7%BC%96%E8%AF%91%E5%8E%9F%E7%90%86%E5%A4%8D%E4%B9%A01/image-20260624225855606-1782313137750-1.png)

图片中展示的是编译原理中极其经典、也是必考的一个标准化操作：**消除直接左递归（Eliminating Direct Left Recursion）**。

在自顶向下（Top-Down）的语法分析中，分析器如果遇到左递归，会陷入无限循环的死胡同。因此，我们必须通过一个固定的**数学公式**，将左递归文法等价转换为右递归（Right Recursion）文法。

以下是具体的拆解与应用过程。

### 一、 消除直接左递归的“万能公式”

对于任何包含直接左递归的产生式，我们都可以将其抽象为以下标准形式：

$A \rightarrow A\alpha \mid \beta$

- **$A$**：发生左递归的非终结符。
- **$\alpha$**（Alpha）：紧跟在递归符 $A$ 后面的那一串符号（可以是一个字符，也可以是多个）。
- **$\beta$**（Beta）：**不以 $A$ 开头**的另一条分支（我们称之为“出口”或“基础分支”）。

**转换公式**：引入一个**新的非终结符（通常记为 $A'$），将原产生式改写为以下两条：**

1. $A \rightarrow \beta A'$
2. $A' \rightarrow \alpha A' \mid \epsilon$

**核心逻辑**：左递归原本表达的意思是**“以 $\beta$ 打头，后面跟着零个或多个 $\alpha$”。**新文法完美继承了这个语义：**先推导出 $\beta$，然后跳入 $A'$ 循环，不断向右追加 $\alpha$，最后用 $\epsilon$（空串）结束循环。**

### 二、 对照图片逐步讲解

图片中的文法 $G$ 包含三个非终结符：$E$、$T$、$F$。我们逐一检查并套用公式。

#### 1. 处理非终结符 $E$

原产生式：$E \rightarrow E+T \mid T$

- **套用模板**：
	- 递归符 $A$ 对应 $E$
	- 尾随部分 $\alpha$ 对应 $+T$
	- 出口分支 $\beta$ 对应 $T$
- **代入公式**：
	1. $E \rightarrow T E'$ （即 $A \rightarrow \beta A'$）
	2. $E' \rightarrow +T E' \mid \epsilon$ （即 $A' \rightarrow \alpha A' \mid \epsilon$）
- **结果**：与图片中 $G'$ 的前两条完全一致。

#### 2. 处理非终结符 $T$

原产生式：$T \rightarrow T*F \mid F$

- **套用模板**：
	- 递归符 $A$ 对应 $T$
	- 尾随部分 $\alpha$ 对应 $*F$
	- 出口分支 $\beta$ 对应 $F$
- **代入公式**：
	1. $T \rightarrow F T'$
	2. $T' \rightarrow *F T' \mid \epsilon$
- **结果**：与图片中 $G'$ 的中间两条完全一致。

#### 3. 处理非终结符 $F$

原产生式：$F \rightarrow (E) \mid i$

- **检查**：右部的两个分支 `(E)` 和 `i` 都**不是**以 $F$ 开头的。
- **结论**：这里**不存在左递归，属于安全的产生式，直接照抄保留即可。**

### 辅助练习题（Exam-Style Practice）

在真实的考试中，题目往往不会像公式那样正好只有一个 $\alpha$ 和一个 $\beta$。通常会有多个递归分支和多个出口分支。你需要掌握**广义的消除公式**。

**广义公式**：

若 $A \rightarrow A\alpha_1 \mid A\alpha_2 \mid \beta_1 \mid \beta_2$

则消除后为：

$A \rightarrow \beta_1 A' \mid \beta_2 A'$

$A' \rightarrow \alpha_1 A' \mid \alpha_2 A' \mid \epsilon$

（即将所有的 $\beta$ 后面都接上 $A'$，所有的 $\alpha$ 后面也都接上 $A'$ 形成右递归）。

#### 【练习题 1：多分支直接左递归消除】

**考查知识点**：熟练运用广义公式处理多个 $\alpha$ 和 $\beta$ 的情况。

**题目（Question）**：

请消除以下文法的直接左递归：

$S \rightarrow Sa \mid Sb \mid c \mid d$

**解答与思路（Answer & Solution）**：

- **Step 1: 划分阵营**。

	寻找递归部分（以 $S$ 开头）：$Sa$ 和 $Sb$。提取出 $\alpha_1 = a$ 且 $\alpha_2 = b$。

	寻找出口部分（不以 $S$ 开头）：$c$ 和 $d$。提取出 $\beta_1 = c$ 且 $\beta_2 = d$。

- **Step 2: 构建新入口 $S$**。

	让所有的 $\beta$ 带上新状态 $S'$。

	产生式变为：$S \rightarrow cS' \mid dS'$。

- **Step 3: 构建新循环 $S'$**。

	让所有的 $\alpha$ 带上 $S'$，并加入 $\epsilon$ 作为终结。

	产生式变为：$S' \rightarrow aS' \mid bS' \mid \epsilon$。

**最终标准答案**：

$S \rightarrow cS' \mid dS'$

$S' \rightarrow aS' \mid bS' \mid \epsilon$

#### 【练习题 2：实际编程语言文法消除】

**考查知识点**：在稍微复杂的、包含括号和标识符的实际文法中精准提取 $\alpha$ 和 $\beta$。

**题目（Question）**：

给定描述数组访问和函数调用的文法：

$L \rightarrow L[E] \mid L(E) \mid id$

（其中 $id$ 代表标识符，`[`、`]`、`(`、`)` 均为终结符）。

请将其改写为无左递归的等价文法。

**解答与思路（Answer & Solution）**：

- **Step 1: 识别元素**。

	左递归非终结符为 $L$。

	存在两个左递归分支：$L[E]$ 的尾随部分是 $\alpha_1 = [E]$；$L(E)$ 的尾随部分是 $\alpha_2 = (E)$。

	只有一个非递归出口：$\beta = id$。

- **Step 2: 套用规则**。

	原入口 $L$ 指向基础分支加上新状态 $L'$。

	新循环 $L'$ 处理两个尾随部分加上自身递归，并闭合于 $\epsilon$。

**最终标准答案**：

$L \rightarrow id L'$

$L' \rightarrow [E] L' \mid (E) L' \mid \epsilon$