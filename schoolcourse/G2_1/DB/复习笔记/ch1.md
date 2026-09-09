# Unit 1: Introduction (复习全汇总)

### 1. 核心定义 (Basic Concepts)

- **DBMS (Database Management System)**
	- **定义 (Definition)**: Consists of a **collection of interrelated data** (**互相关联的数据集合**) and a **set of programs to access the data** (**一组访问数据的程序**).
	- **目标 (Goal)**: Provide an environment that is both **convenient** (**方便**) and **efficient** (**高效**).
- **应用场景**: Banking, Airlines, Universities, Sales, HR, etc. (知道数据库无处不在即可).

------

### 2. 文件系统的缺点 (Drawbacks of File Systems) —— ⭐⭐ 必背简答题

*（曾兵笔记强调：这是第一节课重点，**极可能考简答“Why DB over File System?”**）*

请记住这 **7个英文标题**，并能用中文简单解释：

1. **Data redundancy and inconsistency (数据冗余和不一致)**
	- **Duplication of information** (信息重复) in different files leads to waste and inconsistency.
2. **Difficulty in accessing data (访问数据困难)**
	- Need to write a **new program** for each new task. (**没有通用的查询语言**).
3. **Data isolation (数据孤立)**
	- Data scattered in **multiple files and formats** (数据**分散在不同格式的文件中**).
4. **Integrity problems (完整性问题)**
	- **Integrity constraints** (e.g., balance > 0) are "**buried**" in program code (约束条件埋在代码里), hard to change.
5. **Atomicity of updates (更新的原子性)**
	- Failures may leave database in an **inconsistent state** with **partial updates** (**故障导致部分更新**). DBMS ensures atomicity (all or nothing).
6. **Concurrent access anomalies (并发访问异常)**
	- **Uncontrolled concurrent accesses** (**不受控的并发访问**) can lead to inconsistencies (e.g., two people withdrawing money at the exact same time).
7. **Security problems (安全性问题)**
	- Hard to provide user access to **some, but not all**, data (**很难控制细粒度的权限**).

------

### 3. 数据抽象 (Levels of Abstraction) —— ⭐ 选择/填空重点

请务必分清这三层分别描述了什么：

1. **Physical level (物理层)**
	- Describes **how** a record is stored. (描述**数据如何被存储)**.
	- 关键词: complex low-level data structures, storage details.
2. **Logical level (逻辑层)**
	- Describes **what** data stored in database, and the **relationships** among the data. (**描述**存了**什么**数据以及数据间的**关系**).
	- *注意：DBA 和 Programmers 在这一层工作。*
3. **View level (视图层)**
	- Application programs hide details of data types. Views can also hide information for **security** purposes. (**仅描述数据库的一部分**).

------

### 4. 模式与实例 (Schema and Instance)

- **Instance (实例)**: Collection of information stored in the database at a **particular moment**. (**特定时刻的数据快照**，类似于变量的**值**).
- **Schema (模式)**: The **overall design** of the database. (数据库的**总体设计**，类似于变量的**类型**).
	- **Physical Data Independence (物理数据独立性) —— ⭐ 必背定义**
	- Definition: The ability to modify the **physical schema** without changing the **logical schema**.
	- (**修改物理模式而不改变逻辑模式的能力**。例如：换了硬盘存数据，逻辑代码不用改).

------

### 5. 数据库语言 (DDL & DML)

#### **DDL (Data Definition Language)**

- 用于**定义数据库模式 (Schema)**。
- DDL 的输出存放于 **Data Dictionary (数据字典)**。
	- 数据字典包含 **Metadata** (data about data，元数据)。

#### **DML (Data Manipulation Language)**

- 用于 **Access or manipulate data (访问或操作数据)。**
- 分为两类 (**Two types** - 概念辨析):
	1. **Procedural DML (过程化)**:
		- User specifies **what** data are needed and **how** to get those data. (告诉系统要什么，以及**怎么做**).
	2. **Declarative / Non-procedural DML (声明式/非过程化)**:
		- User specifies **what** data are needed **without specifying how** to get those data. (**只告诉系统要什么，不关心怎么做**).
		- **SQL** 属于这一类。

------

### 6. 数据库架构 (Database Architecture)

- **Centralized (集中式)**: 运行在**单台计算机上。**
- **Client-Server (客户-服务器)**:
	- **Two-tier (两层)**: **User** -- Network -- **DB System**. (应用直接连数据库，用**ODBC/JDBC**).
	- **Three-tier (三层)**: User -- Network -- **Application Server** -- Network -- **DB System**.
		- 这是 **Web 应用的标准架构 (Browser -> Web Server -> DB).**

------

### 7. 数据库用户与管理员 (Users & DBA)

- **Database Users**:
	- **Naive users**: Tellers, agents, web users (使用**现成程序的普通用户**).
	- **Application programmers**: Write application programs (**写代码的程序员**).
	- **Sophisticated users**: Analysts using query tools (**使用查询工具的高级分析师**).
- **DBA (Database Administrator)**:
	- 负责 **Schema definition** (**模式定义**), **Granting of authorization** (**授权**), **Routine maintenance** (**维护**) 等。

------

### ✅ **Unit 1 复习自测 (Checklist)**

1. 能不能不看书说出 **Physical Data Independence** 的定义？(**修改物理模式而不改变逻辑模式的能力**)
2. 能不能列举出至少 4 个 **File System 的缺点**？(数据冗余和不一致,访问数据困难,数据孤立,有完整性,更新的原子性,并发访问异常问题)
3. **Logical Level** 描述的是 "How" 还是 "What"？
4. **SQL** 是 Procedural 还是 Declarative？

### **1. 关系模型基础 (Basic Structure)**

请务必掌握以下 **英文术语** 及其对应的 **中文含义**，考试时经常互换出现。

#### **(1) 基本术语 (Terminology)**

- **Relation (关系)**:
	- 对应平时说的 **Table (表)**。
- **Tuple (元组)**:
	- 对应表中的 **Row (行)**。
	- *讲义强调：A relation is a set of tuples (关系是元组的集合)。*
- **Attribute (属性)**:
	- 对应表中的 **Column (列)**。
	- *例子：在 instructor 表中，ID, name, salary 都是属性。*

#### **(2) 域 (Domain) & 原子性 (Atomicity)**

- **Domain (域)**:
	- 定义：The set of allowed values for each attribute. (每个**属性允许取值的集合**)。
	- *例子：salary 的**域是正整数。***
- **Atomic (原子性) —— ⭐ 关键概念**:
	- **定义**：Domain is **atomic** if elements of the domain are considered to be **indivisible units** (如果域中的元素**被视为不可分割的单元，则该域是原子的**)。
	- **考点**：**关系模型**要求**所有属性的值必须是原子的**。**不允许出现 List（列表）或 Set（集合）作为某个格子里的值**。

#### **(3) 空值 (Null Values)**

- **定义**：A special value **null** is a member of every domain. (一个特殊的值 null 是所有域的成员)。
- **含义**：
	1. **Unknown** (未知)
	2. **Value does not exist** (值不存在)
- *注意：null 会给算术和逻辑运算带来麻烦（后面章节会细讲）。*

#### **(4) 关系的无序性 (Relations are Unordered)**

- **考点**：Order of tuples is irrelevant. (元组的**顺序是不相关的)**。
	- *解释：在逻辑层面上，**表里的行谁在前谁在后没有区别（它是集合 Set，不是列表 List）**。*

#### **(5) 模式与实例 (Schema vs. Instance) —— ⭐ 复习回顾**

*(虽然第一章讲过，但第二章会结合具体写法再考一次)*

- **Database Schema (数据库模式)**: 数据库的**逻辑结构 (Logical structure)**。
	- *写法示例*：$instructor(ID, name, dept\_name, salary)$
- **Database Instance (数据库实例)**:
	- 定义：A snapshot of the data in the database at a given instant in time. (在**特定时刻数据库中数据的快照**)。

------

⚠️ 总结 (一句话考点):

看到 Row 选 Tuple，看到 Column 选 Attribute，看到 Indivisible 选 Atomic。

### **2. 码/键 (Keys)**

在关系数据库中，我们需要一种方法来区分不同的元组（行）。

#### **(1) 超码 (Superkey)**

- **英文术语**：**Superkey**
- **讲义定义**：$K$ is a superkey of $R$ if values for $K$ are sufficient to identify a unique tuple of each possible relation $r(R)$.
- **中文作答/理解**：
	- **定义**：一个**或多个属性的集合**，这些属性的组合可以**唯一标识**关系中的一个元组。
	- **特点**：超码**不一定**是最小的，它**可能包含多余的**属性。
	- *例子*：如果 `ID` **能唯一区分老师**，那么 `{ID}` 是超码，**`{ID, name}` 也是超码（哪怕 `name` 是多余的）。**

#### **(2) 候选码 (Candidate Key)**

- **英文术语**：**Candidate** key
- **讲义定义**：Superkey $K$ is a candidate key if $K$ is minimal.
- **中文作答/理解**：
	- **定义**：**最小的超码**。即：**没有任何真子集也是超码**(No proper subset is a superkey)。
	- **判定**：
		1. 它**是超码**（能唯一标识）。
		2. **去掉任何一个属性后，它就不再是超码了。**
	- *例子*：`{ID}` 是候选码。但 `{ID, name}` 不是候选码，因为**去掉了 `name` 后 `{ID}` 依然能唯一标识。**

#### **(3) 主码 (Primary Key)**

- **英文术语**：Primary key
- **讲义定义**：Candidate key chosen by the database designer as the principal means of identifying tuples within a relation.
- **中文作答/理解**：
	- **定义**：**被数据库设计者选中的**、主要用来区分元组的那个**候选码**。
	- **约束**：
		1. 值不能为 **Null**。
		2. 值**必须唯一且不可变**。
	- **书写习惯**：在**关系模式中**，主码通常用**下划线**标出（例如：<u>ID</u>）。

#### **(4) 外码/外键 (Foreign Key) —— ⭐ 必考概念**

- **英文术语**：Foreign key
- **讲义定义**：Attribute in a relation $r_1$ that includes the primary key of another relation $r_2$.
- **中文作答/理解**：
	- **定义**：关系 $r_1$ 中的属性，它**引用了关系 $r_2$ 的主码。**
	- **两个角色**：
		- **Referencing relation (参照关系)**: **含有外键的那个表**（比如 `course` 表引用了 `dept_name`）。
		- **Referenced relation (被参照关系)**: **主码被引用的那个表**（比如 `department` 表）。
	- **参照完整性约束 (Referential Integrity Constraint)**：
		- 要求**参照关系**中**外键取值**，必须**在被参照关系的主码中实际存在（或者为 Null）**。

------

### **⚠️ 考试避坑指南 (来自讲义逻辑)**

1. **Superkey vs Candidate Key**:
	- 题目如果让你 "List all superkeys"（列出所有超码），记得**把带有多余属性的组合也写上**。
	- 题目如果让你 "List candidate keys"（列出候选码），千万**不要**写带有多余属性的组合，否则会扣分。
	- **判定公式**：Candidate Key $\subseteq$ Superkey。所有的候选码都是超码，但不是所有超码都是候选码。
2. **Primary Key的选择**:
	- 虽然一个表可能有多个 Candidate Keys（比如 `ID` 和 `身份证号` 都是唯一的），但在设计时只能选**一个**作为 Primary Key。
3. **中文名词书写**:
	- 请统一写 **“超码”、“候选码”、“主码”、“外码”**。
	- 写“键”（超键、主键）通常也给分，但教材和讲义标准翻译通常用“码”。**建议用“码”字以防万一。**

收到。我们继续严格依据 **《ch2.pdf》 (Chapter 2: Intro to Relational Model)** 的第 13-20 页内容，为你详细总结 **“3. 关系代数 (Relational Algebra)”**。

这是本章**最重要的部分**。根据《课内数据库系统》的提示，这部分和 SQL 结合考察 **30-35分**，核心要求是 **“会写”**。

以下是详细总结，包含了**英文术语**（试卷上看到的）和**中文标准名称**（作答时写的）。

------

### **3. 关系代数 (Relational Algebra)**

**基本概念**：

- 是一种 **过程化查询语言 (Procedural Query Language)**。
- 你需要告诉系统 **“做什么 (What)”** 以及 **“怎么做 (How)”**。
- 运算的对象是关系（表），运算的结果也是关系（表）。

------

#### **A. 六大基本运算 (6 Basic Operations)**

*(这是构建所有查询的基石，必须死记符号和用法)*

| **符号**     | **英文名称 (English)** | **中文名称 (Chinese)** | **核心功能 (Key Function)**                                  |
| ------------ | ---------------------- | ---------------------- | ------------------------------------------------------------ |
| **$\sigma$** | **Select**             | **选择**               | **横向切**：选出满足特定条件谓词的**行 (Tuples)**。          |
| **$\Pi$**    | **Project**            | **投影**               | **纵向切**：选出指定的**列 (Attributes)**。**注意：会自动去重 (Remove duplicates)**。 |
| **$\cup$**   | **Union**              | **并**                 | **合并**：两个表中所有不重复的元组（A或B）。                 |
| **$-$**      | **Set-difference**     | **差**                 | **减法**：在A中但不在B中的元组（A - B）。                    |
| **$\times$** | **Cartesian product**  | **笛卡尔积**           | **全组合**：A的每一行和B的每一行拼接。                       |
| **$\rho$**   | **Rename**             | **更名**               | **改名**：给表或属性重新命名，用于自连接。                   |

**详细说明与书写规范：**

1. **选择运算 (Select, $\sigma$)**
	- **写法**：$\sigma_{p}(r)$
	- **例子**：$\sigma_{salary > 85000}(instructor)$
	- **中文解释**：从 `instructor` 表中找薪水大于 85000 的老师。
	- *注意：谓词 $p$ 可以用 $\wedge$ (and), $\vee$ (or), $\neg$ (not) 连接。*
2. **投影运算 (Project, $\Pi$)**
	- **写法**：$\Pi_{A_1, A_2, ...}(r)$
	- **例子**：$\Pi_{ID, name, salary}(instructor)$
	- **中文解释**：只显示 ID, name 和 salary **这三列。**
	- *考点提醒：结果中**不会**有重复行。如果有两**行数据完全一样，只保留一行**。*
3. **并运算 (Union, $\cup$)**
	- **写法**：$r \cup s$
	- **要求**：两个关系必须 **同构 (Compatible)**。
		1. **属性数量 (Arity) 相同。**
		2. 对应属性的**域 (Domain) 兼容。**
4. **差运算 (Set-difference, $-$)**
	- **写法**：$r - s$
	- **中文解释**：找出在 $r$ 里有，但在 $s$ 里没有的。
	- **要求**：**必须同构。**
5. **笛卡尔积 (Cartesian product, $\times$)**
	- **写法**：$r \times s$
	- **结果**：如果 $r$ 有 $n_1$ 行， $s$ 有 $n_2$ 行，结果就有 $n_1 \times n_2$ 行。
	- *注意：通常**没什么实际意义，除非配合 $\sigma$ (Select) 使用。***
6. **更名运算 (Rename, $\rho$)**
	- **写法**：$\rho_{x}(E)$ 把**表达式 E 的结果重命名为 x。**
	- **进阶写法**：$\rho_{x(A_1, A_2, ...)}(E)$ 把**表名改为 x，同时把列名改为 $A_1, A_2...$。**
	- *必考场景：**自连接 (Self-Join)**。比如“找出薪水最高的员工”，你**需要把 employee 表 $\times$ employee 表**，这时候**必须用 $\rho$ 给其中一个改名。***

------

#### **B. 附加运算 (Additional Operations)**

*(这些运算**可以用基本运算推导出来，但直接用更方便**，考试强烈建议直接用)*

1. **交运算 (Set-intersection, $\cap$)**
	- **写法**：$r \cap s$
	- **中文解释**：既在 $r$ 中，又在 $s$ 中的元组。
	- *推导公式（选择题考点）：$r \cap s = r - (r - s)$*
2. **自然连接 (Natural Join, $\bowtie$) —— ⭐⭐⭐ 绝对核心**
	- **写法**：$r \bowtie s$
	- **逻辑**：
		1. 找到两个表中**名字相同的所有属性。(而且一行的所有同名属性要全部对应也要相等,不能只单个同名属性值相等就连接)**
		2. 让这些属性的**值相等。**
		3. **去掉重复的属性列** (只保留一份)。
	- *优势：*比写 $$r \bowtie s \equiv \Pi_{r.*, s.非同名属性}(\sigma_{\text{所有同名属性都相等}}(r \times s))$$简单*得多，不易出错。*
3. **赋值 (Assignment, $\leftarrow$)**
	- **写法**：$temp \leftarrow r \times s$
	- **作用**：对于复杂的查询，**可以分步写**，把中间结果存到临时变量 $temp$ 中。
	- *建议：如果大题很复杂，尽量分步写，逻辑清晰，老师容易给分。*

------

#### **C. 扩展运算 (Extended Operations)**

1. **广义投影 (Generalized Projection)**
	- 允许**在 $\Pi$ 的下标里写算术表达式。**
	- **例子**：$\Pi_{ID, salary/12}(instructor)$ —— 查询月薪。
2. **聚合函数 (Aggregation, $\mathcal{G}$)**
	- **符号**：**花体的 G ($\mathcal{G}$)。**
	- **写法**：$_{group\_by\_cols} \mathcal{G}_{function(col)}(r)$
	- **常用函数**：`sum`, `count`, `avg`, `min`, `max`。
	- **例子**：$_{dept\_name} \mathcal{G}_{avg(salary)}(instructor)$
	- **中文解释**：按 `dept_name` 分组，计算每个系的平均薪水。

------

### **⚠️ 做题避坑指南 (Exam Tips)**

1. **符号别画错**：
	- $\sigma$ (Select) 是找行。
	- $\Pi$ (Project) 是找列。
	- 这两个千万别搞反！
2. **书写顺序**：
	- **SQL** 是 `SELECT ... FROM ... WHERE ...`
	- **关系代数**是 $\Pi_{...}(\sigma_{...}(...))$
	- **注意**：**SQL 的 `SELECT` 对应关系代数的 `Project` ($\Pi$)，SQL 的 `WHERE` 对应关系代数的 `Select` ($\sigma$)。**这个**命名差异是历史遗留问题**，一定要清醒。
3. **大题技巧**：
	- 题目：“Find names of instructors...”
	- **起手式**：$\Pi_{name}(...)$
	- **中间层**：用 **$\bowtie$ 把相关表连起来**。
	- 最**内层/条件**：用 **$\sigma$ 过滤。**
	- **标准模板**：$\Pi_{目标列}(\sigma_{筛选条件}(表1 \bowtie 表2))$

### **⚠️ 复习建议 (来自曾兵老师与真题)**

1. **符号别写错**：$\sigma$ (Select) 和 $\Pi$ (Project) 是最容易搞混的，一定要分清！$\sigma$ 是横着切（挑行），$\Pi$ 是竖着切（挑列）。
2. **RA表达式书写**：
	- 如果题目问 "**Find names** of employees..."，你的表达式**最外层一定是 $\Pi_{name}$。**
	- 如果**涉及多张表**，优先考虑 **Natural Join ($\bowtie$)**，**比用 Cartesian Product ($\times$) 再写 Select ($\sigma$) 要简洁得多，也是得分点。**
	- **重点练习**：**自连接（Self-Join）**。例如“找出薪水比这一系平均薪水高的人”，需要**把表 $\rho$ 重命名一份再 Join。**
3. **英文术语**：
	- 题目通常会说 "Find the **Schema**" 或者 "Identify the **Candidate Keys**"，请务必对上号。

收到。根据你上传的 **《ch2.pdf》 (Chapter 2: Intro to Relational Model)** 的第 6 页和第 12 页相关内容，为你详细总结 **“4. 处理空值 (Null Values)”**。

这一部分在第二章只是引入概念，内容不多，但在选择题和判断题中是个“坑点”。

以下是严格依据讲义的详细总结：

### **4. 处理空值 (Null Values)**

#### **(1) 定义 (Definition)**

- **英文术语**：**null**
- **中文含义**：**空值**
- **讲义原文**：A special value null is a member of every domain. (一个特殊的值 null 是所有域的成员。)
- **代表意义**：
	1. **Unknown** (值未知)
	2. **Value does not exist** (值不存在)

#### **(2) 对算术运算的影响 (Operations with Null)**

- **规则**：任何**涉及 null 的算术运算，结果都是 null。**
- **英文表述**：The result of any arithmetic expression involving null is null.
- **例子**：
	- $5 + \text{null} = \text{null}$
	- $\text{null} - \text{null} = \text{null}$ (注意：不是0)

#### **(3) 对比较运算的影响 (Comparisons with Null)**

- **规则**：**涉及 null 的比较运算，结果通常是 Unknown (既不是 True 也不是 False)。**
- **例子**：
	- $5 < \text{null}$ 的结果是 **Unknown**。
	- $\text{null} = \text{null}$ 的结果在标准 SQL 中通常也是 **Unknown** (虽然第二章讲义没展开讲三值逻辑，但要记住不能简单划等号)。

#### **(4) 考试避坑点**

- **主码约束**：**Primary Key (主码)** 的属性值**绝不允许**为 null。
- **外码约束**：**Foreign Key (外码)** 的属性值**允许**为 null (表示该引用暂时不存在)。

------

### **⚠️ 总结 (一句话记忆)**

Null 像个黑洞，**算术碰上它变 Null，比较碰上它变 Unknown**。**主码不能有它，外码可以**有它。

# 疑问

### 题目场景

假设有一张表：$instructor(ID, name, dept\_name, salary)$

目标：找出薪水比该系平均薪水高的教师姓名。

------

### 第一步：计算每个系的平均薪水

你需要**用 聚合函数 ($\mathcal{G}$) 算出平均值，并把它存成一个临时表（或者用重命名）。**

关键点：必须把**算出来的 avg(salary) 改个名（比如叫 avg_salary）**，否则后面没法跟原来的 salary 做比较。

- 表达式：

	

	$$temp\_avg \leftarrow \rho_{temp\_avg(dept\_name, avg\_salary)} (_{dept\_name}\mathcal{G}_{avg(salary)}(instructor))$$

	- **解释**：
		1. $_{dept\_name}\mathcal{G}_{avg(salary)}(instructor)$：**按系分组，算出平均工**资。
		2. $\rho_{temp\_avg(...)}$：把这个**结果表重命名为 `temp_avg`，并且把列名强制指定为 `dept_name` 和 `avg_salary`。**

### 第二步：将原表与平均值表连接 (Join)

现在你**有两张表：**

1. $instructor(ID, name, dept\_name, salary)$
2. $temp\_avg(dept\_name, avg\_salary)$

因为它们都有 `dept_name`，我们可以直接用 **自然连接 ($\bowtie$)**。这会将每个老师和**他所在系的平均工资**拼在同一行。

- 表达式：

	

	$$instructor \bowtie temp\_avg$$

### 第三步：筛选与投影 (Select & Project)

现在每一行都有 `salary`（个人）和 `avg_salary`（系平均），**直接比较**即可。

- **最终完整表达式**：

	$$\Pi_{name} ( \sigma_{salary > avg\_salary} ( instructor \bowtie temp\_avg ) )$$

------

### ⚠️ 考试中的替代写法 (如果不允许用赋值 $\leftarrow$)

如果考试**要求写成一行（不准用中间变量）**，你需要把**第一步嵌进去**：

$$\Pi_{name} \left( \sigma_{salary > avg\_salary} \left( instructor \bowtie \rho_{(dept\_name, avg\_salary)} (_{dept\_name}\mathcal{G}_{avg(salary)}(instructor)) \right) \right)$$

------

### 💡 补充：纯粹的“自连接” (Self-Join without Aggregation)

虽然你问的是“比平均高”，但 **$\rho$ (Rename)** 最常考的其实是另一种**不需要计算平均值**的场景，比如 **“找出薪水比 Einstein 高的人”**。这种题**完全依赖 $\times$ 和 $\rho$。**

**写法（供参考）：**

1. 把**表复制两份：$T1$ 和 $T2$。**
2. 条件：$T1.salary > T2.salary$ 并且 $T2.name = 'Einstein'$。

$$\Pi_{T1.name} ( \sigma_{T1.salary > T2.salary \wedge T2.name='Einstein'} ( \rho_{T1}(instructor) \times \rho_{T2}(instructor) ) )$$

**总结**：

- **只要涉及“表自己和自己比”**（比如**比平均、比某人**），一定离不开 **$\rho$ (Rename)**。
- **如果是“比平均”**，一定离不开 **$\mathcal{G}$ (Aggregation)**。

## 要是有选T1.name和T2.name会怎么样,不会冲突吗

### 1. 为什么中间过程不会冲突？

当你做 $\rho_{T1}(instructor) \times \rho_{T2}(instructor)$ 时，产生的**中间大表（笛卡尔积结果）**会**自动使用 限定名 (Qualified Names) 来消除歧义。**

这张中间表的 Schema（模式）长这样：

{ T1.ID, T1.name, ..., T2.ID, T2.name, ... }

因为**带有前缀 T1. 和 T2.，**所以它们被视为不同的属性名。

### 2. 投影 $\Pi_{T1.name, T2.name}$ 后结果表长什么样？

如果你在投影操作中同时选中了这两列：



$$\Pi_{T1.name, T2.name} ( ... )$$

**结果表（Result Relation）为了避免“列名冲突”，**通常采取以下两种方式之一（取决于具体的理论约定，但教材通常倾向于第一种）：

- 方式 A（**保留前缀**）：

	结果表的列名直接保留限定符。

	表头： | T1.name | T2.name |

	这样就不冲突了。

- 方式 B（位置区分/需重命名）：

	严格的数学定义可能会说这形成了一个未命名的元组对。但在数据库书写和考试中，如果你不显式改名，我们默认它就叫 T1.name 和 T2.name。

------

### ⚠️ 考试时的“满分写法”建议

虽然写 $\Pi_{T1.name, T2.name}$ 通常**会被判对**，但为了展示你对“避免歧义”的深刻理解，或者**如果题目明确要求了输出列的名字（比如 "Output schema: (Employee, Manager)"），你应该在最外层再套一个 $\rho$ (Rename)。**

**场景**：找出员工 (T1) 和他的经理 (T2) 的名字。

写法 1 (标准，一般不会扣分)：



$$\Pi_{T1.name, T2.name} ( ... )$$



**默认列名为 T1.name, T2.name**

写法 2 (高分，严谨，特别是当列名本身很长或容易混淆时)：



$$\rho_{(emp\_name, mgr\_name)} ( \Pi_{T1.name, T2.name} ( ... ) )$$



显式将结果的两列重命名为 emp_name 和 mgr_name，彻底消除歧义。

### 总结

- **会冲突吗？** 如果去掉了前缀 `T1/T2` 直接叫 `name`，**会冲突**。
- **怎么解决？** 关系代数机制会保留 **前缀 (T1.name)** 来区分，或者强迫你 **重命名 ($\rho$)**。
- **考试怎么办？** 直接写 $\Pi_{T1.name, T2.name}$ 是安全的，阅卷老师知道你指的是哪两列。

# ch3

## 数据定义语言 (Data Definition Language - DDL)

------

### **1. 常用数据类型 (Domain Types)**

*(考试时你需要**根据题目描述选择正确的类型**，只需记住这几个最常用的)*

- **`char(n)`**: **固定长度字符串** (Fixed length)。
	- *适用场景*：身份证号、学号、性别（'M'/'F'）。
	- *例子*：`ID char(5)`
- **`varchar(n)`**: **可变长度字符串** (Variable length)。
	- *适用场景*：姓名、地址、系名。**这是最常用的字符串类型。**
	- *例子*：`name varchar(20)`
- **`int`** (or `integer`): **整数**。
	- *适用场景*：年份、数量。
- **`numeric(p, d)`**: **定点数** (Fixed point number)。
	- *定义*：总共有 **p** 位数字，其中小数点后有 **d** 位。
	- *适用场景*：**薪水 (Salary)**、价格。
	- *例子*：`numeric(8, 2)` 表示**最多 8 位数字，其中 2 位是小数**（例如：123456.78）。
- **`real` / `double precision`**: **浮点数**。

------

### **2. 创建表 (Create Table) —— ⭐ 必考题型**

*(题目通常会给你一个关系模式，让你写出创建表的 SQL 语句，并要求包含完整性约束)*

#### **基本语法**

```
create table 表名 (
    列名1 数据类型,
    列名2 数据类型,
    ...,
    完整性约束1,
    完整性约束2
);
```

#### **三大完整性约束 (Integrity Constraints)**

*(讲义 Slide 8 重点强调，必须会写)*

1. **非空约束 (`not null`)**
	- **写法**：**直接跟在属性定义后**面。
	- *例子*：`name varchar(20) not null`
2. **主码约束 (`primary key`)**
	- **写法**：`primary key (属性名1, 属性名2, ...)`
	- *注意*：如果是**联合主码（由多个属性组成）**，必须**写在属性定义的最后，括号里写全所有属性。**
3. **外码约束 (`foreign key`)**
	- **写法**：`foreign key (本表**属性**名) reference**s** 外**表**名`
	- *注意*：千万别漏了 **`references`** 这个词！

#### **满分答题模板 (以讲义中的 Instructor 表为例)**

如果题目让你创建 `instructor` 表，要求 ID 为主码，dept_name 引用 department 表，name 不能为空。

**你应该这样写：**

```sql
create table instructor (
    ID          char(5),
    name        varchar(20) not null,
    dept_name   varchar(20),
    salary      numeric(8,2),
    primary key (ID),
    foreign key (dept_name) references department
);
```

*(注：虽然 foreign key **也可以指定被引用表的列名**，如 `references department(dept_name)`，但讲义 Slide 8 的示例简化为 `references department`，意味着默**认引用对方的主码**。考试时**写全 `references department(dept_name)` 更保险**，或者照着讲义写也行。)*

------

### **3. 修改与删除表 (Drop & Alter)**

*(这部分通常考填空或简单的单行代码题)*

- **删除整个表 (Drop Table)**
	- **语法**：`drop table r;`
	- **含义**：删除表 `r` 的**所有数据**以及**表结构**（Schema）。删完之后这表就**不存在了。**
	- *辨析*：`delete from r` 只是**删光数据，表结构还在。**
- **添加属性 (Add Attribute)**
	- **语法**：`alter table r add A D;`
	- *例子*：`alter table instructor add bonus numeric(8,2);` (给老师**表加个奖金列**)
	- *注意*：**新加的列**，现有数据的该列值会**自动设为 `null`。**
- **删除属性 (Drop Attribute)**
	- **语法**：`alter table r drop A;`
	- *例子*：`alter table instructor drop salary;`
	- *讲义注*：**很多数据库其实不支持**这个操作，但**标准 SQL 里有。**

------

### **⚠️ 做题避坑指南**

1. **标点符号**：每个定义后面是**逗号 (,)**，**最后一句后面不需要逗号**，**整个语句结束要加分号 (;)。**
2. **主码括号**：`primary key` 后面**一定要加括号**，写成 `primary key (ID)`，哪**怕只有一个属性也要加括号**。
3. **顺序**：建议**先定义所有列**，再**在最后统一写 `primary key` 和 `foreign key` 约束**，这样逻辑清晰，不容易出错。
4. **单词拼写**：
	- 是 `varchar` 不是 `varcahr`。
	- 是 `numeric` 不是 `number`。
	- 是 `foreign key` ... `references` (注意 references **有 s)。**

掌握以上这些，第一部分 DDL 的题目就能拿满分了。接下来我们看查询部分。

### **2. 基本查询结构 (Basic Query Structure)**

一个典型的 SQL 查询由三个子句组成：

```
select A1, A2, ..., An
from r1, r2, ..., rm
where P;
```

#### **(1) Select 子句 (The Select Clause) —— 选列**

- **对应关系代数**：**投影运算 ($\Pi$, Projection)。**
- **核心考点**：
	1. **去重 (Duplicates)**：
		- SQL 标准**默认保留重复**行（这点**和关系代数不同！就是说因为对应的是投影**）。
		- **`select distinct`**：**强制去除重复行。**
		- **`select all`**：**显式指定保留重复（通常省略不写）**。
		- *做题技巧*：如果题目说 "Find **all** branches..."，通常**不需要 distinct**；如果说 "Find the **set** of..." 或**明确要求无重复**，必须**加 `distinct`。**
	2. **选所有列**：
		- **`select \*`**：表示选择 `from` 子句结果中的所有属性。
	3. **算术表达式**：
		- 可以在 select **子句中进行加减乘除。**
		- *例子*：`select ID, name, salary * 1.1 from instructor` (显示**涨薪后的结果**，但**不修改**数据库原值)。

#### **(2) From 子句 (The From Clause) —— 选表**

- **对应关系代数**：笛卡尔积 ($\times$, Cartesian Product)。
- **核心考点**：
	- 列出查询中涉及的所有关系（表）。
	- *做题警告*：如果你在 `from` 里写了两个表（如 `from instructor, teaches`），但在 `where` 里**忘了写连接条件**，结果就是**巨大的笛卡尔积**（行数 = 表1行数 × 表2行数），这通常是**零分**的。

#### **(3) Where 子句 (The Where Clause) —— 选行**

- **对应关系代数**：**选择谓词 ($\sigma$, Selection Predicate)。**
- **核心考点**：
	1. **逻辑连接词**：`and`, `or`, `not`。
	2. **比较运算符**：`<`, `<=`, `>`, `>=`, `=`, **`<>` (不等于)。**
	3. **Between 谓词**：`where salary between 90000 and 100000` (**包含边界**值)。

------

### **⚠️ 必考难点：多表查询 (Joins)**

*(这是做题时最容易出错的地方，请仔细看)*

当查询涉及多个表时，你有两种写法，**建议考试时只用一种你最熟练的**，通常推荐用 **`Where` 子句隐式连接**，或者 **`Natural Join`**（如果列名设计规范）。

#### **写法 A：使用 Where 子句 (隐式连接)**

这是**最通用、最不容易出错**的写法。

- **场景**：找出所有老师的名字以及他们所在的系所在的建筑。

- **分析**：

	- 老师名字在 `instructor` 表。
	- 建筑在 `department` 表。
	- 连接点：两个表都有 `dept_name`。

- **代码**：

	SQL

	```
	select name, building
	from instructor, department
	where instructor.dept_name = department.dept_name;
	```

- **注意**：如果有**重名列（如 `dept_name`），必须用 `表名.属性名` 来区分。**

#### **写法 B：使用 Natural Join (自然连接)**

- **语法**：`select ... from r1 natural join r2`
- **原理**：**自动让所有(不是单个,除非指定)同名属性相等**，并**自动去重列**。
- **优点**：代码简洁，不需要写 `where instructor.dept_name = ...`。
- **致命陷阱 (Danger)**：
	- 如果两个表**碰巧有你不想要的同名属性（例如都有 `name`，一个是人名一个是系名），`natural join` 会强行让它们相等**，导致查询结果为空或错误。
- **改进版 —— Using 子句 (安全写法)**：
	- 如果**你想用 Join (笛卡尔积)但怕出错**，可以**用 `using/on` 指定连接列(这样就不会默认连接所有同名属性了(自动连接))。**
	- **语法**：`select ... from r1 join r2 using (dept_name)` 手动连接,但是这样**不可以再用natural修饰了**
	- *注意*：`using` 里的**属性不需要加表名前缀**。

------

### **📝 做题实战套路 (Recipe for Success)**

遇到一个查询题（比如：找出所有在 'Watson' 大楼工作的老师的名字），按以下步骤思考：

1. **Target (要什么)**：名字 $\rightarrow$ `select name`
2. **Source (在哪找)**：
	- 老师表 `instructor`
	- 系表 `department` (因为 'Watson' 是建筑，只有系表里有建筑信息)
	- $\rightarrow$ `from instructor, department`
3. **Condition (条件)**：
	- **连接条件** (必写！)：`instructor.dept_name = department.dept_name`
	- **筛选条件**：`department.building = 'Watson'`
	- $\rightarrow$ `where instructor.dept_name = department.dept_name and building = 'Watson'`

**最终答案**：

```
select name
from instructor, department
where instructor.dept_name = department.dept_name
  and building = 'Watson';
```

***(分号别忘了)***

------

### **总结**

- **Select** = 投影 ($\Pi$)
- **From** = 笛卡尔积 ($\times$)
- **Where** = 选择 ($\sigma$) + **连接条件 (Join Condition)**
- **多表必写 Where 连接**，否则零分。

收到。我们严格按照 **《ch3.pdf》 (Chapter 3: Introduction to SQL)** 的第 26-37 页内容，为你详细总结 **“3. 附加基本操作 (Additional Basic Operations)”**。

这一部分虽然叫“附加”操作，但在考试中（尤其是字符串匹配和自连接）是非常重要的**得分点**。

以下是针对做题的详细复习总结：

------

### **3. 附加基本操作 (Additional Basic Operations)**

#### **(1) 更名运算 (The Rename Operation) —— ⭐⭐ 必考：自连接**

在 SQL 中，我们可以给**表 (Relation)** 或 **属性 (Attribute)** 起别名。

- **语法**：`old_name as new_name`

	- *注*：在某些数据库（如 Oracle）中**，`as` 可以省略**，但**考试请按标准写上 `as`**。

- **场景 1：给列改名 (显示好看)**

	- `select name as instructor_name, course_id from instructor, teaches ...`
	- 这样查询结果的表头就会显示 `instructor_name` 而不是 `name`。

- **场景 2：给表改名 (核心考点：自连接 Self-Join)**

	- **题目类型**：需要比较同一张表内不同行的数据。

	- **经典真题**：“找出所有薪水比 'Biology' 系某个老师高的老师的姓名。”

	- **解题逻辑**：你需要把 `instructor` 表想象成两张表，一张叫 $T$，一张叫 $S$。

	- **代码模板**：

		```
		select distinct T.name
		from instructor as T, instructor as S
		where T.salary > S.salary and S.dept_name = 'Biology';
		```

	- **避坑指南**：**如果不使用 `as T` 和 `as S`，你就无法区分**你在比较哪两个人的薪水。**自连接必须用 `as` 重命名。**

------

#### **(2) 字符串运算 (String Operations) —— ⭐ 选择/填空必考**

SQL **使用 `like` 操作符来进行字符串模式匹配。**

- **两个特殊字符 (必背)**：
	1. **百分号 (`%`)**：匹配 **任意子串 (any substring)**。
		- `'Intro%'`：匹配**以 "Intro" 开头的任何**字符串。
		- `'%Comp%'`：匹配**包含 "Comp" 的任何**字符串（如 "Intro to Computer Science"）。
	2. **下划线 (`_`)**：匹配 **任意一个字符 (any character)**。
		- `'_ _ _'` (三个下划线)：匹配**长度恰好为 3** 的字符串。
		- `'___%'` (三个下划线**加百分号**)：匹配长度**至少为 3** 的字符串。
- **转义字符 (Escape Characters)**：
	- 如果我要找**名字里真的包含 `%` 的人**怎么办？
	- **语法**：`like 'ab\%cd%' escape '\'` escape在这里是**转义字符**
	- *解释*：**定义 `\` 为转义符**，所以 `\%` 代表真的百分号，而最后的 `%` 还是代表任意子串。
- **其他字符串函数 (了解即可)**：
	- `concatenation` (拼接, `||`), `upper()` (转大写), `lower()` (转小写), `char_length()` (长度)。

------

#### **(3) 排列元组的显示次序 (Ordering the Display of Tuples)**

让查询结果按特定顺序排列，默认是**乱序**的。

- **子句**：**`order by`**
- **顺序关键字**：
	- **`desc`** (**Descending**)：**降序**（从大到小）。
	- **`asc`** (**Ascending**)：**升序**（从小到大）。**默认值**，如果不写就是升序。
- **多重排序**：
	- **例子**：`order by dept_name, salary desc`
	- *含义*：先按系名升序排；如果系名一样，再按薪水降序排。

------

#### **(4) Where 子句谓词 (Where Clause Predicates)**

除了基本的 `=`, `<`, `>`，还有更高级的写法。

- **范围查询 (`between ... and ...`)**
	- **语法**：`where salary between 90000 and 100000`
	- **考点**：它是**包含边界 (Inclusive)** 的！等价于 `90000 <= salary <= 100000`。
	- *反义*：`not between ... and ...`
- **元组比较 (Tuple Comparison)**
	- **语法**：`where (instructor.ID, old_dept_name) = (teaches.ID, new_dept_name)`
	- *解释*：**允许直接比较一组值**。等价于 `instructor.ID = teaches.ID and old_dept_name = new_dept_name`。

------

### **📝 做题避坑指南**

1. **大小写敏感**：在 SQL 中，字符串内容（如 `'Biology'`）通常是**大小写敏感**的。写代码时不要把 `'Biology'` 写成 `'biology'`。
2. **Order By 的位置**：`order by` 永远是 SQL 语句的**最后一句**。
	- 错：`select ... order by ... where ...`
	- 对：`select ... where ... order by ...`
3. **Like 的效率**：虽然 `like` 很强大，但如果题目**只需精确匹配（**如找系名是 'Music' 的），请**直接用 `=`，**不要用 `like`。

### **4. 集合运算 (Set Operations)**

**核心规则（死记硬背）**：

1. 所有**集合运算**默认都是 **自动去重 (Automatically eliminate duplicates)** 的。
2. 如果你想**保留重复**，必须在后面加 **`all`**。

我们假设有两个查询结果集合：

- **Query 1**: 找出 2009 年秋季开课的课程 ID。

	```sql
	(select course_id from section where semester = 'Fall' and year = 2009)
	```

- **Query 2**: 找出 2010 年春季开课的课程 ID。

	```sql
	(select course_id from section where semester = 'Spring' and year = 2010)
	```

------

#### **(1) 并运算 (The Union Operation)**

- **英文术语**：`union`
- **中文含义**：**并集**（找出在 Query 1 **或者** 在 Query 2 中出现的课程）。
- **去重规则**：默认**去重**。

**【考试真题范例】**

- **题目**：Find courses that ran in Fall 2009 **or** in Spring 2010. (找出在09年秋季**或**10年春季**开设的课程**)

- **SQL 写法**：

	```
	(select course_id from section where semester = 'Fall' and year = 2009)
	union
	(select course_id from section where semester = 'Spring' and year = 2010);
	```

- **结果分析**：如果 `CS-101` 在**两个学期都开了**，结果中**只会出现一次** `CS-101`。

**【保留重复版本：Union All】**

- **题目**：找出所有开设的课程次（**包括重复的**）。

- **SQL 写法**：

	```
	(select ... )
	union all
	(select ... );
	```

- **结果分析**：如果 `CS-101` 两**个学期都开了**，结果中会出现**两次** `CS-101`。

------

#### **(2) 交运算 (The Intersect Operation)**

- **英文术语**：`intersect`
- **中文含义**：**交集**（找出**既**在 Query 1 **又** 在 Query 2 中出现的课程）。
- **去重规则**：默认**去重**。

**【考试真题范例】**

- **题目**：Find courses that ran in Fall 2009 **and** in Spring 2010. (找出09年秋季**和**10年春季**都**开设了的课程)

- **SQL 写法**：

	```
	(select course_id from section where semester = 'Fall' and year = 2009)
	intersect
	(select course_id from section where semester = 'Spring' and year = 2010);
	```

- **结果分析**：只有同时出现在两个学期的课才会被选出来，且每门课只显示一次。

**【保留重复版本：Intersect All】**

- **概念**：如果 Query 1 中有 4 个 `CS-101`，Query 2 中有 2 个 `CS-101`，那么 `intersect all` 的结果中会有 `min(4, 2) = 2` 个 `CS-101`。
- *注意：`intersect all` 在实际考试中考得较少，但要知道这个概念。*

------

#### **(3) 差运算 (The Except Operation)**

- **英文术语**：`except`
- **中文含义**：**差集**（在 Query 1 中有，**但不在** Query 2 中的课程）。
- **去重规则**：默认**去重**。
- *注意：Oracle 数据库用 `MINUS`，但你的讲义是标准 SQL，**考试请写 `except`**！*

**【考试真题范例】**

- **题目**：Find courses that ran in Fall 2009 **but not** in Spring 2010. (找出09年秋季开了**但在**10年春季**没开**的课程)

- **SQL 写法**：

	```
	(select course_id from section where semester = 'Fall' and year = 2009)
	except
	(select course_id from section where semester = 'Spring' and year = 2010);
	```

- **结果分析**：也就是从 09 秋的课表中，**剔除掉那些 10 春也开过的课。**

**【保留重复版本：Except All】**

- **概念**：如果 Query 1 有 4 个 `CS-101`，Query 2 有 2 个 `CS-101`，`except all` 的结果会有 `max(0, 4-2) = 2` 个 `CS-101`。

------

### **⚠️ 做题避坑指南 (Exam Tips)**

1. **Select vs Set Operations (去重陷阱)**：
	- `Select` 子句默认是 **保留** 重复的。
	- 集合运算 (`union`, `intersect`, `except`) 默认是 **去除** 重复的。
	- *考试填空题常考：如果**要保留重复**，必须**显式写 `ALL`。***
2. **列的兼容性 (Compatibility)**：
	- 进行集合运算的两个 `Select` 语句，选出的**列数必须相同**，且**对应列的数据类型必须兼容**。
	- *错误写法*：`(select ID, name ...) union (select ID ...)` —— **列数不同，直接报错。**
3. **替代写法**：
	- `intersect` 通常**可以用 `where ... in (...)` 替代。**
	- `except` 通常**可以用 `where ... not in (...)` 替代。**
	- *建议*：如果题目**没有强制要求用集合运算，写 `in` / `not in` 通常更符合直觉**，但如果题目明确说 **"Using set operations"**，则必须用上面的写法。

### **5. 空值 (Null Values)**

#### **(1) 核心概念与算术运算**

- **空值 (Null)**：表示值未知 (Unknown) 或不存在 (Does not exist)。
- **算术运算 (Arithmetic Operations)**：
	- **规则**：任何涉及 `null` 的算术表达式结果都是 `null`。
	- **公式**：$5 + \text{null} = \text{null}$

#### **(2) 比较运算与三值逻辑 (Comparison & Three-Valued Logic) —— ⭐ 考试难点**

在 SQL 中，逻辑结果不仅仅是 `true` 和 `false`，还有第三种状态：**`unknown`**。

- **比较规则 (Comparison)**：

	- 任何涉及 `null` 的比较运算（如 `=`, `<`, `>`, `<>`），结果都是 **`unknown`**。
	- **例子**：
		- `5 < null` $\rightarrow$ **`unknown`**
		- `null = null` $\rightarrow$ **`unknown`** (注意：不是 true！)
		- `null <> null` $\rightarrow$ **`unknown`**

- **布尔逻辑规则 (Boolean Operations)：**

	**当 and, or, not 遇到 unknown 时**，遵循以下逻辑（必须背下来，做题全靠它）：

	1. **`AND` 运算**：
		- `true and unknown` = **`unknown`**
		- `false and unknown` = **`false`** (因为 **false 和任何东西都是 false**)
		- `unknown and unknown` = **`unknown`**
	2. **`OR` 运算**：
		- `true or unknown` = **`true`** (因为 **true 或任何东西都是 true**)
		- `false or unknown` = **`unknown`**
		- `unknown or unknown` = **`unknown`**
	3. **`NOT` 运算**：
		- `not unknown` = **`unknown`**

#### **(3) Where 子句的判定规则 —— ⭐ 做题死穴**

- **规则**：`Where` 子句中的谓词 $P$ 只有在结果为 **`true`** 时，该行才会被选中。
- **推论**：如果结果是 `false` 或者 **`unknown`**，该行都**不会**被添加到结果集中。

#### **(4) 检测空值 (Testing for Null Values)**

因为 `null = null` 返回 `unknown`，所以**你不能用 `=` 来检查空值。你必须使用专门的语法：**

- **`is null`**：检查值是否为空。
- **`is not null`**：检查值是否不为空。

------

### **📝 综合实战案例 (Complete Example)**

场景描述：

假设有一张教师表 instructor，包含以下 3 行数据：

| **ID** | **name**    | **salary** |
| ------ | ----------- | ---------- |
| 1      | Einstein    | 95000      |
| 2      | Mozart      | 40000      |
| **3**  | **NullGuy** | **NULL**   |

**请判断以下 SQL 查询会返回哪些人的名字（ID）？**

#### **题目 1：算术与比较**

```
select ID from instructor where salary > 50000;
```

- **分析**：
	- ID 1: $95000 > 50000$ $\rightarrow$ `true` $\rightarrow$ **选中**。
	- ID 2: $40000 > 50000$ $\rightarrow$ `false` $\rightarrow$ 不选中。
	- ID 3: $\text{NULL} > 50000$ $\rightarrow$ **`unknown`** $\rightarrow$ **不选中** (因为 Where 只认 true)。
- **结果**：ID 1

#### **题目 2：逻辑与空值**

SQL

```
select ID from instructor where (salary > 50000) or (salary is null);
```

- **分析**：
	- ID 3:
		- 前半部分 `NULL > 50000` $\rightarrow$ `unknown`
		- 后半部分 `NULL is null` $\rightarrow$ `true`
		- 整体：`unknown OR true` $\rightarrow$ **`true`** $\rightarrow$ **选中**。
- **结果**：ID 1, ID 3

#### **题目 3：最容易错的逻辑陷阱**

SQL

```
select ID from instructor where not (salary = 40000);
```

- **分析**：
	- ID 2 (Mozart): $40000 = 40000$ 是 `true`，取反是 `false` $\rightarrow$ 不选中。
	- **ID 3 (NullGuy)**:
		- 内部：`NULL = 40000` $\rightarrow$ **`unknown`**
		- 取反：`not unknown` $\rightarrow$ **`unknown`** 并**不会转化为true!!!**
		- Where 判定：`unknown` 不是 `true` $\rightarrow$ **不选中！**
	- *(很多同学会以为“薪水不是40000”当然包含“没有薪水”的人，但**在 SQL 里不是这样的！**)*
- **结果**：ID 1

#### **题目 4：正确检测空值**

SQL

```
select ID from instructor where salary is null;
```

- **分析**：
	- ID 3: `NULL is null` $\rightarrow$ `true` $\rightarrow$ **选中**。
- **结果**：ID 3

------

### **✅ 总结 (考试口诀)**

1. **运算**：**碰见 Null 变 Null（算术），碰见比较变 Unknown。**
2. **筛选**：Where 只放行 True，**Unknown 和 False 一律拦住**。
3. **语法**：**查空必须写 `is null`，千万别写 `= null`。**

### **6. 聚合函数 (Aggregate Functions)**

#### **(1) 五大基本函数 (The 5 Built-in Functions)**

*(背下来，这是所有计算的基础)*

- **`avg`**: 平均值 (Average value) —— ***仅限数字***
- **`min`**: 最小值 (Minimum value)
- **`max`**: 最大值 (Maximum value)
- **`sum`**: 总和 (Sum of values) —— ***仅限数字***
- **`count`**: 计数 (Number of values)

#### **(2) 基本用法 (Basic Usage)**

聚合函数通常**在 `Select` 子句中**使用。

- **普通计算**：
	- 题目：计算计算机系 (Comp. Sci.) 老师的平均工资。
	- SQL: `select avg(salary) from instructor where dept_name = 'Comp. Sci.';`
- **去重计数 (`Distinct`)**：
	- 题目：计算在 2010 年春季教过课的老师**总人数（去掉重复的）**。
	- SQL: `select count(distinct ID) from teaches where semester = 'Spring' and year = 2010;`
- **统计行数 (`Count (\*)`)**：
	- 题目：计算 `course` 表里**总共有多少行**。
	- SQL: `select count(*) from course;`

------

#### **(3) 分组聚合 (Group By) —— ⭐⭐⭐ 核心考点**

有时候我们不想算出全校的平均工资，而是想算出**每个系**的平均工资。这就需要 **`group by`**。

- **定义**：将**聚集函数作用到一组元组集**上。
- **规则（死刑规则，必须背诵）**：
	- **出现在 `select` 子句中、但没有被聚合函数包裹的属性，必须出现在 `group by` 子句中！**
	- *错误示范*：`select dept_name, ID, avg(salary) from ... group by dept_name;`
	- *为什么错*：**按系分组后，每个系只有一行结果（平均工资）**，但 **`ID` 有很多个，数据库不知道该显示哪个 `ID`。**

#### **(4) 分组过滤 (Having Clause) —— ⭐ 难点**

如果你想**对分组后的结果进行筛选**（例如：**只显示平均工资 > 42000 的系**），**不能用 `where`**，必须用 **`having`**。

- **区别（必考简答/判断）**：
	- **`where`**：在**分组前**筛选行 (Filters tuples **before** grouping)。
	- **`having`：在分组后筛选组 (Filters groups after grouping)。**
- **执行顺序：**
	1. **`from` (找表)**
	2. **`where` (筛选行)**
	3. **`group by` (分组)**
	4. **`having` (筛选组)**
	5. **`select` (输出)**

#### **(5) 空值与聚合 (Null Values and Aggregates)**

- **通用规则**：**除了 `count(*)` 以外**，所有的聚合函数**都会忽略 `null` 值。**
	- 例子：如果一列数据是 `[100, null, 200]`。
	- `avg` 结果是 `(100+200)/2 = 150` (**忽略 null，分母是2**)。
	- `count` 结果是 `2`。
- **Count(\*) 规则**：`count(*)` 统计的是**物理**行数，**包含** null。因为只要求物理存在,全null也行
- **空集规则**：如果输入是空集（一行都没有），`count` 返回 0，其他函数返回 `null`。

------

### **📝 综合实战例题 (Comprehensive Example)**

*(这个例子涵盖了 Select, From, Where, Group By, Having 全流程，彻底弄懂它，这部分就过关了)*

**题目场景**： 我们有 `instructor` 表 (ID, name, dept_name, salary)。 **任务**：找出所有**平均工资超过 42,000** 的系，列出**系名**和该系的**平均工资**。

**解题步骤**：

1. **要什么 (Select)**：系名 `dept_name`，平均工资 `avg(salary)`。
2. **在哪找 (From)**：`instructor`。
3. **怎么分 (Group By)**：既然要算“每个系”的，当然是 `group by dept_name`。
4. **怎么筛 (Having)**：条件是“平均工资 > 42000”，这是对分组后的属性进行筛选，用 `having avg(salary) > 42000`。

**完整 SQL 代码**：

```
select dept_name, avg(salary) as avg_salary
from instructor
group by dept_name
having avg(salary) > 42000;
```

*(注意：这里 dept_name 在 select 里出现了，也在 group by 里出现了，符合规则。)*

**进阶变种（加入 Where）**： **任务**：只考虑**2010年入职**的员工，找出平均工资超过 42,000 的系。

```
select dept_name, avg(salary)
from instructor
where join_year = 2010   -- 先筛选人 (Where)
group by dept_name       -- 再按系分组
having avg(salary) > 42000; -- 最后筛选系 (Having)
```

### **✅ 考试避坑总结**

1. **检查 Group By**：写完 SQL 检查一遍，**Select 里没加 `avg/sum/max` 的列，是不是都在 Group By 里**？
2. **Having vs Where**：
	- **过滤具体的行**（如 `salary > 5000`）用 `Where`。
	- **过滤聚合后的值**（如 `avg(salary) > 5000`）用 `Having`。
	- 只要看到题目条件里**有“平均”、“总和”等字眼作为筛选条件，立刻想到 `Having`。**

### **7. 嵌套子查询 (Nested Subqueries)**

**定义**：子查询是**嵌套在另一个查询中的 `select-from-where` 表达式。通常出现在 `where` 子句**中，**也可以出现在 `from` 子句中。**

#### **(1) 集合成员资格 (Set Membership) —— `in` / `not in`**

- **核心逻辑**：判断**某个属性的值是否在一个集合（子查询结果）中**。
- **连接词**：
	- **`in`**: 在集合里。
	- **`not in`**: 不在集合里。

【必考典型例题】

题目：找出在 2009 年秋季和 2010 年春季都开课的课程 ID。（也就是求交集，不用 Intersect 怎么写？）

- **思路**：**先选出 09 秋的课**，然后**要求这些课的 ID 在 “10 春开课的 ID 集合”里**。

- **SQL 写法**：

	SQL

	```
	select distinct course_id
	from section
	where semester = 'Fall' and year = 2009
	and course_id in (
	    select course_id
	    from section
	    where semester = 'Spring' and year = 2010
	);
	```

【反例：差集逻辑】

题目：找出在 2009 年秋季开课，但 不在 2010 年春季开课的课程 ID。

- **SQL 写法**：将上面的 `in` 改为 **`not in`** 即可。

------

#### **(2) 集合比较 (Set Comparison) —— `some` / `all`**

*(这部分是选择题和填空题常客，容易把含义搞反)*

- **`> some` (大于某一个)**：
	- **含义**：**至少**比集合中的**某一个**大 (At least one)。
	- **等价于**：`> min(...)`。
	- **考点**：`= some` 等价于 **`in`**。
- **`> all` (大于所有)**：
	- **含义**：比集合中的**所有**元素都大 (Greater than all)。
	- **等价于**：`> max(...)`。
	- **考点**：`<> all` 等价于 **`not in`**。

【完整例子】

题目：找出薪水**比 Biology 系某一个老师高的老师姓名**。

- **SQL 写法**：

	SQL

	```
	select name
	from instructor
	where salary > some (
	    select salary
	    from instructor
	    where dept_name = 'Biology'
	);
	```

**题目**：找出薪水比 Biology 系**所有**老师都高的老师姓名。

- **SQL 写法**：将上面的 `> some` 改为 **`> all`**。

------

#### **(3) 空关系测试 (Test for Empty Relations) —— `exists` / `not exists` (⭐⭐ 难点)**

这是考试中最强大的工具，特别是 **相关子查询 (Correlated Subquery)**。

- **`exists`**: 如果子**查询的结果非空** (not empty)，返回 `true`。
- **`not exists`**: 如果子查询的结果**为空** (empty)，返回 `true`。

**【概念：相关变量 (Correlation Variables)】**

- 指的是**在外层查询中定义的变量（如 `S`），在内层子查询中被使用了（如 `S.ID`）**。这种**子查询不能单独运行，必须依赖外层每一行的数据**。

【必考典型例题：**模拟交集**】

题目：找出 2009 秋和 2010 春都开课的课程（用 exists 写）。

- **思路**：对于 09 秋的**每一门课 `S`，检查是否存在一门 10 春的课 `T`，它们的 ID 相同。**

- **SQL 写法**：

	SQL

	```
	select course_id
	from section as S
	where semester = 'Fall' and year = 2009
	and exists (
	    select *
	    from section as T
	    where semester = 'Spring' and year = 2010
	    and S.course_id = T.course_id  -- 相关条件
	);
	```

【⭐⭐ 终极难点：**全称量词 / 包含关系**】

题目：找出**选修了 Biology 系开设的所有课程的学生** (Students who have taken all courses offered in Biology)。

- **逻辑转换**：如果一个学生**选了 Biology 的所有课** $\Leftrightarrow$ **不存在**一门 Biology 的课，是这个学生**没选**的。

- **SQL 模板 (双重 Not Exists)**：

	```
	select S.ID, S.name
	from student as S
	where not exists (
	    (select course_id from course where dept_name = 'Biology') -- 集合 A: Biology 的所有课
	    except
	    (select T.course_id from takes as T where S.ID = T.ID)     -- 集合 B: 学生 S 选的所有课
	);
	```

	*(注：讲义 slide 66 用的逻辑是：**检查 (Biology 所有课 - 该学生选的课) 是否为空**。如果**为空，说明他全选**了。)*

------

#### **(4) From 子句中的子查询**

- **核心**：把**子查询的结果当成一张临时表**来用。
- **注意**：**必须给这个子查询结果起个别名 (Alias)。**

【完整例子】

题目：找出平均工资 > 42000 的系及其平均工资。（不用 Having 怎么写？）

- **SQL 写法**：

	```
	select dept_name, avg_salary
	from (
	    select dept_name, avg(salary) as avg_salary
	    from instructor
	    group by dept_name
	) as dept_avg  -- 必须起别名！
	where avg_salary > 42000;
	```

------

#### **(5) With 子句 (The With Clause)**

- **作用**：**定义临时视图 (Temporary View)**，让复杂的查询逻辑更清晰。
- **位置**：**写在 `select` 之前。**

【完整例子】

题目：找出**拥有最大预算 (Budget) 的系**。

- **SQL 写法**：

	```
	with max_budget(value) as (
	    select max(budget)
	    from department
	)
	select dept_name
	from department, max_budget
	where department.budget = max_budget.value;
	```

### **1. 查询目标 (Goal)**

找出拥有 **最大预算 (Maximum Budget)** 的那个（或那些）系的名称 (`dept_name`)。

------

### **2. 结构拆解 (Step-by-Step Analysis)**

这条语句分成了两部分：**定义临时关系** 和 **使用临时关系**。

#### **第一部分：With 子句 (定义临时视图)**

```
with max_budget(value) as (
    select max(budget)
    from department
)
```

- **含义**：定义了**一个名为 `max_budget` 的 临时关系 (Temporary Relation)。**
- **`(value)`**：这是**在给临时表的属性重命名。**
	- 子查询算出的是 `max(budget)`**（一个数字）。**
	- 我们**将这个列强制命名为 `value`。**
- **结果**：此时，内存里产**生了一张只有 一行一列 的临时小表 `max_budget`**，里面的内容就是全校最高的那个预算值（例如：`100000`）。

#### **第二部分：主查询 (Main Query)**

```
select dept_name
from department, max_budget
where department.budget = max_budget.value;
```

- **`from department, max_budget`**：
	- 这里做了一个 **笛卡尔积 (Cartesian Product)**。
	- 但是因为 `max_budget` 表里只有 **1 行** 数据，所以**实际上就是把那个最大值“贴”到了 `department` 表的每一行旁边。**
- **`where department.budget = max_budget.value`**：
	- **筛选条件**：让**每个系的 `budget` 去和那个最大值 (`value`) 做比较。**
	- 只有当某个系的预算 **等于 全校最高预算时，这一行才会被保留。**
- **`select dept_name`**：
	- 最后只输出满足条件的系名。

------

### **3. 为什么要这样写？(Exam Note)**

你可能会问：*“为什么不直接用 `where budget = (select max(budget)...)`？”*

虽然写成嵌套子查询也可以，但 **With 子句** 有以下优势（也是讲义强调的）：

1. **逻辑清晰 (Clarity)**：**先把“最大值”算出来放在一边，给它起个名字**，主查询读起来就像英语一样自然（"Where budget equals max value"）。
2. **避免重复计算**：如果**主查询里多次用到这个最大值，用 With 子句只需要算一次。**

### **总结 (一句话记忆)**

**先用 With 造一个只装最大值的小表**，再**用主表去跟这个小表 对暗号 (Where = )**，对上了就是我们要找的系。

### **✅ 考试避坑指南**

1. **Scope (作用域)**：在**子查询里可以使用外层的变量（相关子查询）**，但在外层不能使用子查询内部定义的变量。
2. **Exists vs In**：**`exists` 通常效率更高，且能处理更复杂的逻辑（如多列比较）**。考试时如果**逻辑复杂，优先想 `exists`。**
3. **Unique**：讲义 Slide 64 提到了 `unique` 结构（检查**子查询是否有重复元组**），但目前很多数据库不支持，如果题目没明确要求，**尽量别用**，**用 `count(...) <= 1` 代替更稳妥。**
4. **Not Exists 的双重否定：看到 "All" (所有)、"Contains" (包含) 这类词，直接套用 `not exists (A except B)` 的模板。**

### **8. 数据库修改 (Modification of the Database)**

#### **(1) 删除 (Deletion) —— `delete from`**

- **基本语法**：`delete from 表名 where 条件`
- **注意**：
	- `delete` 命令只能删除 **整个元组 (tuples/rows)**，**不能只删除某一列**的值（**删列值要用 `update` 设为 null**）。
	- 如果不写 `where` 子句，比如 `delete from instructor`，会**清空整张表**，但表结构还在。

**【典型例题】**

- **题目 A (简单)**：删除 Finance 系的**所有老师。**

	```
	delete from instructor
	where dept_name = 'Finance';
	```

- **题目 B (带子查询 - 难点)**：删除**所有在 "Watson" 大楼工作的**系的老师。

	- *思路*：**先在 `department` 表里找哪些系在 Watson，**然后**删 `instructor` 表里对应的人。**

	- *代码*：

		SQL

		```
		delete from instructor
		where dept_name in (
		    select dept_name
		    from department
		    where building = 'Watson'
		);
		```

- **题目 C (更难 - 平均工资问题)**：删除薪水**低于全校平均水平的老师。**

	- *代码*：

		SQL

		```
		delete from instructor
		where salary < (
		    select avg(salary)
		    from instructor
		);
		```

------

#### **(2) 插入 (Insertion) —— `insert into`**

- **基本语法 A (插单行)**：`insert into 表名 values (值1, 值2, ...)`
- **基本语法 B (插查询结果)**：`insert into 表名 select ...`

**【典型例题】**

- **题目 A (插单行)**：插入一个新课程 CS-437，Computer Science 系，4学分。

	SQL

	```
	insert into course
	values ('CS-437', 'Database Systems', 'Comp. Sci.', 4);
	```

	- *注意*：值的**顺序必须和 `create table` 时的列顺序一致**。如果**不确定顺序，可以写成 `insert into course(course_id, title, ...) values (...)`。**

- **题目 B (插查询结果 - 必考)**：

	- **场景**：我们要**给每个老师都建立一个学生账号（比如让他免费听课），把所有老师的信息插入到 `student` 表中，且总学分 `tot_cred` 设为 0。**

	- *代码*：

		SQL

		```
		insert into student
		select ID, name, dept_name, 0
		from instructor;
		```

	- *逻辑*：先执行 `select` 查出结果，再**把结果批量插进去**。讲义特别提到，Select 语句**会在插入动作开始前完全执行完毕**，所以**即使是查自己插自己（比如 `insert into table1 select * from table1`）也是安全的（如果没有主码冲突）。**

------

#### **(3) 更新 (Updates) —— `update ... set`**

- **基本语法**：`update 表名 set 属性 = 新值 where 条件`
- **场景**：我们**不想删人，只想改数据（比如涨工资）。**

**【典型例题】**

- **题目 A (简单涨薪)**：给所有工资低于 70000 的人涨薪 5%。

	```
	update instructor
	set salary = salary * 1.05
	where salary < 70000;
	```

- **题目 B (带子查询的更新)**：

	- **场景**：给所有工资低于“全校平均工资”的老师涨薪 5%。

		```
		update instructor
		set salary = salary * 1.05
		where salary < (
		    select avg(salary)
		    from instructor
		);
		```

------

#### **(4) 条件更新与 Case 语句 (Case Statement) —— ⭐⭐ 考试终极陷阱**

- **问题背景**：如果你要**执行两条更新语句：**

	1. 给工资 > 100,000 的人涨 3%。
	2. 给工资 <= 100,000 的人涨 5%。

	- *陷阱*：如果你先执行第 2 条（涨 5%），某个原本 99,000 的人涨完变成 103,950，结果他又满足了第 1 条（> 100,000），又涨了 3%。**顺序会导致错误！**（Slide 74）

- **解决方案**：使用 **`case`** 语句，在一条 SQL 里完成。

- **语法模板 (背下来)**：

	SQL

	```
	update 表名
	set 属性 = case
	    when 谓词1 then 结果1
	    when 谓词2 then 结果2
	    else 默认结果
	end
	```

**【完整必考例子 (Slide 75)】**

- **题目**：对所有老师涨薪。如果工资 <= 100,000，涨 5%；如果工资 > 100,000，涨 3%。

- **满分代码**：

	SQL

	```
	update instructor
	set salary = case
	    when salary <= 100000 then salary * 1.05
	    else salary * 1.03
	end;
	```

- ***注意*：`case` 结构以 `end` 结尾。**

------

### **✅ 考试避坑总结**

1. **Delete 是删行**：别试图用 `delete` **删掉某个格子里**的数字（那叫 update set null）。
2. **Insert Select**：当你需要从一张表**“复制”数据到另一张表时，别傻乎乎地一条条写，直接用 `insert into ... select ...`。**
3. **Update 顺序**：只要题目出现“**对不同条件的人做不同幅度的修改**”，脑子里立刻弹幕预警：**用 Case 语句！用 Case 语句！** 否则**顺序搞错会扣分。**
4. **Where 子查询**：增删改的 `Where` 子句里完全**可以写复杂的嵌套子查询（in, exists, < avg 等），这和查询 (Select) 的写法是一模一样**的。



# 疑问

## 在执行group by后的中间结果到底是什么形式的?

### **1. 没分组前的原始表**

假设我们要对 `instructor` 表按 `dept_name` 分组。 原始表是**平铺直叙的行**（Tuple）：

| ID   | name     | dept_name      | salary |
| ---- | -------- | -------------- | ------ |
| 101  | Einstein | **Physics**    | 95000  |
| 102  | Gold     | **Physics**    | 87000  |
| 201  | Katz     | **Comp. Sci.** | 75000  |
| 202  | Brandt   | **Comp. Sci.** | 92000  |
| 301  | Mozart   | **Music**      | 40000  |

------

### **2. 执行 `GROUP BY dept_name` 后的“中间结果”**

当数据库执行完 Group By 后，逻辑上你可以把它想象成**把表切成了好几块小表（Groups）**。

这个中间状态**不再是一个简单的二维表**，而是一个**“组”的集合**。**每个组都有一个“标签”（Group Key）**，后面**挂着属于这个组的所有行**。

**【形象化中间结果】**

- **Group 1 (Key: 'Physics')**
	- `{ (101, Einstein, Physics, 95000), (102, Gold, Physics, 87000) }`
- **Group 2 (Key: 'Comp. Sci.')**
	- `{ (201, Katz, Comp. Sci., 75000), (202, Brandt, Comp. Sci., 92000) }`
- **Group 3 (Key: 'Music')**
	- `{ (301, Mozart, Music, 40000) }`

------

### **3. 为什么 Select 规则那么严格？**

理解了这个中间结果，你就能瞬间秒懂为什么有那个“死刑规则”：

> "出现在 Select 中但没被聚合函数包裹的属性，必须出现在 Group By 中。"

如果你写：`SELECT dept_name, salary ... GROUP BY dept_name`

- **数据库看着 Group 1 ('Physics')**：
	- **`dept_name`？简单，就是标签 `'Physics'`。**
	- `salary`？**傻眼了**。这一组里**有两个 salary** (95000 和 87000)，**我该给你显示哪一个？** 数据库**没法选**，所以报错。

如果你写：`SELECT dept_name, avg(salary) ... GROUP BY dept_name`

- **数据库看着 Group 1 ('Physics')**：
	- `dept_name` → `'Physics'`
	- `avg(salary)` → **拿出一组里的 `[95000, 87000]` 算出平均值 `91000`。**
	- **搞定！** **生成结果的一行：`(Physics, 91000)`。**

------

### **总结**

执行 `GROUP BY` 后的中间结果，是一堆**“挂着标签的一组一组的数据”**。

- **标签（Group Key）是唯一的，可以直接 Select。**
- 组里的其他数据（ID, name, salary）是**多值**的，想拿出来必须用**聚合函数**（avg, sum, max）**把它们“压”成一个单值。**

收到。我们严格按照 **《ch3.pdf》 (Chapter 3: Introduction to SQL)** 的第 61-72 页内容，为你详细总结 **“7. 嵌套子查询 (Nested Subqueries)”**。

这是 SQL 查询中最灵活、也是**大题中最难**的部分。考试经常会考“不用集合运算写出交集/差集”，或者“全称量词（For all）”的逻辑，必须熟练掌握。

## Exists vs In

### **1. 核心定义对比**

#### **(1) IN (集合成员资格 - Set Membership)**

- **讲义位置**：Slide 61
- **逻辑**：**“检查某个值是否在结果列表中”**。
- **运作方式**：子查询返回**一个值的列表 (List of values)**。外层查询拿着**某个属性的值，去这个列表里找**，看在不在里面。
- **适用场景**：简单的**单列匹配。**
- **语法特征**：`Where 属性名 in (子查询)`

#### **(2) EXISTS (空关系测试 - Test for Empty Relations)**

- **讲义位置**：Slide 65-66
- **逻辑**：**“检查子查询是否有返回结果”** (True if not empty)。
- **运作方式**：子查询**通常不返回具体的数据，只返回 `True` (有行) 或 `False` (空集)。**
- **关键特征**：通常涉及 **相关子查询 (Correlated Subquery)**，即**子查询内部用到了外层查询的变量（比如 `S.ID = T.ID`）。**
- **语法特征**：`Where exists (子查询)`

------

### **2. 完整实战案例 (Complete Example)**

为了让你彻底明白区别，我们使用讲义中经典的 **“求交集” (Intersection)** 题目： **题目：找出在 2009 年秋季 AND 2010 年春季都开课的课程 ID。**

#### **解法 A：使用 IN (直观写法)**

- **思路**：先找到 09 秋的所有课程，然后看它的 ID 是否 **在** “10 春课程 ID 列表”里。

- **SQL**：

	```
	select distinct course_id
	from section
	where semester = 'Fall' and year = 2009
	and course_id in (          -- 拿着外层的 course_id 去列表里找
	    select course_id        -- 子查询返回一列 ID
	    from section
	    where semester = 'Spring' and year = 2010
	);
	```

#### **解法 B：使用 EXISTS (相关子查询写法)**

- **思路**：对于 09 秋的每一门课 S，检查 **是否存在** 一门 10 春的课 T，满足 S 和 T 的 ID 相同。

- **SQL**：

	```
	select course_id
	from section as S           -- 1. 必须给外层表起个别名 S
	where semester = 'Fall' and year = 2009
	and exists (
	    select * -- 2. 这里选什么不重要，通常写 *
	    from section as T
	    where semester = 'Spring' and year = 2010
	    and S.course_id = T.course_id -- 3. 关键！连接外层 S 和内层 T
	);
	```

------

### **3. 考试做题：到底该选哪一个？**

这是我在复习讲义时总结的**“黄金判断法则”**：

#### **情况 1：选 IN 的时候**

- **特征**：题目逻辑很简单，**只是单纯的“属性 A 是否在集合 B 里”。**

- **例子**：找出所有叫 "Mozart" 或 "Einstein" 的老师。

	SQL

	```
	select name from instructor where name in ('Mozart', 'Einstein');
	```

- **做题优势**：代码短，不需要给表起别名，不需要写复杂的连接条件。

#### **情况 2：必须选 EXISTS 的时候 (⭐⭐ 必考难点)**

- **特征 1 (多列比较)**：如果你**要同时比较两个属性（比如 ID 和 Section 都一样）**，用 `IN` 会很麻烦，**用 `EXISTS` 配合连接条件 (`S.ID=T.ID and S.Sec=T.Sec`) 非常容易。**

- **特征 2 (全称量词/包含关系)**：**这是死穴，IN 做不到！**

	- 题目出现 **"All" (所有)**、**"Every" (每一个)**。

	- **例子**：找出选修了 Biology 系开设的**所有**课程的学生。

	- **解法**：必须使用 **`NOT EXISTS (A EXCEPT B)`** 的逻辑（即：不存在一门 Biology 的课是他没选的）。

	- **代码模板 (背下来)**：

		```
		select S.ID, S.name
		from student as S
		where not exists (
		    (select course_id from course where dept_name = 'Biology') -- 集合A
		    except
		    (select T.course_id from takes as T where S.ID = T.ID)     -- 集合B
		);
		```

------

### **4. 总结对比表 (Cheat Sheet)**

| 特性           | IN (集合成员)               | EXISTS (空关系测试)                     |
| -------------- | --------------------------- | --------------------------------------- |
| **关注点**     | 关注**值** (Value) 是否匹配 | 关注**逻辑** (Boolean) 是否有结果       |
| **子查询返回** | **一列**具体的值            | True 或 False                           |
| **相关变量**   | 通常不需要                  | **必须使用** (S.id = T.id)              |
| **处理 "All"** | **很难做到**                | **唯一解法** (用 Not Exists ... Except) |
| **推荐场景**   | **简单单列**筛选            | 复杂逻辑、**多列**关联、全称量词        |

**一句话复习建议**： 如果题目只是**简单的“找在...里面的”，用 `IN`；**如果题目涉及“**两个表之间的复杂关系”或者“找所有(All)”**，立刻**切换到 `EXISTS` 并记得给表起别名**！

# ch4

### **1. 连接操作基础 (Join Operations Overview)**

- **定义**：连接操作接受两个关系（表），并返回一个新的关系作为结果 。
- **本质**：它是笛卡尔积 (Cartesian product) 的**变体**，但要求**两个关系中的元组必须匹配 (match) 。**
- **通常用法**：作为 `from` 子句中的子查询表达式使用 。

### **2. 连接类型 (Join Types) —— 决定“保留谁”**

这是考试中判断用哪个词的关键。连接类型定义了**如何处理不匹配 (mismatched) 的元组** 。

#### **(1) 内连接 (Inner Join)**

- **英文术语**：`inner join`
- **行为**：**默认**的连接方式。只保留**两个表中都能匹配上的元组。**
- **丢失信息**：如果**某行数据在另一张表里找不到对应项**，这行数据就会在结果中**消失** 5。

#### **(2) 左外连接 (Left Outer Join) —— ⭐ 考试最常考**

- **英文术语**：`left outer join`
- **行为**：计算连接结果，并**保留左边关系**中**那些不匹配的元组。**
- **空值填充**：**右边关系中缺失的属性用 null 填充 。**
- **做题口诀**：**“只管左边有没，不管右边有没”**。如果题目说“列出所有课程，**即使它没有先修课**”，必须**用 Left Outer Join。**

#### **(3) 右外连接 (Right Outer Join)**

- **英文术语**：`right outer join`
- **行为**：**保留右边关系**中那些不匹配的元组。**左边缺失的填 null**。

#### **(4) 全外连接 (Full Outer Join)**

- **英文术语**：`full outer join`
- **行为**：**保留两边关系**中所有的元组。**无论哪边缺了，都填 null** 。

### 3. 连接条件 (Join Conditions) —— 决定“怎么连”

#### **(1) Natural (自然连接)**

- **语法**：`natural join`
- **规则**：自动匹配两个表中**所有同名**的属性 。
- **特点**：结果中**去重**，**同名属性只保留一列**。

#### **(2) On `<predicate>` (指定条件) —— ⭐ 推荐写法**

- **语法**：`join ... on ...`
- **规则**：允许你指定**任意的谓词作为连接条件** 。
- **特点**：结果中**不去重**，**同名属性会都保留**（**除非你在 Select 中筛选**）。
- **例子**：`course inner join prereq on course.course_id = prereq.course_id` 。

#### **(3) Using `(A1, A2...)`**

- **语法**：`join ... using (Attributes)`
- **规则**：指定用哪几个**同名属性**来连 。

### **4. 📊 完整复习实例 (The Complete Example)**

为了涵盖上述知识点，我们使用讲义 Slide 1014-1017 中的 **Course (课程表)** 和 **Prereq (先修课表)** 数据。这是最经典的考试场景。

#### **【原始数据】**

- **表 A: course (课程表)**
	- 包含：`BIO-301` (Genetics), `CS-190` (Game Design), **`CS-315` (Robotics)**。
	- *注意：`CS-315` 在这里有，但**在 Prereq 表里没有（它没有先修课）**。*
- **表 B: prereq (先修课表)** 15
	- 包含：`BIO-301` 的**先修课**, `CS-190` 的先修课, **`CS-347`** 的先修课。
	- *注意：`CS-347` 在这里有，但**在 Course 表里没有（可能是一门新课还没录入详情）。***

#### **【场景 1：Inner Join (内连接)】**

**题目**：找出**所有既有课程详情又有先修课信息的课程**。

- **SQL**:

	```
	select * from course inner join prereq on course.course_id = prereq.course_id;
	```

- **结果**：

	- **保留**：`BIO-301`, `CS-190`。
	- **丢弃**：`CS-315` (因为它没先修课), `CS-347` (因为它没课程详情)。

#### **【场景 2：Left Outer Join (左外连接) —— 必会】**

**题目**：列出**所有课程**的详情**及其先修课ID（如果有的话）**，**即使该课程没有先修课也要列出**。

- **SQL**:

	```
	select * from course left outer join prereq on course.course_id = prereq.course_id;
	```

	*(或者用 Natural: `select \* from course natural left outer join prereq;`)* 16

- **结果** 17：

	- `BIO-301` ... `BIO-101` (匹配)
	- `CS-190` ... `CS-101` (匹配)
	- **`CS-315` ... `null`** (**关键点**：`CS-315` 被保留下来了，`prereq_id` 填了 null)。

#### **【场景 3：Right Outer Join (右外连接)】**

**题目**：列出所有先修课关系及其对应的课程详情，**即使该课程详情缺失也要列出**。

- **SQL**:

	```
	select * from course natural right outer join prereq;
	```

- **结果** 18：

	- `BIO-301` ... (匹配)
	- `CS-190` ... (匹配)
	- **`null` ... `CS-347`** (**关键点**：`CS-347` 被保留了，但左边的 `title`, `dept_name` 等全是 null)。

#### **【场景 4：Full Outer Join (全外连接)】**

**题目**：列出所有的课程和所有的先修课信息，无论是否匹配。

- **SQL**:

	```
	select * from course natural full outer join prereq;
	```

- **结果** 19：

	- `BIO-301`, `CS-190` (匹配的)
	- `CS-315` ... `null` (左边有的)
	- `null` ... `CS-347` (右边有的)
	- **全部保留！**

### **💡 考试做题技巧总结**

1. **看到 "All ... even if no match" / "List every ..."** → **Outer Join**。
2. **看到 "All courses" (左边表主体)** → **Left Outer Join**。
3. **看到 "Pairs that match"** → **Inner Join**。
4. **写 SQL 时**：如果不确定两个表有没有意外的同名列，**不要用 Natural Join**，请老老实实写 `Join ... On ...`，这样最稳妥。

### **2. 视图 (Views)**

#### **(1) 核心概念 (Concept)**

- **定义**：视图是一种机制，用于向某些用户隐藏数据。它是**虚拟关系 (Virtual Relation)** 。
- **本质**：视图**不存储**实际数据（**除非是物化**视图），它只存储**查询表达式**。当你**查询视图时，系统会把它替换为底层的 SQL 查询**（这叫 **View Expansion**）。

#### **(2) 定义视图 (View Definition) —— ⭐ Part III 必考**

- **语法**：

	```
	create view v as <query expression>
	```

	- `v` 是视图的名字。
	- `<query expression>` 是任意合法的 SQL 查询语句 。

【完整考试实例：**隐藏薪水**】

题目：创建一个名为 faculty 的视图，包含讲师的 ID、姓名和系名，但不包含薪水信息。

- **SQL 写法** ：

	```
	create view faculty as
	select ID, name, dept_name
	from instructor;
	```

- **使用视图**：

	一旦定义好，你就**可以像查普通表一样查它**：

	```
	select name
	from faculty
	where dept_name = 'Biology';
	```

【复杂实例：带聚合函数的视图】

题目：创建一个视图 departments_total_salary，列出每个系的系名和总薪水。

- **SQL 写法** ：

	```
	create view departments_total_salary(dept_name, total_salary) as
	select dept_name, sum(salary)
	from instructor
	group by dept_name;
	```

	*(注意：如果 Select 子句中有表达式（如 `sum(salary)`），最好在 `create view` 的括号里**显式指定列名**，如 `(dept_name, total_salary)`)*。

#### **(3) 视图的更新 (Update of a View) —— ⭐ Part II 判断题/选择题死穴**

这是最容易丢分的地方。考试会给你一个视图定义，然后问：*“**能否向这个视图插入/更新数据？**”*

- **一般规则**：绝大多数 SQL 实现**只允许**更新 **简单视图 (Simple Views)** 。
- **简单视图的 4 大苛刻条件 (必须背诵)** ：
	1. **From 子句**：只能包含 **1 个** 数据库关系（**不能是多表连接**）。
	2. **Select 子句**：只能包含关系的**属性名**（Attribute names），**不能有 表达式**（如 `salary/12`）、聚合函数（如 `sum`）或 `distinct` 声明。
	3. **Null 约束**：任何**没有出现**在 Select 子句中的属性，在**原表中必须允许为 Null（或者有默认值）**。
	4. **无分组**：查询中**不能有** `group by` 或 `having` 子句。

**【判断题实战演练】**

**案例 A：**

```
create view history_instructors as
select *
from instructor
where dept_name = 'History';
```

- **问题**：可以向它插入 `('25566', 'Brown', 'Biology', 100000)` 吗？
- **分析**：满足所**有简单视图条件。**
- **结果**：**可以插入**。虽然你插了个 'Biology' 的人进 'History' 视图（这很怪，而且插完你在视图里看不见他），但**在 SQL 层面是合法的，它会直接写进底层的 `instructor` 表 。**

**案例 B：**

```
create view instructor_info as
select ID, name, building
from instructor, department
where instructor.dept_name = department.dept_name;
```

- **问题**：可以更新这个视图吗？
- **分析**：From 子句包含了 **2 个表** (`instructor`, `department`)。
- **结果**：**不可以**。**违反条件 1** 。

**案例 C：**

```
create view salary_stats as
select dept_name, avg(salary)
from instructor
group by dept_name;
```

- **问题**：可以更新这个视图吗？
- **分析**：包含了 `avg(salary)` 和 `group by`。
- **结果**：**不可以**。**违反条件 2 和 4。**

#### **(4) 物化视图 (Materialized Views) —— 概念题**

- **定义**：不仅仅存储查询逻辑，而是**真的创建了一张物理表**来存结果 。
- **特点**：
	- 查询速度快（不用每次都重算）。
	- **维护困难**：如果**底层关系更新了**，物化视图的数据就会**过时 (Out of date)** 。需要**系统定期刷新**。

### **💡 考试做题总结**

1. **写 SQL 题**：看到“Create a view ...”，直接套用 `create view ... as select ...` 模板。
2. **判断题**：看到“Can we insert/update this view?”，立刻拿出**4大条件**去卡它：
	- 是不是**多表**？(是 → No)
	- 有没有 **sum/avg/distinct**? (有 → No)
	- 有没有 **Group by**? (有 → No)
	- **缺的列能不能为空**? (不能 → No)

### **3. 事务 (Transactions)**

#### **(1) 核心定义 (Key Definitions)**

- **定义**：事务是一个 **工作单元 (Unit of work)** 。
- **原子性 (Atomic transaction)**：这是事务**最重要的特性。**
	- **含义**：事务要么**完全执行 (fully executed)**，要么**完全回滚 (rolled back)**，就像它从未发生过一样 。
	- *简单理解*：**要么全做，要么全不做，不能做一半。**
- **隔离性 (Isolation)**：事务应**与其他并发执行的事务隔离**开来 。

#### **(2) SQL 中的事务控制 (SQL Statements)**

- **开始**：在 SQL 中，事务通常是 **隐式开始 (begin implicitly)** 的，不需要专门写“Start Transaction” 。
- **结束 (Ended by)** ：
	1. **`commit work`**：**提交**工作。表示事务成功完成，将更改**永久保存**到数据库。
	2. **`rollback work`**：**回滚**工作。表示事务失败或被取消，**撤销**该事务中所有已做的更改。
- **默认行为**：大多数数据库默认开启 **自动提交 (Auto commit)**，即**每执行一条 SQL 语句就自动 Commit** 。如果要作为一个整体执行，**需要关闭自动提交**。

#### **(3) 📊 完整复习实例 (Complete Example)**

虽然讲义这页没有代码例子，但为了让你理解 `commit` 和 `rollback` 的考点，我为你构造一个最经典的**“银行转账”**例子（涵盖原子性）：

**场景**：从账户 A 转账 100 元给账户 B。 这包含两个步骤：

1. 账户 A 扣 100 元。
2. 账户 B 加 100 元。

**如果不用事务（Auto Commit）：**

- 执行完步骤 1（A 扣了钱），突然停电了。
- 结果：A 钱少了，**B 没收到钱。数据不一致。**

**使用事务的逻辑（Atomic Transaction）：**

```
-- 事务隐式开始
Update account set balance = balance - 100 where ID = 'A';
Update account set balance = balance + 100 where ID = 'B';

-- 判定点
If (所有步骤都成功) Then
    Commit Work;  -- 提交：两个更新都生效，永久保存
Else
    Rollback Work; -- 回滚：撤销第一步的扣款，A 的钱变回去，就像没操作过一样
End If;
```

### **💡 考试做题总结**

1. **判断题**：如果问“事务可以部分执行吗？”，答案是 **No**（必须 Atomic）。
2. **关键词**：看到 **Atomic**，对应 **Rollback**；看到 **Persist/Save**，对应 **Commit**。
3. **语法**：记住**标准 SQL 结束事务的两个命令是 `Commit work` 和 `Rollback work`。**

### **4. 完整性约束 (Integrity Constraints)**

#### **(1) 单个关系上的约束 (Constraints on a Single Relation)**

这些约束直接写在 `create table` 语句中，用于限制属性值的合法性。

- **非空约束 (`not null`)** 1
	- **含义**：该属性的值**不能为空。**
	- **写法**：`name varchar(20) not null`
- **唯一约束 (`unique`)** 2
	- **含义**：声明某些属性（候选码）的值在表中**必须是唯一的。**
	- **考点（与 Primary Key 的区别）**：`unique` 定义的**候选码允许为 null** ，而**主码不允许**。
	- **写法**：`unique (name, email)`
- **检查子句 (`check (P)`)** 
	- **含义**：****指定一个谓词 𝑃，所有元组都必须满足该条件。****
	- **真题常考场景**：限制**数值范围（如预算>0）**、限制**枚举值（如季节只能是春夏秋冬）**。
	- **写法**：`check (semester in ('Fall', 'Winter', 'Spring', 'Summer'))` 

#### **(2) 参照完整性 (Referential Integrity) —— ⭐ 必考级联**

这是关于 **外码 (Foreign Key)** 的约束。

- **定义**：保证一个关系（**从表**）中某属性集的**值**，**必须出现在另一个关系（主表）的主码中**。
- **级联操作 (Cascading Actions)** 
	- 当主表（被引用的表）中的数据被删除或更新时，**从表（引用它的表）该怎么办？**
	- **`on delete cascade`**：主表删了某行，从表引用该行的记录**自动跟着删**。
	- **`on update cascade`**：主表改了主码，从表引用的值**自动跟着改**。
	- *其他选项（了解）*：`set null` (设为空), `set default` (设为默认值)。

### **📝 考试实战：满分综合实例 (Master Example)**

题目场景：

创建一个课程表 course，要求：

1. **course_id** 是主码。
2. **title** (课程名) 不能为空。
3. **credits** (学分) 必须大于 0。
4. **dept_name** (系名) 引用 `department` 表的主码。
5. **级联规则**：如果 `department` 表里的系名删除了，这里也自动删除；如果系名改了，这里也自动更新。

**满分 SQL 代码 (背诵这个结构)** ：

```
create table course (
    course_id    char(5) primary key,
    title        varchar(20) not null,
    dept_name    varchar(20),
    credits      numeric(2,0),
    
    -- 检查约束 (Check Constraint)
    check (credits > 0),

    -- 外码与级联约束 (Foreign Key with Cascade)
    foreign key (dept_name) references department
        on delete cascade
        on update cascade
);
```

### **💡 考试做题避坑指南**

1. **Check 约束的写法**：**如果是枚举值（如性别、季节），一定要用 `in (...)`**，别写成一堆 `or`。
	- ✅ `check (gender in ('M', 'F'))`
2. **Unique vs Primary Key**：如果题目说“This field is unique but can be null”，必须用 `unique`，不能用 `primary key`。
3. **级联的语法位置**：`on delete cascade` 是**紧跟在 `foreign key` 语句**的**最后的，不要写到分号外面去。**

### **5. SQL 数据类型与模式 (Data Types and Schemas)**

#### **(1) 内置数据类型 (Built-in Data Types) —— ⭐ 写 DDL 题备用**

除了基础的 `int`, `varchar`，SQL 还支持**与时间相关的类型** 。

- **日期 (`date`)**: 包含年（4位）、月、日。
	- **例子**: `date '2005-7-27'`
- **时间 (`time`)**: 一天中的时间，包含时、分、秒。
	- **例子**: `time '09:00:30'`
- **时间戳 (`timestamp`)**: 日期加上时间。
	- **例子**: `timestamp '2005-7-27 09:00:30.75'`
- **时间间隔 (`interval`)**: 一段时间。
	- **例子**: `interval '1' day`
	- **运算规则**:
		- 日期/时间/时间戳 - 日期/时间/时间戳 = **时间间隔 (Interval)**
		- 日期/时间/时间戳 + 时间间隔 = **新的日期/时间/时间戳**

#### **(2) 索引创建 (Index Creation) —— ⭐ 提速关键**

- **定义**: 索引是一种数据结构，用于**加速 (speed up)** 对特定属性值的记录的访问 。

- **语法**:

	```
	create index <索引名> on <表名>(<属性名>);
	```

- **完整考试实例**:

	- **题目**: 为 `student` 表的 `ID` 属性创建一个名为 `studentID_index` 的索引。

	- **SQL 写法**:

		```
		create index studentID_index on student(ID);
		```

	- **作用**: 当执行 `select * from student where ID='12345'` 时，系统会**利用索引快速找到记录，而不需要扫描全表 。**

#### **(3) 用户自定义类型 (User-Defined Types) —— 了解**

- **语法**: `create type`

- **例子**: 定义一个名为 `Dollars` 的类型，本质是数字 。

	```
	create type Dollars as numeric (12,2) final;
	
	create table department (
	    dept_name varchar (20),
	    building varchar (15),
	    budget Dollars  -- 使用自定义类型
	);
	```

#### **(4) 域 (Domains) —— ⭐ 带约束的类型**

- **定义**: `create domain` 可以创建用户定义的域类型。

- **与 Type 的区别**: 域可以添加 **约束 (Constraints)**（如 `not null`, `check`），而 **Type 不能直接加约束** 。

- **完整考试实例**:

	- **题目**: 创建一个名为 `degree_level` 的域，要求**必须是 'Bachelors', 'Masters', 'Doctorate' 之一。**

	- **SQL 写法** :

		```
		create domain degree_level varchar(10)
		constraint degree_level_test
		check (value in ('Bachelors', 'Masters', 'Doctorate'));
		```

#### **(5) 大对象类型 (Large-Object Types) —— 选择题考点**

当需要存储照片、视频或超长文本时使用 。

- **`blob` (Binary Large Object)**: **二进制**大对象。
	- 用于存储：**未解释的二进制**数据（如**图片、视频**、CAD文件）。
- **`clob` (Character Large Object)**: **字符**大对象。
	- 用于存储：大量的**字符**数据（如**长文档**）。
- **查询特性**: 当查询返回大对象时，通常返回的是一个 **指针 (pointer)**，而不是对象本身（因为太大了）。

### **💡 考试做题总结**

1. **写表定义时**: 如果**题目涉及到“入学日期”，记得用 `date` 类型。**
2. **写索引时**: 记住语法 `create index ... on ...`，别写反了。
3. **概念辨析**: 如果问 `Type` 和 `Domain` 的区别，关键在于 **Domain 可以加约束 (Constraints)**。
4. **填空/选择**: 看到“Photo/Video”，选 **BLOB**；看到“Large Text/Document”，选 **CLOB**。

### **6. 授权 (Authorization)**

#### **(1) 权限的分类 (Forms of Authorization)**

我们主要关注对数据的操作权限 ：

- **Read (读)**: 允许读取数据，但不能修改。
- **Insert (插入)**: 允许插入新数据，但不能修改现有数据。
- **Update (更新)**: 允许修改现有数据，但不能删除。
- **Delete (删除)**: 允许删除数据。

还有针对数据库模式 (Schema) 的权限 ：

- **Index**: 允许创建和删除索引。
- **Resources**: 允许创建新关系（表）。
- **Alteration**: 允许添加或删除关系中的属性。
- **Drop**: 允许删除关系。

#### **(2) 授予权限 (Granting Authorization) —— 核心语法**

- **前提**：授予者 (Grantor) 必须**自己已经拥有该权限**（或是数据库管理员）。

- **语法** ：

	```
	grant <privilege list>
	on <relation name or view name>
	to <user list>;
	```

- **关键点**：

	- **`<privilege list>`**: 可以是 `select`, `insert`, `update`, `delete`，或者用 **`all privileges`** 代表**所有权限 。**
	- **`<user list>`**: 可以是具体的 `user-id`，也可以是关键词 **`public`**（代表所有合法用户），或者是 **角色 (Role)** 。
	- **视图权限独立性**：**在视图 (View) 上授予权限**，**并不意味着自动授予了底层关系** (Underlying relations) 的权限 。

#### **(3) 收回权限 (Revoking Authorization)**

- **语法** ：

	```
	revoke <privilege list>
	on <relation name or view name>
	from <user list>;
	```

- **级联收回 (Cascading Revocation)** ：

	- 这是判断题的高频考点。如果 A 授予权限给 B，B 又授予给 C。当 A 收回 B 的权限时，**C 的权限也会被自动收回**（**除非使用了特定的 restrict 选项**，但讲义未详述 restrict，重点记住**默认会级联收回**）。

- **Public 的收回** ：

	- 如果从 `public` 收回权限，只有那些**被显式 (explicitly)** 授予了该权限的用户才能保留权限，其他人都会失去。

#### **(4) 角色 (Roles) —— 权限的打包**

- **概念**：角色本质上是一个“职位”或“组”，可以将一组权限**赋给角色，再将角色赋给用户** 。
- **操作流程**：
	1. **创建角色**：`create role instructor;` 
	2. **给角色授权**：`grant select on takes to instructor;` 
	3. **给用户赋角色**：`grant instructor to Amit;` 
- **角色的继承 (Chain of roles)** ：
	- 角色**可以授予给另一个角色。**
	- *例子*：如果有角色 `teaching_assistant`，执行 `grant teaching_assistant to instructor;`，**那么 `instructor` 角色就继承了 `teaching_assistant` 的所有权限。**

#### **(5) 视图上的授权 (Authorization on Views) —— 逻辑判断题难点**

- **创建视图的要求**：如果你**想创建一个视图（比如 `se_instructor`）**，你必须**拥有底层表（`instructor`）的 `select` 权限**，否则系统会拒绝创建 。
- **使用视图的权限**：
	- 假设 Bob 创建了视图 `se_instructor` 并**把该视图的 `select` 权限授予给了 Alice**。
	- **关键判断**：即使 Alice **没有**底层表 `instructor` 的权限，她**依然可以查询 `se_instructor` 视图（前提是 Bob 有底层表的权限）**。这就是视图作为安全机制的作用——**向用户隐藏数据** 。

### **📝 综合复习实例 (Comprehensive Example)**

假设我们有一个教务系统，包含表 `takes` (学生选课表)。

**场景步骤**：

1. **DBA 创建角色**：创建一个名为 `student_role` 的角色。

	```
	create role student_role;
	```

2. **授予权限给角色**：允许该角色**读取** `takes` 表，并能**插入**新记录。

	```
	grant select, insert on takes to student_role;
	```

3. **分配角色给用户**：将此角色分配给用户 `ZhangSan`。

	```
	grant student_role to ZhangSan;
	```

4. **直接授权**：DBA 直接给用户 `LiSi` 授予 `takes` 表的所有权限。

	```
	grant all privileges on takes to LiSi;
	```

5. **收回权限**：发现学生不能随意插入选课记录，决定收回角色的插入权限。

	```
	revoke insert on takes from student_role;
	```

	- *结果判断*：此时 `ZhangSan` 失去了插入权限，但 `LiSi` 依然拥有所有权限（因为他是被单独显式授权的）。

### **💡 考试做题避坑指南 (Judgment/Choice)**

1. **级联逻辑**：如果题目说 "Revoking a privilege from a user never affects other users"（从一个用户收回权限从不影响其他用户），这是 **False**。因为**存在级联收回 (Cascading)**。
2. **视图权限**：如果题目说 "To query a view, a user must have select privilege on the underlying relations"（要查询视图，**用户必须拥有底层关系的查询权限**），这是 **False**。用**户只需要视图的权限**，**视图的所有者才需要底层关系的权限**。
3. **Public 陷阱**：`grant ... to public` 意味着**所有人**都有了权限。 `revoke ... from public` 意味着收**回了这个“公共权限”**，但**不会收回那些“指名道姓”给某人的**权限。

### **复习建议**

这一章的**重点**在于 **Join (连接)** 和 **Integrity Constraints (完整性约束)**。

1. **Joins**: 务必搞清楚 Left Outer Join 和 Inner Join 在**处理不匹配数据时的区别（是否补 Null）**。
2. **Constraints**: 考试常考在 `Create Table` 语句中**写出 `check` 约束或 `foreign key ... on delete cascade`。**
3. **Views**: 记住**视图更新的苛刻条件**。

# ch6

收到。我们严格依据 **《ch6.pdf》 (Chapter 6: Formal Relational Query Languages)** 的内容（第 8 页，第 28-39 页），为你详细总结 **“4. 扩展关系代数 (Extended Relational-Algebra)”**。

这一部分是关系代数中对应 SQL 高级功能（如算术运算、Group By、Outer Join）的关键部分。考试中，**Query Processing (查询处理)** 部分经常要求你将带有 Group By 的 SQL 语句转换为对应的关系代数表达式。

### **4. 扩展关系代数 (Extended Relational-Algebra)**

#### **(1) 广义投影 (Generalized Projection)**

- **对应 SQL**: `SELECT` 子句中的算术运算（如 `salary * 1.1`）。

- **定义**：扩展了标准的投影运算 (Π)，允许在投影列表中使用 **算术表达式 (Arithmetic functions)**，而**不仅仅是属性名**。

- 符号：

	Π𝐹1,𝐹2,...,𝐹𝑛(𝐸)

	- 𝐸: 关系代数表达式。
	- 𝐹𝑖: 可以是属性名，也**可以是涉及常量和属性的算术表达式。**

【完整考试实例】

题目：给定关系 instructor(ID, name, dept_name, salary)，其中 salary 是年薪。请查询每个老师的 ID、名字以及月薪 (Monthly Salary)。

- 关系代数写法 ：

	Π𝐼𝐷,𝑛𝑎𝑚𝑒,𝑑𝑒𝑝𝑡_𝑛𝑎𝑚𝑒,𝑠𝑎𝑙𝑎𝑟𝑦/12(𝑖𝑛𝑠𝑡𝑟𝑢𝑐𝑡𝑜𝑟)

- **结果**：结果关系中，第四列的值**将是原表中 `salary` 除以 12 的结果。**

#### **(2) 聚合函数 (Aggregate Functions) —— ⭐ 必考：Group By 的转换**

- **对应 SQL**: `GROUP BY` 子句以及 `AVG`, `SUM`, `COUNT` 等函数。

- 符号：使用手写体 𝐺 (Calligraphic G) 表示 3。

	𝐺1,𝐺2...𝐺𝐹1(𝐴1),𝐹2(𝐴2)...(𝐸)

- **参数详解** ：

	- **左下标 (𝐺1,𝐺2...)**：**分组属性列表** (List of attributes on which to group)。如果为空，则**表示对整个关系进行聚合（不分组）**。
	- **右下标 (𝐹1(𝐴1)...)**：**聚合函数列表**。𝐹𝑖 是**函数名 (avg, min, max, sum, count)**，𝐴𝑖 是属性名。
	- **𝐸**：输入的**关系表达式**。

【完整考试实例】

题目：找出每个系的平均工资 (Find the average salary in each department)。

- **SQL 思路**：`Select dept_name, avg(salary) ... Group By dept_name`。

- 关系代数写法 5：

	𝑑𝑒𝑝𝑡_𝑛𝑎𝑚𝑒𝐺𝑎𝑣𝑔(𝑠𝑎𝑙𝑎𝑟𝑦) 𝑎𝑠 𝑎𝑣𝑔_𝑠𝑎𝑙𝑎𝑟𝑦(𝑖𝑛𝑠𝑡𝑟𝑢𝑐𝑡𝑜𝑟)

	(注意：**可以使用 `as` 给计算出的结果列重命名，就像 SQL 一样** )。

【无分组实例】

题目：计算**全校**所有老师的总工资 (Sum of all salaries)。

- 写法：

	∅𝐺𝑠𝑢𝑚(𝑠𝑎𝑙𝑎𝑟𝑦)(𝑖𝑛𝑠𝑡𝑟𝑢𝑐𝑡𝑜𝑟)

	(左下标**为空，表示不分组**)。

### **💡 考试做题技巧 (Exam Tips)**

1. **Group By 怎么写**：
	- 只要看到 SQL 里的 `GROUP BY A, B`，在关系代数里直接把 A, B 写在 𝐺 的**左下角**。
	- 只要看到 SQL 里的 `Select avg(C)`，直接把 `avg(C)` 写在 𝐺 的**右下角**。
	- **公式**：`SQL: Group By X Select F(Y)` ⇔ **RA**: 𝑋𝐺𝐹(𝑌)。
2. **Null 的处理**：
	- 在关系代数中，涉及 Null 的算术运算结果总是 **Null** 12。
	- 在聚合函数中，Null 通常被**忽略** (Ignore) 13。
3. **符号规范**：
	- 考试手写时，𝐺 要写得**像花体字**，以**区别于普通的 G。**
	- 广义投影仍然用 Π，只是**下标里多了加减乘除。**

收到。我们严格按照 **《ch6.pdf》 (Chapter 6: Formal Relational Query Languages)** 的第 40-41 页内容，为你详细总结 **“数据库修改 (Modification of the Database)”**。

需要特别注意的是，这一章讲的是 **关系代数 (Relational Algebra)**，所以这里的“修改”是用代数表达式（赋值操作）来表示的，而不是你之前在 SQL（第三章）里学的 `DELETE FROM` 或 `UPDATE` 语句。考试如果考这一章的修改，要求你写的是**带有箭头 (←) 的代数公式**。

以下是严格依据讲义的详细总结与实例：

### **7. 数据库修改 (Modification of the Database)**

在关系代数中，我们使用 **赋值操作 (Assignment Operation, ←)** 来表达对数据库内容的修改 。

#### **(1) 删除 (Deletion)**

- **原理**：删除本质上是 **集合差运算 (Set Difference)**。从原关系 𝑟 中减去要删除的元组集合 𝐸。

	𝑟←𝑟−𝐸

	- 𝑟：要修改的关系（表）。
	- 𝐸：一个关系代数表达式，计算出**所有需要删除的元组**。

【完整考试实例】

题目：从 instructor 表中删除所有 "Finance" 系的老师。

- **分析**：

	1. 先找出要删的人（集合 𝐸）：𝜎𝑑𝑒𝑝𝑡_𝑛𝑎𝑚𝑒=′𝐹𝑖𝑛𝑎𝑛𝑐𝑒′(𝑖𝑛𝑠𝑡𝑟𝑢𝑐𝑡𝑜𝑟)
	2. 从原表中减去这些人。

- 关系代数写法：

	𝑖𝑛𝑠𝑡𝑟𝑢𝑐𝑡𝑜𝑟←𝑖𝑛𝑠𝑡𝑟𝑢𝑐𝑡𝑜𝑟−𝜎𝑑𝑒𝑝𝑡_𝑛𝑎𝑚𝑒=′𝐹𝑖𝑛𝑎𝑛𝑐𝑒′(𝑖𝑛𝑠𝑡𝑟𝑢𝑐𝑡𝑜𝑟)

#### **(2) 插入 (Insertion)**

- **原理**：插入本质上是 **集合并运算 (Union)**。将原关系 𝑟 与要插入的新元组集合 𝐸 合并。

	𝑟←𝑟∪𝐸

	- 𝐸：包含待插入元组的集合（通常是一个常量关系）。

【完整考试实例】

题目：向 course 表中插入一门新课：ID为 "CS-101"，名为 "Intro to CS"，系为 "Comp. Sci."，学分为 4。

- **分析**：

	1. 构造要插入的元组集合 𝐸：{(′𝐶𝑆−101′,′𝐼𝑛𝑡𝑟𝑜𝑡𝑜𝐶𝑆′,′𝐶𝑜𝑚𝑝.𝑆𝑐𝑖.′,4)}
	2. 将其并入原表。

- 关系代数写法：

	𝑐𝑜𝑢𝑟𝑠𝑒←𝑐𝑜𝑢𝑟𝑠𝑒∪{(′𝐶𝑆−101′,′𝐼𝑛𝑡𝑟𝑜𝑡𝑜𝐶𝑆′,′𝐶𝑜𝑚𝑝.𝑆𝑐𝑖.′,4)}

#### **(3) 更新 (Updating)**

- **原理**：更新本质上是 **广义投影 (Generalized Projection)**。我们并不直接修改原来的行，而是计算出一个**新**的关系，其中某些属性的值发生了变化，然后把这个新关系赋值给原关系名。

	𝑟←Π𝐹1,𝐹2,...,𝐹𝑛(𝑟)

	- 𝐹𝑖：如果是**不需要修改**的属性，直接写属性名（第 𝑖 个属性）。
	- 𝐹𝑖：如果是**需要修改**的属性，写成涉及常量和属性的**算术表达式**。

【完整考试实例】

题目：给 instructor 表（属性为 ID, name, dept_name, salary）中的所有老师涨薪 5%。

- **分析**：

	- ID, name, dept_name 保持不变。
	- salary 变为 `salary * 1.05`。

- 关系代数写法：

	𝑖𝑛𝑠𝑡𝑟𝑢𝑐𝑡𝑜𝑟←Π𝐼𝐷,𝑛𝑎𝑚𝑒,𝑑𝑒𝑝𝑡_𝑛𝑎𝑚𝑒,𝑠𝑎𝑙𝑎𝑟𝑦∗1.05(𝑖𝑛𝑠𝑡𝑟𝑢𝑐𝑡𝑜𝑟)

【难点：带条件的更新】

如果题目说“只给 Finance 系的老师涨薪”，在关系代数里**很难直接用一条投影写出来（因为投影会对所有行生效）。**

通常需要拆分成两步（虽然讲义没展开，但逻辑如下，供参考）：

1. 找出 Finance 系的人并涨薪，存为临时关系 𝑡1。

2. 找出非 Finance 系的人（保持原薪），存为临时关系 𝑡2。

3. 𝑖𝑛𝑠𝑡𝑟𝑢𝑐𝑡𝑜𝑟←𝑡1∪𝑡2。

	(注：考试主要考查上面那种“全体更新”的简单投影形式)

### **💡 考试做题避坑指南**

1. **符号区别**：看到题目问 **"Relational Algebra Expression for Deletion"**，千万别写 SQL 的 `DELETE FROM ...`！一定要写 𝑟←𝑟−...。
2. **更新的逻辑**：更新不是用 update 算子，而是用 **投影 (Π)**。这很反直觉，要特别记忆。
3. **多重集 (Multiset) 的影响** 5：
	- 虽然**标准关系代数是基于集合（去重）的**，但实际数据库（SQL）是基于 **Multiset (多重集/包)** 的。
	- 在 **Multiset 模式下：**
		- **并 (∪)**：𝑟 有 𝑚 个副本，𝑠 有 𝑛 个副本，**结果有 𝑚+𝑛 个副本。**
		- **差 (−)**：**结果有 𝑚𝑎𝑥(0,𝑚−𝑛) 个副本。**
	- *这一点通常考选择题概念，写公式时只需按标准写法即可。*

### **8. SQL 与 关系代数的对应 (SQL and Relational Algebra)**

#### **(1) 基础查询 (Basic Query without Aggregation)**

最基本的 `Select-From-Where` 结构。

- **SQL 模板** 1:

	SQL

	```
	select A1, A2, ..., An
	from r1, r2, ..., rm
	where P
	```

- 对应的关系代数 (RA)2:

	Π𝐴1,𝐴2,...,𝐴𝑛(𝜎𝑃(𝑟1×𝑟2×...×𝑟𝑚))

- **转换步骤 (做题口诀)**：

	1. **From 子句** → 变成 **笛卡尔积 (×)**。把所有表连起来：𝑟1×𝑟2×...
	2. **Where 子句** → 变成 **选择 (𝜎𝑃)**。包裹在笛卡尔积外面。
	3. **Select 子句** → 变成 **投影 (Π...)**。包裹在最外面，只保留 SQL 里要求的列。

#### **(2) 带聚合与分组的查询 (Aggregation and Group By)**

当 SQL 中出现 `group by` 和聚合函数（如 `sum`, `avg`）时，必须使用扩展关系代数的 **聚合算子 (𝐺)**。

- **SQL 模板** 3:

	SQL

	```
	select A1, A2, sum(A3)
	from r1, r2, ..., rm
	where P
	group by A1, A2
	```

- 对应的关系代数 (RA)**注意分组需要写在外面,注意执行顺序:**

	𝐴1,𝐴2𝐺𝑠𝑢𝑚(𝐴3)(𝜎𝑃(𝑟1×𝑟2×...×𝑟𝑚))

- **转换步骤 (做题口诀)**：

	1. **From & Where** → 先处理笛卡尔积和选择：𝜎𝑃(𝑟1×...)。
	2. **Group By 子句** → 变成 𝐺 的 **左下标** (分组属性)：𝐴1,𝐴2𝐺。
	3. **Select 中的聚合函数** → 变成 𝐺 的 **右下标** (函数)：𝐺𝑠𝑢𝑚(𝐴3)。
	4. **注意**：在这种简单情况下（Select 的非聚合列 = Group By 列），**不需要**再写最外层的 Π。

#### **(3) 进阶：Select 列少于 Group By 列 (General Case)**

这是考试的一个**陷阱**。如果 SQL 按 `A1, A2` 分组，但最后**只输出了 `A1` 和 `sum(A3)`（没输出 A2），关系代数里必须在聚合之后再加一个 投影 (Π)。**

- **SQL 模板** 5:

	SQL

	```
	select A1, sum(A3)      -- 注意：这里没有 A2
	from r1, r2, ..., rm
	where P
	group by A1, A2         -- 但是按 A1, A2 分组
	```

- 对应的关系代数 (RA)6:

	Π𝐴1,𝑠𝑢𝑚𝐴3(𝐴1,𝐴2𝐺𝑠𝑢𝑚(𝐴3) as 𝑠𝑢𝑚𝐴3(𝜎𝑃(𝑟1×...×𝑟𝑚)))

- **关键点**：

	1. 先做聚合 𝐴1,𝐴2𝐺...，计算**出所有分组的结果。**
	2. 再在外面套一个 Π𝐴1,𝑠𝑢𝑚𝐴3，**把不需要显示的 `A2` 去掉。**
	3. **重命名 (Rename)**：为了投影方便，通常会在**聚合时给结果起个别名（`as sumA3`）**，然后在投影里引用这个名字。

### **📝 考试实战：完整转换实例 (Complete Example)**

题目：

将以下 SQL 语句转换为等价的关系代数表达式。

场景：找出每个系的平均工资，但只考虑工资大于 50000 的老师，并且只输出系名和平均工资值。

**SQL 语句**：

```
select dept_name, avg(salary)
from instructor
where salary > 50000
group by dept_name;
```

**解题步骤**：

1. **识别 From 和 Where**：

	- 源表：`instructor`
	- 条件：`salary > 50000`
	- RA 部分：𝜎𝑠𝑎𝑙𝑎𝑟𝑦>50000(𝑖𝑛𝑠𝑡𝑟𝑢𝑐𝑡𝑜𝑟)

2. **识别 Group By (左下标)**：

	- 分组列：`dept_name`
	- RA 部分：𝑑𝑒𝑝𝑡_𝑛𝑎𝑚𝑒𝐺

3. **识别 Select 聚合 (右下标)**：

	- 函数：`avg(salary)`
	- RA 部分：𝐺𝑎𝑣𝑔(𝑠𝑎𝑙𝑎𝑟𝑦)

4. 组合：

	𝑑𝑒𝑝𝑡_𝑛𝑎𝑚𝑒𝐺𝑎𝑣𝑔(𝑠𝑎𝑙𝑎𝑟𝑦)(𝜎𝑠𝑎𝑙𝑎𝑟𝑦>50000(𝑖𝑛𝑠𝑡𝑟𝑢𝑐𝑡𝑜𝑟))

(进阶变体)：

如果 SQL 是 select avg(salary) from ... group by dept_name (不显示系名，只显示一堆数字)。

- RA 写法：需要在外面加投影。

	Π𝑎𝑣𝑔_𝑠𝑎𝑙(𝑑𝑒𝑝𝑡_𝑛𝑎𝑚𝑒𝐺𝑎𝑣𝑔(𝑠𝑎𝑙𝑎𝑟𝑦) as 𝑎𝑣𝑔_𝑠𝑎𝑙(𝜎𝑠𝑎𝑙𝑎𝑟𝑦>50000(𝑖𝑛𝑠𝑡𝑟𝑢𝑐𝑡𝑜𝑟)))

### **💡 考试避坑指南**

1. **执行顺序**：永远记住 **𝜎 (Select) 先做**，**Π (Project) 最后做**。对应到 SQL **就是 Where 先执行，Select 最后输出。**
2. **符号规范**：
	- 普通 Select (选列) 用 **Π**。
	- 条件 Select (选行) 用 **𝜎**。
	- Group By 用 **𝐺**。
	- 这三个符号千万别搞混，写错了直接 0 分。
3. **多表连接**：如果 `From` 后面有多个表，记得在 𝜎 里面写 𝑟1×𝑟2，或者直接写 𝑟1⋈𝑟2 (如果 **Where 里有连接条件**)。讲义公式用的是 ×，**写 × 最保险** 。

### **复习建议**

1. **符号记忆**: 必须记准 𝜎 (选行) 和 Π (选列)，这是最容易混淆的。
2. **表达式书写**: 练习。例如：“找出 Physics 系工资大于 90000 的老师名字”。
	- SQL: `Select name from instructor where dept_name='Physics' and salary > 90000`
	- RA: Π𝑛𝑎𝑚𝑒(𝜎𝑑𝑒𝑝𝑡_𝑛𝑎𝑚𝑒=′𝑃ℎ𝑦𝑠𝑖𝑐𝑠′∧𝑠𝑎𝑙𝑎𝑟𝑦>90000(𝑖𝑛𝑠𝑡𝑟𝑢𝑐𝑡𝑜𝑟))
3. **除法运算**: 虽然讲义这里没详细展开“除法 (Division)”，但在关系代数完整体系中 `Query 1` (找出选修了所有Biology课程的学生) 实际上隐含了除法逻辑，可以用两重差集表示 (参考 Ch3 笔记)。

# ch7

### **1. 基本概念 (Modeling Concepts)**

#### **(1) 实体与实体集 (Entities and Entity Sets)**

- **实体 (Entity)**:
	- **定义**: 现实世界中存在的、可以与其他对象区分开来的**对象 (object)**。
	- *例子*: 一个具体的人（比如 Einstein）、一家具体的公司、一次具体的事件。
	- 每个实体都有一组 **属性 (Attributes)** 来描述它（例如人的名字、地址）。
- **实体集 (Entity Set)**:
	- **定义**: **具有相同属性的同类实体的集合 (set of entities of the same type)。**
	- *例子*: `instructor` (所有老师的集合), `student` (所有学生的集合)。
	- **外延 (Extension)**: 实体集在某一时刻的具体实际内容（即具体有哪些人）。

#### **(2) 属性 (Attributes) —— ⭐ 必考：符号与分类**

实体**通过属性来表示**。每个属性都有一个 **域 (Domain)**，即**允许取值的集合。**

**属性的分类 ( Attribute Types )**：

1. **简单属性 (Simple Attributes)**:
	- **不可再分的属性**。例如：`ID`。
2. **复合属性 (Composite Attributes)**:
	- 可以**进一步划分为子属性 (Sub-attributes)。**
	- *例子*: `name` 可以分为 `first_name`, `middle_initial`, `last_name`。
	- *作用*: 当用户需要**分别访问组成部分时使用。**
3. **单值属性 (Single-valued Attributes)**:
	- 对于一个特定的实体，该**属性只有一个值**。例如：`ID`（每人只有一个工号）。
4. **多值属性 (Multivalued Attributes)**:
	- 对于一个特定的实体，该**属性可能有多个值**。
	- *例子*: `{phone_number}` (一个人**可能有多个电话号码)**。
	- *画图符号*: **双边框椭圆 (Double Ellipse)**。
5. **派生属性 (Derived Attributes)**:
	- 该属性的值**可以从其他属性或数据计算出来**，并**不直接存储**。
	- *例子*: **`age` (年龄) 可以根据 `date_of_birth` (出生日期) 和当前日期计算得出。**
	- *画图符号*: **虚线椭圆 (Dashed Ellipse)**。

#### **(3) 联系与联系集 (Relationship and Relationship Sets)**

- **联系 (Relationship)**:

	- 多个实体之间的**关联 (association)**。
	- *例子*: 老师 `Peltier` 是学生 `Hayes` 的 `advisor` (导师)。

- **联系集 (Relationship Set)**:

	- **同类联系的数学关系**。如果 𝐸1,𝐸2,...,𝐸𝑛 是实体集，**联系集 𝑅 是它们的笛卡尔积的一个子集：**

		𝑅⊆{(𝑒1,𝑒2,...𝑒𝑛)|𝑒1∈𝐸1,𝑒2∈𝐸2,...,𝑒𝑛∈𝐸𝑛}

	- *例子*: `advisor` 是一个**联系集**，包含**所有 (instructor, student) 的配对。**

- **联系的属性 (Attributes of a Relationship)**:

	- **联系本身也可以有属性，**用来**描述这个关联的特性**。
	- *例子*: `advisor` 联系集可以**有一个属性 `date`，表示“哪天开始指导的”。**

- **角色 (Roles)**:

	- **实体在联系中扮演的功能**。**通常是隐含**的，但如果**同一个实体集参与同一个联系集多次（自环）**，则需要**显式标明角色**。
	- *例子*: `course` 实体集**参与 `prereq` (先修课) 联系集**，**一个角色是 `course_id` (本课)**，**另一个角色是 `prereq_id` (先修课)。**

- **度 (Degree)**:

	- **参与联系集的实体集的数量**。
	- **二元 (Binary)**: 度为 **2 (最常见)**。
	- **三元 (Ternary)**: 度为 **3 (例如：学生、老师、项目 三者之间的联系)。**

### **📚 完整复习实例 (Comprehensive Example)**

为了涵盖上述所有知识点，我们构建一个 **Customer (客户)** 实体集的例子 。

场景描述：

银行系统中有一个实体集 Customer。

1. **ID**: 客户的唯一标识（**简单属性**）。
2. **Name**: 客户姓名，**由 `first_name`, `middle_initial`, `last_name` 组成**（**复合属性**）。
3. **Address**: 地址，**由 `street` (可再分为 `street_number`, `street_name`, `apt_number`), `city`, `state`, `zip` 组成**（**嵌套的复合属性**）。
4. **Phone_number**: 客户**可能有多个电话**（**多值属性**，**画双椭圆**）。
5. **Age**: 客户年龄，由出生日期**算出**（**派生属性**，**画虚线椭圆**）。
6. **Loan (贷款)**: 这是一个 **联系集**，**连接 `Customer` 和 `Loan_Account`，**并且这个**联系上有一个属性 `access_date` (最近访问日期)**。

**💡 考试做题/画图技巧**：

- **画图时**：看到“**多个**电话”、“多种技能”，立刻画 **双椭圆**。看到**“年龄”、“教龄”，立刻画 虚线椭圆。**
- **转换表时**：
	- **简单属性/派生属性** → **一列**（**派生属性通常不存**，或者**存计算值**）。
	- **复合属性** → **打散成多个列**（如 `name_first`, `name_last`），**不要把 `name` 自己当一列。**
	- **多值属性** → **必须新建一张表**，**不能直接放在原表里**（这是 Part IV 转换题的考点）。

### **2. 约束 (Constraints)**

约束主要分为两类：**映射基数 (Mapping Cardinalities)** 和 **参与约束 (Participation Constraints)**。

#### **(1) 映射基数 (Mapping Cardinalities) —— ⭐ 决定箭头画在哪**

- **定义**: 表示**一个实体通过联系集能关联多少个其他实体。**
- **符号规则 (必背)**:
	- **箭头 (→)**: 代表 **“一” (One)**。即“至**多一个**”。
	- **直线 (—)**: 代表 **“多” (Many)**。即**“任意数量（0个或多个）”**。
	- *记忆口诀*: **箭头指向谁，谁就是“唯一的那个” (Point to the "One")**。

【四种类型与完整实例】

假设我们有**实体集 instructor (老师) 和 student (学生)，联系集是 advisor (指导)。**

1. **一对一 (One-to-One, 1:1)**:
	- **语义**: 一个老师**至多指导一个学生，一个学生至多有一个导师。**
	- **画法**: `instructor` ← `advisor` → `student`。
	- *(两边都是箭头)*。
2. **一对多 (One-to-Many, 1:N)**:
	- **语义**: 一个老师可以指导**多个**学生，但一个学生**至多有一个导师。**
	- **画法**: `instructor` ← `advisor` —— `student`。
	- *(箭头指向老师，表示老**师是“1”的一方**；**学生那边是直线**，**表示“多”**)*。
3. **多对一 (Many-to-One, N:1)**:
	- **语义**: 一个老师至多指导一个学生，但一个学生可以有**多个**导师。
	- **画法**: `instructor` —— `advisor` → `student`。
	- *(箭头指向学生，表示学生是“1”的一方)*。
4. **多对多 (Many-to-Many, M:N)**:
	- **语义**: 一个老师指导多个学生，一个学生有多个导师。
	- **画法**: `instructor` —— `advisor` —— `student`。
	- *(**两边都是直线**)*。

#### **(2) 参与约束 (Participation Constraints) —— ⭐ 决定单线还是双线**

- **定义**: 实体集中的实体是否**必须**参与到联系中。

1. **全部参与 (Total Participation)**:
	- **含义**: **实体集中**的 **每一个 (Every) 实体**都**必须至少参与一个联系。**
	- **符号**: **双实线 (Double line)**。
	- *例子*: “每个学生**必须有一个导师”**。
	- *画法*: `student` 用 **双线** 连接到 `advisor`。
2. **部分参与 (Partial Participation)**:
	- **含义**: 实体集中的实体 **可以不 (May not)** 参与联系。
	- **符号**: **单实线 (Single line)**。
	- *例子*: “老师 **不一定 要指导学生**”（有的老师可能只搞科研不带学生）。
	- *画法*: `instructor` 用 **单线** 连接到 `advisor`。

### **📚 综合考试实例 (Comprehensive Exam Example)**

题目描述：

设计一个 ER 图片段，描述 Student (学生) 和 Department (系) 之间的 Major_in (主修) 关系。

1. **Cardinality**: 一个学生可以有多个主修专业（例如双学位），但一个系显然有多个学生。这是一个 **多对多 (Many-to-Many)** 关系。
2. **Participation**:
	- 每个学生 **必须 (Must)** **至少属于一个系**（不能没有专业）。
	- **每个系 不一定 (May not) 有学生**（比如新成立的系，暂时没人）。

**【满分画法描述】**

1. **实体**：画**矩形 `Student` 和 `Department`。**
2. **联系**：画**菱形 `Major_in` 在中间。**
3. **连线 (基数)**：
	- `Student` 到 `Major_in`：**直线** (代表多)。
	- `Department` 到 `Major_in`：**直线** (代表多)。
	- *(因为是多对多)*
4. **连线 (参与)**：
	- `Student` 到 `Major_in`：**双线** (Double line)。
		- *理由*: 题目说了“学生 **必须** 属于一个系” → **全部参与**。
	- `Department` 到 `Major_in`：**单线** (Single line)。
		- *理由*: 题目说了“系 **不一定** 有学生” → **部分参与**。

**💡 考试做题技巧**：

- **找关键词**：
	- 看到 **"At most one"** → 画 **箭头**。
	- 看到 **"Must", "Every", "Required"** → 画 **双线**。
- **默认规则**：如果题目没特别说明基数，通常生活中的“学生-导师”默认是 **多对一** (一个学生一个导师)；“学生-课程”默认是 **多对多**。如果没说**参与度**，通常默认是 **部分参与** (单线)，**除非有显式的 "must"。**

### **3. 码 (Keys)**

#### **(1) 实体集的码 (Keys for Entity Sets)**

- **超码 (Superkey)**:
	- **定义**: 一个或多个属性的集合，其值能够**唯一标识 (uniquely identify)** 实体集中的每一个实体。
	- *例子*: 对于 `instructor` (老师) 实体集，`{ID}` 是超码，因为每个老师 ID 不同。`{ID, name}` **也是超码**，因为 ID 已经唯一了，加上名字**更唯一**。
- **候选码 (Candidate Key)**:
	- **定义**: **最小的超码 (minimal superkey)**。即：它的任意真子集都无法成为超码。
	- *例子*: `{ID}` 是候选码（因为去掉 ID 就空了，没法标识）。但 `{ID, name}` 不是候选码（因为去掉 name 后，剩下的 `{ID}` 依然能唯一标识）。
- **主码 (Primary Key)**:
	- **定义**: 数据库**设计者**从候选码中选出来作为主要标识符的那个候选码。
	- **ER 图画法**: 在**主码属性下方画 下划线 (underscore)。**
	- *例子*: 选中 `ID` 作为 `instructor` 的主码，所以在 ER 图中 `ID` **下面要画横线。**

#### **(2) 联系集的码 (Keys for Relationship Sets) —— ⭐ 难点与考点**

联系集（菱形）本身**没有像实体那样的“自带属性”作为码**，它的码是由 **参与该联系的实体集的主码** 组合而成的。

我们要根据 **映射基数 (Mapping Cardinalities)** 来**决定联系集的主码是什么**。假设联系集 𝑅 关联了实体集 𝐸1 和 𝐸2，它们的**主码分别是 𝑃𝐾1 和 𝑃𝐾2。**

**规则总结 (必背)**：

1. **多对多 (Many-to-Many, M:N)**:
	- **主码**: **双方实体集主码的并集 (Union of primary keys)**。
	- ***公式\*: 𝑃𝑟𝑖𝑚𝑎𝑟𝑦_𝐾𝑒𝑦(𝑅)=𝑃𝐾1∪𝑃𝐾2**
	- *逻辑*: **既然是多对多**，你需要**知道“哪一个老师”和“哪一个学生”配对**，才能唯一确定这个联系。
2. **多对一 (Many-to-One, N:1)**:
	- **主码**: **“多” (Many) 那一方的主码**。
	- *逻辑*: 比如 `student` (多) —— `advisor` → `instructor` (一)。**一个学生只能有一个导师**。所以，**只要给出一个 `student_ID`，我就能唯一确定这个指导关系（因为这个学生不可能对应两个导师）。**
	- *注意*: 在 1:N 或 N:1 中，**箭头指向“一”，直线连接“多”**。主码选的是 **直线那头 (Many side)** 的主码。
3. **一对一 (One-to-One, 1:1)**:
	- **主码: 任意一方 的主码**都可以作为联系集的主码。
	- *逻辑*: 因为两边都是唯一的，选谁都能唯一确定这个关系。

### **📚 完整考试实例 (Comprehensive Exam Example)**

**场景**：

- 实体集 **`student`**，主码是 `s_id`。
- 实体集 **`instructor`**，主码是 `i_id`。
- 联系集 **`advisor`**。

**情况 A: 多对多 (M:N)**

- **语义**: 学生有多个导师，导师带多个学生。
- **ER 图**: `student` —— `advisor` —— `instructor` (**全是直线**)。
- **联系集的码**: `{s_id, i_id}`。
	- *解释*: **必须同时指明是哪个学生和哪个老师**，才能定位到这条指导记录。

**情况 B: 多对一 (N:1)** (即**学生是多**，老师是一)

- **语义**: 一个学生只能有一个导师，导师带多个学生。
- **ER 图**: `student` —— `advisor` → `instructor`。
- **联系集的码**: `{s_id}`。
	- *解释*: **因为 `s_id` 是“多”的一方。**只要定了学生，导师就定了。

**情况 C: 一对一 (1:1)**

- **语义**: 导师只带一个学生，学生只有一个导师。
- **ER 图**: `student` ← `advisor` → `instructor` (全是箭头)。
- **联系集的码**: **可以是 `{s_id}`，也可以是 `{i_id}`。**

### **💡 考试做题技巧**

1. **找“直线”**：在判断联系集的码时，哪怕你记不住复杂的规则，只要记住一点——**谁那边是直线（代表“多”），就选谁的主码**。
	- 如果两边都是直线（多对多），那就**两边都选。**
	- 如果两边都是箭头（一对一），那就**随便选一个。**
2. **主码下划线**：画图时千万别忘了**给属性加下划线，这是给分点**。只有实体集的主码才画下划线，**联系集在 ER 图上通常不标属性**，所以**不需要给联系集画下划线**（除非联系集有显式的描述性属性，但描述性属性通常不做主码）。

### **4. ER 图符号 (E-R Diagram Symbols)**

#### **(1) 基础组件 (Basic Components)**

- **矩形 (Rectangle)**: 代表 **实体集 (Entity Set)**。
	- *例子*: 画一个方框，里面写 `Student`。
- **菱形 (Diamond)**: 代表 **联系集 (Relationship Set)**。
	- *例子*: 画一个菱形，里面写 `advisor`。
- **椭圆 (Ellipse)**: 代表 **属性 (Attribute)**。
	- *例子*: 画一个**椭圆**，**里面写 `name`。**
- **线段 (Line)**:
	- 将**属性连接**到**实体**集。
	- 将**实体集连接**到**联系**集。

#### **(2) 属性的特殊符号 (Attribute Variations)**

- **实下划线 (Solid Underline)**: 代表 **主码 (Primary Key)**。
	- *必须画*: 任何**强实体集**都必须有**至少一个属性带下划线。**
- **双椭圆 (Double Ellipse)**: 代表 **多值属性 (Multivalued Attribute)**。
	- *例子*: `{phone_number}` (一个人有多个电话)。
- **虚线椭圆 (Dashed Ellipse)**: 代表 **派生属性 (Derived Attribute)**。
	- *例子*: `age` (由**生日算出，不直接存储**)。

#### **(3) 联系的约束符号 (Relationship Constraints) —— ⭐ 易错点**

- **箭头 (→)**: 代表基数中的 **“一” (One)**。
	- *用法*: 从联系集指向实体集，表示该实体集中的实体至多参与一次。
- **直线 (—)**: 代表基数中的 **“多” (Many)**。
	- *用法*: 没有任何箭头的普通线段。
- **双线 (Double Line)**: 代表 **全部参与 (Total Participation)**。
	- *含义*: 实体集中的**每一个**实体都必须参与该联系。
	- *画法*: 在实体集和联系集之间画两条平行的线。

#### **(4) 弱实体集符号 (Weak Entity Sets) —— ⭐ 高级考点**

- **双边框矩形 (Double Rectangle)**: 代表 **弱实体集 (Weak Entity Set)**。
	- *例子*: `Section` (课程的**开课班次，离了课程就不存在**)。
- **双边框菱形 (Double Diamond)**: 代表 **标识性联系 (Identifying Relationship)**。
	- *含义*: 连**接弱实体集和它依赖的强实体集的那个联系**。
- **虚下划线 (Dashed Underline)**: 代表弱实体集的 **分辨符 (Discriminator)**。
	- *含义*: 弱实体集自己**没有主码**，**只有“部分码”（分辨符）**。

#### **(5) 继承符号 (Inheritance)**

- **三角形 (Triangle)**: 标有 **ISA** 字样，代表 **特化/概化 (Specialization/Generalization)**。
	- *画法*: **超类 (Superclass) 在上，子类 (Subclass) 在下**，中间**用 ISA 三角形连接。**

### **📚 综合绘图实例 (Comprehensive Drawing Example)**

题目要求：

请画出以下场景的 ER 图：

1. **实体集 `Instructor` (老师)**：包含属性 `ID` (主码), `name`。
2. **实体集 `Student` (学生)**：包含属性 `ID` (主码), `tot_cred` (总学分)。
3. **联系集 `Advisor` (导师)**：
	- 一个学生必须有一个导师 (**全部参与**)，且只能有一个导师 (**多对一**)。
	- 一个老师可以带多个学生，也可以不带学生。

**【满分画图步骤】**：

1. **画实体**：
	- 左边画一个**矩形**写 `Instructor`。
	- 右边画一个**矩形**写 `Student`。
2. **画属性**：
	- `Instructor` 上方连两个**椭圆**：`ID` 和 `name`。给 `ID` 画**实下划线**。
	- `Student` 上方连两个**椭圆**：`ID` 和 `tot_cred`。给 `ID` 画**实下划线**。
3. **画联系**：
	- 中间画一个**菱形**写 `Advisor`。
4. **连线 (基数与参与)**：
	- **Student 侧**：
		- 题目说“学生必须有导师” → 画 **双线 (Double Line)** 连接 `Student` 和 `Advisor`。
		- 题目说“学生只能有一个导师” → 这是“多对一”关系中的“多”方（因为多个学生对应一个老师，通常 ER 图箭头指向“一”方，直线连接“多”方，但在 Slide 32 的标准中，**直线 (Line)** 代表 Many，**箭头 (Arrow)** 代表 One）。
		- *修正*: 按照 Slide 35 的 N:1 画法（Instructor 是 1，Student 是 N）：
			- `Advisor` 指向 `Instructor` 画 **箭头 (→)**。
			- `Advisor` 连接 `Student` 画 **直线 (—)**。
	- **Instructor 侧**：
		- 题目**说“老师可以不带学生” → 单线 (Single Line) (普通线)。**
		- **箭头已在上面画过。**

最终图形描述：

Student (双线, 直线) ———— <Advisor> ———— → (单线, 箭头) Instructor

(注意：Student 端的双线表示全部参与，直线表示它是 N 端；Instructor 端的单线表示部分参与，箭头表示它是 1 端)

### **5. 弱实体集 (Weak Entity Sets)**

#### **(1) 核心定义 (Definition)**

- **弱实体集 (Weak Entity Set)**:
	- **定义**: 一个**没有足够的属性来形成 主码 (Primary Key)** 的实体集。
	- *理解*: 它**必须依赖于另一个实体集才能唯一存在**。比如，“第1章”单独存在没意义，必须是“《数据库》书的第1章”。
- **强实体集 (Strong Entity Set)**:
	- 又称 **标识实体集 (Identifying Entity Set)**。
	- **定义**: **弱实体集所依赖的那个有主码**的实体集。
- **标识性联系 (Identifying Relationship)**:
	- 连接弱实体集和强实体集的**那个联系**。

#### **(2) 分辨符 (Discriminator / Partial Key)**

- **定义**: 虽然弱实体集没有主码，但**它有一些属性**可以用来在 **同一个强实体内** 区分不同的弱实体。这些属性叫 **分辨符** 或 **部分码 (Partial Key)**。
- **ER 图符号**: **虚下划线 (Dashed Underline)**。
- *例子*: 对于 `section` (课程班次)，`sec_id` (班号 1, 2, 3) 就是分辨符。因为单独的“1班”没意义，但**对于“CS101这门课”来说，“1班”是唯一的。**

#### **(3) 弱实体集的主码 (Primary Key) —— ⭐ 计算必考**

弱实体集既然**没有主码，那如果我们要把它转成表，或者唯一标识它，该怎么办**？

公式 (必背):

弱实体集的主码强实体集的主码弱实体集的分辨符弱实体集的主码=强实体集的主码 (PK)+弱实体集的分辨符 (Discriminator)

- *注意*: 这意味着弱实体集的主码是一个 **组合属性**。

#### **(4) 约束与符号 (Constraints & Symbols) —— ⭐ 作图必考**

1. **弱实体集**: 画 **双边框矩形 (Double Rectangle)**。
2. **标识性联系**: 画 **双边框菱形 (Double Diamond)**。
3. **参与度**: 弱实体集对标识性联系的参与**永远是 全部参与 (Total Participation)（即画 双线）。**
	- *理由*: 弱实体集依赖强实体集存在，**没有强实体集，弱实体集就不存在。**
4. **基数**: 通常是 **一对多 (1:N)**，**强实体集是“1”，弱实体集是“N”。**

### **📚 完整考试实例 (Complete Example)**

场景描述：

学校课程系统中有两个实体集：

1. **`course` (课程)**：强实体集。主码是 `course_id`。
2. **`section` (开课班次)**：弱实体集。属性有 `sec_id` (班号), `semester` (学期), `year` (年份)。
	- *逻辑*: 不同的课都有“1班”，所以 `sec_id` 自己不能做主码，必须依赖 `course` 才能区分。

**【ER 图画法】**

1. **画 `course`**:
	- **单边框矩形**。
	- 属性 `course_id` 下画 **实下划线** (Solid Underline)。
2. **画 `section`**:
	- **双边框矩形** (Double Rectangle)。
	- **属性 `sec_id`, `semester`, `year` 下画 虚下划线 (Dashed Underline)。\*(注意：这些是分辨符)\***
3. **画联系 `sec_course`**:
	- **双边框菱形** (Double Diamond)。
4. **连线**:
	- 修正: 根据讲义 Slide 59 的图示，`section` 作为多的一方（N），`course` 作为一的一方（1）。
	- 讲义图中：`course` -- `sec_course` == `section`。
	- 也就是：`section` 这一侧用 **双线** 连接（表示全部参与），**且没有箭头（表示多）**。`course` **这一侧如果有箭头则表示** 。

**【主码构成】**

- **`section` 的主码** = `{course_id, sec_id, semester, year}`。
- *(即：**强实体集的主码 `course_id` + 弱实体集的所有分辨符**)*

### **💡 考试避坑指南**

1. **画图别偷懒**：看到“弱实体”三个字，立刻把 **矩形** 和 **菱形** 都**描两遍变成双框**。
2. **虚线别忘**：弱实体集的属性下划线必须是 **虚线**，画成实线会被认为是强实体集的主码，导致扣分。
3. **转换表时**：如果题目让你**把这个 ER 图转成 Relational Schema**，记得 `section` 表里要把 `course_id` 加进去作为 **外码 (Foreign Key)**，同时它也是 `section` 表 **联合主码** 的一部分。

### **6. 扩展 ER 特性 (Extended E-R Features)**

#### **(1) 特化与概化 (Specialization and Generalization)**

这实际上是**同一个层级结构（Hierarchy）的两种不同看待视角。**

1. **特化 (Specialization)**:
	- **定义**: 一个 **自上而下 (Top-down)** 的设计过程。将一**个实体集（超类）细分为多个子类，因为某些属性只适用于特定的子集。**
	- **例子**: 有一个实体集 `person`。我们发现**有些人是 `employee`，有些人是 `student`**。于是我们**在 `person` 之下创建两个子类。**
	- **目的**: 为了**区分不同类型的实体**，或者**表达某些实体特有的属性/联系。**
2. **概化 (Generalization)**:
	- **定义**: 一个 **自下而上 (Bottom-up)** 的设计过程。将**多个共享相同特性的实体集（子类）合并为一个更高层的实体集（超类）**。
	- **例子**: 发现 `instructor` 和 `secretary` 都有 `name`, `salary` 属性，于是**把它们合并成 `employee`。**

- **符号**: 在 ER 图中，使用一个 **三角形 (Triangle)** 组件，标**记为 ISA (意思是 "is a")。**三角形**指向超类（上面），连接子类（下面）**。

#### **(2) 属性继承 (Attribute Inheritance) —— ⭐ 核心概念**

- **定义**: 低层实体集（子类）**自动继承高层实体集（超类）**的所有 **属性** 和 **联系**。
- **例子**: 如果 `person` 有属性 `name` 和 `address`，那么 `student`（作为 `person` 的子类）自动拥有 `name` 和 `address`，即使我们在 `student` 的框里只画了 `tot_cred`。

#### **(3) 设计约束 (Design Constraints on Generalization)**

这些约束**决定了一个实体是否可以同时属于多个子类，或者是否必须属于某个子类**。

1. **成员资格约束 (Membership/Disjointness Constraint)**:
	- **不相交 (Disjoint)**: 一个实体 **至多 (at most)** 属于一个子类。
		- *符号*: 在 ISA 三角形旁标注 **disjoint**。
		- *例子*: `person` 分为 `employee` 和 `student`，如果是 disjoint，一个人不能既是员工又是学生。
	- **重叠 (Overlapping)**: 一个实体**可以 同时 (simultaneously) 属于多个子类**。
		- *符号*: 在 ISA 三角形旁标注 **overlapping**（或者不标 disjoint，通常默认为 overlapping，但讲义未明确默认值，考试最好看是否有标注）。
2. **完备性约束 (Completeness Constraint)**:
	- **全部 (Total)**: **高层**实体集中的 **每个 (Every)** 实体都 **必须 属于至少一个低层实体集。**
		- *符号*: 使用关键词 **total** 标注（或者在部分教材中用双线，但本讲义主要强调文字标注或上下文）。
	- **部分 (Partial)**: 高层实体集中的实体 **可以不 (Some may not) 属于任何低层实体集。**

#### **(4) 转换为关系模式 (Reduction to Relation Schemas) —— ⭐⭐⭐ 大题必考**

当我们要把这种继承关系（ISA 结构）转成数据库表时，有两种标准方法。考试时通常需要根据具体情况（是否 disjoint，是否 total）选择最合适的一种。

**方法 1：建立高层表 + 低层表 (Form a schema for the higher-level & each lower-level)**

- **适用场景**: 通用方法，适用于任何情况。
- **做法**:
	1. **高层实体集 (超类)**: 建一张表，包含**它自己的属性。**
	2. **低层实体集 (子类)**: **各建一张表**，包含：
		- 它 **特有** 的属性。
		- **高层实体集的主码** (作为**主**码，同时**也作为外码指向高层表**)。
- **例子**:
	- `person` 表: `(ID, name, street, city)`
	- `student` 表: `(ID, tot_cred)` *(**ID 既是主码也是外码**)*
	- `employee` 表: `(ID, salary)` *(**ID 既是主码也是外码**)*

**方法 2：仅建立低层表 (Form a schema for each lower-level entity set only)**

- **适用场景**: 仅当 **不相交 (Disjoint)** 且 **全部 (Total)** 时才好用。如果**不满足全部参与，会丢失既不是 A 也不是 B 的父类实体**；如果**不满足不相交，会导致数据重复存储。**
- **做法**:
	- **不建** 高层实体集的表。
	- **低层实体集 (子类)**: **各建一张表**，包含：
		- 它 **特有** 的属性。
		- **继承自高层** 的所有属性。
		- **主码** 即为**继承来的主码。**
- **例子**: (假设 `person` 是抽象的，每个人必须是 `student` 或 `employee` 且不能重叠)
	- `student` 表: `(ID, name, street, city, tot_cred)`
	- `employee` 表: `(ID, name, street, city, salary)`
	- *注意*: 这种方法**不需要 `person` 表。**

### **📚 完整复习实例 (Comprehensive Example)**

**场景描述**：

- **`Person`**: **超类。属性 `ID` (主码),** `name`, `address`。
- **`Student`**: 子类。**特有属性 `tot_cred`。**
- **`Employee`**: 子类。**特有属性 `salary`。**
- 关系：`Student` **ISA** `Person`, `Employee` **ISA** `Person`。

**考试题目 1：画图**

- 画三个矩形：`Person` (上), `Student` (左下), `Employee` (右下)。
- 画一个 **三角形**，里面写 **ISA**。
- 从 `Person` **连线到三角形顶点**，从三角形**底边连线到 `Student` 和 `Employee`**。
- 属性正常画，注意 `Student` 和 `Employee` 不需要画 `ID`、`name` 这些继承属性，只画特有的。

**考试题目 2：转换为表 (Schema)**

- **方案 A (推荐/通用)**:
	1. `Person(ID, name, address)`
	2. `Student(ID, tot_cred)` —— FK: `Student.ID` **引用** `Person.ID`
	3. `Employee(ID, salary)` —— FK: `Employee.ID` **引用** `Person.ID`
- **方案 B (仅当 Disjoint & Total)**:
	1. `Student(ID, name, address, tot_cred)`
	2. `Employee(ID, name, address, salary)`

### **💡 考试做题技巧**

- **转表题**：如果**没有特别说明是 disjoint/total**，或者为了保险起见，**永远优先使用“方法 1”**（分别建表，通过 ID 关联）。这是**最标准、最不会出错的方法。**
- **画图题**：看到“是...的一种 (is a type of)”或者“分类”这样的词，立刻想到画 **ISA 三角形**。不要忘了继承性，子类不要重复画父类的属性。

### **7. 转换为关系模式 (Reduction to Relation Schemas)**

#### **(1) 强实体集的转换 (Strong Entity Sets)**

- **规则**: 为**每个强实体集**创建一个独立的 **模式 (Schema/Table)**。
	- **属性**: 实体集的**所有简单属性直接变成表的列**。
	- **主码**: 实体集的**主码直接变成表的主码**。
- **复合属性 (Composite Attributes)**:
	- **规则**: **打散 (Flatten)**。**不要为复合属性本身建列**，而是**为它的每个 子属性 建列。**
	- *例子*: 如果 `name` **由 `first_name` 和 `last_name` 组成**，表中**应该有 `first_name` 和 `last_name` 两列，而没有 `name` 这一列。**

**【考试实例】**

- **实体集**: `student`

- **属性**: `ID` (主码), `name` (**复合: first, last**), `tot_cred`.

- **转换结果**:

	```
	student (ID, first_name, last_name, tot_cred)
	Primary Key: ID
	```

#### **(2) 弱实体集的转换 (Weak Entity Sets) —— ⭐ 必考难点**

- **规则**: 为**每个弱实体集创建一个独立的模式**。
	- **属性**:
		1. **包含弱实体集自己的属性。**
		2. **必须加入** 它所依赖的 **强实体集的主码** (作为**外码 Foreign Key**)。
	- **主码**:
		- **组合主码** = **强实体集的主码** + **弱实体集的分辨符 (Discriminator)**。
- **注意**: 对应的**标识性联系（双菱形）不需要 再单独建表**了，因为**联系信息已经通过外码包含在弱实体表中了。**

**【考试实例】**

- **弱实体集**: `section` (**开课班次**)。分辨符: `sec_id`, `semester`, `year`。

- **依赖的强实体集**: `course` (主码: `course_id`)。

- **转换结果**:

	```
	section (course_id, sec_id, semester, year)
	Primary Key: {course_id, sec_id, semester, year}
	Foreign Key: course_id references course(course_id)
	```

#### **(3) 联系集的转换 (Relationship Sets) —— ⭐ 核心考点**

转换方式**取决于 基数约束 (Cardinality Constraints)**（即 1:N, M:N 等）。

**A. 多对多联系 (Many-to-Many Relationship)**

- **规则**: **必须新建一张表**。
	- **属性**:
		1. 参与**联系的双方实体集的 主码。**
		2. **联系集本身的 描述性属性 (如果有)。**
	- **主码**: **双方主码的并集 (Union of Primary Keys)**。

**【考试实例】**

- **联系**: `advisor` (连接 `student` 和 `instructor`，多对多)。

- **转换结果**:

	```
	advisor (s_ID, i_ID)
	Primary Key: {s_ID, i_ID}
	Foreign Key: s_ID references student(ID)
	Foreign Key: i_ID references instructor(ID)
	```

**B. 一对多 / 多对一联系 (Many-to-One / One-to-Many Relationship)**

- **规则**: **通常不新建表 (Optimization)**，而是采用 **合并 (Linking)** 的方式。
- **操作**: 将 **“一” (One)** 方的主码，添加到 **“多” (Many)** 方的表中作为**外码**。
	- *记忆口诀*: **谁是多，谁加列 (Add to the "Many" side)**。
	- *理由*: 因为“**多”方的一个实体只对应“一”方的一个实体**，所以**加一列就能存下这个关系，不需要额外建表**。
- **特例**: 如果**“多”方是 部分参与 (Partial Participation)**，这种合并**会导致大量的 NULL 值**。如果为了**避免 NULL，也可以选择新建一张表（同 M:N 的做法）**，但考试通常优先考察合并优化。

**【考试实例】**

- **联系**: `inst_dept` (连接 `instructor` 和 `department`)。

- **基数**: 一个系有多个老师，一个老师属于一个系。 (`instructor` 是 **多**, `department` 是 **一**)。

- **转换结果**:

	- **不创建 `inst_dept` 表。**
	- **在 `instructor` 表中增加一列 `dept_name`。**

	```
	instructor (ID, name, salary, dept_name)
	Foreign Key: dept_name references department(dept_name)
	```

**C. 一对一联系 (One-to-One Relationship)**

- **规则**: 也可以**通过扩展其中任意一张表来实现（即加外码）**，**通常选择 全部参与 (Total Participation) 的那一方**来加列，可以避免 NULL 值。

#### **(4) 多值属性的转换 (Multivalued Attributes) —— ⭐ 易错点**

- **规则**: **必须新建一张表**。
	- **属性**:
		1. 所在实体集的 **主码**。
		2. **多值属性本身**。
	- **主码**: **所有属性的组合** (即 `主码 + 值` **才能唯一**)。
	- *注意*: 千万**不要试图把多值属性硬塞进原表里（比如 phone1, phone2）**，那是错误的。

**【考试实例】**

- **实体**: `instructor` (ID, name)。

- **多值属性**: `{phone_number}` (一个人可能有多个电话)。

- **转换结果**:

	- **`instructor` 表保持不变**：`(ID, name)`。
	- **新建表 `inst_phone`:**

	```
	inst_phone (ID, phone_number)
	Primary Key: {ID, phone_number}
	Foreign Key: ID references instructor(ID)
	```

### **⚠️ 考试常见错误 (Common Errors Checklist)**

根据 **《常见ER错误.pdf》**，请在做题时特别注意：

1. **不要在 ER 图里画外码**:
	- 在**画 ER 图时**，`student` **实体里不要写 `dept_name` 属性**。**`dept_name` 是通过转换 `student-department` 联系时自动加进去的**。如果**在 ER 图里就写了，转换时就会重复**，或者逻辑混乱。
2. **不要把邻居属性加给自己**:
	- **转表时**，不要“顺手”把**邻居的所有属性都拿过来**。**只拿 主码 做外码**即可。
3. **不要创建单属性表**:
	- 如果你**转出了一个表像 `stud_dept(ID)`**，那肯定是**错的**。**联系表至少要有两个外码（连接两头）**。

### **📝 综合转换大题模板 (Template)**

**题目**：将以下 ER 图转换为关系模式。 *(假设图中有：Student, Department, Advisor(M:N), Multivalued Phone)*

**参考答案格式**：

1. **Schema for Strong Entities**:
	- `Student (s_id, name, tot_cred)`
	- `Department (dept_name, building, budget)`
2. **Schema for Weak Entities** (如果有):
	- `Section (course_id, sec_id, ...)`
3. **Schema for M:N Relationships**:
	- `Advisor (s_id, i_id)` -- *FKs: s_id -> Student, i_id -> Instructor*
4. **Schema for Multivalued Attributes**:
	- `Student_Phone (s_id, phone_number)`
5. **Handling 1:N Relationships**:
	- *Note: The relationship 'student_dept' is 1:N, so we add 'dept_name' to 'Student' schema.*
	- (修正后的 Student): ``Student (s_id, name, tot_cred, dept_name)``

# ch8

### **1. 好的关系设计特点 (Features of Good Relational Design)**

我们进行数据库设计的目标是：消除数据冗余，并确保在分解表时不会丢失信息。

#### **(1) 原子域与第一范式 (Atomic Domains and First Normal Form)**

- **原子性 (Atomicity)**：
	- 如果一个域的元素被认为是 **不可分割的单元 (indivisible units)**，则该域是原子的 。
	- **非原子 (Non-atomic) 的例子**：
		- **名字集合**（Set of names）、**复合属性**（composite attributes） 。
		- 像 `CS101` 这样的 **ID 号，如果它可以拆分成两部分（CS 表示系，101 表示课号）**，那它就**不是原子的 。**
- **第一范式 (First Normal Form, 1NF)**：
	- 如果一个关系模式 R 的 **所有属性的域都是原子的**，则称 R 属于第一范式 。
	- *注意*：我们在这一章**假设所有关系都满足 1NF** 。

#### **(2) 避免冗余 (Avoid Redundancy)**

- **坏的设计 (Bad Design)**：
	- 如果我们把 `instructor` 和 `department` 硬拼**成一张**大表 `inst_dept(ID, name, salary, dept_name, building, budget)` 。
	- **后果**：信息的重复 (repetition of information)。
		- 例如：系名 `Comp. Sci.` **对应的 `Taylor` 大楼和 `100000` 预算**，会随着每一个该系的老师重复出现。如果有 100 个计算机系老师，这些**信息就重复存了 100 次 。**
- **好的设计 (Good Design)**：
	- “好”意味着 **“没有冗余” (no redundancy)** 。
	- 解决方法是 **分解 (Decomposition)**：将**大表拆分成小表**，例如拆成 `instructor` 和 `department`。

#### **(3) 分解：有损与无损 (Decomposition: Lossy vs. Lossless) —— ⭐ 必考计算/判断**

当我们把一张表 R 分解成 R1 和 R2 时，必须保证我们**能通过连接 (R1⋈R2) 完全还原 回原来的 R**，既不能少数据，也**不能多出错误的“垃圾数据”。**

- **有损分解 (Lossy Decomposition)**：
	- **定义**：如果我们**无法重构原始关系（通常是因为连接后产生了错误的元组）**，这就是有损分解 。
	- *注意*：这里的“Lossy”是指**丢失了“原表即真理”的信息**，实际上是因为多出了虚假的元组导致我们分不清真假。

**【📚 完整考试实例：有损分解 (A Lossy Decomposition)】** **场景**： 假设有一个员工表 `employee(ID, name, street, city, salary)`。 我们把它**分解为两张表：**

1. `employee1(ID, name)`
2. `employee2(name, street, city, salary)` *(注意：这里是用 name 来连接，而不是 ID)*

**数据** ： 原表中有两行数据：

- `(57766, Kim, Main, Perryridge, 75000)`
- `(98776, Kim, North, Hampton, 67000)` *(注意：这两个人**都叫 Kim，但 ID 不同**)*

**分解结果** ：

- `employee1`: `(57766, Kim)`, `(98776, Kim)`
- `employee2`: `(Kim, Main, Perryridge, 75000)`, `(Kim, North, Hampton, 67000)`

**还原尝试 (Natural Join)** ： 当我们做 `employee1 natural join employee2` 时，因为**连接键是 `name` ('Kim')，数据库会把所有的 Kim 进行笛卡尔积匹配。结果会变成 4 行：**

1. `(57766, Kim, Main...)` ✅ 正确
2. `(57766, Kim, North...)` ❌ **错误！** (ID 57766 的 Kim **实际上不住在 North**)
3. `(98776, Kim, Main...)` ❌ **错误！**
4. `(98776, Kim, North...)` ✅ 正确

**结论**： 因为**产生了额外的错误元组，我们无法区分哪个才是真**的，所以这是一个 **有损分解 (Lossy Decomposition)**。

- **无损连接分解 (Lossless-join Decomposition)**：
	- **定义**：对于所有可能的合法关系实例，**都有 r=ΠR1(r)⋈ΠR2(r) 。**
	- **判定规则 (死记硬背)** ： 将 **R 分解为 R1 和 R2 是无损**的，**当且仅当**以下函数**依赖集 F+ 中的至少一个成立：**
		1. R1∩R2→R1 （**公共属性是 R1 的超码**）
		2. R1∩R2→R2 （**公共属性是 R2 的超码**）
	- *简单人话*：**两个表切开处的“公共属性”，必须能唯一标识其中一张表。**

### **💡 考试做题总结**

1. **判断分解好坏**：首先看是不是 **无损 (Lossless)**。如果是无损的，再看是不是 **保依赖 (Dependency Preserving)**。
2. **判断无损的方法**：
	- 找出两个表的**公共属性（Intersection）。**
	- 检查这个公共属性**是不是其中某张表的 Superkey (超码)。**如果**是，就是无损**；如果**两边都不是超码，就是有损。**
	- *比如上面的例子，公共属性是 Name，**Name 不是任何一张表的 Key（因为有重名）**，所以是**有损**。*

### **2. 函数依赖理论 (Functional Dependency Theory)**

#### **(1) 函数依赖定义 (Functional Dependencies - FDs) —— 基础概念**

- **定义**：设 𝑅 是**一个关系模式**，𝛼⊆𝑅 且 𝛽⊆𝑅。
	- 𝛼→𝛽 成立（即 **𝛼 函数决定** 𝛽），**当且仅当**对 𝑅 的**任意一个合法实例 𝑟 中的任意两个元组 𝑡1 和 𝑡2，如果 𝑡1[𝛼]=𝑡2[𝛼]，那么必然有 𝑡1[𝛽]=𝑡2[𝛽] 1。**
	- *通俗理解*：只要 **𝛼 的值一样，𝛽 的值就必须一样**（像 𝐼𝐷→𝑁𝑎𝑚𝑒）。
- **平凡依赖 (Trivial FD)**：
	- 如果 𝛽⊆𝛼，则 𝛼→𝛽 **是平凡的（永远成立）**。
	- *例子*：𝐼𝐷,𝑁𝑎𝑚𝑒→𝐼𝐷 2。

#### **(2) 属性集的闭包 (Closure of Attribute Sets, 𝛼+) —— ⭐⭐⭐ 考试计算核心**

这是**解决几乎所有 FD 问题的万能工具。**

- **定义**：给定一个**属性集** 𝛼，在**函数依赖集 𝐹 下**，**𝛼 能函数决定的所有属性的集合**，记为 𝛼+ 。
- **计算算法 (Algorithm)** 4：
	1. 初始化：𝑟𝑒𝑠𝑢𝑙𝑡=𝛼。
	2. **循环检查 𝐹 中的每个依赖 𝛽→𝛾：**
		- **如果 𝛽⊆𝑟𝑒𝑠𝑢𝑙𝑡**（即 **𝛽 的所有属性都在当前结果里**），**则把 𝛾 加进 𝑟𝑒𝑠𝑢𝑙𝑡。**
	3. **重复步骤 2**，**直到 𝑟𝑒𝑠𝑢𝑙𝑡 不再变化为止。**

【📚 完整考试计算实例】 (源自 Slide 1413-1423)

题目：

关系模式 𝑅=(𝐴,𝐵,𝐶,𝐺,𝐻,𝐼)

函数依赖集 𝐹={𝐴→𝐵,𝐴→𝐶,𝐶𝐺→𝐻,𝐶𝐺→𝐼,𝐵→𝐻}

任务：计算 (𝐴𝐺)+。

**计算步骤**：

1. **初始化**：𝑟𝑒𝑠𝑢𝑙𝑡={𝐴,𝐺}。
2. **第一轮扫描**：
	- 看 𝐴→𝐵：𝐴 在 result 里吗？在。⇒ 加入 𝐵。𝑟𝑒𝑠𝑢𝑙𝑡={𝐴,𝐺,𝐵}。
	- 看 𝐴→𝐶：𝐴 在 result 里吗？在。⇒ 加入 𝐶。𝑟𝑒𝑠𝑢𝑙𝑡={𝐴,𝐺,𝐵,𝐶}。
	- 看 𝐶𝐺→𝐻：**𝐶,𝐺 都在** result 里吗？**𝐶 刚加进来**，现在有了。⇒ 加入 𝐻。𝑟𝑒𝑠𝑢𝑙𝑡={𝐴,𝐺,𝐵,𝐶,𝐻}。
	- 看 𝐶𝐺→𝐼：𝐶,𝐺 都在。⇒ 加入 𝐼。𝑟𝑒𝑠𝑢𝑙𝑡={𝐴,𝐺,𝐵,𝐶,𝐻,𝐼}。
	- 看 𝐵→𝐻：𝐵 在，𝐻 已经在里面了，不变。
3. **结果**：(𝐴𝐺)+={𝐴,𝐵,𝐶,𝐺,𝐻,𝐼}。

#### **(3) 属性闭包的用途 (Uses of Attribute Closure) —— ⭐ 必考应用**

学会了算 𝛼+，你就能做以下三类题：

**用途 A：验证函数依赖是否成立 (Testing for Validity of FD)**

- **规则**：要检查 𝛼→𝛽 是否成立，只需**计算 𝛼+，然后看是否 𝛽⊆𝛼+** 。
- *例子*：检查 𝐴𝐺→𝐼 是否成立？
	- 算 (𝐴𝐺)+ 得到 {𝐴,𝐵,𝐶,𝐺,𝐻,𝐼}。
	- 𝐼 在里面吗？**在。⇒ 成立。**

**用途 B：判断超码 (Testing for Superkey)**

- **规则**：要检查 𝛼 是否是 **超码 (Superkey)**，只需**计算 𝛼+，看它是否包含 𝑅 中的 所有 属性** 。
- *例子*：𝐴𝐺 是超码吗？
	- (𝐴𝐺)+ **包含了所有属性** {𝐴,𝐵,𝐶,𝐺,𝐻,𝐼}。⇒ 是超码。

**用途 C：寻找候选码 (Finding Candidate Keys)**

- **规则**：候选码是 **最小的超码** (Minimal Superkey)。
	- 第一步：找**一个超码 𝛼。**
	- 第二步：**尝试去掉 𝛼 中的属性(试出来一个就行,注意不是用𝛼+)**，看**剩下的部分闭包是否还能推出所有属性**。如果**不能再缩小了，就是候选码** 。

#### **(4) 正则覆盖 (Canonical Cover, 𝐹𝑐) —— ⭐ 高级计算**

为了**消除冗余**，我们需要**计算正则覆盖**。这也是**无损分解算法的前置步骤。**

- **无关属性 (Extraneous Attributes)** ：
	- 如果**去除某个依赖中的属性后**，新的**依赖集与原集等价**，则**该属性是无关的。**
	- **测试方法**：
		- **在左边 (LHS) 无关**：对于 𝛼→𝛽，如果 𝐴∈𝛼，计算 (𝛼−{𝐴})+，看是否包含 𝛽。如果**包含，说明 𝐴 多余。**
		- **在右边 (RHS) 无关**：对于 𝛼→𝛽，如果 𝐴∈𝛽，**从 𝐹 中去掉 𝐴 得到 𝐹′，**计算 **𝛼+ (在 𝐹′ 下)**，看**是否包含 𝐴。如果包含，说明 𝐴 多余。**
- **计算 𝐹𝑐 的算法 (Algorithm)循环不断地做** ：
	1. **合并 (Union Rule)**：把**左边相同的依赖合并**。例如 𝐴→𝐵,𝐴→𝐶 **合并为 𝐴→𝐵𝐶。**
	2. **去无关 (Remove Extraneous)**：检查**每个依赖的左**边和**右**边**是否有无关属性，有则删掉。**
	3. **循环**：**重复上述**步骤**直到 𝐹 不再变化。**

【📚 完整考试计算实例】 (源自 Slide 1921-1931)

题目：

𝑅=(𝐴,𝐵,𝐶)

𝐹={𝐴→𝐵𝐶,𝐵→𝐶,𝐴→𝐵,𝐴𝐵→𝐶}

任务：计算 𝐹𝑐。

**步骤**：

1. **合并**：𝐴→𝐵 和 𝐴→𝐵𝐶 **可以合并**（**其实 𝐴→𝐵𝐶 已经包含了 𝐵**）。

	- 目前 𝐹={𝐴→𝐵𝐶,𝐵→𝐶,𝐴𝐵→𝐶}。

2. **去无关 (检查 𝐴𝐵→𝐶)**：

	- 𝐴 在 𝐴𝐵→𝐶 **左边是无关的吗？**
	- 计算 (𝐴𝐵−{𝐴})+ 即 𝐵+。在 {𝐴→𝐵𝐶,𝐵→𝐶} 下， 𝐵+={𝐵,𝐶}。
	- 𝐵+ **包含 𝐶 吗？**包含。⇒ 所以 **𝐴 是无关的。**
	- 依赖 𝐴𝐵→𝐶 **简化为 𝐵→𝐶。**
	- 现在的 𝐹={𝐴→𝐵𝐶,𝐵→𝐶,𝐵→𝐶} ⇒ 去重后 {𝐴→𝐵𝐶,𝐵→𝐶}。

3. **去无关 (检查 𝐴→𝐵𝐶)**：

	- 𝐵 在 𝐴→𝐵𝐶 **右边是无关的吗**？
		- 暂时去掉 𝐵，变成 𝐴→𝐶。利用其他依赖 (𝐵→𝐶) 能推出 𝐵 吗？**不能。保留。**
	- 𝐶 在 𝐴→𝐵𝐶 **右边**是无关的吗？
		- 暂时去掉 𝐶，变成 𝐴→𝐵。剩下的 𝐹′={𝐴→𝐵,𝐵→𝐶}。
		- 在 𝐹′ 下计算 𝐴+：𝐴→𝐵,𝐵→𝐶⇒𝐴+={𝐴,𝐵,𝐶}。
		- 𝐴+ 包含 𝐶 吗？**包含 (**因为 𝐴→𝐵→𝐶)。⇒ 所以 **𝐶 在 𝐴→𝐵𝐶 中是多余的。**
	- 依赖 𝐴→𝐵𝐶 简化为 𝐴→𝐵。

4. 最终结果：

	𝐹𝑐={𝐴→𝐵,𝐵→𝐶}。

### **💡 考试做题总结**

1. **拿到题先看依赖集**：如果让你判断 Key 或者分解，**起手式**永远是先算 𝛼+。
2. **Superkey vs Candidate Key**：
	- 只要 𝛼+ **包含全集，𝛼 就是 Superkey。**
	- 只有当 𝛼 的**任何真子集**的闭包都不包含全集时，𝛼 **才是 Candidate Key。**
3. **正则覆盖**：考试如果考 𝐹𝑐，记得**先用 Union Rule 合并左边**，**然后按部就班检查左右两边的**无关属性。特别是**利用 传递律 (如 𝐴→𝐵,𝐵→𝐶 导致 𝐴→𝐶 中的 𝐶 冗余)** 是最常见的考点。

### **3. 范式 (Normal Forms)**

#### **(1) BCNF (Boyce-Codd Normal Form) —— 最严格的范式**

- 定义 (Definition) 1：

	一个关系模式 𝑅 属于 BCNF，当且仅当**对于 𝐹+ 中所有**形如 𝛼→𝛽 的函数依赖（其中 𝛼⊆𝑅 且 𝛽⊆𝑅），**至少**满足以下两个条件**之一**：

	1. 𝛼→𝛽 是 **平凡的 (trivial)** (即 𝛽⊆𝛼)。
	2. 𝛼 是 𝑅 的 **超码 (superkey)**。

- **人话理解**：在 BCNF 中，**箭头的左边必须是超码**（**除非是废话依赖**）。如果**有一个依赖的左边不是超码**，就**不满足 BCNF**。

- **判断方法**：

	1. 计算**所有依赖左边 𝛼 的闭包 𝛼+。**
	2. 如果 𝛼+ **包含了 𝑅 的所有属性，则它是超码**，通过。
	3. 如果**有任何一个非平凡依赖的左边不是超码**，则 𝑅 **不是** BCNF。

**【📚 完整考试实例：判断 BCNF】** (源自 Slide 1236-1241)

- **关系模式**：`instr_dept (ID, name, salary, dept_name, building, budget)`
- **函数依赖**：
	1. `ID -> name, salary, dept_name` (ID 是 Key)
	2. `dept_name -> building, budget`
- **判断步骤**：
	- 检查依赖 `dept_name -> building, budget`。
	- 计算左边的闭包：`{dept_name}^+` = `{dept_name, building, budget}`。
	- 它包含所有属性吗？**不包含** (**缺了 ID, name** 等)。
	- **结论**：因为 `dept_name` **不是超码**，所以该模式 **不是 BCNF**。

#### **(2) 3NF (Third Normal Form) —— 适度宽松的范式**

- **背景**：BCNF **虽然好（无冗余）**，但**有时候分解成 BCNF 会导致 丢失依赖 (Dependency Preservation)**。为了解决这个问题，**引入了 3NF** 2。

- 定义 (Definition) ：

	一个关系模式 𝑅 属于 3NF，当且仅当对于 𝐹+ 中**所有**形如 𝛼→𝛽 的函数依赖，至少满足以下**三个条件之一：**

	1. 𝛼→𝛽 是 **平凡的 (trivial)** (𝛽⊆𝛼这里只是指能被决定)。
	2. 𝛼 是 𝑅 的 **超码 (superkey)**。
	3. **𝛽−𝛼 中的每个属性**都包含在 𝑅 的**某个 候选码 (candidate key)** 中。
		- *注意*：第三点是 **3NF 对 BCNF 的“放水”条款**。**只要右边的属性是“主属性 (Prime Attribute，即候选码的一部分)”**，**哪怕左边不是超码，也算过关**。

**【📚 完整考试实例：判断 3NF】**

- **关系模式**：`dept_advisor (s_ID, i_ID, dept_name)`
- **函数依赖** 𝐹：
	1. `s_ID, dept_name -> i_ID`
	2. `i_ID -> dept_name`
- **分析候选码**：
	- Key 1: `{s_ID, dept_name}`
	- Key 2: `{i_ID, s_ID}` (因为 `i_ID` 能推 `dept_name`，结合 `s_ID` 就能推所有)
- **判断步骤**：
	- **检查依赖 1** (`s_ID, dept_name -> i_ID`)：
		- 左边 `{s_ID, dept_name}` **是超码吗**？**是**。**满足条件 2。→ Pass**。
	- **检查依赖 2** (`i_ID -> dept_name`)：
		- 左边 `{i_ID}` 是超码吗？**不是** (它推不出 s_ID)。→ **BCNF 失败。**
		- **但在 3NF 中**：看**右边的属性 `dept_name`。**
		- `dept_name` 是**某个候选码的一部分吗**？**是** (它是 Key 1 `{s_ID, dept_name}` 的一部分)。
		- 满足条件 3。→ Pass。
- **结论**：该模式 **是 3NF** (虽然**它不是 BCNF**)。

#### **(3) BCNF 与 3NF 的对比 (Comparison) —— ⭐ 选择题/填空题**

这是一张必考的对比表 ：

| **特性 (Feature)**                             | **BCNF**                  | **3NF**                    |
| ---------------------------------------------- | ------------------------- | -------------------------- |
| **无损连接分解 (Lossless-join Decomposition)** | **Yes** (总是可以做到)    | **Yes** (总是可以做到)     |
| **保持依赖 (Dependency Preserving)**           | **No** (**不一定能做到**) | **Yes** (**总是可以做到**) |
| **允许冗余 (Redundancy)**                      | **No** (无冗余)           | **Yes** (**允许部分冗余**) |

### **💡 考试做题总结**

1. **做题顺序**：拿到一个关系模式，**先判断是不是 BCNF**。
	- **只要所有依赖的左边都是 Superkey**，它就是 BCNF（同时也是 3NF）。
2. **如果不是 BCNF**，再看是不是 **3NF**。
	- 找出**所有“破坏”了 BCNF 的依赖**（即**左边不是 Key 的依赖**）。
	- 检查这些依赖的**右边**的属性。
	- 如果**右边的属性都是候选码的成员（Prime Attributes）**，那就是 3NF。
	- 如果右边**有属性不在任何候选码里**，那**连 3NF 都不是**。
3. **记忆口诀**：
	- **BCNF**：**左边必须是大哥 (Superkey)**。
	- **3NF**：要么左边是大哥，要么右边是大哥的人 (Candidate Key 的一部分)。

### **4. 分解的性质 (Properties of Decomposition)**

#### **(1) 无损连接分解 (Lossless-join Decomposition) —— ⭐ 必须满足**

- **定义**：对于 𝑅 的分解 𝑅1,𝑅2，如果我们将它们重新自然连接（Natural Join）起来，结果必须与原来的关系 𝑟 **完全一致**（既不丢失信息，也不产生错误的元组）。

	- 公式表达：𝑟=Π𝑅1(𝑟)⋈Π𝑅2(𝑟) 1。

- 判定法则 (The Golden Rule - 必背) 2：

	将 𝑅 分解为 𝑅1 和 𝑅2 是无损的，**当且仅当这两个关系模式的 公共属性 (Intersection) 是其中 至少一个 关系模式的 超码 (Superkey)。**

	- 即以下函数依赖集 𝐹+ 中至少有一个成立：
		1. 𝑅1∩𝑅2→𝑅1
		2. 𝑅1∩𝑅2→𝑅2

#### **(2) 保持依赖 (Dependency Preservation) —— ⭐ 尽量满足**

- **定义**：分解后的**每个小关系 𝑅𝑖 都有自己的一套函数依赖集 𝐹𝑖。**如果这些 𝐹𝑖 的**合集（𝐹1∪𝐹2∪...）的闭包**，**能够等价于原函数依赖集 𝐹 的闭包**，那么这个分解**就是保持依赖的**。
	- *人话理解*：我们**不需要把表 Join 起来**，**就能检查所有的约束条件（FDs）**。
- **判定方法**：
	- 检查原集合 𝐹 中的**每一个依赖 𝛼→𝛽。**
	- 看它**是否能被分解后的各个子模式中的依赖所推导出来**。
	- 如果有一个依赖（比如 𝐽𝐾→𝐿）涉及的**属性被拆得四分五裂，导致必须 Join 才能检查它**，那就是 **不保持依赖** 。

### **📝 完整考试实例：双重性质判定 (Comprehensive Example)**

为了涵盖这两个知识点，我们使用讲义 Slide 1595-1610 中的经典案例。

**【题目背景】**

- 关系模式：𝑅=(𝐴,𝐵,𝐶)
- 函数依赖集：𝐹={𝐴→𝐵,𝐵→𝐶}
- *(注：由此可推导出 𝐴→𝐶，所以 𝐴 是 Key)*

【分解方案 1】

将 𝑅 分解为 𝑅1=(𝐴,𝐵) 和 𝑅2=(𝐵,𝐶)。

1. **判断无损连接** 5：
	- **公共属性**：𝑅1∩𝑅2={𝐵}。
	- **检查 B 的地位**：在 𝑅2(𝐵,𝐶) 中，因为**有依赖 𝐵→𝐶**，所以 𝐵 是 𝑅2 的 **超码 (Superkey)**。
	- **结论**：满足“**公共属性是超码**”，所以是 **无损分解 (Lossless)**。
2. **判断保持依赖** 6：
	- 依赖 𝐴→𝐵：属性 𝐴,𝐵 都在 𝑅1 中，**保留**。
	- 依赖 𝐵→𝐶：属性 𝐵,𝐶 都在 𝑅2 中，**保留**。
	- **结论**：**所有依赖都还在**，所以是 **保持依赖 (Dependency Preserving)**。
	- *(这是最好的分解，即 BCNF 分解)*。

【分解方案 2】

将 𝑅 分解为 𝑅1=(𝐴,𝐵) 和 𝑅2=(𝐴,𝐶)。

1. **判断无损连接** 7：
	- **公共属性**：𝑅1∩𝑅2={𝐴}。
	- **检查 A 的地位**：在 𝑅1(𝐴,𝐵) 中，𝐴→𝐵，所以 𝐴 是 𝑅1 的超码。在 𝑅2(𝐴,𝐶) 中，𝐴→𝐶（由传递性得），所以 𝐴 也是 𝑅2 的超码。
	- **结论**：满足条件，是 **无损分解 (Lossless)**。
2. **判断保持依赖** ：
	- 依赖 𝐴→𝐵：**在 𝑅1 中**，**保留**。
	- 依赖 𝐵→𝐶：
		- **𝐵 在 𝑅1 里，𝐶 在 𝑅2 里。**
		- 我们**无法在不 Join 𝑅1 和 𝑅2 的情况下检查 𝐵→𝐶。**
	- **结论**：丢失了 𝐵→𝐶，所以 **不保持依赖 (Not Dependency Preserving)**。

### **💡 考试做题总结**

1. **拿到分解题，先画图**：把属性分配到两个框里，**找出中间重叠的部分（公共属性）**。
2. **无损判据（死记）**：**重叠部分** → 左边？ 或者 重叠部分 → 右边？只要有一个成立，就是无损。如果重叠部分谁都推不出来，就是有损。
3. **依赖判据**：看 𝐹 里的每个箭头。如果箭头的左边和右边都在同一个框里，那就是保留了。如果**有一个箭头的左边在框 A，右边在框 B，那多半是丢了**（除非**能通过其他路径推导出来**）。

### **💡 复习建议**

1. **必练计算题**: 给定一个关系模式 𝑅 和依赖集 𝐹，求：
	- **候选码** (Candidate Keys)。
	- 判断是**几范式** (2NF, 3NF, BCNF)。
	- 如果**不是 BCNF/3NF，进行无损分解**。
	- 判断分解**是否保依赖**。
2. **概念辨析**: 3NF 和 BCNF 的**包含关系 (BCNF ⊂ 3NF)**，以及它们在“保依赖”和“去冗余”上的权衡。

这是 **Part IV** 的重头戏，尤其是 **Attribute Closure (属性闭包)** 的计算，它是解决所有范式问题的万能钥匙，必须熟练掌握。

# 疑问

## 解释一下什么是从 F 中去掉 A 得到 F',F不是函数依赖集吗怎么去除单个属性的

讲义里的“从 𝐹 中去掉 𝐴 得到 𝐹′”，是一句**简略的口语表达**。

它的准确数学含义是：**将 𝐹 中正在被检测的那条函数依赖 𝛼→𝛽，修改为 𝛼→(𝛽−{𝐴})，然后用这个“变弱了”的新依赖，替换掉原来的旧依赖，形成一个新的集合 𝐹′。**

并不是把 𝐴 从 𝐹 的所有依赖里都删掉，而仅仅是**修改当前正在检查的那一条**。

### **详细步骤解释**

假设我们有一个函数依赖集 𝐹，我们**正在检查其中一条具体的依赖**：

𝛼→𝛽

我们要检查 右边 (RHS) 的属性 𝐴 （其中 𝐴∈𝛽）是不是多余的。

**所谓的“从 𝐹 中去掉 𝐴 得到 𝐹′”，操作步骤如下：**

1. **定位**：找到当前**正在检查的这条依赖** 𝛼→𝛽。

2. **剔除**：在**这条依赖**的**右边**，把**属性 𝐴 删掉**。

	- 这条依赖就变成了：𝛼→(𝛽−{𝐴})。

3. **替换**：**在原集合 𝐹 中，删掉旧的 𝛼→𝛽，加入新的 𝛼→(𝛽−{𝐴})。**

4. **形成新集**：这个**经过“替换”操作后的新集合，就是 𝐹′。**

	- 

		*公式表示*：𝐹′=(𝐹−{𝛼→𝛽})∪{𝛼→(𝛽−𝐴)} 1。

### **为什么要这样做？（核心逻辑）**

我们在测试：“如果我**把 𝐴 从这条依赖里扔掉**，**剩下的依赖集还能不能把 𝐴 给推导出来**？”

- 如果能推导出来（即 在下𝛼+ (在 𝐹′ 下) 依然包含 𝐴），说明 𝐴 在这里是**废话**，本来就可以通过其他依赖（或者传递律）得到，所以它是**多余的 (Extraneous)**。
- 如果推导不出来，说明这个 𝐴 很重要，扔了就丢信息了，所以它**不是多余的**。

### **📚 完整举例说明**

为了让你彻底明白，我们用一个经典的例子：**传递依赖。**

题目：

设 𝑅=(𝐴,𝐵,𝐶)，函数依赖集 𝐹 如下：

1. 𝐴→𝐵𝐶 （注意：这**其实等价于 𝐴→𝐵 和 𝐴→𝐶**可以**用于简化**）
2. 𝐵→𝐶

任务：

检查依赖 𝐴→𝐵𝐶 中，**右边的属性 𝐶 是否是无关的**？

**解题过程**：

1. **确定对象**：
	- 当前**检查的依赖**：𝐴→𝐵𝐶
	- 当前**检查的属性**：𝐶 （在右边）
2. **构造 𝐹′ (“去掉 𝐶”)**：
	- 把 𝐴→𝐵𝐶 中的 𝐶 **拿走**，它变成了 𝐴→𝐵。
	- **用 𝐴→𝐵 替换掉 𝐹 里的 𝐴→𝐵𝐶。**
	- 得到新集合 **𝐹′={𝐴→𝐵,𝐵→𝐶}**。
	- *(解释：**其他**的依赖 𝐵→𝐶 **保持不变**，只改了我们盯着的这一条)*
3. **计算闭包 𝛼+ (在 𝐹′ 下)**：
	- 我们要**算左边 𝛼（也就是 𝐴）的闭包。**
	- 在 𝐹′ 环境下：
		- 有 𝐴→𝐵，所以 𝐴 能推出 𝐵。
		- 有 𝐵→𝐶，所以 𝐵 能推出 𝐶。
		- 根据**传递性**，𝐴→𝐵→𝐶。
	- 所以，**𝐴+={𝐴,𝐵,𝐶}**。
4. **判断**：
	- 看 **𝐴+ 里是否包含刚才扔掉的** 𝐶？
	- **包含！** (𝐶∈{𝐴,𝐵,𝐶})
	- **结论**：说明**即使把 𝐶 从 𝐴→𝐵𝐶 里删掉**，它依然可以通过 𝐴→𝐵 和 𝐵→𝐶 **偷偷地“长”回来。**
	- 所以，𝐶 在 𝐴→𝐵𝐶 中是**无关属性 (Extraneous)**。我们可以把 𝐴→𝐵𝐶 **简化为 𝐴→𝐵。**

### **总结**

下次看到“从 F 中去掉 A”，请自动脑补为：

“把 𝐹 里当前**那条依赖的右半边里的 𝐴 抠掉**，形成一个临时的依赖集 𝐹′。”

## 考试如果考 F_c，记得先用 Union Rule 合并左边，然后按部就班检查左右两边的无关属性意思是不是可以第一次一下就把左边合并完,然后只要一直再尝试删左右就可以了?

**不可以只做一次合并。在删除无关属性的过程中，可能会产生新的“合并机会”，因此必须反复检查 Union Rule。**

根据 **《ch8.pdf》 (Slide 74)** 的算法描述，我们来看一下为什么要这样做。

### **1. 讲义中的算法逻辑 (Slide 74)**

讲义明确指出了算法是一个 **Repeat ... Until** 的循环结构 ：

1. **Repeat** (循环开始)
2. 使用 **Union Rule 合并左边相同的依赖** (α→β1,α→β2⇒α→β1β2)。
3. 寻找并删除 **无关属性 (Extraneous Attributes)**。
4. **Until** (直到 F 不再发生变化)。

并且，讲义在 Slide 74 的底部特别用红字（或强调）标注了 ：

> **"Note: Union rule may become applicable after some extraneous attributes have been deleted, so it has to be re-applied"** （注意：在删除某些无关属性后，**Union Rule 可能会再次变得适用**，因此必须**重新应用**。）

### **2. 为什么需要重新合并？(举例说明)**

假设我们有以下函数依赖集：

F={A→B,AB→C}

**第一回合：尝试合并**

- 目前的左边分别是 A 和 AB，**不一样**。
- 所以，**Union Rule 此时无法使用**。如果你只在开头做一次，这里就结束了。

**第二回合：去无关属性**

- 检查 AB→C 左边的 B 是否无关？
- 计算 A+ (在 F 下)：因为 A→B，所以 A+={A,B}。
- A+ 包含了 B，说明 B 是无关的。
- **修改依赖**：把 AB→C 变成 A→C。
- 现在的 F 变成了：{A→B,A→C}。

**第三回合：必须再次合并！**

- 发现了吗？因为刚才删掉了 B，导致**第二条依赖的左边变成了 A。**
- 现在**有两条左边都是 A 的依赖：A→B 和 A→C。**
- **必须再次应用 Union Rule**，将它们**合并为 A→BC。**

**最终结果 (Fc)**：

A→BC

### **3. 考试做题建议**

虽然理论上是“循环”，但在**考试做题（手算）\**时，你可以采用更高效的\**“动态维护法”**，不用真的每一轮都傻傻地重头遍历：

1. **起手式**：先用 Union Rule 把能合并的都合并了（比如 A→B,A→C 变成 A→BC）。
2. **去无关**：开始检查左右两边的无关属性。
3. **警惕点**：**一旦你在某一步删除了“左边”的属性**（比如把 AB→C 变成了 A→C），**立刻**停下来看一眼：
	- *“哎？现在有没有其他的依赖也是以 A 开头的？”*
	- 如果有，**立刻原地合并**，然后再继续往下检查。

**总结**：只要你没动左边，Union Rule 一般不用管；**一旦动了左边，必须回头看合并。**

## F^+是什么?

**F+ (Closure of a Set of Functional Dependencies)** 是连接基本依赖集 F 和属性闭包 α+ 的桥梁。

简单来说：**F 是给定的“几条规则”，而 F+ 是由此推导出的“所有真理的集合”。**

以下是严格依据讲义（Slide 26-29, 41）为你总结的 F+：

### **3. 函数依赖集的闭包 (Closure of a Set of FDs, F+)**

#### **(1) 定义 (Definition)**

- **F+**：被给定的函数依赖集 F **逻辑蕴含 (logically implied)** 的 **所有** 函数依赖的集合 。
- 也就是说，只要 F **里的规则成立，那么 F+ 里的所有规则也都必须成立。**
- F⊆F+ （**F 只是 F+ 的一部分**）。

#### **(2) 为什么要有 F+？(Intuition)**

举个简单的例子：

- 假设 F={A→B,B→C}。
- 这只是题目给你的两条规则。
- 但是在逻辑上，**A→C 也是成立的（传递律）。**
- A→AB 也是成立的**（增广律）。**
- F+ 就是把这些**隐含的、推导出来的**所有**依赖全部打包在一起的大集合**。

#### **(3) 如何计算 F+？(Computation)**

讲义提供了两种方法，考试中主要考察对 **方法 B** 原理的理解（因为算**完整的 F+ 往往太**庞大）。

方法 A：使用**阿姆斯特朗公理 (Armstrong's Axioms)  这是理论基础**，通过反复应用以下三条公理来生成新依赖，直到不能生成为止：

1. **自反律 (Reflexivity)**：如果 β⊆α，则 α→β。
2. **增广律 (Augmentation)**：如果 α→β，则 γα→γβ。
3. **传递律 (Transitivity)**：如果 α→β 且 β→γ，则 α→γ。

方法 B：使用**属性闭包 α+ (Algorithm using Attribute Closure)**  这是更具操作性的算法思路：

1. 对于关系模式 R 中的 **每一个** 可能的属性子集 γ（注意是每一个子集，这很多！）：
2. 计算该子集的属性闭包 γ+。
3. 对于 γ+ 中的任意子集 S，生成依赖 γ→S 并加入 F+。

- *注意*：讲义特别提到，这个计算过程的复杂度是指数级的 (O(2n))，所以在实际做题中，我们**几乎不会**被要求写出完整的 F+，而是**利用 α+ 来检查某个特定的依赖是否属于 F+**。

### **💡 重点辨析：F+ vs α+**

这是考试中最容易混淆的概念，请务必区分：

| 概念           | 符号   | 含义                     | 结果的样子                               |
| -------------- | ------ | ------------------------ | ---------------------------------------- |
| **属性闭包**   | **α+** | 属性 α 能决定哪些属性？  | 一个**属性的集合** (如 {A,B,C})          |
| **依赖集闭包** | **F+** | **F 能推导出**哪些规则？ | 一个**函数依赖的集合** (如 {A→B,A→C...}) |

**它们的关系**： 我们通常**不直接计算 F+**，而是通过计算 **α+** 来解决问题。

- **题目问**：A→C 是否在 F+ 中？（**即 A→C 是否成立？**）
- **你只需算**：**A+**。**如果 C∈A+，答案就是 Yes。**

### **📝 考试复习策略**

- **概念理解**：知道 F+ 是“所有成立的依赖的集合”。
- **做题技巧**：虽然 F+ 定义很重要，但解题（找 Key、判断范式、保依赖）的核心工具是 **α+**。你之所以觉得我刚才“跳过”了 F+，是因为在实战计算中，我们是用 α+ 来架空 F+ 的——我们不需要算出整个 F+ 就能做题。

## 𝛽⊆𝛼到底是什么意思,这样的话𝛽−𝛼不是会得到0吗

对应着一个专门的概念——**平凡函数依赖 (Trivial Functional Dependency)**。

让我为你详细拆解一下这个概念及其在考试中的意义：

### **1. 数学含义：什么是 𝛽⊆𝛼？**

- **含义**：𝛽 是 𝛼 的子集。也就是说，𝛽 里的**每一个属性，都已经在 𝛼 里了**。
- **差集运算**：𝛽−𝛼 的意思是“在 𝛽 中但 **不在** 𝛼 中的属性”。
- **结果**：既然 𝛽 里的都在 𝛼 里了，那么“在 𝛽 中但不在 𝛼 中”的属性**自然就是一个也没有**，即 **空集 (∅)**。

### **2. 数据库含义：平凡依赖 (Trivial FD)**

在讲义 1 中，明确定义了：

> "In general, 𝛼→𝛽 is trivial if 𝛼⊇𝛽"
>
> (通常，如果 𝛼 包含 𝛽，则 𝛼→𝛽 是平凡的。)

- **直观理解**：这是一句“**正确的废话**”。
- **例子**：
	- 假设 𝛼={𝐼𝐷,𝑁𝑎𝑚𝑒}，𝛽={𝑁𝑎𝑚𝑒}。
	- 依赖：𝐼𝐷,𝑁𝑎𝑚𝑒→𝑁𝑎𝑚𝑒。
	- **解读**：如果你已经知道了这个人的“ID”和“名字”，那你能不能确定他的“名字”？
	- **答案**：**当然能**！这不需要查数据库，这是逻辑上的必然。

### **3. 为什么 3NF/BCNF 定义里会出现 𝛽−𝛼？**

在 3NF 的定义中，有一条规则是检查 **𝛽−𝛼** 中的属性是否包含在候选码中 2。

- **如果 𝛼→𝛽 是平凡依赖 (𝛽⊆𝛼)**：
	- 那么 𝛽−𝛼=∅。
	- **判定结果**：既然集合是空的，就没有属性需要被检查。这意味着 **平凡依赖永远不会违反范式规则**。
	- 所以在判断 BCNF 或 3NF 时，我们通常**直接忽略**平凡依赖，因为它们既不会导致数据冗余，也不会引起异常，它们是**无害的。**

### **总结**

- **你的理解**：𝛽−𝛼=0 (空集) 是**完全正确**的。
- **它的名字**：这种情况叫 **平凡依赖**。
- **考试对策**：看到这种“左边包含了右边”的依赖（如 𝐴𝐵→𝐴），**直接判定**为 **Trivial**，在**检查范式违例时可以直接跳过它，因为它永远是合法的**。

## 如果对所有函数依赖不满足𝛼是 R 的 超码 (superkey),是不是肯定不满足𝛼→𝛽是 平凡的 (trivial)

即使 α 不是超码，α→β 依然**有可能**是平凡的。

请让我为你详细拆解其中的逻辑：

### **1. 逻辑公式分析**

根据 BCNF 的定义 ，一个函数依赖 α→β **是“合法”（允许存在）的，**只要它满足以下 **两个条件之一**：

1. **条件 A**：α→β 是 **平凡的 (trivial)** （即 β⊆α）。
2. **条件 B**：α 是 R 的 **超码 (superkey)**。

这是一个 **“或” (OR)** 的关系。

你的问题是：“**如果不满足条件 B（不是超码），是不是一定不满足条件 A（不是平凡）**？” 用逻辑语言说就是：`NOT B => NOT A` ？ 这是 **不成立** 的。因为可能出现 `NOT B` 且 `A` 的情况（**既不是超码，但是是平凡的**）。

### **2. 反例证明 (Counter-Example)**

最典型的例子就是 **“自己推自己”** 或者 **“大推小”**。

**场景**：

- 关系模式 R(A,B,C)。
- 唯一的非平凡依赖是 B→C。
- 显然，Key 是 AB。**A 单独不是超码**。

**考察依赖 A→A**：

- **是不是超码？**
	- 左边是 A。因为 A 推不出 B 和 C，所以 A **不是超码**。（不满足条件 B）
- **是不是平凡？**
	- 右边 A 包含于左边 A (β⊆α)。所以它是 **平凡的**。（**满足条件 A**）

**结论**： A→A 这一项 **不满足超码条件**，但它 **是平凡的**。所以你的推断“肯定不满足平凡”是不对的。

### **3. 考试中的实际意义**

在考试判断 BCNF/3NF 时，我们**通常会 直接忽略 平凡依赖（如 A→A 或 AB→A）。**

- 为什么忽略？因为**它们永远满足 条件 A，**所以它们永远是“合法”的，**绝不可能是破坏范式的“凶手”**。
- **真正的凶手（BCNF 违例）** 必须同时满足两个“坏”条件：
	1. **不是平凡的**（**排除废话**）。
	2. **左边不是超码**（**排除合法的大哥**）。

**总结一句话**： 只有当你确认一个依赖是 **非平凡 (Non-trivial)** 的时候，如果它的左边还不是超码，那它才是违规的。**平凡依赖虽然左边往往不是超码，但它是无辜的。**

## 区分1NF和2NF和BCNF和3NF

**特别注意**：讲义中**没有专门介绍 2NF (Second Normal Form)**。这是现代数据库教材（如本书）的常见处理方式，因为 2NF 的核心概念（消除部分函数依赖）已经被包含在更严格的 BCNF 和 3NF 定义中了。

为了不误导你，我将**严格基于讲义内容**为你详细解释 1NF、BCNF 和 3NF，并说明它们之间的区别。

### **1. 第一范式 (1NF - First Normal Form)**

**核心概念**：**原子性 (Atomicity)**

- **定义**：一个关系模式 R 如果**所有属性的域都是原子的 (Atomic)**，则称其属于第一范式 。
- **什么是“原子”？**：元素是**不可分割的单元 (indivisible units)。**
- **反例 (不符合 1NF 的情况)** ：
	- **集合值属性**：例如**一个属性存了 `{ "张三", "李四" }` 多个名字**。
	- **复合属性**：例如 **`address` 属性里包含了 `street`, `city`, `zip`。**
	- **可拆分的 ID**：例如 `CS101`，如果业务逻辑里**需要单独拆出 `CS` (系) 和 `101` (课号)**，那它**就不是原子的。**
- **讲义态度**：我们在这一章**默认所有关系都已经满足 1NF 。**

### **2. BCNF (Boyce-Codd Normal Form) —— “**黄金标准”

**核心概念**：**消除所有基于函数依赖的冗余**

- **定义** ： 关系模式 R 属于 BCNF，当且仅当对于 F+ 中所有形如 α→β 的非平凡函数依赖，**α 必须是 R 的超码 (Superkey)**。
	- *(简单说：只要你**能决定别的属性**，你就**必须是能决定全表的 Superkey**。**不允许“小弟”决定**别人。)*
- **BCNF 的违例 (Violation) 例子** ：
	- 表：`instr_dept (ID, name, salary, dept_name, building, budget)`
	- 依赖：`dept_name -> building, budget`
	- **分析**：
		- `dept_name` 能决定 `building` 和 `budget`。
		- 但 `dept_name` **不是**整张表的超码（因为它决定不了 `ID` 或 `salary`）。
		- 所以：左边 (α) 不是超码，**违反 BCNF**。

### **3. 第三范式 (3NF - Third Normal Form)**

**核心概念**：**允许适度冗余以换取“保持依赖”**

- **为什么需要 3NF？**
	- BCNF 虽然消除了冗余，但有时会导致分解后的关系 **无法保持依赖 (Not Dependency Preserving)**。为了解决这个问题，定义了稍微宽松一点的 3NF 
- **定义** ： 关系模式 R 属于 3NF，当且仅当对于 F+ 中所有形如 α→β 的非平凡函数依赖，满足以下**任一**条件：
	1. α 是 R 的 **超码 (Superkey)** （即满足 BCNF 的条件）。
	2. **或者**，β−α 中的每个属性都包含在 R 的某个 **候选码 (Candidate Key)** 中。
		- *(简单说：如果左边不是大哥，但右边是“皇亲国戚”（主属性），那也可以放过。)*
- **3NF 的“放水”例子** ：
	- 表：`dept_advisor (s_ID, i_ID, dept_name)`
	- 依赖：`i_ID -> dept_name` （导师决定系），`s_ID, dept_name -> i_ID`。
	- 候选码 Key 1：`{s_ID, dept_name}`。
	- **分析依赖 `i_ID -> dept_name`**：
		- 左边 `i_ID` 是超码吗？不是（它决定不了 `s_ID`）。→ **违反 BCNF**。
		- 右边 `dept_name` 是候选码的一部分吗？**是**（它是 Key 1 的一部分）。→ **满足 3NF**。

### **4. 区分与总结 (Distinction & Summary)**

| 特性          | **BCNF**                         | **3NF**                                                      | **1NF**           |
| ------------- | -------------------------------- | ------------------------------------------------------------ | ----------------- |
| **定义核心**  | 左边必须是 **Superkey**          | 左边是 Superkey **或** 右边是 **Prime Attribute** (候选码成员) | 属性值 **原子化** |
| **无损连接?** | Yes (总是无损)                   | Yes (总是无损)                                               | N/A               |
| **保持依赖?** | **No** (不一定)                  | **Yes** (总是保持)                                           | N/A               |
| **允许冗余?** | **No** (无冗余)                  | **Yes** (允许部分冗余)                                       | N/A               |
| **包含关系**  | BCNF ⊂ 3NF (BCNF 是更严格的 3NF) | 3NF 包含 BCNF                                                | 基础              |

**关于 2NF 的说明**： 虽然讲义没有专门讲，但为了你理解：**2NF** 处理的是“非主属性对码的部分依赖”。

- 在 **BCNF/3NF** 的定义中，如果左边必须是超码（BCNF），或者右边必须是主属性（3NF），这就**自然排除了 2NF 所禁止的“部分依赖”情况。**
- **考试策略**：如果题目让你判断范式，直接套用 **BCNF** 和 **3NF** 的定义即可。**如果一个关系连 3NF 都不是，那它肯定也不是 BCNF**。

# ch10

### **2. 磁盘 (Magnetic Disks)**

#### **(1) 物理构造 (Physical Structure) —— 小题考点**

*(这部分通常考选择题或填空题，只需记住核心名词定义)*

- **盘片 (Platter)**: 磁盘由多个盘片组成，表面**覆盖磁性材料。**
- **磁头 (Read-write head)**: 每个盘片的表面**都有一个磁头，用于读写数据**。所有磁头固定在同一个 **盘臂组件 (Arm Assembly)** 上，同步移动。
- **磁道 (Track)**: 盘面被**划分为多个同心圆环，称为磁道**。
- **扇区 (Sector)**: 每个磁道被划分为更小的弧段，称为扇区。
	- 它是磁盘读写的**最小单位**。
	- 通常大小为 **512 bytes**。
- **柱面 (Cylinder)**: 所有盘片上**半径相同**的磁道集合。
	- *考试意义*：因为所有磁头是同步移动的，所以访问同一个柱面上的数据**不需要**移动磁臂（即不需要 Seek Time）。
- **磁盘控制器 (Disk Controller)**: 处理校验和 (Checksum) 以确保数据正确读写。

#### **(2) 性能指标 (Performance Measures) —— 计算题基础**

*(做计算题前必须理解这三个时间)*

要访问磁盘上的数据，总时间 = **寻道时间 + 旋转延迟 + 传输时间**。

1. **寻道时间 (Seek Time)**:
	- **定义**: **磁臂移动**到正确的 **柱面 (Cylinder)** 所需的时间。
	- *性质*: 最耗时的部分。
2. **旋转延迟 (Rotational Latency)**:
	- **定义**: 等待**指定的 扇区 (Sector) 旋转到**磁头下**放所需的时间。**
	- ***平均**延迟*: 通常按**旋转一周时间的一半**计算 (12 revolution time)。
3. **数据传输率 (Data-transfer Rate)**:
	- **定义**: 数据从磁盘**读出或写入的速率。**

#### **(3) 磁盘 I/O 成本计算 (Disk IO Cost Calculation) —— ⭐⭐⭐ 大题核心**

*(这是本章最可能考大题的地方，请务必掌握随机访问与顺序访问的区别)*

我们用 **块 (Block)** 作为**存储分配和传输的单位**。

- **随机访问 (Random Access)**:
	- 每次**读一个块**，**都需要重新定位**。
	- **成本公式**: Cost=Seek+Rotational Latency+Transfer。
- **顺序访问 (Sequential Access)**:
	- 读取**一系列 连续** 的块。
	- **成本公式**:
		- **第 1 个块**: 需要 Seek+Rotational Latency+Transfer。
		- **后续块**: 只需要 Transfer (因为**磁头已经在正确位置**了)。

【📚 完整考试计算实例 (Comprehensive Calculation Example)】

(基于讲义 Slide 18 的经典例题)

**题目背景**:

- 有两个表存储在磁盘上：
	- **Student 表 (s)**: 包含 10 条记录，占用 **5 个磁盘块** (s1 到 s5)。
	- **Takes 表 (t)**: 占用 **5 个磁盘块** (t1 到 t5)。
- **假设**:
	- 内存**缓冲区**只能容纳 **2 个块** (Block A, Block B)。
	- 忽略计算时间，**只估算 磁盘 I/O 次数 (包括 Seek 和 Transfer)。**
	- 为了简化，我们**假设每次“切换读取的目标文件”或“跳跃读取”都需要一次 Seek**。

场景 1：访问序列 (Access Sequence 1)

s1, t1, t2, t3, s2, t4

**计算步骤**:

1. **读 s1**:
	- 这是开始，需要定位 `Student` 表的位置。
	- **成本**: **1次 Seek** + 1次 Transfer。
	- *(Buffer: s1, Empty)*
2. **读 t1**:
	- 从 `Student` 表**跳到 `Takes` 表，需要重新寻道。**
	- **成本**: **1次 Seek** + 1次 Transfer。
	- *(Buffer: s1, t1)*
3. **读 t2**:
	- `t2` 紧挨着 `t1` (**顺序读取)。不需要 Seek。**
	- **成本**: 0次 Seek + 1次 Transfer。
	- *(Buffer: s1, t2) -> **覆盖了 t1***
4. **读 t3**:
	- `t3` 紧挨着 `t2` (顺序读取)。
	- **成本**: 0次 Seek + 1次 Transfer。
	- *(Buffer: s1, t3)*
5. **读 s2**:
	- 从 `Takes` 表跳回 `Student` 表 (且 `s2` 可能不挨着刚才的 `t3` 位置)，**需要 Seek**。
	- **成本**: 1次 Seek + 1次 Transfer。
	- *(Buffer: s2, t3)*
6. **读 t4**:
	- 从 `Student` 表**跳回 `Takes` 表**，需要 Seek。
	- **成本**: 1次 Seek + 1次 Transfer。

**总成本**:

- **Seek 次数**: 4次 (s1, t1, s2, t4 这四次跳跃)。
- **Transfer 次数**: 6次 (一共读了6个块)。

场景 2：访问序列 (Access Sequence 2) —— 考察 Buffer 命中

s1, t1, t2, t3, s1, s2, t4

(注意：中间多了一次 s1 的访问)

**计算步骤**:

1. **读 s1**: 1 Seek + 1 Transfer。 *(Buffer: s1)*
2. **读 t1**: 1 Seek + 1 Transfer。 *(Buffer: s1, t1)*
3. **读 t2**: 0 Seek + 1 Transfer。 *(Buffer: s1, t2) -> t1被替换*
4. **读 t3**: 0 Seek + 1 Transfer。 *(Buffer: s1, t3) -> t2被替换*
5. **读 s1**:
	- 检查 Buffer，发现 **s1 还在内存中** (Pinned or just lucky)。
	- **成本**: **0 I/O** (**直接从内存读**)。
6. **读 s2**:
	- `s2` 紧挨着 `s1` (假设是顺序的)，但刚才磁头在 `t3` 的位置，现在要读 `s2`，需要跳回 Student 表。
	- **成本**: 1 Seek + 1 Transfer。
7. **读 t4**:
	- 跳回 Takes 表。
	- **成本**: 1 Seek + 1 Transfer。

**总成本**:

- **Seek 次数**: 4次。
- **Transfer 次数**: 6次 (因为 s1 第2次读没发生 I/O)。

### **💡 考试做题技巧**

1. **区分 Seek 和 Transfer**:
	- 题目如果问 **"Number of block transfers"**，就是数一数**一共读了几个块。**
	- 题目如果问 **"Number of Seeks"**，就**要看从上一个块到这一个块是否“连续”**。
		- **连续**: 0 Seek。
		- **不连续 (跳表、随机)**: 1 Seek。
2. **看清 Buffer 大小**:
	- 如果 **Buffer 很大**，以前读过的块可能还在，不需要重读。
	- 如果 **Buffer 很小**（如上例），新块会把旧块挤出去，下次再用旧块就得重读。
3. **顺序 vs 随机**:
	- **顺序读取 N 个块**: Cost = 1 Seek + N Transfer。
	- **随机读取 N 个块**: Cost = N Seek + N Transfer。
	- *(显然**顺序读取快得多**)*。

收到。根据 **《ch10.pdf》 (Chapter 10: Storage and File Structure)** 的第 9-13 页内容，为你详细总结 **“3. 磁盘性能与调度 (Performance Measures & Scheduling)”**。

这一部分通常考察 **两个方面**：

1. **小计算题**：计算平均旋转延迟（Rotational Latency）或平均访问时间。
2. **概念题/算法模拟**：理解电梯算法（Elevator Algorithm）是如何工作的。

### **3. 磁盘性能与调度 (Performance Measures & Scheduling)**

#### **(1) 性能指标 (Performance Measures) —— ⭐ 计算题基础**

衡量磁盘性能主要看 **访问时间 (Access Time)**，即从发出读写指令到数据开始传输所需的总时间。

它由三个部分组成：

1. **寻道时间 (Seek Time)**：

	- **定义**：磁臂移动到指定 **柱面 (Cylinder)** 所需的时间。
	- **性质**：这是最慢的一步，通常是**毫秒级**（如 2ms - 10ms）。
	- *考试注意*：题目通常会**直接给出平均寻道时间**，不用你算。

2. **旋转延迟 (Rotational Latency)**：

	- **定义**：等待指定的 **扇区 (Sector)** 旋转到磁头下方所需的时间。

	- **计算公式 (必背)**：

		Average Latency=1/2×**旋转一周的时间**

	- **关联指标**：磁盘**转速 (RPM, Rotations Per Minute)**。

	**【📝 必考计算实例：算延迟】**

	- **题目**：假设一个磁盘的转速是 **7200 RPM** (每分钟 7200 转)，求**平均旋转延迟。**
	- **解题步骤**：
		1. 先算**每秒转多少圈**：7200÷60=120 转/秒。
		2. 算**转一圈要多久**：1÷120≈0.00833 秒 =8.33 毫秒 (ms)。
		3. 算**平均延迟（转半圈）**：8.33÷2≈4.17 ms。
	- **答案**：4.17 ms。

3. **传输时间 (Data-Transfer Time)**：

	- **定义**：数据从磁盘读出并写入内存的时间。
	- *公式*：数据传输率数据大小。

4. **平均故障时间 (MTTF - Mean Time To Failure)**：

	- **定义**：衡量磁盘可靠性的指标，表示磁盘在坏掉之前平均能跑多久。
	- *注意*：MTTF 很大（比如 100万小时），但这并不意味着这块盘肯定能跑这么久，而是说如果有 1000 块盘，每 1000 小时可能坏一块。

#### **(2) 磁盘调度 (Disk Scheduling) —— ⭐ 算法逻辑**

由于 **寻道时间 (Seek Time)** 是最耗时的，操作系统或磁盘控制器**会通过重新排列 I/O 请求的顺序来减少磁臂移动的总距离。**

讲义主要提到了 **电梯算法 (Elevator Algorithm)**，也叫 **SCAN 算法**。

- **核心逻辑**：
	- **磁臂像电梯**一样，**保持一个方向移动**（比如**从内圈向外圈），沿途处理所有经过的柱面的请求。**
	- **直到到达磁盘边缘（**或**该方向没有更多请求**），才 **反向** 移动。
- **优点**：
	- 相比于“先来先服务 (FCFS)”，它大幅减少了磁臂来回摆动的总距离，提高了吞吐量。

**【📝 完整考试实例：电梯算法模拟】**

**题目**： 假设磁盘磁头当前位于 **柱面 50**，正在向 **大柱面方向（向外）** 移动。 等待处理的请求队列（按到达顺序）：**98, 183, 37, 122, 14, 124, 65, 67**。 请写出 **电梯算法** 下的处理顺序。

**解题思路**：

1. **当前位置**: 50。
2. **当前方向**: 向**大数方向** (→)。
3. **第一阶段（向大）**:
	- 找比 50 大的数，按从小到大顺序处理。
	- 50 → **65** → **67** → **98** → **122** → **124** → **183**。
4. **第二阶段（反向）**:
	- 到了 183 后，假设没有更大的了，**掉头向小数方向 (←)。**
	- 找比 50 小的数，按从大到小顺序处理。
	- 183 → **37** → **14**。

**答案顺序**： `65, 67, 98, 122, 124, 183, 37, 14`。

### **💡 重点总结**

- **计算题**：给我转速 RPM，你会算平均延迟（**除以 60，倒数，再除以 2**）。
- **概念题**：知道 **Access Time = Seek + Latency + Transfer**。
- **算法题**：电梯算法就是 **“顺路带人”**，**不到尽头不回头**。

### **4. 磁盘 IO 成本计算 (Disk IO Cost Calculation)**

在做计算题时，我们需要关注两个主要指标：

1. **磁盘块传输次数 (Number of block transfers)**：即真正从磁盘读取数据到内存的次数。
2. **寻道次数 (Number of seeks)**：即**磁头重新定位的次数**（这是最耗时的操作）。

#### **(1) 基础原则 (Basic Rules)**

- **顺序访问 (Sequential Access)**:
	- 定义：连续读取磁盘上相邻的块。
	- **成本**: **1次 Seek** (定位第一个块) + **N次 Transfer** (连续读 N 个块)。
	- *原理*: 只有第一次需要移动磁臂，后面只需要**等盘片转过来**（旋转延迟通常包含在 Transfer 计算中或忽略，主要数 Seek）。
- **随机访问 (Random Access)**:
	- 定义：**跳跃读取不相邻的**块。
	- **成本**: **N次 Seek** + **N次 Transfer**。
	- *原理*: 每次读都要重新找位置，成本极高。

#### **(2) 完整考试计算实例 (Comprehensive Calculation Example)**

**【题目来源】**：讲义 Slide 18 经典例题。

**【题目背景】**

- **表结构**:
	- `Student` 表 (记为 `s`): 共有 10 条记录，存放在 **5 个磁盘块** 中 (𝑠1 到 𝑠5)。
	- `Takes` 表 (记为 `t`): 存放在 **5 个磁盘块** 中 (𝑡1 到 𝑡5)。
- **硬件限制**:
	- **缓冲区 (Buffer)**: 内存中只有 **2 个块** 的空间 (Block A, Block B)。
- **假设**:
	- 如果**读取的块紧挨着上一个读取的块**，则**不需要 Seek**。
	- 如果**从表 `s` 切换到表 `t`（或反之）**，**或者跳跃读取**，则需要 1 次 Seek。

【场景 1：普通访问序列】

访问序列: s1, t1, t2, t3, s2, t4

**逐步计算解析**:

1. **读 𝑠1**:
	- 动作: 开始读取 `Student` 表。
	- 开销: **1 Seek** (初始定位) + **1 Transfer**。
	- *Buffer*: {𝑠1}
2. **读 𝑡1**:
	- 动作: 从 `s` 表跳到 `t` 表。
	- 开销: **1 Seek** (切换文件) + **1 Transfer**。
	- *Buffer*: {𝑠1,𝑡1}
3. **读 𝑡2**:
	- 动作: 𝑡2 紧挨着 𝑡1。
	- 开销: **0 Seek** (顺序读取) + **1 Transfer**。
	- *Buffer*: {𝑠1,𝑡2} (覆盖了 𝑡1，**因为 𝑠1 被钉住用于对比)**
4. **读 𝑡3**:
	- 动作: 𝑡3 紧挨着 𝑡2。
	- 开销: **0 Seek** (顺序读取) + **1 Transfer**。
	- *Buffer*: {𝑠1,𝑡3} (覆盖了 𝑡2)
5. **读 𝑠2**:
	- 动作: 从 `t` 表**跳回 `s` 表。**
	- 开销: **1 Seek** + **1 Transfer**。
	- *Buffer*: {𝑠2,𝑡3} (𝑠1 任务结束，换入 𝑠2)
6. **读 𝑡4**:
	- 动作: 从 `s` 表跳回 `t` 表。
	- 开销: **1 Seek** + **1 Transfer**。
	- *Buffer*: {𝑠2,𝑡4} (覆盖 𝑡3)

**【最终答案】**

- **Total Seeks**: **4 次** (𝑠1,𝑡1,𝑠2,𝑡4)。
- **Total Transfers**: **6 次** (一共读了 6 个块)。

【场景 2：考察 Buffer 命中 (Buffer Hit)】

访问序列: s1, t1, t2, t3, s1, s2, t4

(注意：中间多了一次对 𝑠1 的访问)

**逐步计算解析**:

1. **读 𝑠1**: 1 Seek + 1 Transfer。 *(Buffer: 𝑠1)*
2. **读 𝑡1**: 1 Seek + 1 Transfer。 *(Buffer: 𝑠1,𝑡1)*
3. **读 𝑡2**: 0 Seek + 1 Transfer。
	- *策略*: 保留 𝑠1 (Pinned)，替换 𝑡1。 *(Buffer: 𝑠1,𝑡2)*
4. **读 𝑡3**: 0 Seek + 1 Transfer。
	- *策略*: 保留 𝑠1，替换 𝑡2。 *(Buffer: 𝑠1,𝑡3)*
5. **读 𝑠1** (关键点):
	- 检查缓冲区，发现 𝑠1 **已经在内存中**。
	- 开销: **0 Seek + 0 Transfer**。
	- *Buffer*: 状态不变 {𝑠1,𝑡3}。
6. **读 𝑠2**:
	- 动作: 𝑠2 紧挨着 𝑠1（假设顺序存储），但磁头当前可能停在 𝑡3 的位置（物理上），或者我们认为从 𝑠1 逻辑切换到 𝑠2。
	- *修正*: 根据讲义 Slide 18 的上下文，通常在嵌套循环连接中，读完内层 𝑡 后再读外层 𝑠 的下一个块，需要跳回去。
	- 动作: 从 `Takes` 表区域跳回 `Student` 表区域读取 𝑠2。
	- 开销: **1 Seek** + **1 Transfer**。
	- *Buffer*: {𝑠2,𝑡3} (𝑠1 被替换)。
7. **读 𝑡4**:
	- 动作: 从 `s` 跳到 `t`。
	- 开销: **1 Seek** + **1 Transfer**。

**【最终答案】**

- **Total Seeks**: **4 次**。
- **Total Transfers**: **6 次** (虽然访问序列长了，但 𝑠1 第二次访问是免费的)。

### **💡 考试做题总结 (Exam Tips)**

1. 什么时候算 Seek？

	*

	- **文件切换**: 从表 A 跳到表 B。
	- **随机跳跃**: 比如从 𝑠1 跳到 𝑠5。
	- **回头草**: 从 𝑡5 跳回 𝑠2。
	- *只要不是读“下一个 (Next)”块，通常都要 Seek。*

2. **什么时候不算 Seek？**

	- **顺序读取**: 𝑡1→𝑡2→𝑡3。只有读 𝑡1 时算 1 次 Seek，后面都是 0 Seek。
	- **Buffer 命中**: 如果题目暗示块还在内存里（Pinned Block），那么 Seek 和 Transfer 都是 0。

3. **什么时候算 Transfer？**

	- 只要数据不在内存里，需要从磁盘读，就算 1 次 Transfer。
	- 如果数据已经在内存里 (Buffer Hit)，则 Transfer = 0。

4. 大题必背公式:

	

	Total Cost=(Seek Count×Avg Seek Time)+(Transfer Count×Avg Transfer Time)

	

	(如果题目只问 Count，就算次数；如果问时间，代入题目给的毫秒数)

### **5. 缓冲区管理 (Buffer Management)**

#### **(1) 核心任务 (Goal) —— 小题基础**

- **缓冲区管理器 (Buffer Manager)**: 负责**在内存中分配空间以存储磁盘块**，并在**需要时将块从磁盘读入内存或写回磁盘**。
- **工作流程**:
	1. 程序请求某个磁盘块。
	2. 管理器检查该块**是否已在缓冲区中**。
		- **命中 (Hit)**: 如果在，直接返回内存地址。
		- **未命中 (Miss)**: 如果不在，**分配空间（可能需要替换旧块）**，从磁盘读取。

#### **(2) 关键机制 (Key Mechanisms) —— ⭐ 重要概念**

1. **钉住 (Pinned Blocks)**:
	- **定义**: 为了**防止正在被使用的数据被意外替换或写回**，系统将该块 **“钉住” (Pin)**。
	- **作用**: 被钉住的块 **不允许** 被写回磁盘。
	- *例子*: 之前的计算题中，当**外层循环正在使用 `s1` 块进行对比**时，`s1` 就必须被钉住，否则读下一个 `t` 块时可能会把 `s1` 挤出去，导致后续对比出错。
2. **脏块 (Dirty Block)**:
	- 如果一个块在缓冲区中**被修改过，它就是“脏”的。**在**替换它之前，必须将其 写回 (Write back) 磁盘**。如果是**“干净”的（未修改），直接覆盖即可**。

#### **(3) 替换策略 (Replacement Strategies) —— ⭐⭐⭐ 核心大题考点**

数据库系统通常不直接使用操作系统通用的替换策略，而是**根据访问模式（Access Pattern）选择最优策略**。

**1. 最近最少使用 (LRU - Least Recently Used)**:

- **规则**: 替换掉那个 **过去最长时间未被引用** 的块。
- **假设**: 最近用过的块，**马上还会再用（局部性原理）**。
- **缺点**: 对于数据库中常见的 **“重复顺序扫描” (Repeated Sequential Scans)**，LRU 是 **最差** 的策略。这种现象称为 **顺序泛洪 (Sequential Flooding)**。

**2. 立即丢弃 (Toss-immediate)**:

- **规则**: 一旦某个块的最后一条元组**被处理完，立刻释放**该块占用的空间。
- **适用场景**: 该块在当前操作中再也不会被用到了（例如：内层循环的非钉住块）。

**3. 最近最常使用 (MRU - Most Recently Used)**:

- **规则**: 替换掉那个 **最近刚刚被使用过/刚刚被解钉 (Unpinned)** 的块。
- **适用场景**: **循环扫描 (Cyclic Scan)**。MRU **会保留“老”的块（通常是文件的开头）**，这正是**下次扫描首先需要的**。

### **📚 完整考试实例：顺序泛洪 (Sequential Flooding)**

*(这是涵盖本节核心知识点的经典例子，用来解释**为什么数据库不用 LRU**)*

**【场景描述】**

- **操作**: 对一个文件进行 **重复多次扫描**（例如**嵌套循环连接中的内层表**）。
- **文件大小**: 假设文件有 **5 个块** (𝐵1,𝐵2,𝐵3,𝐵4,𝐵5)。
- **缓冲区大小**: 假设内存只能容纳 **4 个块**。

**【LRU 的表现 (Worst Case)】**

1. **第一轮扫描**:
	- 读 𝐵1,𝐵2,𝐵3,𝐵4。缓冲区满 {𝐵1,𝐵2,𝐵3,𝐵4}。
	- 读 𝐵5: LRU 替换掉 **最老** 的 𝐵1。缓冲区变为 {𝐵5,𝐵2,𝐵3,𝐵4}。
2. **第二轮扫描** (从头开始读):
	- 需要读 𝐵1: 发现 𝐵1 刚被踢走！LRU 替换掉当前最老的 𝐵2。缓冲区 {𝐵5,𝐵1,𝐵3,𝐵4}。
	- 需要读 𝐵2: 发现 𝐵2 刚被踢走！LRU 替换掉当前最老的 𝐵3。
	- ...

- **结果**: 每一次读取都会导致 **未命中 (Page Fault)**。LRU **总是精确地把下次马上要用的块踢走。**

**【MRU 的表现 (Best Case)】**

1. **第一轮扫描**:
	- 读 𝐵1,𝐵2,𝐵3,𝐵4。缓冲区满。
	- 读 𝐵5: MRU 替换掉 **最近** 的 𝐵4（或者说系统知道 𝐵1 这种开头的块更重要，保留它们）。缓冲区变为 {𝐵1,𝐵2,𝐵3,𝐵5}。
2. **第二轮扫描**:
	- 需要读 𝐵1: **命中！** (它还在内存里)。
	- 需要读 𝐵2: **命中！**
	- 需要读 𝐵3: **命中！**
	- 需要读 𝐵4: 未命中，读入。

- **结果**: MRU 成功保留了文件开头的块，大大减少了 I/O 次数。

### **💡 考试做题技巧**

- **如果问**: "Explain Sequential Flooding"（**解释顺序泛洪**）。
	- **答**: LRU strategy replaces the block that will be needed soonest (the beginning of file) in repeated scans, causing excessive I/O.
- **如果问**: "Which strategy is better for nested-loop joins?"（嵌套循环连接选哪个策略？）
	- **答**: **MRU** (Most Recently Used) or **Toss-immediate** is better than LRU.

### **6. 文件组织 (File Organization)**

这一节讨论的是：在一个单独的磁盘块（Block）内部，如何摆放多条记录。

#### **(1) 定长记录 (Fixed-Length Records) —— 简单但有删除难题**

- **基本思想**:
	- 假设每条记录长度固定为 𝑛 字节。
	- 第 𝑖 条记录简单地存放在从偏移量 𝑛×(𝑖−1) 开始的位置。
	- **优点**: 访问简单，计算位置快。
- **删除带来的问题 (The Deletion Problem)**:
	- 如果删除了中间的一条记录（比如第 3 条），会留下一个“空洞 (Hole)”，浪费空间。
- **解决方案 (重点)**:
	1. **移动最后一条记录 (Move the last record)**:
		- 把文件最后一条记录搬过来填补空洞。
		- *缺点*: 会改变那条被搬运记录的物理地址（RID），导致指向它的外部指针失效。通常不推荐。
	2. **空闲列表 (Free List) —— ⭐ 推荐方法**:
		- **机制**: 不移动记录，而是把所有“被删除形成的空洞”用链表串起来。
		- **文件头 (File Header)**: 保存一个指针，指向**第一个**空闲记录的位置。
		- **被删记录的内容**: 既然被删了，旧数据没用了，就利用这块空间存一个指针，指向**下一个**空闲记录。
		- *例子*: Header → Record 3 (空) → Record 7 (空) → Null。当插入新记录时，优先填补 Record 3 的位置。

#### **(2) 变长记录 (Variable-Length Records) —— ⭐⭐⭐ 核心大题考点**

变长记录常见于包含 `varchar` 类型的表。由于记录长短不一，不能简单按位置计算，必须使用 **分槽页结构 (Slotted Page Structure)**。

**【分槽页结构详解】(必须背诵结构组成)**

在一个磁盘块 (Block) 内部，布局如下：

1. **块头 (Block Header)**: 位于块的**最前端**。包含：
	- **记录条数 (Number of record entries)**。
	- **空闲空间末尾指针 (End of free space)**: 指向块中空闲区域的最后一个字节位置。
	- **条目数组 (Location & Size array)**: 每一条记录对应一个数组元素，存储该记录的 `(偏移量 offset, 长度 size)`。
2. **记录存放区**: 实际的记录内容从块的**最末端**开始，**向前**（向头部方向）连续堆叠。
3. **空闲空间 (Free Space)**: 位于块头和记录存放区之间的夹层。

**【操作逻辑 (考试会考)】**:

- **插入新记录**:
	- 在空闲空间分配内存（记录放在 Free Space 的尾部，即紧挨着上一条记录）。
	- 在块头的数组中增加一项，记录新记录的位置和大小。
- **移动记录**:
	- 如果块内进行碎片整理（移动记录位置），只需要修改**块头中的指针**。
	- **关键点**: 外部指向该记录的指针（如索引）使用的是 `(Page ID, Slot ID)`，即使记录在页内搬家了，它的 `Slot ID` (数组下标) 没变，所以**外部指针不需要修改**。这是分槽页最大的优点！

### **7. 文件中记录的组织 (Organization of Records in Files)**

这一节讨论的是：在**宏观的文件级别（包含很多个块）**，如何安排记录。

#### **(1) 堆文件组织 (Heap File Organization)**

- **定义**: 记录可以**放在文件的任何地方，只要有空地。**
- **特点**: **插入极快（直接扔到末尾）**，但**查找特定记录很慢**（通常需要全表扫描）。
- **维护**: 通常使用“空闲空间映射图 (Free-space map)”来寻找哪里有空位。

#### **(2) 顺序文件组织 (Sequential File Organization)**

- **定义**: 记录按照 **搜索码 (Search Key)** 的**顺序物理存储**。
- **优点**: 非常适合 **范围查询 (Range Scan)**（如 `SELECT * FROM student WHERE ID BETWEEN 100 AND 200`）。
- **难点 (插入与链表)**:
	- 为了保持物理顺序，插入新记录可能需要移动大量数据（**像数组插入**一样）。
	- **优化**: 实际上**很难时刻保持物理顺序**。通常使用 **指针链 (Pointer Chain)**。如果应该插入的位置没地儿了，就**放到溢出块 (Overflow Block) 里，然后用指针连起来。**
	- *代价*: 随着溢出链越来越长，性能下降，需要定期 **重组 (Reorganize)** 文件。

#### **(3) 散列文件组织 (Hashing File Organization)**

- **定义**: 对记录的**搜索码应用 Hash 函数**，计算出它应该去的 Block 地址。
- **特点**: 随机读写（精确查找 `WHERE ID = 10`）极快，但**不支持范围查询**。

#### **(4) 多表聚簇文件组织 (Multitable Clustering File Organization)**

- **常规做法**: 通常**一张表存一个文件**。
- **聚簇做法**: 将 **不同表 (Tables)** 的、**相关联** 的记录存储在 **同一个磁盘块** 中。

**【📚 完整考试实例：聚簇的好处与坏处】**

**场景**:

- 表 `department` (系) 和表 `instructor` (老师)。
- 关系: 一个系有多个老师。

**聚簇存储结构**:

- 在一个磁盘块里，先存 `Computer Science` 系的 `department` 记录。
- 紧接着存所有 `Computer Science` 系的 `instructor` 记录（Katz, Srinivasan, Brandt...）。
- **然后再存 `Physics` 系的记录，接着是 `Physics` 的老师。**

**分析:**

- **优点 (Good for Join)**:
	- 执行 `department` ⋈ `instructor` (**连接查询**) 时**效率极高。**
	- *理由*: 读入一个块，这个系和它的老师**都在内存里了**，**一次 I/O 搞定连接**。
- **缺点 (Bad for Single Table)**:
	- 执行 `SELECT * FROM department` 时效率变低。
	- *理由*: 因为 `department` 记录之间**夹杂了大量的 `instructor` 记录**，原本 1 **个块能存下的系信息，现在分散在 10 个块里**，导致 I/O 次数暴增。

### **💡 考试做题总结**

1. **变长记录怎么存？** → **Slotted Page Structure**。
	- *必背*: Header 在头，Records 在尾，中间是 Free Space。Header 里存指针数组。
2. **定长记录删除怎么处理？** → **Free List**。
	- *原理*: 把**空洞连成链表，头指针指向第一个空洞。**
3. **什么是 Clustering？**
	- *定义*: 把**相关的表存一起。**
	- *适用*: **经常做 Join** 操作。
	- *不适用*: **经常全表扫描其中某一个单独**的表。

# ch11

### **1. 基本概念 (Basic Concepts)**

#### **(1) 索引与搜索码 (Index & Search Key)**

- **索引 (Indexing)**:
	- **定义**: 一种用于**加速访问所需数据的机制。**
	- *类比*: 就像图书馆的作者目录，或者书本背后的关键词索引。
- **搜索码 (Search Key)**:
	- **定义**: 用于**在文件中查找记录的一个属性或属性集 (Attribute or set of attributes)。**
	- *注意*: 虽然叫 "Key"，但**搜索码 不一定 是主码 (Primary Key)**，甚至**不一定是唯一的**（比如**按“系名”查找，会有多个学生**）。

#### **(2) 索引文件结构 (Index File Structure)**

- **索引文件 (Index File)**:

	- 由一系列 **索引项 (Index Entries)** 组成的**辅助文件**。
	- 通常**比原数据文件小得多**。

- **索引项格式 (Index Entry Form)**:

	- 结构为：**`(search-key, pointer)`**
	- **Search-key**: 你要查的值（比如 ID = 101）。
	- **Pointer (指针)**: 指向该记录在磁盘上的物理位置（Block ID + Offset）。

	**【📝 完整例子】** 假设我们有一个 `Student` 表，你想根据 `ID` 查找学生。

	- **索引项**: `(10101, Pointer_A)`
	- **含义**: 这一行告诉数据库，“学号为 10101 的学生**数据**，**存放在 `Pointer_A` 指向的磁盘块里**”。
	- 如果**没有索引，数据库必须扫描整个 `Student` 表（全表扫描）**才能找到 10101；有了索引，**直接读这个小文件就能定位**。

#### **(3) 索引的两大分类 (Two Basic Kinds of Indices) —— ⭐ 常考辨析**

1. **有序索引 (Ordered Indices)**:
	- **机制**: 搜索码按 **排序顺序 (Sorted Order)** 存储。
	- **适用**: **既适合查找特定**值，也特别适合 **范围查询 (Range Search)**（例如：`WHERE salary > 50000`）。
2. **哈希索引 (Hash Indices)**:
	- **机制**: 使用 **哈希函数 (Hash Function)** 将搜索码**均匀分布**到不同的 **桶 (Buckets)** 中。
	- **适用**: 非常适合 **点查询 (Point Search)**（例如：`WHERE ID = 22222`），但 **不适合 范围查询**。

#### **(4) 索引评价指标 (Index Evaluation Metrics)**

*(考试可能会问：我们在设计索引时考虑哪些因素？)*

1. **支持的访问类型 (Access types)**: 是查单条记录高效，还是查范围高效？
2. **访问时间 (Access time)**: 读数据的速度。
3. **插入时间 (Insertion time)**: 插入新数据时更新索引的代价。
4. **删除时间 (Deletion time)**: 删除数据时更新索引的代价。
5. **空间开销 (Space overhead)**: 索引文件占用的额外磁盘空间。

### **💡 考试做题技巧**

- **看到 "Sorted"**: 选 Ordered Index (B+ Tree)。
- **看到 "Uniformly distributed"**: 选 Hash Index。
- **看到 "Range query"**: 选 Ordered Index。
- **记住结构**: 索引项 = `(搜索码, 指针)`。

### **2. 有序索引 (Ordered Indices)**

**核心思想**：为了快速找到数据，索引文件中的 **搜索码 (Search Keys)** 是按照顺序排列的。

#### **(1) 聚集索引与非聚集索引 (Clustering vs Non-clustering Indices)**

这是根据 **索引顺序** 与 **数据物理存储顺序** 是否一致来划分的。

1. **聚集索引 (Clustering Index)**:
	- **定义**: 包含记录的文件本身是**按照某个搜索码的顺序 物理存储 (Physically ordered)** 的，那么建立在该搜索码上的索引就是聚集索引。
	- **别名**: 也叫 **主索引 (Primary Index)**。
		- *注意*: 这里的**“主索引”不一定非要是主码 (Primary Key)**，只要文**件是按它排序的就行**。但**通常主索引就是建立在主码上**的。
	- **特点**: 一个数据文件 **只能有一个 聚集索引**（因为数据只能物理上按一种顺序排）。
2. **非聚集索引 (Non-clustering Index)**:
	- **定义**: 搜索码的顺序与文件**记录的物理顺序** **不一致**。
	- **别名**: 也叫 **辅助索引 (Secondary Index)**。
	- **特点**: 一个数据文件可以有 **多个 非聚集索引。**

**【📝 完整考试实例：区分聚集/非聚集】** 假设有一个学生表 `Student`，记录在磁盘上是按照 **学号 (ID)** 从小到大物理存储的。

- **场景 A**: 我们在 **`ID`** 上建索引。
	- **索引**里的 `ID` 是**有序**的，**文件里**的 `ID` **也是有序**的。
	- **结论**: 这是一个 **聚集索引 (Clustering Index)**。
- **场景 B**: 我们在 **`dept_name` (系名)** 上建索引。
	- 索引里 `dept_name` 是按字母序排的（Biology, CS, History...）。
	- 但是文件里记录是按 `ID` 排的，同一个系的记录可能分散在文件各个角落。
	- **结论**: 这是一个 **非聚集索引 (Non-clustering Index)**。

#### **(2) 稠密索引与稀疏索引 (Dense vs Sparse Indices) —— ⭐ 必考点**

这是根据 **索引项的数量** 来划分的。

1. **稠密索引 (Dense Index)**:
	- **定义**: **数据文件**中的 **每一个 (Every) 搜索码值**，在**索引文件**中**都有一个对应的索引项。**
	- **查找方式**: **直接在索引里二分查找**，找到后**顺着指针就能拿到**数据。
2. **稀疏索引 (Sparse Index)**:
	- **定义**: **只有 部分 (Some) 搜索码值**在索引文件中有**对应的索引项。**
	- **适用条件 (关键死穴)**: **必须是聚集索引 (Clustering Index)** 才能**建立稀疏索引。**
		- *理由*: 只有当数据是**按顺序排好的，我们才能利用“大概位置”往下找**。如果是**乱序的（非聚集）**，**稀疏索引就找不到漏掉的**那些数了。
	- **查找机制 (必背)**:
		1. 在索引中找到 **小于等于** 目标值 K 的 **最大** 搜索码。
		2. 跳转**到该记录**。
		3. **从该记录开始**，在数据文件中 **顺序向后扫描 (Sequential Scan)**，直到找到 K。

**【📝 完整考试实例：稠密 vs 稀疏的查找对比】**

**数据文件**: 按 ID 排序存储: `101, 102, 103, 301, 302, 401`.

- **稠密索引**:
	- 索引内容: `[101 -> P1], [102 -> P2], [103 -> P3], [301 -> P4] ...` (全都有)
	- **任务**: 找 `102`。
	- **过程**: **查索引直接找到** `[102 -> P2]`，一步到位。
- **稀疏索引**:
	- 索引内容: **每隔一个存一个** `[101 -> P1], [301 -> P4], [401 -> P6]`.
	- **任务**: 找 `102`。
	- **过程**:
		1. 查索引，找 ≤102 的最大值。找到 `101`。
		2. **跳转到 `101` 对应的物理位置。**
		3. **从 `101` 开始往后读磁盘**：读下一条，发现是 `102`。找到！

**【对比总结 (Trade-offs)】**

- **稠密索引**: 查找更快 (Faster location)，但索引**占用空间大**。
- **稀疏索引**: 索引占用空间小 (Less space)，**维护开销小，但查找慢一点**（要顺序扫描）。
- **实际应用**: 真正的数据库系统通常使用 **稀疏索引** 作为主索引（例如 **B+ 树的非叶子节点就是一种稀疏索引**的概念），且通常 **每个磁盘块 (Block) 建立一个索引项**。

#### **(3) 多级索引 (Multilevel Index)**

- **背景**: 随着数据量变大，**索引文件本身** 也会变得非常大，大到内存装不下。
- **解决方案**: 给索引**再建一个索引**。
	- **内层索引 (Inner Index)**: 指向实际数据的索引（通常是**主索引**）。
	- **外层索引 (Outer Index)**: **指向内层索引块的稀疏**索引。
- **查找**: 先查**外层** → 确定**内层块** → 查**内层** → 找到**数据**。
- *注*: 这其实就是 **B+ 树的原型。**

### **💡 考试做题技巧 (Tips for Exam)**

1. **判断题**: "Secondary indices must be dense." (**辅助**索引**必须是稠密的**。)
	- **答案**: **True**。
	- **解释**: 辅助索引（非聚集）的数据是乱序的。如果是稀疏的，你**指到一个位置**，后面**紧挨着的并不是下一个搜索码**，所以**没法顺序扫描**，**必须每个值都有指针**。
2. **选择题**: "Which index requires less space?" (哪个索引省空间？)
	- **答案**: **Sparse** Index。
3. **操作题**: 如果题目问 "Locate record with key K using **sparse** index"，记得写出两步：
	- Step 1: Find **largest index entry ≤K**.
	- Step 2: **Search sequentially** in the file from that position.

### **3. B+ 树索引 (B+-Tree Index Files)**

#### **(1) 结构与性质 (Structure & Properties) —— ⭐ 计算与填空基础**

**定义**: B+ 树是一种 **平衡树 (Balanced Tree)**，从根节点到所有叶子节点的路径长度相同。

**节点结构**:

- 每个节点包含**最多 n−1 个搜索码 (Search Keys) 和 n 个指针 (Pointers)**。
- 格式: P1,K1,P2,K2,...,Pn−1,Kn−1,Pn。
	- Ki: **搜索码**，且 K1<K2<...<Kn−1。
	- Pi: 指向子节点或记录的**指针**。

**节点约束 (必背，画图检查依据)**:

1. **根节点 (Root)**: **至少有 2 个子节点** (**除非树只有一层**)。
2. **非叶子节点 (Non-leaf Nodes)**:
	- **指针数**: **⌈n/2⌉(上取整)** 到 n。
	- *理解*: **至少半满**。
3. **叶子节点 (Leaf Nodes)**:
	- 包含的**值个数**: **⌈(n−1)/2⌉** 到 **n−1**。
	- **特殊结构**: 最后一个**指针 Pn 指向** **下一个叶子节点 (Next Leaf Node)**，形成**链表，方便范围查询。**

#### **(2) 查询 (Queries) —— 小题**

- **算法**: 从根开始，**找 K 所在的区间。**
	- 如果 K<K1，去 P1。
	- 如果 Ki≤K<Ki+1，去 Pi+1。
	- 如果 K≥Km−1，去 Pm。
- **一直走到叶子节点，在叶子节点中找 K**。如果找到，返回记录；否则说明不存在。

#### **(3) 插入 (Insertion) —— ⭐⭐⭐ 大题核心 (画图)**

**基本步骤**:

1. 查找 K **应该所在的叶子节点 L。**
2. 如果 L **未满** (个数 <n−1)：直接**按顺序插入。**
3. **如果 L 已满** (Splitting a Node)：
	- **分裂 (Split)**: 将 L **分裂为两个节点 L 和 L′。**
	- **分配**: **前 ⌈n/2⌉ 个值留在 L**，**剩下的移到 L′。**
	- **复制 (Copy up)**: 将 L′ 中的 **最小搜索码** **复制到父节点中作为索引。**
4. **父节点处理**:
	- 如果父节点也满了，**继续向上分裂。**
	- **注意 (关键区别)**: **非叶子节点**分裂时，中间的键值是 **上提 (Push up)** 而不是复制。这**意味着中间那个值会从原节点消失，只出现在父节点中**。

**【📚 完整考试实例：插入操作】** 假设 n=4 (最多 3 个 key, 4 个指针)。 初始树包含: 2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31。 **(参考讲义 Slide 27-30 的逻辑)**

- **场景 1: 插入 "Adams" (简单插入)**
	- 找到叶子节点，发现有空位，直接插。
- **场景 2: 插入 "Lamport" (叶子节点分裂)**
	- 假设某个叶子节点已有 `Gupta, Kim, Ni` (3个满了)。
	- 插入 `Lamport`，排序后为 `Gupta, Kim, Lamport, Ni` (4个)。
	- **分裂**:
		- 左节点: `Gupta, Kim` (2个)。
		- 右节点: `Lamport, Ni` (2个)。
	- **向上操作**: 把右边最小的 `Lamport` **复制 (Copy)** 到父节点。
- **场景 3: 插入导致非叶子节点分裂 (Propagate Split)**
	- 如果父节点插了 `Lamport` 后也满了 (比如变成 `... Gold, Katz, Lamport, ...` 超限)。
	- **分裂非叶子节点**:
		- 假设排序后是 `Gold, Katz, Lamport, Kim` (假定超限)。
		- 中间值是 `Lamport`。
		- **操作**: `Lamport` 被 **提上去 (Push up)** 放入爷爷节点。`Gold, Katz` 留左边，`Kim` 去右边。**注意 `Lamport` 在这层不保留！**

**💡 插入口诀**:

- **叶子满**: **中间分，右边小值复制上** (Copy Middle)。
- **非叶满**: **中间分，中间值提上去** (Push Middle)。

#### **(4) 删除 (Deletion) —— ⭐⭐⭐ 大题最难点**

**基本步骤**:

1. 找到 K 所在的叶子节点 L，删除 K。
2. **检查下溢 (Check Underflow)**:
	- 如果删除后，**节点太少 (指针数 <⌈n/2⌉)，需要调整。**
3. **调整策略 A: 借位 (Redistribution)**:
	- 看**兄弟节点 (Sibling)。**如果兄弟节点 **很富裕** (借出一个后不会下溢)。
	- 从兄弟**借一个键值过来，同时更新父节点的索引值。**
4. **调整策略 B: 合并 (Coalescing / Merge)**:
	- 如果兄弟节点 **也是刚过及格线** (借不了)。
	- 将两个节点 **合并** 成一个。
	- **关键**: 合并后，父节点中**指向被删节点的那个指针和对应的键值就没用了，**需要从父节点中 **删除 (Delete)**。这**可能引发父节点的下溢（递归向上）。**

**【📚 完整考试实例：删除操作】** 假设 n=4 (非叶子节点最少 2 个指针，叶子节点最少 2 个值)。 **(参考讲义 Slide 32-36 的逻辑)**

- **场景 1: 删除 "Srinivasan" (合并 - Merge)** [Slide 34]
	- 删除后，叶子节点只剩 1 个值 (Underflow)。
	- 看左兄弟，左兄弟也只有 2 个值 (借不了，借了它就挂了)。
	- **操作**: 将自己和左兄弟 **合并**。
	- **向上**: 父节点中的分割键 (比如 `Srinivasan`) 必须被 **删除**。这可能导致父节点下溢。
- **场景 2: 删除 "Singh" (借位 - Redistribution)** [Slide 35]
	- 删除后，叶子节点下溢。
	- 看左右兄弟，发现左兄弟比较富裕 (有 3 个值)。
	- **操作**: 把左兄弟最大的值挪过来。
	- **向上**: 修改父节点的索引值，使其正确分隔两个子节点。
- **场景 3: 非叶子节点合并** [Slide 36]
	- 如果父节点合并，需要把 **父节点的父节点 (爷爷)** 里的分隔值 **拉下来 (Pull down)** 放到合并后的新节点中间。
	- *这与插入时的 Push up 正好相反*。

**💡 删除口诀**:

- **富兄弟**: **借一个，改父值**。
- **穷兄弟**: **并起来，删父值** (若是非叶子合并，记得从上面拉个值下来填缝)。

### **⚠️ 考试画图注意事项**

1. **指针画法**: 叶子节点之间要有箭头连接 (链表)。非叶子节点指向下一层。
2. **值的大小**: 严格保证左小右大。
3. **节点检查**: 每次画完变动，数一下每个节点的 Key 数量和 Pointer 数量，看是否违反了 ⌈n/2⌉ 的限制。如果违反了没处理，必扣分。
4. **中间过程**: 如果题目问 "Show the steps"，不要只画最后结果，画出 分裂/合并 的中间状态。

### **4. B 树索引文件 (B-Tree Index Files)**

#### **(1) 核心定义与结构 (Structure) —— ⭐ 概念辨析**

- **定义**: B 树也是一种平衡树，但在**存储搜索码的方式上与 B+ 树不同。**
- **关键区别 (必背)**:
	1. **无冗余存储 (No Redundant Storage)**:
		- 在 B+ 树中，非叶子节点的搜索码**只是“路标”**，它们**会在叶子节点中再次出现。**
		- 在 **B 树** 中，**搜索码只出现一次**。如果**一个搜索码出现在非叶子节点中**，它就**不会再出现在任何其他节点（包括叶子节点）**中。
	2. **指针位置 (Pointer Location)**:
		- 在 B+ 树中，**只有叶子节点存储指向**实际记录 (File Record) 的指针。
		- 在 **B 树** 中，**所有节点 (包括非叶子节点)** 都存储指向实际记录的指针。
			- 具体来说，对于非叶子节点中的每个搜索码 𝐾𝑖，旁边都有一个对应的指针指向数据库中搜索码为 𝐾𝑖 的记录。

#### **(2) 节点结构图解 (Node Structure)**

一个典型的 B 树节点结构如下：



𝑃1,(𝐾1,RecordPtr1),𝑃2,(𝐾2,RecordPtr2),...,𝑃𝑛

- **𝑃𝑖 (Tree Pointer)**: 指向子节点的指针（和 B+ 树一样）。
- **𝐾𝑖 (Search Key)**: 搜索码。
- **RecordPtr𝑖 (Bucket Pointer)**: **直接指向实际数据记录**（这是 B+ 树非叶子节点没有的）。

【📝 完整例子：查找过程对比】

假设我们要查找 ID = 30。

- **在 B+ 树中**:
	- 根节点有 30 → 只是路标，告诉你“往右走”。
	- ...一路走到叶子节点。
	- 在叶子节点找到了 30，读取对应的记录指针。
	- *结论*: 必须走到叶子节点才能拿到数据。
- **在 B 树中**:
	- 根节点有 30。
	- **惊喜！** 30 旁边直接就挂着数据指针。
	- 直接读取数据，结束查询。
	- *结论*: **可能**在根节点或中间层就找到数据，不用走到叶子。

#### **(3) B 树 vs. B+ 树：优缺点对比 (Advantages & Disadvantages) —— ⭐ 简答题核心**

这是考试最喜欢问的：“为什么数据库系统通常偏爱 B+ 树而不是 B 树？”

**B 树的优点**:

1. **查找可能更快**: 如果你要找的记录**刚好在根节点或靠近根的层，不需要走到叶子节点就能直接拿到**数据指针。
2. **空间利用率 (理论上)**: 因为**没有重复存储键值**，理论上存同样多的键值需要的节点数可能少一点（但不绝对，见缺点）。

**B 树的缺点 (为什么我们不用它?)**:

1. **扇出率低 (Lower Fan-out) → 树更高**:
	- 非叶子节点除了存键值，还得存指向数据的指针（Record Pointer）。这些指针占用了空间，导致一个节点能存的子节点指针 (𝑃𝑖) 变少了。
	- **结果**: 树的**层数 (Height) 变高了，增加了磁盘 I/O 次数**。
2. **范围查询困难 (Range Query Difficulty)**:
	- B+ 树的叶子节点连成了一个链表，范围查询只要找到起点，顺着链表跑就行。
	- B 树的键值**分散在整棵树的各个层级里。要做范围查询（比如 10 到 100），必须在树里爬上爬下 (In-order traversal)**，效率极低。
3. **删除操作复杂**:
	- 在 B 树非叶子节点删除一个键值，需要**从子节点找个替补上来，由于键值不重复，这涉及到复杂的结构调整**。**B+ 树只用处理叶子节点，**更简单。

### **💡 考试做题总结**

1. **辨析题**: 看到“非叶子节点包含数据指针 (pointers to records)” → **B 树**。看到“非叶子节点只起索引作用” → **B+ 树**。
2. **简答题**: 如果问 B 树的区别，答两点：
	- Search keys are not repeated (no redundant storage).
	- Record pointers are stored in non-leaf nodes.
3. **做题策略**: 除非题目明确要求画 B 树，否则做索引操作大题时，**默认画 B+ 树**（这是讲义的重点）。不要把 B 树的“不重复”特性套用到 B+ 树上。

收到。我们严格按照 **《ch11.pdf》** 的第 48-67 页内容，为你详细总结 **“5. 静态哈希 (Static Hashing)”** 和 **“6. 动态哈希 (Dynamic Hashing)”**。

**重点提醒**：**可扩展哈希 (Extendable Hashing)** 的 **插入、分裂和目录翻倍** 是这部分最重要的大题考点，请务必仔细阅读第 6 部分的步骤和示例。

### **5. 静态哈希 (Static Hashing)**

这部分通常考概念小题（选择/判断），重点在于理解**“溢出”怎么处理。**

#### **(1) 结构 (Structure)**

- **桶 (Bucket)**: 存储一个或多个记录的单位。
- **哈希函数 (Hash Function)**: 设 ℎ 为哈希函数，它将搜索码 𝐾 映射到桶的地址 𝐵。
	- 插入时：计算 ℎ(𝐾)，将记录**存入对应的桶。**
	- 查询时：计算 ℎ(𝐾)，去**对应的桶里找。**

#### **(2) 溢出处理 (Handling of Bucket Overflows) —— 小题核心**

当一个桶满了（Bucket is full），再往里插数据怎么办？

- **原因**:
	1. **桶不足**: 记录总数超过了所有桶的总容量。
	2. **偏斜 (Skew)**: 哈希函数设计不好，或者某些值的记录特别多，导致大家都挤在一个桶里。
- **解决方案**: **溢出链 (Overflow Chaining)**。
	- 如果桶 𝑗 满了，系统会分配一个新的 **溢出桶 (Overflow Bucket)**。
	- 用**链表（指针）将原桶和溢出桶串联起来。**
	- 这叫做 **封闭哈希 (Closed Hashing)** 或 **拉链法**。

**缺点**: 随着数据量增加，**溢出链会越来越长**，查询不再是 𝑂(1)，性能会**退化成线性扫描**。这就引入了**动态哈希**的需求。

### **6. 动态哈希 (Dynamic Hashing)**

核心考点: **可扩展哈希 (Extendable Hashing)。**

考试形式: 给定初始状态和一系列插入操作，让你画出每一次 目录翻倍 或 桶分裂 后的结构图。

#### **(1) 核心结构 (Structure) —— ⭐ 画图基础**

可扩展哈希由两部分组成：

1. **目录 (Directory)**:
	- 一个指向桶的 **指针数组**。
	- **全局深度 (Global Depth, 𝑑)**: 决定目录的大小。目录包含 2𝑑 个条目。
	- 我们使用哈希值的 **前 𝑑 位 (High-order bits)** 来定位目录项。
2. **桶 (Bucket)**:
	- 实际存储数据的地方。
	- **局部深度 (Local Depth, 𝑖𝑑)**: 每个桶都有自己的深度。
	- 𝑖𝑑 表示该桶里的所有记录，在哈希值的 **前 𝑖𝑑 位** 上是完全相同的。
	- 约束条件: **𝑖𝑑≤𝑑**。

#### **(2) 插入算法 (Insertion Algorithm) —— ⭐⭐⭐ 大题解题步骤**

假设我们要插入记录 𝐾，计算哈希值 ℎ(𝐾)。

1. **定位**: 根据 ℎ(𝐾) 的前 𝐺𝑙𝑜𝑏𝑎𝑙_𝐷𝑒𝑝𝑡ℎ 位找到目录项，顺着指针找到桶 𝐵。
2. **情况 A: 桶 𝐵 未满**: 直接插入。
3. **情况 B: 桶 𝐵 已满，且 𝐿𝑜𝑐𝑎𝑙_𝐷𝑒𝑝𝑡ℎ<𝐺𝑙𝑜𝑏𝑎𝑙_𝐷𝑒𝑝𝑡ℎ**:
	- **操作**: **只分裂桶，不翻倍目录**。
	- **分裂**: 将**桶 𝐵 分裂为 𝐵1 和 𝐵2。**
	- **深度增加**: 两个新桶的**局部深度都变为** 𝑂𝑙𝑑_𝐿𝑜𝑐𝑎𝑙_𝐷𝑒𝑝𝑡ℎ+1。
	- **重分配**: 把原桶的数据按新的一位（第 𝑖𝑑+1 位）是 0 还是 1，分发到 𝐵1 和 𝐵2。
	- **更新目录**: 让目录中原本指向 𝐵 的指针，一半指向 𝐵1，一半指向 𝐵2。
4. **情况 C: 桶 𝐵 已满，且 𝐿𝑜𝑐𝑎𝑙_𝐷𝑒𝑝𝑡ℎ==𝐺𝑙𝑜𝑏𝑎𝑙_𝐷𝑒𝑝𝑡ℎ**:
	- **操作**: **目录翻倍 (Directory Doubling)** + **分裂桶**。
	- **目录翻倍**: Global Depth 加 1。目录大小变为原来的 2 倍。原来的指针复制一份（例如，原来前缀 `0` 指向桶 A，现在 `00` 和 `01` 都指向桶 A）。
	- **分裂桶**: 现在 𝐿𝑜𝑐𝑎𝑙_𝐷𝑒𝑝𝑡ℎ<𝐺𝑙𝑜𝑏𝑎𝑙_𝐷𝑒𝑝𝑡ℎ 了（因为全局变大了），转回 **情况 B** 继续处理（分裂桶 𝐵，更新指针）。

#### **(3) 📚 完整考试实例：可扩展哈希的插入 (必看)**

**题目设定**:

- 桶容量 (Bucket Size) = 2。
- 初始 Global Depth = 1。
- 初始目录: `0` → Bucket A (Local Depth 1), `1` → Bucket B (Local Depth 1)。
- 哈希函数 ℎ(𝑥) 直接取二进制。
- **插入序列**: `1010`, `1111` (已存在), 插入 `0101`, `0000`, `1100`。

**初始状态**:

- Global Depth: 1
- Directory:
	- `0` → Bucket A (LD=1): `Empty`
	- `1` → Bucket B (LD=1): `1010`, `1111` (已满)

**步骤 1: 插入 `0101` (Hash Prefix: 0)**

- 看 Global Depth (1位)，前缀是 `0`。指向 Bucket A。
- Bucket A 未满，直接插入。
- **状态**: Bucket A: `0101`。

**步骤 2: 插入 `0000` (Hash Prefix: 0)**

- 前缀 `0` 指向 Bucket A。Bucket A 现有 `0101`，再加 `0000`，满。
- **状态**: Bucket A: `0101`, `0000`。

**步骤 3: 插入 `1100` (Hash Prefix: 1) —— 触发翻倍！**

- 前缀 `1` 指向 Bucket B。
- Bucket B 内容: `1010`, `1111`。**已满！**
- 检查深度: Bucket B 的 Local Depth (1) **等于** Global Depth (1)。
- **动作 1: 目录翻倍**
	- Global Depth 变为 2。
	- 目录项: `00`, `01`, `10`, `11`。
	- 指针调整:
		- 原来 `0` 指向 A → 现在 `00`, `01` 都指向 A。
		- 原来 `1` 指向 B → 现在 `10`, `11` 都指向 B。
- **动作 2: 分裂 Bucket B**
	- 创建新桶 B1, B2。
	- 新 Local Depth = 1 + 1 = 2。
	- **重分配 (Redistribute)** 原 B 中的数据 `1010`, `1111` 以及新来的 `1100`：
		- 看第 2 位（Global Depth=2）：
		- `1010` (前缀 `10`) → 放入 B1。
		- `1111` (前缀 `11`) → 放入 B2。
		- `1100` (前缀 `11`) → 放入 B2。
- **动作 3: 更新目录指针**
	- 目录 `10` 指向 B1。
	- 目录 `11` 指向 B2。
	- *(注意：Bucket A 没分裂，所以 `00`, `01` 依然同时指向 Bucket A，且 Bucket A 的 Local Depth 保持为 1)*。

**最终结果**:

- **Global Depth**: 2
- **Directory**:
	- `00` → Bucket A (LD=1): `0101`, `0000`
	- `01` → Bucket A (LD=1): *(同上)*
	- `10` → Bucket B1 (LD=2): `1010`
	- `11` → Bucket B2 (LD=2): `1111`, `1100`

### **💡 动态哈希做题总结**

1. **什么时候翻倍？** 只有当 **要插入的桶满了** 并且 **该桶的 Local Depth == Global Depth** 时，目录才翻倍。
2. **什么时候只分裂不翻倍？** 桶满了，但 **Local Depth < Global Depth**。这说明目录已经够细了，只是这个桶包含了多个目录项（比如 `00`和`01`都指着它），只需要把桶拆开，让目录项分别指过去就行。
3. **Local Depth 含义**: 总是表示“这个桶里的数，前几位是完全一样的”。Bucket A 的 LD=1，说明里面的数第 1 位都是 0，但第 2 位不一定（有 `00` 也有 `01`）。Bucket B2 的 LD=2，说明里面的数前 2 位必须是 `11`。

### **⚠️ 重点复习建议**

1. **死磕 B+ 树的插入/删除**：一定要自己手画几遍 Split 和 Merge 的过程，特别是 Split 时中间那个键值是“提上去”还是“复制上去”（B+树叶子分裂是复制，非叶子是上提）。
2. **死磕可扩展哈希**：搞清楚什么时候目录翻倍，什么时候只分裂桶。
3. **区分 Dense/Sparse**：选择题常考。

接下来请告诉我，你想先详细复习哪一个部分？（建议从 **B+树** 或 **基本概念** 开始）。

### **7. 比较 (Comparison)**

我们在为数据库设计选择索引类型时，必须考虑查询的 **类型 (Pattern of queries)**。没有一种索引是万能的，必须权衡利弊。

#### **(1) 核心对比逻辑 (The Core Trade-off) —— ⭐ 必背考点**

1. **哈希索引 (Hash Indices)**:
	- **优势**: 对于 **点查询 (Point Queries / Exact Match)** 非常高效。
		- *定义*: 查询条件是具体的等值匹配，例如 `column = value`。
		- *效率*: 理论上接近 𝑂(1)，直接定位到桶。
	- **劣势**: 极其 **不适合** **范围查询 (Range Queries)**。
		- *定义*: 查询条件包含范围，例如 `column > value` 或 `value1 < column < value2`。
		- *原因*: 哈希函数将数据打散了，物理上相邻的数值（比如 100 和 101）可能被哈希到完全不同的桶里。要找 >100 的数，哈希索引帮不上忙，只能全表扫描。
2. **有序索引 (Ordered Indices / B+ Trees)**:
	- **优势**: 同时支持 **点查询** 和 **范围查询**。
		- *效率*: 范围查询时，找到起点后顺着叶子节点的链表走即可。
	- **劣势**: 对于纯粹的点查询，速度略慢于哈希索引（需要 𝑂(log𝑁) 走树的高度，而哈希是直接计算地址）。

#### **(2) 完整的考试实例 (Comprehensive Example)**

假设我们有一个 `Instructor` (讲师) 表，包含 `ID` 和 `Salary` (薪水) 两个属性。

**场景 A: 身份验证系统**

- **SQL 语句**: `SELECT name FROM instructor WHERE ID = '22222';`
- **分析**: 这是一个 **等值查询 (Equality Look-up)**。
- **推荐索引**: **哈希索引 (Hash Index)**。
	- *理由*: 哈希能在常数时间内直接定位到 `ID='22222'` 的位置，速度最快。B+ 树也能做，但需要多读几个节点（树高）。

**场景 B: 薪资统计系统**

- **SQL 语句**: `SELECT name FROM instructor WHERE Salary > 80000;`
- **分析**: 这是一个 **范围查询 (Range Look-up)**。
- **推荐索引**: **有序索引 (Ordered Index / B+ Tree)**。
	- *理由*:
		1. B+ 树能快速找到 `80000` 这个值在叶子节点的位置。
		2. 然后沿着叶子节点的 **指针链 (Pointer Chain)** 向右遍历，直到结束。
		3. 如果是哈希索引，因为 `80001` 和 `90000` 可能在完全无关的桶里，系统被迫扫描整个文件，索引完全失效。

#### **(3) 设计时的考量因素 (Factors to Consider)**

在考试中，如果题目让你讨论 "Issues in Index Selection"（选择索引时的考量），请列出以下几点：

1. **定期重组的代价 (Cost of periodic re-organization)**:
	- 静态哈希需要定期重组（解决溢出链过长），而 B+ 树和可扩展哈希能自动适应数据增长。
2. **插入和删除的频率 (Frequency of insertion and deletion)**:
	- 如果数据变动极快，由于 B+ 树需要频繁分裂合并，可能开销较大；但可扩展哈希也要处理目录翻倍，需要具体分析。通常 B+ 树是通用选择。
3. **平均访问时间 (Average access time)**:
	- 主要取决于查询类型（点查询还是范围查询）。

### **💡 考试做题技巧**

- **看到 "Range" (范围)**: 毫不犹豫选 **Ordered Index (B+ Tree)**。
	- *关键词*: `BETWEEN`, `>`, `<`, `ORDER BY`, `GROUP BY` (经常需要排序)。
- **看到 "Exact match" (精确匹配) 且强调 "Fastest" (最快)**: 选 **Hash Index**。
	- *关键词*: `Equality search`, `Key lookup`, `Unique ID`.
- **如果题目未明确查询类型**: 默认推荐 **B+ 树**。因为它是现代数据库系统中应用最广泛的、最通用的索引结构，能应付所有情况而不至于退化。

# 重点

### **2. B+ 树索引 (B+-Tree Index Files)**

#### **(1) 核心结构与性质 (Structure & Properties) —— ⭐ 小题/填空考点**

**定义**: B+ 树是一种 **平衡树 (Balanced Tree)**。

- **核心特征**: 从根节点 (Root) 到所有叶子节点 (Leaf nodes) 的路径长度 **完全相同** 1。这意味着所有叶子都在同一层，查询效率稳定。

节点约束 (Node Constraints) —— ⭐ 必须背下来，用于计算 n 2

假设一个节点最多能容纳 𝑛 个指针（即 𝑛 是节点的容量/扇出 Fan-out）：

1. **根节点 (Root)**:
	- 如果**树不止一层**，根节点至少要有 **2 个子节点**。
2. **非叶子节点/内部节点 (Internal Nodes)**:
	- 起**“路标”作用**，存储**搜索码**和指向**下一层的指针**。
	- **指针数量**: 必须在 **⌈𝑛/2⌉ 到 𝑛** 之间。
	- *(理解：**至少半满**)*
3. **叶子节点 (Leaf Nodes)**:
	- 存储**实际的搜索码**和**指向记录的指针（或记录本身）**。
	- **值 (Values) 的数量**: 必须在 **⌈(𝑛−1)/2⌉ 到 𝑛−1** 之间。
	- **链表结构**: 所有的叶子节点通过指针 (𝑃𝑛) 串联在一起，形成一个线性链表。这使得 B+ 树非常适合 **范围查询 (Range Query)** 3。

#### **(2) B+ 树的高度与 I/O 计算 (Height & I/O Cost) —— ⭐⭐⭐ 大题核心**

考试中通常会给你数据量、块大小，让你算 B+ 树有多高，或者查一条记录需要几次 I/O。

核心公式 (必背)4444:

ℎ≤⌈log⌈𝑛/2⌉⁡(𝐾)⌉

- **𝐾 (或 𝑚)**: 搜索码的**总数** (Total number of search-key values)。
- **𝑛**: 一个节点**最多能存多少个指针** (即 Block Size / Entry Size)。
- **⌈𝑛/2⌉**: 分母是节点的**最小填充因子**（**最坏情况**下节点**只有半满**）。
- **ℎ**: **树的高度** (也就是**查询索引所需的 I/O 次数**)。

**I/O 次数判定**:

- 查询**一条记录**的索引 I/O 次数 = **树的高度 ℎ**。
- **如果还要把实际数据块读出**来，总 I/O = **ℎ+1**。
	- *(注：通常 B+ 树的**根节点常驻内存**，实际物理 I/O 可能会少 1 次，但考试如果没有特殊说明，**按树高算**即可)*。

#### **📚 完整考试计算实例 (Calculation Example)**

**(基于讲义 Slide 34 的经典数据)**

题目:

假设我们有一个包含 1,000,000 (100万) 条记录的表。

- **磁盘块大小 (Block Size)** = 4 KB (4096 Bytes)。
- **索引项大小 (Index Entry Size)** = 40 Bytes (包含**搜索码和指针**)。
- 请计算：如果使用 B+ 树索引，查找一条记录大约需要几次磁盘 I/O？

**解题步骤**:

第一步：计算 𝑛 (每个节点**最多能存多少个指针**)

𝑛=Block SizeEntry Size=409640≈100

*(讲义 Slide 34 取了近似值 100)* 5

第二步：确定底数 (最坏情况下的分支因子)

根据 B+ 树性质，非叶子节点**至少半满**：

Base=⌈𝑛/2⌉=⌈100/2⌉=50

第三步：计算树高 ℎ

ℎ=logBase⁡(Total Records)=log50⁡(1,000,000)

计算对数：

501=50

502=2,500

503=125,000

504=6,250,000

因为 1,000,000<6,250,000，所以树的高度 **ℎ≈4** (或者说**不超过 4 层**) 。

**第四步：对比结论 (如果题目问优势)**

- **B+ 树 I/O**: 大约 **4 次**。
- **二叉树 (Binary Tree)**: log2⁡(1,000,000)≈20 次。
- **结论**: B+ 树通过**增加节点的“宽度”（𝑛 很大），极大地降低了树的“高度”**，从而显著减少了磁盘 I/O 次数 。

#### **(3) B+ 树 vs. 哈希索引 (Comparison)**

*(结合《课内数据库系统.pdf》提到的考点：给你表和块，问 B+ 树几 I/O，哈希几 I/O)*

- **B+ 树 (Ordered Index)**:
	- **I/O**: 通常 **3~4 次** (取决于树高)。
	- **适用**: **范围查询 (Range Search)** (e.g., `> 500`) 和 等值查询 8。
- **哈希索引 (Hash Index)**:
	- **I/O**: 理想情况下 **1 次** (直接定位桶) 。如果桶溢出 (Overflow) 会多一点，但通常少于 B+ 树。
	- **适用**: 仅限 **等值查询 (Point Search)** (e.g., `= 500`)。**完全不支持** 范围查询 。

**💡 考试做题总结**:

1. **算 I/O**: 套公式 log⌈𝑛/2⌉⁡(𝐾)。
2. **做选择**: 看到“范围查询 (Range Query)”、“排序 (Sorted)”、“顺序访问 (Sequential Access)” -> 选 **B+ 树**。
3. **做判断**: B+ 树非叶子节点只是路标，不存数据；叶子节点连成链表。

### **3. 哈希索引 (Hash Indices)**

#### **(1) 核心概念 (Key Concepts) —— ⭐ 填空/选择考点**

- **桶 (Bucket)**:
	- 存储一个或多个记录的存储单元，通常对应一个 **磁盘块 (Disk Block)** 2。
- **哈希函数 (Hash Function)**:
	- **定义**: 一个函数 ℎ，将搜索码 𝐾 映射到**桶的地址 𝐵 3。**
	- **理想性质**:
		1. **均匀 (Uniform)**: 每个桶分配到的搜索码数量大致相同 4。
		2. **随机 (Random)**: 无论搜索码的实际分布如何，每个桶被分配到的记录数不受影响 5。
- **溢出处理 (Overflow Handling)**:
	- **溢出链 (Overflow Chaining)**: 当一个桶满了（Bucket Overflow），系统会分配一个新的溢出桶，并用链表（指针）将其连接起来。这种方法也称为 **封闭哈希 (Closed Hashing)** 6。

#### **(2) I/O 代价计算 (I/O Cost Calculation) —— ⭐⭐⭐ 考试必考 (计算/对比)**

考试常考场景：给定一个表和特定的查询，问你用 **Hash 索引** 需要几次 I/O？用 **B+ 树** 需要几次 I/O？

**计算规则**:

1. **点查询 (Point Query / Equality Search)** (例如 `WHERE ID = 101`):
	- **哈希索引 I/O**: **1 次** (理想情况)。
		- *解释*: 计算 ℎ(𝐼𝐷) 直接得到桶地址，读取该桶（1个 Block）即可找到记录。
		- *注意*: 如果发生溢出（Overflow），需要**沿着溢出链读取**，可能需要 >1 次 I/O，但在考试估算中，通常假设哈希函数设计良好，**默认按 1 次计算** 7。
	- **对比 B+ 树**: 需要 **树高 ℎ** 次 I/O（通常 **3~4 次**）。
	- **结论**: 在点查询上，**Hash 索引 (1 I/O) 优于 B+ 树索引 (ℎ I/O)**。
2. **范围查询 (Range Query)** (例如 `WHERE ID > 100`):
	- **哈希索引 I/O**: **非常大 (所有块的数量)**。
		- *解释*: 哈希函数**把数据打散了**，`101` 和 `102` 可能在完全不同的桶里。要找所有 >100 的数据，哈希索引完全失效，只能进行 **全表扫描 (Linear Scan)**。
	- **对比 B+ 树**: 只需要定位起点后顺着叶子链表走，效率极高。
	- **结论**: 在范围查询上，**Hash 索引性能极差**。

#### **📚 完整考试实例 (Comprehensive Exam Example)**

题目背景:

有一个 Student 表，包含 1,000,000 (100万) 条记录。

- 每条记录占用 100 Bytes。
- 磁盘块大小 (Block Size) = 4 KB (4000 Bytes)。
- 因此，总共有 1,000,000×100/4000=25,000 个数据块。
- 假设 B+ 树的高度 ℎ=4。

**问题 1: 查询 `SELECT \* FROM Student WHERE ID = 12345` (点查询)**

- **使用 Hash 索引**:
	- 计算 ℎ(12345) → 定位到具体的一个桶（Block）。
	- **I/O 开销**: **1 次** (读取那个桶)。
- **使用 B+ 树索引**:
	- 从根节点往下找叶子节点。
	- **I/O 开销**: **4 次** (树高)。
- **结论**: Hash 索引更快。

**问题 2: 查询 `SELECT \* FROM Student WHERE ID > 12345` (范围查询)**

- **使用 Hash 索引**:
	- 哈希无法支持范围，必须扫描整个表。
	- **I/O 开销**: **25,000 次** (读取**所有数据块)**。
- **使用 B+ 树索引**:
	- 先花 4 次 I/O 找到 `12345`。
	- 然后顺着链表往后读，**假设符合条件的数据占 10 个块。**
	- **I/O 开销**: 4+10=14 次。
- **结论**: B+ 树索引完胜。

#### **(3) 哈希与有序索引的对比 (Comparison) —— ⭐ 简答/选择题核心**

这是讲义中明确强调的对比点 。

| **特性**       | **哈希索引 (Hash Index)**                            | **有序索引 (Ordered Index / B+ Tree)** |
| -------------- | ---------------------------------------------------- | -------------------------------------- |
| **核心优势**   | **点查询 (Equality Search)** 最快                    | **范围查询 (Range Search)** 支持最好   |
| **典型 I/O**   | **1** (理想情况)                                     | **ℎ** (树高，通常 3-4)                 |
| **范围查询**   | **不支持** (需全表扫描，代价极高)                    | **支持** (效率高)                      |
| **适用场景**   | `WHERE ID = '22222'` 9                               | `WHERE Salary > 80000` 10              |
| **复合键问题** | 查询必须包含**所有**索引字段才能用 (e.g., hash(A,B)) | 支持前缀匹配 (e.g., index(A,B) 可查 A) |

### **💡 考试做题总结 (Exam Tips)**

1. **看到 "Calculate I/O cost"**:
	- 如果是 **Hash** 且是 **Equality/Point search** → 答案通常是 **1**。
	- 如果是 **B+ Tree** → 答案通常是 **树高 (Height)** (题目通常会给出或让你算出是 3 或 4)。
2. **看到 "Range Query" (>, <, BETWEEN)**:
	- 千万别选 Hash 索引，它的代价是全表扫描。必须选 **B+ 树**。
3. **看到 "Worst case of Hash"**:
	- 最坏情况是所有记录都映射到了同一个桶（哈希冲突严重），导致退化成线性链表扫描。理想情况是 **Uniform (均匀)** 分布 。

# ch12

### **1. 基本步骤 (Basic Steps)**

查询处理 (Query Processing) 是指从数据库接收 SQL 查询到返回结果的全过程。它主要包含 **三个核心步骤** 1：

#### **步骤 1: 解析与翻译 (Parsing and translation)**

- **主要任务**:
	1. **翻译 (Translate)**: 将用户写的查询（Query）翻译成系统内部形式，最终转换为 **关系代数表达式 (Relational-algebra expression)** 
	2. **检查 (Check)**:
		- **语法检查 (Syntax check)**: SQL 写得对不对？
		- **验证关系 (Verify relations)**: 表名、列名是否存在？3
- **输出**: **关系代数表达式。**

#### **步骤 2: 优化 (Optimization) —— ⭐ 概念核心**

这是查询处理的“大脑”。哪怕是同一个 SQL 语句，也可以有**无数种执行方式**，优化的目的就是找出 **代价最低 (Lowest Cost)** 的那一种。

- **为什么需要优化？**
	1. **等价表达式 (Equivalent expressions)**: 一个关系代数表达式可能有许多等价形式。
		- *例子*: 先做积 (Product) 再做选择 (Selection)，通常不如先做选择再做积快 。
	2. **多种算法 (Different algorithms)**: 每一个关系代数操作（如 Join）都有多种算法实现（如 Nested-loop, Merge-join, Hash-join）5。
- **执行计划 (Evaluation Plan / Execution Plan)**:
	- **定义**: 一个带有详细注释的表达式，规定了具体的 **执行策略 (Evaluation Strategy)** 6。
	- *内容*: 不仅决定了运算顺序，还指定了具体的算法（例如：是用索引查找还是全表扫描？）。
- **优化的依据**:
	- 优化器 (Optimizer) 利用数据库目录中的 **统计信息 (Statistical Information)**（如表中有多少行、元组大小等）来估算各种计划的代价 。

#### **步骤 3: 执行 (Evaluation)**

- **执行引擎 (Query-execution engine)**: 接收优化器生成的 **执行计划 (Execution Plan)**，执行它，并将结果返回给用户 8。

------

### **📚 完整考试实例 (Example)**

为了涵盖上述所有知识点，请看以下从 SQL 到最终执行的完整流程例子：

**用户查询 (SQL)**:

```
SELECT salary FROM instructor WHERE salary < 75000;
```

**1. 解析与翻译 (Parsing & Translation)**:

- 系统检查语法无误，表 `instructor` 和列 `salary` 存在。

- 转换为**关系代数表达式**：

	$$\Pi_{salary}(\sigma_{salary < 75000}(instructor))$$

**2. 优化 (Optimization)**:

- 优化器生成了两个可能的 **执行计划 (Evaluation Plans)**：
	- **Plan A**: 对 `instructor` 表进行 **全表扫描 (Complete relation scan)**，逐行检查 `salary < 75000`。
	- **Plan B**: 使用 `salary` 字段上的 **索引 (Index)** 来直接定位 `salary < 75000` 的记录 9。
- **估算**: 优化器查看统计信息，发现 `instructor` 表很大，但**满足条件的记录很少**。
- **决策**: **计算出 Plan B 的代价更低，因此选择 Plan B。**

**3. 执行 (Evaluation)**:

- 执行引擎**按照 Plan B（使用索引）运行**，快速找到数据并返回结果。

------

### **💡 考试避坑指南 (Exam Tips)**

1. **区分术语**:
	- **Relational-algebra expression**: 只**是逻辑上的表达式**（做什么）。
	- **Evaluation plan**: 是**具体的实施方案**（怎么做，用什么算法，用不用索引）。
	- *考点*: 优化器的输出是 **Evaluation plan**，不是 Relational-algebra expression。
2. **统计信息**:
	- 如果填空题问“优化器基于什么来估算代价？”，答案是 **Statistics (统计信息)** 。
3. **流程顺序**:
	- **Parser $\rightarrow$ Relational Algebra $\rightarrow$ Optimizer $\rightarrow$ Execution Plan $\rightarrow$ Evaluation Engine** 。

### **2. 查询代价的度量 (Measures of Query Cost)**

#### **(1) 核心度量标准 (The Metric)**

- **总运行时间 (Total elapsed time)** 是最通用的标准，但为了简化和考试估算，我们主要关注 **磁盘存取 (Disk Access)** 。
	- *原因*：磁盘 I/O 通常是数据库操作中的**主要开销 (Predominant cost)**，而且相对容易估算。
- **忽略项 (Ignored Costs)** :
	- **CPU 代价**：通常忽略不计（虽然真实系统中会考虑）。
	- **写出结果的代价 (Writing output to disk)**：通常不包含在代价公式中。

#### **(2) 代价计算公式 (Cost Formula) —— ⭐⭐⭐ 大题计算基础**

这是你做计算题时必须列出的基本公式符号。

- **符号定义** 3:

	- **$t_T$**: 传输**一个块**的时间 (**Time to transfer one block**)。
	- **$t_S$**: **一次磁盘寻道**的时间 (**Time for one seek**)。
	- **$b$**: 传输的**磁盘块数量** (Number of block transfers)。
	- **$S$**: **寻道次数** (Number of seeks)。

- 总代价公式 (Total Cost):

	$$Cost = b \times t_T + S \times t_S$$

	(考试时通常让你**分别算出 b 和 S 是多少**，或者代入题目给的**毫秒数**计算总时间)

#### **(3) 缓冲区假设 (Buffer Assumption) —— ⭐ 关键前提**

在计算代价（特别是 Worst Case）时，必须基于一个关于内存（缓冲区）大小的假设。

- **最坏情况 (Worst Case)** :
	- 假设缓冲区 **只能容纳极少的数据 (hold only a few blocks)**。
	- 通常假设每个关系 (Relation) 在内存中只有 **1 个块** 的空间。
	- *考试意义*：这意味着如果我们要反复扫描一个表，**每次都得从磁盘重新读，因为内存存不下**，刚读进来的会被挤出去。**这是计算 Nested-Loop Join 代价巨大的根本原因。**
- **最佳情况 (Best Case)**:
	- 所有数据都**能读入缓冲区**。
	- *考试意义*：只需读一次，之后**都在内存操作**，没有额外 I/O。

------

### **📚 完整考试计算实例 (Calculation Example)**

为了让你理解 $S$ (Seek) 和 $b$ (Transfer) 的区别，我们看一个基础的文件扫描例子。

题目：

有一个关系（表）Student，它包含 100 个磁盘块 (100 Blocks)。

请计算在以下两种情况下读取该表所有数据的代价（$Cost$）。

**情况 A：顺序扫描 (Sequential Scan)**

- **场景**：磁盘磁头定位到文件的开头，然后连续读取这 100 个块。
- **计算**:
	- **Seek ($S$)**: **1 次** (只需起步时定位一次)。
	- **Transfer ($b$)**: **100 次** (要读 100 个块)。
- **代价**: $1 \times t_S + 100 \times t_T$。

**情况 B：随机访问 (Random Access) / 最坏情况**

- **场景**：磁盘**极其碎片化，或者我们按某种跳跃顺序读取这 100 个块**，导致每读一个块都要重新移动磁头。
- **计算**:
	- **Seek ($S$)**: **100 次** (每读一个块都要重新寻道)。
	- **Transfer ($b$)**: **100 次**。
- **代价**: $100 \times t_S + 100 \times t_T$。

💡 总结：

做大题时，你不需要纠结 $t_S$ 和 $t_T$ 具体是多少毫秒（题目没给就保留符号），你只需要数清楚：

1. **一共读了多少个块 ($b$)**？
2. **磁头跳了几次 ($S$)**？（记住：连续读算 1 次跳，重新回头读算 +1 次跳）。

### **3. 选择运算 (Selection Operation)**

我们主要讨论两种基本策略：**线性搜索 (File Scan)** 和 **索引扫描 (Index Scan)**。

#### **(1) 线性搜索 (A1. Linear Search / File Scan) —— ⭐ 基础保底算法**

- **适用场景**:
	- **没有索引。**
	- 或者主要文件并未按某种顺序排列。
	- 或者要检索的记录数很多，用索引反而更慢（因为索引可能导致随机 I/O）。
- **算法逻辑**: 扫描文件中的 **每一个** 磁盘块，测试所有记录是否满足条件。
- **代价计算 (Cost Estimate)** 1:
	- **$b_r$**: 关系 $r$ 占用的磁盘块总数。
	- **最坏情况 (Worst Case)**:
		- **Transfer**: $b_r$ (**读完所有块**)。
		- **Seek**: $1$ (初始**定位到文件开头**)。
		- *公式*: $Cost = 1 \times t_S + b_r \times t_T$
	- **平均情况 (Average Case)**:
		- 仅当选择条件是 **“主码等值查询 (Key attribute equality)”** 时（找唯一的那个它）。
		- **平均只要读一半文件**就能找到。
		- **Transfer**: $b_r / 2$。
		- **Seek**: $1$。

------

#### **(2) 使用索引的选择 (Selections Using Indices) —— ⭐⭐⭐ 计算重点**

当查询条件涉及的属性上有 **索引 (Index)** 时，我们可以用索引来加速。代价取决于索引类型（主/副）和查询类型（等值/范围）。

**符号**: $h$ = **索引高度 (B+树高度)。**

A2. 主索引，**等值查询** (Primary Index, Equality on Key) 2

- **场景**: `WHERE ID = '101'`，且 `ID` 是主键（主索引）。
- **结果**: 只有 1 条记录。
- **代价**:
	- **Seek**: $h$ (**查索引**) $+ 1$ (**查数据**) $= h + 1$。
	- **Transfer**: $h$ (**读索引**块) $+ 1$ (**读数据块**) $= h + 1$。
	- *注：这是效率最高的查询。*

A3. 主索引，**非键等值查询** (Primary Index, Equality on Nonkey) 3

- **场景**: `WHERE dept_name = 'Biology'`，`dept_name` 是主索引但不是主键（虽然少见，指聚集索引）。
- **结果**: 多条记录。但因为是主索引（聚集的），这些记录在物理上是 **连续存储 (Consecutive)** 的。
- **代价**:
	- **Seek**: $h + 1$ (同上，定位到第一条记录)。
	- **Transfer**: $h + b$。
		- 其中 $b$ 是**包含匹配记录的 块数。**因为记录**是连续的**，只要顺着读 $b$ 个块就行，不需要额外 Seek。

A4. 辅助索引，等值查询 (Secondary Index, Equality) —— ⚠️ 坑点 4

- **场景**: `WHERE salary = 80000`，`salary` 上**有辅助索引（非聚集）。**
- **结果**: $n$ 条记录。
- **代价 (最坏情况)**:
	- 因为是辅助索引，记录在物理磁盘上是 **乱序** 的。每读一条记录，**可能都需要去不同的块，甚至可能重新寻道**。
	- **Seek**: $h + n$ (查索引 $h$ + **每条记录 $1$ 次 Seek)**。
	- **Transfer**: $h + n$。
	- *警告*: 如果 $n$ 很大，这个**代价会极高（甚至超过线性扫描 A1）。**

------

#### **(3) 涉及比较的查询 (Selections Involving Comparisons) —— 范围查询**

查询形如 `A > V` 或 `A <= V`。

A5. 主索引，**比较** (Primary Index, Comparison) 

- **场景**: 表已经按属性 $A$ 排序（主索引）。
- **$A \le V$**:
	- **不使用索引**。直接从文件头开始 **线性扫描 (File Scan)**，直到遇到第一个 $> V$ 的记录为止。
	- *理由*: 开头的数据都满足条件，直接读块最快。
- **$A \ge V$**:
	- **使用索引**。**先在索引中找到第一个 $\ge V$ 的值，**然后从那个位置开始 **顺序扫描 (Sequential Scan)** 文件，直到结束。

A6. **辅助索引，比较** (Secondary Index, Comparison) 6

- **场景**: 表未按 $A$ 排序，但 $A$ 上有辅助索引。
- **算法**: 使用索引的叶子节点链表，找到满足条件的指针，然后一个个去取数据。
- **代价**: 极高！
	- 每一条满足条件的记录都可能导致一次随机 I/O。
	- **结论**: 除非满足条件的记录非常少，否则 **线性扫描 (Linear Scan)** 通常更便宜 7。

------

### **📚 完整考试实例 (Comprehensive Calculation Example)**

题目:

有一个 Student 表：

- 总块数 ($b_r$) = 1000。
- 总记录数 ($n_{total}$) = 10,000。
- B+ 树索引高度 ($h$) = 4。
- 查询：`SELECT * FROM Student WHERE ...`

请计算以下场景的 **Block Transfers (传输次数)** 和 **Seeks (寻道次数)**：

**场景 1: `WHERE ID = 12345`** **(ID 是主键，有主索引**)

- **算法**: **A2 (Primary Index, Key)**
- **Transfer**: $h + 1 = 4 + 1 = \mathbf{5}$。
- **Seek**: $h + 1 = \mathbf{5}$。

**场景 2: `WHERE dept_name = 'CS'`** (dept_name **有辅助索引，共有 10 条匹配记录**)

- **算法**: **A4 (Secondary Index)**
- **匹配记录数 ($n$)**: **10**。
- **Transfer**: $h + n = 4 + 10 = \mathbf{14}$。
- **Seek**: $h + n = 4 + 10 = \mathbf{14}$。
- *(解释：最坏情况下，这 10 个学生**分散在 10 个不同的块里**，每读一个都要跳过去)*。

**场景 3: 全表扫描 (无索引或条件不匹配)**

- **算法**: **A1 (Linear Search)**
- **Transfer**: $b_r = \mathbf{1000}$。
- **Seek**: $\mathbf{1}$ (初始定位)。

**💡 总结与对比**:

- **A2 (主键)** **最快：代价是个位数。**
- **A4 (辅助索引)** **风险最大**：如果匹配记录 ($n$) 很多（比如 2000 条），代价就是 2004 次 I/O，比全表扫描（1000 次）还慢！这就是为什么数据库优化器有时会放弃索引而选择全表扫描。

### **4. 连接运算 (Join Operation)**

**【符号定义 (必背)】**

- **$r$**: **外层**关系 (Outer relation)。
- **$s$**: **内层**关系 (Inner relation)。
- **$n_r, n_s$******: 元组数****量 (Number of tuples)。
- **$b_r, b_s$**: **磁盘块数**量 (Number of blocks)。
- **$t_T$**: 传输**一个块的时间** (Transfer time)。
- **$t_S$**: **一次寻道的时间** (Seek time)。

------

#### **(1) 嵌套循环连接 (Nested-Loop Join) —— ⭐ 基础算法**

- **算法逻辑**:
	- 双重循环：对于 $r$ 中的 **每一条记录**，扫描一遍 $s$。
- **代价计算 (最坏情况 Worst Case)**:
	- 假设内存**只能容纳 1 个 $r$ 的块和 1 个 $s$ 的块。**
	- **Block Transfers**: $n_r \times b_s + b_r$
		- *(解释：读 1 次 $r$ 的所有块 + $r$ 的**每一行都要读一遍整个 $s$)***
	- **Seeks**: $n_r + b_r$
		- *(解释：**读 $r$ 的块需寻道** + $r$ 的**每一行都要重新定位 $s$ 的开头**)*
- **小题考点**: 如果必须用这个算法，应该选 **小表 (Small relation)** 作为外层关系 ($r$)，以减少总代价。

------

#### **(2) 块嵌套循环连接 (Block Nested-Loop Join) —— ⭐ 优化算法**

- **算法逻辑**:
	- 以 **块 (Block)** 为单位。对于 $r$ 的 **每一个块**，扫描一遍 $s$。
- **代价计算 (最坏情况 Worst Case)**:
	- **Block Transfers**: $b_r \times b_s + b_r$
		- *(解释：**$r$ 的每个块**都要**把 $s$ 的所有块读一遍 + 读 $r$ 本身**)*
	- **Seeks**: $2 \times b_r$
		- *(解释：每次**读入 $r$ 的一块需要 Seek**，每次**重新扫描 $s$ 也需要 Seek**。即**每处理 $r$ 的一块产生 2 次 Seek**)*
- **代价计算 (最佳情况 Best Case)**:
	- 假设内存足够大，能把整个内层关系 $s$ 装进去。
	- **Total Cost**: $b_r + b_s$
		- *(解释：两个表**各读一遍就行了**)*

------

#### **(3) 索引嵌套循环连接 (Indexed Nested-Loop Join) —— ⭐⭐⭐ 绝对重点 (大题必考)**

这是真题预测中明确提到的考点。当内层关系 $s$ 在连接属性上有 **索引 (Index)** 时，我们**不需要扫描整个 $s$，而是直接查索引。**

- **算法逻辑**:

	- 对于 $r$ 中的每一个元组 $t_r$，利用**索引在 $s$ 中查找匹配的元组**。

- 代价公式 (必背):

	$$Cost = b_r(t_T + t_S) + n_r \times c$$

	- **$b_r(t_T + t_S)$**: 读取**外层关系 $r$ 的代价（全表扫描）**。
	- **$n_r \times c$**: 对于 $r$ 的 **$n_r$ 条记录**，每一条**都要进行一次索引查找**。
	- **$c$ (单次查找代价)**: **遍历索引的高度** + 读取**实际数据块**。

【📚 完整考试计算实例 (必须掌握)】

(参考讲义 Slide 37-39)

**题目**:

- 关系 `student` ($r$): $n_r = 5000$ 行, $b_r = 100$ 块。
- 关系 `takes` ($s$): $n_s = 10000$ 行, $b_s = 400$ 块。
- **索引**: `takes` 表在连接属性上有 **B+ 树** 索引。
- **B+ 树参数**: 树高 $h=3$ (即查找索引需要 3 次 I/O)，读取**最终数据还需要 1 次 I/O。即 $c = 3 + 1 = 4$。**
- **任务**: 计算 **Indexed Nested-Loop Join** 的 Block Transfers 和 Seeks。

**计算步骤**:

1. **外层循环 ($r$) 代价**:
	- **Transfer: $b_r = 100$。**
	- **Seek: $1$ (顺序扫描)。**
2. **内层查找 ($s$) 代价**:
	- **循环次数: $n_r = 5000$ 次。**
	- **每次查找代价 ($c$):** 4 次 Transfer (**3索引+1数据**) + 4 次 Seek (**每次读块假设都不连续)**。
	- 总计: $5000 \times 4 = 20,000$ Transfer, $5000 \times 4 = 20,000$ Seek。
3. **总代价**:
	- **Total Transfers**: $100 + 20,000 = \mathbf{20,100}$。
	- **Total Seeks**: $1 + 20,000 = \mathbf{20,001}$。

------

#### **(4) 归并连接 (Merge-Join)**

- **适用条件**: 两个关系都已按连接属性 **排序 (Sorted)**。
- **算法逻辑**: 双指针同时扫描两个有序文件，匹配归并。
- **代价计算**:
	- **Block Transfers**: $b_r + b_s$
		- *(解释：每个块只需读一次)*
	- **Seeks**: $\lceil b_r / b_b \rceil + \lceil b_s / b_b \rceil$
		- *(解释：取决于内存缓冲区能存多少块，Seek 次数很少)*
- **注意**: 如果表未排序，需要先加上 **排序代价 (Sort Cost)**。

------

#### **(5) 哈希连接 (Hash-Join)**

- **适用条件**: 等值连接 (Equi-join)。
- **算法逻辑**:
	1. **分区 (Partitioning)**: 用哈希函数 $h$ 把 $r$ 和 $s$ 划分到 $n$ 个桶中 ($r_0,..,r_{n-1}$ 和 $s_0,..,s_{n-1}$)。
	2. **连接 (Build & Probe)**: 读入 $s_i$ 建立内存哈希表，读入 $r_i$ 进行探测匹配。
- **代价计算 (无递归分区)**:
	- **Block Transfers**: $3(b_r + b_s)$
		- *(解释：1. 读入全表进行分区 ($b_r+b_s$)。2. 写出分区到磁盘 ($b_r+b_s$)。3. 连接时再读入分区 ($b_r+b_s$)。共 3 倍)*
	- **Seeks**: $2(\lceil b_r/b_b \rceil + \lceil b_s/b_b \rceil) + 2n_h$
		- *(解释：分区和连接阶段都需要 Seek，稍微复杂，通常考 Transfers 较多)*
- **特殊情况**: 如果整个内层关系 $s$ 能装入内存，则不需要分区，代价为 $b_r + b_s$。

------

### **💡 考试做题技巧 (Exam Tips)**

1. **公式记忆口诀**:
	- **Nested-Loop**: **$n_r \times b_s$** (行乘块，最慢)。
	- **Block Nested**: **$b_r \times b_s$** (块乘块，快一点)。
	- **Index Nested**: **$n_r \times c$** (行乘树高，大题重点)。
	- **Merge**: **$b_r + b_s$** (加法，需排序)。
	- **Hash**: **$3(b_r + b_s)$** (3倍加法)。
2. **大题陷阱**:
	- 计算 **Index Join** 时，不要忘了加上读取外层表 $r$ 本身的 $b_r$ 开销。
	- 题目如果没给树高 $h$，可能需要你根据 $Log_{\text{fanout}}(\text{Records})$ 先算出来（参考第 11 章复习内容）。
3. **Transfer vs Seek**:
	- Transfer 关注读了多少数据量。
	- Seek 关注磁头跳了多少次（通常 Nested Loop 的 Seek 次数多到离谱）。

### **5. 表达式的计算 (Evaluation of Expressions)**

当我们有一个包含多个运算的关系代数表达式（例如：先选择，再连接，最后投影）时，系统有两种主要的执行策略：**物化 (Materialization)** 和 **流水线 (Pipelining)** 1。

#### **(1) 物化 (Materialization) —— ⭐ 稳健但慢**

- **核心思想**:
	- 像“做菜”一样，切好菜放在盘子里，再炒，炒好盛出来，再端上桌。一步一步来。
	- **一次计算一个运算**。从最低层开始计算，将中间结果 **存储 (Store/Materialize)** 到磁盘上的 **临时关系 (Temporary Relations)** 中，然后再读入这些临时关系来计算下一层 2。
- **代价分析**:
	- **缺点**: 必须要等待上一步完全算完并写入磁盘，下一步才能开始。
	- **额外开销**: 我们之前的代价公式忽略了写磁盘的开销，但在物化策略中，**写入临时文件和再次读出的代价** 必须被计入，这通常很高 3。
	- **双缓冲 (Double Buffering)**: 一种优化手段。用两个缓冲块，一个在写磁盘时，CPU 可以处理另一个，减少等待 4。

#### **(2) 流水线 (Pipelining) —— ⭐ 高效但有局限**

- **核心思想**:
	- 像“工厂流水线”一样。
	- **同时计算多个运算**。一个运算产生一个元组 (Tuple)，立刻 **传递 (Pass)** 给上一层运算使用，**不存储到磁盘** 5。
- **优点**:
	- 不需要创建临时关系，省去了大量的磁盘 I/O (写和读)。
	- 第一条结果能很快输出来 (Response time 短)。
- **局限性**6:
	- 并不是所有算法都能流水线化。
	- **阻塞操作 (Blocking Operations)**: 例如 **排序 (Sort)** 或 **哈希连接 (Hash-Join)**。你必须看完所有输入才能排好序输出第一个结果，这种操作会打断流水线。

------

#### **(3) 流水线的实现 (Implementation of Pipelining) —— ⭐ 考试重点**

流水线主要有两种实现模式：

A. 需求驱动 (Demand Driven) / 惰性求值 (Lazy Evaluation) 7

- **机制**: 上层操作不断向下层操作 **“要” (Request)** 数据。
	- 系统向顶层操作请求“下一条元组”。
	- 顶层操作就向它的子操作请求“下一条”，一直传到底层。
- **实现方式**: **迭代器 (Iterator)** 8。
	- 每个操作节点都实现三个函数：
		1. **`open()`**: 初始化状态（如定位文件指针）。
		2. **`next()`**: 尽力算出并返回 **下一条** 元组。
		3. **`close()`**: 清理资源。
	- *考点*: 这种模式最常用，因为它很容易通过函数调用实现。

B. 生产者驱动 (Producer Driven) / 急切求值 (Eager Pipelining) 9

- **机制**: 底层操作不等上层要，只要自己算出来了，就主动 **“塞”** 给上层。
- **缓冲区**: 两个操作之间需要一个 **缓冲区 (Buffer)**。子节点往里写，父节点从里面读。如果缓冲区满了，子节点就得停下来等。

------

### **📚 完整考试实例 (Example)**

**(参考 Slides 56-59 的例子)**

**查询语句**:

```
SELECT name
FROM instructor NATURAL JOIN department
WHERE building = 'Watson';
```

**关系代数树**: $\Pi_{name} ( \sigma_{building='Watson'}(department) \bowtie instructor )$

**策略 A: 物化 (Materialization)** 10101010

1. **Step 1**: 扫描 `department` 表，找到 `building='Watson'` 的记录。
	- 将结果 **写入磁盘**，存为**临时表** `Temp1`。
2. **Step 2**: 读取 `Temp1` 和 `instructor` 表，执行 **Join**。
	- 将 Join 的结果 **写入磁盘**，存为临时表 `Temp2`。
3. **Step 3**: 读取 `Temp2`，执行 **Projection** (选出 name)。
	- 输出最终结果。

**策略 B: 流水线 (Pipelining)** 11

1. **Selection 节点**: 一旦在 `department` 中找到一条 `Watson` 的记录，**立刻** 把它传给上面的 Join 节点。（**不写盘**）
2. **Join 节点**: 收到一条记录，立刻去 `instructor` 里找匹配，如果匹配成功，**立刻** 传给上面的 Projection **节点**。
3. **Projection 节点**: 收到结果，**立刻输出**给用户。

- **对比**: 策略 B 没有 `Temp1` 和 `Temp2` 的**磁盘读写**，速度快得多。

------

### **💡 考试做题总结 (Exam Tips)**

1. **Iterator (迭代器) 是什么？**
	- 如果是简答题问“如何实现 Demand-driven pipelining?”，关键词是 **Iterator** 以及它的三个操作：`open()`, `next()`, `close()`。
2. **为什么不能总是流水线？**
	- 因为有些操作是 **Blocking (阻塞)** 的，比如 **Sort** (必须读完所有数据才能输出第一个排序结果) 或 **Hash-Join** (必须读完 Build relation 才能开始探测)。
	- *例外*: **Double-pipelined join (双流水线连接)** 是一种特殊的 Hash Join 变体，试图解决这个问题 12，但通常 Sort 是绝对的阻塞。
3. **代价区别**:
	- Materialization Cost = Sum of operations + **Cost of writing/reading temporary relations**.
	- Pipelining Cost = Sum of operations (无临时读写)。

### **💡 针对 2025 考试的复习建议 (基于真题提示)**

1. **死磕 Indexed Nested-Loop Join 的计算**：
	- 根据讲义 Slides 35-40 的例子：
	- 已知：表 Student ($r$) 和 Takes ($s$)。$s$ 上有 B+ 树索引。
	- 计算步骤：
		1. **算 B+ 树高度**: $Fanout^h \ge Records$ (例如 $20^h \ge 5000 \Rightarrow h=3$) 22。
		2. **算 $r$ 的代价**: 读取 $b_r$ 个**块**。
		3. **算 $s$ 的代价**: 对于 $r$ 的**每一条记录 ($n_r$)**，都要**查一次索引**。
		4. **单次索引查找**: 树高 $h$ (读索引块) + 1 (**读数据块**)。
		5. **总 Transfer**: $b_r + n_r \times (h + 1)$。
	- *一定要会算这个！真题预测明确提到了这个点。*
2. **区分不同 Join 的适用场景**:
	- **Merge-Join**: 适用于**已排序的数据。**
	- **Hash-Join**: 适用于**等值连接**，通常比 **Nested-loop 快。**
	- **Nested-Loop**: **万能，但慢**。
3. **主要关注 Transfer 和 Seek 的数量**:
	- 不需要死记硬背复杂的 $t_S, t_T$ 毫秒数，而是要记住 **次数公式**（如 $b_r + n_r \times b_s$）。

# ch13

### **1. 基本概念 (Introduction)**

#### **(1) 查询优化的核心逻辑 (The Core Logic)**

对于同一个 SQL 查询，系统可以生成多个 **等价 (Equivalent)** 的关系代数表达式，而每个表达式又可以用多种不同的算法（如 Index Scan vs File Scan）来执行。

- **差异**: 不同执行方式的 **代价 (Cost)** 可能相差几个数量级（例如：秒级 vs 小时级）。
- **目标**: 查询优化器 (Query Optimizer) 的责任是在所有可能的计划中，找到一个 **估算代价最低 (Lowest Estimated Cost)** 的执行计划。

#### **(2) 等价表达式 (Equivalent Expressions) —— ⭐ 基础概念**

- **定义**: 如果两个关系代数表达式对于 **任何 (Any)** 合法的数据库实例，都能产生 **相同的元组集 (Same set of tuples)**，则称这两个表达式是等价的。
	- *注意*: 元组的 **顺序 (Order)** 无关紧要，只要内容一样就行。

【📝 完整考试实例：什么样的表达式是等价的？】

假设我们要查询“Music 系的所有老师的工资”。

- 表达式 A (先连接后选择):

	$$\sigma_{dept\_name="Music"}(instructor \bowtie teaches)$$

	- *过程*: 先把成千上万的老师和课程连接起来（产生巨大中间表），再从这个大表里挑出 Music 系的。
	- *缺点*: 中间结果太大，慢。

- 表达式 B (**先选择后连接**):

	$$(\sigma_{dept\_name="Music"}(instructor)) \bowtie teaches$$

	- *过程*: 先从老师表里挑出 Music 系的几个人（数据量骤减），再拿这几个人去连接课程表。
	- *优点*: 参与连接的数据量**小，快**。

- **结论**: 表达式 A 和 B 是 **等价 (Equivalent)** 的，因为它们的结果一模一样，但 B 的效率通常远高于 A。优化器会自动把 A 转换成 B。

#### **(3) 执行计划 (Evaluation Plan) —— ⭐ 术语辨析**

- **定义**: 仅仅有关系代数表达式还不够，我们还需要指定 **具体的算法**。这种带有算法注解的树结构称为 **执行计划**。
- **包含内容**:
	1. **运算顺序**: **先算谁，后算谁**。
	2. **具体算法**: Join 是用 Hash Join 还是 Merge Join？Selection 是用 Index Scan 还是 Linear Scan？
	3. **协调方式**: 是用**流水线 (Pipeline) 还是物化 (Materialization)**？

**【💡 考试小题点】**

- **Relational Algebra Expression**: 只描述“算什么” (What)。
- **Evaluation Plan**: 描述“怎么算” (How)。
- *判断题*: "Query optimization works by finding the plan with the actual lowest cost." (错误。优化器是基于 **Estimated Cost (估算代价)**，因为真的跑一遍才知道实际代价，不可能每个都试一遍)。

------

### **💡 总结**

- **等价性**: 结果集相同（忽略顺序）。
- **优化目标**: 寻找 **Estimated Cost** 最低的 **Evaluation Plan**。

### **2. 关系表达式的转换 (Transformation of Relational Expressions)**

**核心思想**：利用 **等价规则 (Equivalence Rules)** 将一个查询表达式转换为另一个执行代价更低的等价表达式。

- **等价 (Equivalent)**：指两个表达式对数据库的任何实例都产生 **相同的元组集** (忽略顺序)。

以下是你必须掌握的 **6条核心规则**：

#### **规则 1：级联选择 (Cascade of $\sigma$)**

- **公式**: $\sigma_{\theta_1 \land \theta_2}(E) = \sigma_{\theta_1}(\sigma_{\theta_2}(E))$
- **含义**: 一个**大的“AND”选择条件，可以拆分成两个小的选择操作串联起来。**
- **考试用途**: 拆分后，可以将其中一个简单的条件先执行（比如下推），从而减小中间结果。
- **【📝 完整例子】**:
	- 查询：`SELECT * FROM instructor WHERE salary > 50000 AND dept_name = 'Physics'`
	- 原表达式: $\sigma_{salary > 50000 \land dept\_name = 'Physics'}(instructor)$
	- 转换后: $\sigma_{salary > 50000}(\sigma_{dept\_name = 'Physics'}(instructor))$

#### **规则 2：选择交换律 (Commutativity of $\sigma$)**

- **公式**: $\sigma_{\theta_1}(\sigma_{\theta_2}(E)) = \sigma_{\theta_2}(\sigma_{\theta_1}(E))$
- **含义**: 先筛选 $\theta_1$ 还是先筛选 $\theta_2$，**结果一样。**
- **考试用途**: 总是先执行那个能 **过滤掉更多数据 (Most Restrictive)** 的选择操作。

#### **规则 3：投影级联 (Sequence of $\Pi$)**

- **公式**: $\Pi_{L_1}(\Pi_{L_2}(...(\Pi_{L_n}(E))...)) = \Pi_{L_1}(E)$
- **条件**: $L_1 \subseteq L_2 \subseteq ... \subseteq L_n$
- **含义**: 一连串的投影操作，只有 **最后做 (最外层)** 的那个有效，**中间的都没用（只要属性都在就行）。**
- **【📝 完整例子】**:
	- 如果先投影出 `(ID, name, dept_name)`，再从中投影出 `(name)`。
	- 等价于：直接从原表中投影 `(name)`。

#### **规则 4：连接交换律 (Commutativity of Join) —— ⭐ 重点**

- **公式**: $E_1 \bowtie E_2 = E_2 \bowtie E_1$
- **含义**: 谁做外层循环 (Outer)、谁做内层循环 (Inner) 不影响结果内容，但**严重影响性能** (还记得 Chapter 12 的 Loop Join 吗？**小表做外层！**)。
- **注意**: 这是一个非常重要的优化点，优化器**会尝试交换顺序以减少 I/O**。

#### **规则 5：连接结合律 (Associativity of Join) —— ⭐ 重点**

- **公式**: $(E_1 \bowtie E_2) \bowtie E_3 = E_1 \bowtie (E_2 \bowtie E_3)$
- **含义**: 连接的先后顺序可以改变。
- **【📝 完整例子】**:
	- $(r \bowtie s) \bowtie t$：先算 $r$ 和 $s$ 的连接（产生中间表），再和 $t$ 连。
	- $r \bowtie (s \bowtie t)$：先算 $s$ 和 $t$ 的连接（产生中间表），再和 $r$ 连。
	- **优化策略**: 如果 $s \bowtie t$ 产生的**结果集很小**，而 $r \bowtie s$ 的结果集很大，那么显然应该 **先算 $(s \bowtie t)$**。这就是优化器利用结合律做的事情。

#### **规则 6：选择对连接的分配律 (Distribution of $\sigma$ over $\bowtie$) —— ⭐⭐⭐ 绝对大题考点**

这是实现 **“选择下推 (Pushing Selections Down)”** 的理论基础，必须背过。

- **情况 A (条件只涉及一边)**:
	- **公式**: $\sigma_{\theta_0}(E_1 \bowtie E_2) = (\sigma_{\theta_0}(E_1)) \bowtie E_2$
	- **条件**: 选择条件 $\theta_0$ **只涉及** $E_1$ 的属性。
	- **含义**: 既然条件只跟 $E_1$ 有关，那**就别等连接完再筛选了**，**先在 $E_1$ 上筛选**，数据量变小了再去连接，效率倍增！
- **情况 B (条件涉及两边)**:
	- **公式**: $\sigma_{\theta_1 \land \theta_2}(E_1 \bowtie E_2) = (\sigma_{\theta_1}(E_1)) \bowtie (\sigma_{\theta_2}(E_2))$
	- **条件**: $\theta_1$ 只涉及 $E_1$，$\theta_2$ 只涉及 $E_2$。
	- **含义**: 把**复杂的 AND 条件拆开（利用规则1）**，然后分别推到两边先执行。
- **【📝 完整大题实例：选择下推】**
	- **原始查询**: `SELECT * FROM instructor, teaches WHERE instructor.ID = teaches.ID AND instructor.dept_name = 'Music'`
	- **原始代数**: $\sigma_{instructor.dept\_name = 'Music'}(instructor \bowtie teaches)$
	- **优化步骤 (Transformation)**:
		1. 发现选择条件 `dept_name = 'Music'` 只涉及 `instructor` 表。
		2. 应用 **分配律 (Rule 6)**。
		3. **优化后**: $(\sigma_{instructor.dept\_name = 'Music'}(instructor)) \bowtie teaches$
	- **效果**: 假设有 1000 个老师，**只有 5 个是 Music 系的。**
		- 原始：1000 个老师 $\bowtie$ 10000 条课程记录 = 巨大中间结果 $\to$ 筛选。
		- 优化后：**先筛选出 5 个老师** $\to$ 用这 5 条记录去 $\bowtie$ 课程表 = 极快。

------

### **💡 考试做题技巧**

1. **看到 "Pushing selections down"**: 马上想到 **规则 6 (分配律)**。这是 Heuristic Optimization (启发式优化) 的核心第一步。
2. **看到 Join 顺序变化**: 马上想到 **规则 4 (交换律)** 和 **规则 5 (结合律)**。
3. **判断等价性**:
	- $\sigma_{\theta}(E_1 \bowtie E_2) \stackrel{?}{=} \sigma_{\theta}(E_1) \bowtie \sigma_{\theta}(E_2)$
	- **注意**: 如果 **$\theta$ 涉及两个表的属性**（比如 `E1.salary > E2.salary`），则 **不能** 下推到两边，**只能在连接后做**。所以这个等式通常是 **False**（**除非 $\theta$ 仅仅是常数比较**）。
4. **做大题时**: 如果题目让你画“Optimized Logical Query Plan”，第一步永远是 **把 $\sigma$ 沿着树往下压，压到离 Base Table 最近的地方**。



### **⚠️ 重点复习建议**

1. **结果集估算**: 一定要会算 $n_r / V(A,r)$ 以及 Join **的 size 公式**。这是本章最硬的计算点。
2. **左深树**: 记住它的定义（右边必须是 Base Table）和优点（**适合 Pipeline**）。
3. **启发式规则**: 记住**“早选择、早投影”**六字真言。

收到。我们严格按照 **《ch13.pdf》** (Slides 25-26) 的内容，为你详细总结 **“3. 统计信息 (Statistical Information)”**。

这一部分是下一节 **“结果集大小估算 (Size Estimation)”** 的**地基**。考试中，题目会直接给出这些符号对应的值（例如 $n_r=10000, V(A,r)=50$），你必须一眼看懂它们代表什么，否则后面的公式全都会代错数。

------

### **3. 统计信息 (Statistical Information)**

为了准确估算各种查询计划的代价，数据库目录 (System Catalog) 中存储了关于表（关系）和索引的统计数据。

#### **(1) 核心符号定义 (Key Notations) —— ⭐ 必背 (计算题的已知条件)**

在做计算题时，请务必分清以下 5 个符号：

1. **$n_r$**: 关系 $r$ 中的 **元组总数 (Number of tuples)**。
	- *通俗理解*: 表里有多少行数据。
2. **$b_r$**: **关系 $r$** 占用的 **磁盘块数 (Number of blocks)**。
	- *通俗理解*: 这个表在硬盘上占了多少个格子。
3. **$l_r$**: 关系 $r$ 中每个元组的 **平均长度 (Average size of a tuple)**。
	- *单位*: 字节 (Bytes)。
4. **$f_r$**: **块因子 (Blocking factor)**。
	- *定义*: 一个磁盘块 (Block) **能装下多少个元组。**
	- *关系公式*: $f_r = \lfloor \frac{\text{Block Size}}{l_r} \rfloor$。
	- *推导*: 也就是 $b_r = \lceil \frac{n_r}{f_r} \rceil$ (**总行数除以每块能装的行数**)。
5. **$V(A, r)$**: 属性 $A$ 在关系 $r$ 中的 **不同值个数 (Number of distinct values)**。
	- *通俗理解*: 去重后**还剩多少个值。**
	- *重要考点*: 如果 **$V(A, r) = n_r$**，说明属性 $A$ 的每个值都是唯一的，因此 $A$ 是关系 $r$ 的 **码 (Key)**。

------

#### **(2) 📚 完整考试实例 (Comprehensive Example)**

为了让你在考试中能迅速对应上这些参数，我们来看一个具体的 **Student** 表例子。

【题目背景】

假设有一个 Student 表，包含以下信息：

- 总共有 **10,000** 名学生。
- 每条学生记录（包含 ID, Name, Dept_name 等）平均占用 **100 Bytes**。
- 数据库的磁盘块大小 (Block Size) 为 **4000 Bytes**。
- 学校共有 **50** 个不同的系 (Dept_name)。
- 性别 (Gender) 只有 'M' 和 'F' **2** 种。

【参数提取】

考试时你需要能从上面的文字中提取出以下符号值：

1. **$n_{student}$ (总行数)**:
	- **10,000**。
2. **$l_{student}$ (每行大小)**:
	- **100 Bytes**。
3. **$f_{student}$ (块因子)**:
	- 计算: $4000 / 100 = \mathbf{40}$。
	- (意味**着一个块能存 40 个学生**)。
4. **$b_{student}$ (总块数)**:
	- 计算: $10,000 / 40 = \mathbf{250}$ 个块。
	- *(做 IO 代价计算题时，全表扫描就是**读这 250 个块)***。
5. **$V(ID, student)$**:
	- 因为 **ID 是主键**，每个学生都不一样，所以 $= \mathbf{10,000}$ ($= n_r$)。
6. **$V(dept\_name, student)$**:
	- 系名有 50 个不同的，所以 $= \mathbf{50}$。
	- *(这个数值在估算 `SELECT \* FROM student WHERE dept_name = 'CS'` 的结果行数时至关重要，意味着平均每个系有 $10000/50 = 200$ 人)*。
7. **$V(gender, student)$**:
	- 性别只有 2 个，所以 $= \mathbf{2}$。

------

### **💡 考试做题技巧**

- **看到 $V(A, r)$**: 马上反应这是 **“不同值的个数”**。
	- 如果题目问：`SELECT * FROM r WHERE A = 'value'` 大约返回多少行？
	- **公式**: $n_r / V(A, r)$。 (假设均匀分布)
- **判断 Key**: 如果题目给的数据显示 $V(A, r) == n_r$，那么 $A$ 就是 Key。
- **单位统一**: 计算 $b_r$ 时，确保 $l_r$ 和 Block Size 单位一致（通常都是 Bytes）。

### **4. 结果集大小估算 (Estimation of Statistics)**

**核心符号复习**:

- **$n_r$**: 关系 $r$ 的总元组数。
- **$V(A, r)$**: 属性 $A$ 在关系 $r$ 中的不同值个数 (Distinct values)。

------

#### **(1) 选择运算的大小估算 (Size of Selection) —— 小题/基础计算**

我们用 **$E$** 表示选择操作的结果行数估算值。

**1. 等值查询 (Equality Selection): $\sigma_{A=v}(r)$**

- **场景**: `SELECT * FROM r WHERE A = 'value'`

- 公式:

	$$E = \frac{n_r}{V(A, r)}$$

- **假设**: 属性 $A$ 的值在表中是 **均匀分布 (Uniformly Distributed)** 的。每个值出现的概率都是 $1/V(A,r)$。

**2. 比较查询 (Comparison Selection): $\sigma_{A \le v}(r)$**

- **场景**: `SELECT * FROM r WHERE A <= 100`

- 公式 (如果知道最大最小值):

	$$E = n_r \times \frac{v - \min(A, r)}{\max(A, r) - \min(A, r)}$$

	(原理: 线性插值，看 $v$ 在整个取值范围里占了多大比例)

- 公式 (如果不知道最大最小值):

	$$E = \frac{n_r}{2}$$

	(原理: 简单粗暴地假设能选出一半的数据)

**3. 复合选择 (Complex Selection)**

- **合取 (Conjunction / AND)**: $\sigma_{\theta_1 \land \theta_2}(r)$
	- **步骤**: 先算出 $\theta_1$ 的选择率 $P_1$ 和 $\theta_2$ 的选择率 $P_2$。
	- **公式**: $E = n_r \times P_1 \times P_2$。
- **析取 (Disjunction / OR)**: $\sigma_{\theta_1 \lor \theta_2}(r)$
	- **公式**: $E = n_r \times (1 - (1 - P_1) \times (1 - P_2))$。
- **否定 (Negation / NOT)**: $\sigma_{\neg \theta}(r)$
	- **公式**: $E = n_r - \text{Size}(\sigma_{\theta}(r))$。

------

#### **(2) 连接运算的大小估算 (Size of Join) —— ⭐⭐⭐ 大题必考**

我们要估算 **$r \bowtie s$** 的结果行数。假设连接属性是 **$r.A$** 和 **$s.A$**。

**情况 1: 笛卡尔积 (Cartesian Product)**

- **场景**: 两个表没有公共属性，或者没有指定连接条件。
- **公式**: $E = n_r \times n_s$。

**情况 2: 基于码的连接 (Join on Key)**

- **场景 A**: 连接属性 $A$ 是 **$r$ 的码 (Key of r)**。
	- 也就是 $r$ 中的 $A$ 是唯一的。
	- **公式**: $E \le n_s$。
	- *解释*: $s$ 中的每一条记录，在 $r$ 中最多只能找到 1 个匹配对象。
- **场景 B**: 连接属性 $A$ 是 **$s$ 的码 (Key of s)**。
	- **公式**: $E \le n_r$。
	- *解释*: $r$ 中的每一条记录，在 $s$ 中最多只能找到 1 个匹配对象。
	- *注意*: 这是外键连接的典型情况，结果集大小通常就是“外键表”的大小。

**情况 3: 通用连接 (General Case) —— 最常考**

- **场景**: 连接属性 $A$ 既不是 $r$ 的码，也不是 $s$ 的码（即 $A$ 在两边都有重复值）。

- 公式 (必须背诵):

	$$E = \min \left( \frac{n_r \times n_s}{V(A, r)}, \frac{n_r \times n_s}{V(A, s)} \right)$$

- **记忆技巧**:

	- 分子总是 $n_r \times n_s$。
	- 分母取 **$V(A)$ 较大的那个**。
	- *原理*: 实际上是假设较小的那个值集合包含于较大的值集合中。分母越大，结果越小（限制越强）。

------

#### **(3) 其他运算估算**

- **投影 (Projection)**: $\Pi_A(r)$
	- 估算值 $E = V(A, r)$。
- **聚合 (Aggregation)**: $_G \gamma _A(r)$
	- 如果是按 $G$ 分组，估算值 $E = V(G, r)$。（有多少个组，就有多少行结果）。

------

### **📚 完整考试计算实例 (Comprehensive Calculation Example)**

【题目背景】

假设有两个关系（表）：

- **Student ($s$)**: 学生表
	- $n_s = 1,000$ (1000名学生)
	- $V(dept\_name, s) = 25$ (共有25个不同的系)
	- $V(ID, s) = 1,000$ (ID是主键)
- **Takes ($t$)**: 选课表
	- $n_t = 10,000$ (10000条选课记录)
	- $V(dept\_name, t) = 20$ (选课记录中涉及20个不同的系)
	- $V(ID, t) = 1,000$ (共有1000个不同的学生选了课)

【问题 1: 选择估算】

计算查询 SELECT * FROM Student WHERE dept_name = 'Music' 的估算行数。

- **类型**: 等值查询。

- 计算:

	

	$$E = \frac{n_s}{V(dept\_name, s)} = \frac{1,000}{25} = \mathbf{40}$$

- **答案**: 估算有 40 名 Music 系的学生。

【问题 2: 连接估算 (基于码)】

计算 Student NATURAL JOIN Takes 的估算行数。

- **连接属性**: $ID$。

- **分析**: $ID$ 是 `Student` 的 **Key** ($V(ID, s) = n_s$)。

- 计算: 根据规则，如果连接属性是某一方的 Key，结果集大小 $\le$ 另一方的大小。

	

	$$E = n_t = \mathbf{10,000}$$

- **答案**: 10,000 行。 (每条选课记录都能对应上 1 个学生)。

【问题 3: 连接估算 (通用情况)】

计算 Student 和 Takes 在属性 dept_name 上连接的估算行数 (虽然逻辑上不通，仅作为计算练习)。

- **连接属性**: $dept\_name$。

- **分析**: $dept\_name$ 在 `Student` ($V=25$) 和 `Takes` ($V=20$) 中都不是 Key，都有重复。

- 计算: 套用通用公式。

	

	$$E = \min \left( \frac{n_s \times n_t}{V(dept\_name, s)}, \frac{n_s \times n_t}{V(dept\_name, t)} \right)$$

	$$E = \min \left( \frac{1,000 \times 10,000}{25}, \frac{1,000 \times 10,000}{20} \right)$$

	$$E = \min \left( 400,000, 500,000 \right)$$

	$$E = \mathbf{400,000}$$

- **答案**: 400,000 行。

- **直观理解**: 为什么要除以较大的 $V$ (25)？因为 $V$ 越大，匹配的概率越低（数据越稀疏），限制条件越强，所以结果集越小。

------

### **💡 考试做题总结**

1. **Select 等值**: 直接除以 $V$。
2. **Join 有主键**: 结果行数 = 外键表行数 (非主键表行数)。
3. **Join 无主键 (多对多)**: **分子乘积，分母取大 $V$**。
4. **注意**: 所有的估算都是基于“均匀分布”和“值包含”的假设，考试时直接代公式即可，不用纠结特例。

# ch14

### **1. 事务概念 (Transaction Concept)**

#### **(1) 定义 (Definition)**

- **事务 (Transaction)**：是访问并可能更新各种数据项的程序执行单元 (Unit of program execution)。
- **定界**: 事务通常由 `begin transaction` 开始，以 `commit` 或 `rollback` 结束。

#### **(2) ACID 特性 (ACID Properties) —— ⭐ 必背核心考点**

为了保证数据的完整性，数据库系统必须维护事务的四个特性。考试常考名词解释或通过例子判断破坏了哪个特性。

【📚 经典案例：转账 (Fund Transfer)】

事务 $T$：从账户 A 转 50 美元到账户 B。

1. `read(A)`
2. `A := A - 50`
3. `write(A)`
4. `read(B)`
5. `B := B + 50`
6. `write(B)`

**四个特性的具体含义**：

1. **原子性 (Atomicity)**:
	- **定义**: 事务的所有**操作要么全部正确反映在数据库中，要么全部不反映 (All or nothing)**。
	- *例子*: 如果事务在第 3 步 `write(A)` 后崩溃了，第 6 步 `write(B)` 没执行。**原子性保证系统会 回滚 (Rollback) 之前的操作**，A 的 50 块钱不会凭空消失。
2. **一致性 (Consistency)**:
	- **定义**: **隔离执行**事务时（没有其他干扰），必须保持**数据库的一致性**。
	- *例子*: 无论转账成功还是失败，**A + B 的总金额** 必须保持不变。这是由写程序的人（程序员）负责逻辑正确的。
3. **隔离性 (Isolation)**:
	- **定义**: 尽管多个事务**可能并发执行**，但每**个事务都感觉不到**系统中有其他事务在执行。即 $T_i$ 感觉不到 $T_j$ 的存在。
	- *例子*: 在 $T$ 执行到一半（改了 A 但还没改 B）时，另一个查询余额的事务进来，**不能** 让它看到“A 少了钱但 B 还没加钱”这个不一致的状态。
4. **持久性 (Durability)**:
	- **定义**: 事务**一旦成功完成（提交）**，它对数据库的**改变必须是永久的，即使系统之后发生故障。**
	- *例子*: 一旦第 6 步执行完并由系统确认提交，即使下一秒断电，重启后 A 和 B 的余额也必须是转账后的结果。

------

### **2. 事务状态 (Transaction State)**

#### **(1) 状态流转图 (State Diagram) —— ⭐ 必须掌握流转方向**

事务在执行过程中会处于不同的状态。考试可能会给你一个状态图让你填空，或者问某个状态能去哪。

**5 个状态及定义**：

1. **Active (活动)**: 初始状态，事务**正在执行中。**
2. **Partially Committed (部分提交)**: 当 **最后一条语句** 被执行后。
	- *注意*: 这时还未真正把数据写死到磁盘，只是内存里做完了。
3. **Failed (失败)**: 发现正常执行**无法继续后（可能是硬件错误、逻辑错误）。**
4. **Aborted (中止)**: 事务**回滚 (Rolled back) 并恢复数据库到开始前的状态后。**
	- 到达此状态后，系统可以选择：
		- **Restart (重启)**: 如果是由于**软硬件**错误导致的。
		- **Kill (杀死)**: 如果是**事务内部逻辑错误（如死循环）**。
5. **Committed (提交)**: 成功**完成后（数据已持久化）。**

**【🔄 关键流转路径 (必记)】**：

- **成功路径**: Active $\rightarrow$ Partially Committed $\rightarrow$ Committed。
- **失败路径 1**: Active $\rightarrow$ Failed $\rightarrow$ Aborted。
- **失败路径 2 (重点)**: Partially Committed $\rightarrow$ Failed $\rightarrow$ Aborted。
	- *解释*: 即使语句都跑完了（部分提交），如果此时往磁盘写数据时发生了硬件故障，依然会变成 Failed 状态。

------

### **3. 并发执行 (Concurrent Executions)**

这一部分主要是为后面的“调度”和“可串行化”做铺垫。

#### **(1) 为什么要并发？**

- **提高吞吐量 (Improved throughput)**: 单位时间内完成**更多事务。**
- **提高资源利用率 (Improved resource utilization)**: CPU 和磁盘 I/O 可**以并行工作。**
- **减少等待时间 (Reduced waiting time)**: 短事务**不用等长事务跑完再跑。**

#### **(2) 调度 (Schedule) —— 基本概念**

- **定义**: 指令序列，指定了并发事务指令在系统中的**执行顺序。**
- **约束**: 必须保留每个单独事务内部指令的顺序（即 $T_1$ 内部先 Read A 后 Write A，在调度里也必须是这个先后顺序）。

#### **(3) 串行调度 (Serial Schedule)**

- **定义**: 一个事务完全执行完，才执行下一个事务。指令**没有交错。**
- **特点**: 只要每个事务**本身是正确的，**串行调度产生的结果 **一定** 是正确的（Consistent）。但**效率极低，无法利用并发优势。**

------

### **💡 考试做题总结**

- **ACID**: 记住 **A** (回滚/全做全不做), **C** (总和不变/逻辑正确), **I** (互不干扰), **D** (断电不丢)。
- **状态图**: 特别注意 **Partially Committed** 后面可以接 **Committed** 也可以接 **Failed**。
- **术语**: 考试时如果问 "sequence of instructions...", 答案通常是 **Schedule**。

### **4. 可串行化 (Serializability)**

在并发执行中，我们只关心一种调度：**冲突可串行化 (Conflict Serializability)**。如果一个并发调度通过交换“不冲突”的指令顺序，能变成一个串行调度，那它就是正确的。

#### **(1) 冲突指令 (Conflicting Instructions) —— 基础判据**

我们需要判断两条指令 I (来自事务 Ti) 和 J (来自事务 Tj) 是否冲突。 **冲突条件 (必背)**: 当且仅当以下 **两个条件同时满足** 时，指令 I 和 J 冲突：

1. 它们操作 **同一个数据项 (Same Item)** Q。
2. 其中 **至少有一个是 Write 操作**。

**结论**:

- `read(Q)` vs `read(Q)` → **不冲突** (No Conflict)。
- `read(Q)` vs `write(Q)` → **冲突** (Conflict)。
- `write(Q)` vs `read(Q)` → **冲突** (Conflict)。
- `write(Q)` vs `write(Q)` → **冲突** (Conflict)。

------

#### **(2) 冲突可串行化判别：优先图 (Precedence Graph) —— ⭐⭐⭐⭐⭐ 大题核心**

考试中给定一个调度，问你是否是“冲突可串行化”的？ **解题唯一方法**：画 **优先图 (Precedence Graph)**。

**作图规则**:

1. **节点 (Nodes)**: 每个事务 T **是一个节点。**
2. **边 (Edges)**: 画**一条从 Ti 指向 Tj 的边 (Ti→Tj)**，如果满足以下 **冲突顺序**：
	- Ti **读/写 Q**，**之后** Tj 读/写 **同一个** Q。
	- 且两者中**至少有一个是 `write`。**
	- *(口诀：谁**先操作冲突数据，谁就是箭头起点**)*

**判别结论**:

- **有环 (Cycle)** → **不可串行化 (Not Conflict Serializable)**。
- **无环 (Acyclic)** → **可串行化 (Conflict Serializable)**。
	- *附加*: 如果无环，可以通过 **拓扑排序 (Topological Sort)** 得到**等价的串行顺序。**

------

#### **📚 完整考试实例 (Precedence Graph Example)**

**【题目】**： 考虑以下调度 S（涉及事务 T1 和 T2），请判断它是否是冲突可串行化的？如果是，写出等价的串行顺序。

| T1         | T2         |
| ---------- | ---------- |
| `read(A)`  |            |
| `write(A)` |            |
|            | `read(A)`  |
|            | `write(A)` |
| `read(B)`  |            |
| `write(B)` |            |
|            | `read(B)`  |
|            | `write(B)` |

**【解题步骤】**:

1. **找冲突 (Find Conflicts)**: 我们在 T1 和 T2 之间寻找针**对同一数据的 Read-Write 或 Write-Write 冲突。**
	- **关于数据 A**:
		- T1 **先执行 `write(A)` (第2步)，然后 T2 执行 `read(A)` (第3步)。**
		- 冲突类型: **W-R 冲突。**
		- **画边**: **T1→T2**。
	- **关于数据 B**:
		- T1 先执行 `write(B)` (第6步)，然后 T2 执行 `read(B)` (第7步)。
		- 冲突类型: **W-R 冲突。**
		- **画边**: **T1→T2**。
2. **画图 (Draw Graph)**:
	- 节点: T1, T2。
	- 边: **只有从 T1 指向 T2 的边。**
3. **判断环 (Check Cycle)**:
	- 图中只有 T1→T2，**没有环**。

**【结论】**:

- 该调度 **是** **冲突可串行化的。**
- 等价的串行顺序是: **T1→T2**。

------

**【反例：有环的情况】** 如果调度改成这样：

1. T1: `read(A)`
2. T2: `write(A)`  → 冲突！T1 先读，**边 T1→T2**。
3. T1: `write(A)`  → 冲突！T2 先写，T1 后写，**边 T2→T1**。

- **结果**: 图中有 T1→T2 和 T2→T1，形成闭环。
- **结论**: **不可串行化**。

------

### **5. 可恢复性 (Recoverability)**

这部分主要解决“如果**一个事务失败了，其他读取了它数据的事务该怎么办**”的问题。

#### **(1) 可恢复调度 (Recoverable Schedule) —— ⭐ 概念辨析**

- **定义**: 对于每对事务 Ti 和 Tj，如果 T**j 读取了由 Ti 写入的数据（**即 Tj **读了 Ti 的脏数据**），那么 **Ti 的提交 (Commit) 操作必须在 Tj 的提交操作 之前。**
- **口语理解**: “我读了你的数据，你的数据**还没存盘（Commit）之前，我绝对不敢先存盘**。万一**你回滚了，我存的东西就是错的**。”
- **不可恢复带来的后果**: 如果 Tj 先提交了，结果 **Ti 后来回滚了**，Tj **已经提交的数据就变成了“基于错误数据的永久结果**”，这**违反了持久性**，数据库就乱了。

#### **(2) 无级联调度 (Cascadeless Schedule) —— ⭐ 进阶概念**

- **问题 (Cascading Rollback)**: 即使调度是可恢复的，如果 Ti 失败回滚，所有读取了 Ti 数据的 Tj 也**必须跟着回滚（因为读了脏数据）**。这叫 **级联回滚**。这**很浪费资源。**
- **定义**: 为了避免级联回滚，我们要求：对于每对事务 Ti 和 Tj，如果 Tj 读取了 Ti 写入的数据，那么 Ti 必须在 Tj **读取该数据之前** 就**已经提交。**
- **口语理解**: “我**只读你已经 Commit 的数据**。你没 Commit，**我就等着，绝不读。”**

------

#### **📚 完整考试实例 (Recoverability Example)**

**调度 A (不可恢复)**:

Plaintext

```
T8: write(A)
T9: read(A)    <-- 读了 T8 写的数据 (Dirty Read)
T9: commit     <-- T9 先提交了！(危险)
T8: abort      <-- T8 挂了，回滚。
```

- **分析**: T9 读了 T8 的数据，但 **T9 在 T8 之前提交。一旦 T8 回滚，T9 的提交就成了错误的。这是 Non-recoverable。**

**调度 B (可恢复，但有级联回滚)**:

Plaintext

```
T8: write(A)
T9: read(A)
T8: abort      <-- T8 回滚
T9: abort      <-- T9 被迫跟着回滚 (Cascading Rollback)，因为读了脏数据且 T8 没提交。
```

- **分析**: 这是 **Recoverable** 的（因为 **T9 还没提交，还有机会回滚**），但**不是 Cascadeless。**

**调度 C (无级联调度)**:

```
T8: write(A)
T8: commit     <-- T8 先提交
T9: read(A)    <-- T9 再读
```

- **分析**: T9 读的是**干净数据**。这是 **Cascadeless**。

------

### **💡 考试做题总结**

1. **大题做题技巧 (可串行化)**:
	- 看到调度表，立刻拿**出笔画圈圈（节点）**。
	- 逐行扫描：
		- 看到 `read(X)`: 往**上看有没有别的事务 `write(X)`？**如果有，画边。往**下看有没有别的事务 `write(X)`？如果有，画边。**
		- **看到 `write(X)`: 往**上看/往下看**有没有别的事务 `read(X)` 或 `write(X)`？**如果有，画边。
	- **注意**: 同一个事务内部的操作不画边。
	- **最后**: 找环。无环 = Serializable。
2. **概念题技巧 (可恢复性)**:
	- **Recoverable**: 关注 **Commit 顺序** (Twriter commit before Treader commit)。
	- **Cascadeless**: 关注 **Read 时机** (Twriter commit before Treader reads)。

### **1. 基于锁的协议 (Lock-Based Protocols)**

#### **(1) 锁的模式与兼容性 (Lock Modes & Compatibility) —— ⭐ 小题考点**

为了控制并发访问，事务在访问数据前必须先获得锁。

- **两种基本锁模式 (Lock Modes)**1:

	1. **共享锁 (Shared mode, denote by S)**:
		- 如果不止一个事务拿着 S 锁，它们可以**同时读**，但谁都**不能写**。
		- *用途*: 用于只读操作 (Read-only)。
	2. **排他锁 (Exclusive mode, denote by X)**:
		- 如果一个事务拿着 X 锁，其他任何事务都不能读也不能写该数据。
		- *用途*: 用于**读写**操作 (Read and Write)。

- 兼容性矩阵 (Compatibility Matrix) —— 必背:

	考试中常让你填空或者判断两个请求是否冲突。

	- **规则**: 只有 **S 和 S** 是兼容的 (True)。只要有一方是 **X**，就不兼容 (False)。

	| **\**             | **Shared (S)**   | **Exclusive (X)** |
	| ----------------- | ---------------- | ----------------- |
	| **Shared (S)**    | **True** (兼容)  | **False** (冲突)  |
	| **Exclusive (X)** | **False** (冲突) | **False** (冲突)  |

	- *理解*: 读读不冲突，读写冲突，写写冲突。

------

#### **(2) 两阶段封锁协议 (Two-Phase Locking Protocol, 2PL) —— ⭐⭐⭐ 核心考点**

这是保证**调度可串行化的最重要协议。**

- **协议定义**: 事务在执行过程中，**对锁的申请和释放分为两个截然不同的阶段**:
	1. **增长阶段 (Growing Phase)**:
		- 事务可以 **获得锁 (Obtain locks)**。
		- 事务 **不能释放 (Release)** 任何锁。
	2. **缩减阶段 (Shrinking Phase)**:
		- 事务可以 **释放锁 (Release locks)**。
		- 事务 **不能获得** 任何新锁。
- **封锁点 (Lock Point)**: 事务**获得最后一个锁的时间点**。**过了这个点，事务就开始释放锁了**。
- **重要性质 (Properties) —— 考试常考判断/简答** 4:
	1. **可串行化 (Serializability)**: **2PL 保证** **调度是冲突可串行化**的。
	2. **死锁 (Deadlock)**: **2PL 不保证** 不发生死锁。 (**可能会发生死锁**)
	3. **级联回滚 (Cascading Rollback)**: **普通 2PL 不保证** 无级联回滚。 (**可能会发生级联回滚**)

------

#### **(3) 2PL 的变体 (Variants of 2PL) —— ⭐⭐⭐ 大题辨析**

为了解决级联回滚问题，引入了更严格的变体。考试常让你区分 Strict 2PL 和 Rigorous 2PL。

1. **严格两阶段封锁 (Strict 2PL)** 5:
	- **规则**: 除了遵守 2PL 外，要求所有的 **排他锁 (Exclusive locks / X-locks)** **必须一直持有**，直到事务 **提交 (Commit)** 或 **中止 (Abort)** 后才能释放。
	- **优点**: 保证 **无级联回滚 (Cascadeless)**。因为在 $T_i$ **提交前**，别人**读不到 $T_i$ 写的数据**（**X锁没放**），所以**不用担心 $T_i$ 回滚牵连别人**。
	- *注意*: **S 锁可以提前释放。**
2. **强两阶段封锁 (Rigorous 2PL)**6:
	- **规则**: 所有的锁 (**S 锁和 X 锁**) 都必须持有到事务 **提交 (Commit)** 或 **中止 (Abort)**。
	- **优点**: 实现简单，**也是无级联的。**
	- *对比*: 比 Strict 2PL **更严**，**并发度可能略低**，但容易实现（事务结束时一把全放了）。

------

#### **(4) 锁转换 (Lock Conversions) —— 了解**

为了提高并发度，协议允许锁在特定阶段变身 

- **升级 (Upgrade)**: 从 **S-lock** 变成 **X-lock**。
	- *发生阶段*: 必须在 **增长阶段 (Growing Phase)**。
- **降级 (Downgrade)**: 从 **X-lock** 变成 **S-lock**。
	- *发生阶段*: 必须在 **缩减阶段 (Shrinking Phase)**。

------

### **📚 完整考试实例 (Comprehensive Example)**

题目背景:

考虑事务 $T_1$ 需要读取数据 A，然后更新数据 A。

**场景 A: 违反 2PL 的情况**

```
lock-S(A)   <-- 获得锁
read(A)
unlock(A)   <-- 释放锁 (进入缩减阶段)
...
lock-X(A)   <-- 错误！缩减阶段不能再申请新锁！违反 2PL。
write(A)
unlock(A)
```

- **分析**: 这**不是 2PL，不能保证可串行化。**

**场景 B: 标准 2PL (Basic 2PL)**

Plaintext

```
1. lock-S(A)    (增长阶段)
2. read(A)
3. lock-X(A)    (升级: 增长阶段 S -> X)
4. write(A)
5. unlock(A)    (缩减阶段开始: 释放锁)
6. ... (做其他不需要锁的事)
7. commit
```

- **分析**: 所有的**加锁都在解锁之前**。符合 2PL。
- **隐患**: 如果在第 5 步释放锁之后、**第 7 步 Commit 之前，另一个事务 $T_2$ 读到了 A 的新值**，然后 $T_1$ **突然故障回滚了。那么 $T_2$ 也必须回滚**。这就是 **级联回滚**。

**场景 C: 严格 2PL (Strict 2PL) —— 推荐**

```
1. lock-S(A)
2. read(A)
3. lock-X(A)    (升级)
4. write(A)
...
(一直不释放 X 锁)
...
5. commit       (事务结束)
6. unlock(A)    (只有提交后才释放 X 锁)
```

- **分析**: 因为**直到 Commit 才释放 X 锁**，其他事务在 $T_1$ **提交前根本读不到 A**。如果 $T_1$ **回滚，别人也没读过脏数据**，所以 **无级联回滚**。

------

### **💡 考试做题总结 (Exam Tips)**

1. **判断题**:
	- "2PL ensures deadlock freedom." -> **False** (会死锁)。
	- "2PL ensures conflict serializability." -> **True**。
	- "Strict 2PL avoids cascading rollbacks." -> **True**。
2. **大题策略**:
	- 如果题目让你设计加锁顺序，**Rigorous 2PL** 是最简单的写法：**开始时申请所有需要的锁，Commit 时统一释放**。
	- 如果问你某个调度是否遵循 2PL：找一下有没有“**先 unlock 某物，后来又 lock 某物**”的情况。如果有，就违反了 2PL。

### **3. 多粒度封锁 (Multiple Granularity)**

#### **(1) 核心概念 (Key Concepts)**

- **粒度层次 (Granularity Hierarchy)**:

	- 数据库被组织成一个树形结构：**Database $\to$ Area $\to$ File $\to$ Record**。
	- **细粒度 (Fine Granularity)**: 锁住具体的记录 (Record)。并发度高 (High concurrency)，但锁的管理开销大 (High overhead)。
	- **粗粒度 (Coarse Granularity)**: 锁住整个文件 (File) 或数据库。并发度低，但开销小。

- 意向锁 (Intention Lock Modes) —— ⭐ 必考定义:

	为了解决“如果我要锁住整个文件，必须先检查里面有没有哪条记录被别人锁住了”这个效率问题，引入了意向锁。

	- **IS (Intention Shared)**: 打算在树的更底层节点加 **S 锁** (显式加锁)。
	- **IX (Intention Exclusive)**: 打算在树的更底层节点加 **X 锁** (显式加锁)。
	- **SIX (Shared and Intention Exclusive)**:
		- 当前节点加 **S 锁** (读整个子树)。
		- **并且** 打算在更底层节点加 **X 锁** (修改部分记录)。
		- *(理解: SIX = S + IX。比如我要统计全表数据，同时还要更新其中几行)*。

------

#### **(2) 兼容性矩阵 (Compatibility Matrix) —— ⭐⭐⭐⭐⭐ 绝对死记硬背**

考试必考！请务必记住以下规律：

1. **X** 和谁都不兼容。
2. **IS** 最随和，除了 **X** 谁都兼容。
3. **关键冲突点**:
	- **IX 和 S 不兼容**: 你打算改下面的某行 (IX)，我想读整个表 (S)，不行！(因为你改了我就读到脏数据了)。
	- **IX 和 SIX 不兼容**: 同理。

| **\**   | **IS**   | **IX**   | **S**    | **SIX**  | **X** |
| ------- | -------- | -------- | -------- | -------- | ----- |
| **IS**  | **True** | **True** | **True** | **True** | False |
| **IX**  | **True** | **True** | False    | False    | False |
| **S**   | **True** | False    | **True** | False    | False |
| **SIX** | **True** | False    | False    | False    | False |
| **X**   | False    | False    | False    | False    | False |

*(注：True 表示兼容，False 表示冲突/等待)*

------

#### **(3) 封锁协议规则 (Locking Protocol) —— 了解流程**

- **加锁顺序 (Locking)**: **自上而下 (Top-down)**。
	- 要给节点 $Q$ 加锁，必须先给它的 **父节点 (Parent)** 加意向锁。
	- *例子*: 要锁 Record，必须先锁 File (IX/IS)。
- **解锁顺序 (Unlocking)**: **自下而上 (Bottom-up)**。
	- 必须先释放子节点的锁，才能释放父节点的锁。

------

#### **📚 完整考试实例 (Comprehensive Example)**

为了涵盖意向锁的所有知识点，我们来看一个经典的 **文件-记录 (File-Record)** 场景。

【场景设定】

数据库中有一个文件 $F_1$，里面包含记录 $R_1, R_2, ...$。

事务 $T_1$: 想要 修改 记录 $R_1$。

事务 $T_2$: 想要 读取 整个文件 $F_1$ (例如做统计)。

事务 $T_3$: 想要 读取 记录 $R_2$。

**【执行过程与兼容性分析】**

1. **$T_1$ 修改 $R_1$ (加锁过程)**:
	- $T_1$ 必须沿着树往下走。
	- Root (DB): 加 **IX** 锁。
	- File ($F_1$): 加 **IX** 锁。 (意为：我打算修改 $F_1$ 里的某条记录)
	- Record ($R_1$): 加 **X** 锁。 (真正锁定记录)
	- *状态*: $F_1$ 上现在有 $T_1$ 的 **IX** 锁。
2. **$T_2$ 读取文件 $F_1$ (冲突检测)**:
	- $T_2$ 想要读取整个 $F_1$，它试图请求 $F_1$ 上的 **S** 锁。
	- **查矩阵**: 系统检查 $F_1$ 上的现有锁。
	- 现有: **IX** ($T_1$) vs 请求: **S** ($T_2$)。
	- **结果**: **False (不兼容)**。
	- *解释*: $T_2$ 必须等待。这是对的，因为 $T_1$ 正在改 $R_1$，如果不拦着，$T_2$ 读整个文件就会读到可能不一致的数据。
3. **$T_3$ 读取记录 $R_2$ (并发执行)**:
	- $T_3$ 想要读 $R_2$。
	- Root (DB): 请求 **IS**。与 $T_1$ 的 IX 兼容 (IS vs IX -> True)。
	- File ($F_1$): 请求 **IS**。与 $T_1$ 的 IX 兼容 (IS vs IX -> True)。
		- *注意*: 虽然 $T_1$ 要改 $F_1$ 里的东西，但 $T_3$ 只是想读 $F_1$ 里的东西，只要不是同一行就行，所以 **IS 和 IX 是兼容的**，这是多粒度封锁提高并发的关键！
	- Record ($R_2$): 请求 **S**。$R_2$ 上没锁，成功。
	- **结果**: $T_3$ 可以和 $T_1$ 并发执行。

**【总结这个例子】**:

- **IX 和 S 冲突**: 阻止了 $T_2$ (全表读) 干扰 $T_1$ (单行写)。
- **IX 和 IS 兼容**: 允许了 $T_3$ (单行读) 和 $T_1$ (单行写) 并行，只要它们操作的不是同一行。

------

### **💡 考试做题总结 (Exam Tips)**

1. **做填空/选择题**:
	- 看到 **IX** 和 **S** 放一起 $\to$ **False (冲突)**。这是最常考的坑点。
	- 看到 **IS** 和 **IX** 放一起 $\to$ **True (兼容)**。这是多粒度的核心优势。
	- **SIX** = S + IX。所以 SIX 和 IX 肯定冲突（因为 S 和 IX 冲突）。
2. **做大题**:
	- 如果题目问“为什么要引入意向锁 (Intention Locks)？”，答案是：**为了提高在层级结构中加锁的效率**，避免在锁父节点时需要遍历检查所有子节点的锁状态。