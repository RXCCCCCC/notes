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