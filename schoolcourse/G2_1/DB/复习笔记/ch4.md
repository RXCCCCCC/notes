# ch4

### **1. 连接操作基础 (Join Operations Overview)**

- **定义**：连接操作接受两个关系（表），并返回一个新的关系作为结果 。
- **本质**：它是笛卡尔积 (Cartesian product) 的**变体**，但要求**两个关系中的元组必须匹配 (match) 。**
- **通常用法**：作为 `from` 子句中的子查询表达式使用 。

------

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

------

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

------

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

1. **看到 "All ... even if no match" / "List every ..."** $\rightarrow$ **Outer Join**。
2. **看到 "All courses" (左边表主体)** $\rightarrow$ **Left Outer Join**。
3. **看到 "Pairs that match"** $\rightarrow$ **Inner Join**。
4. **写 SQL 时**：如果不确定两个表有没有意外的同名列，**不要用 Natural Join**，请老老实实写 `Join ... On ...`，这样最稳妥。

### **2. 视图 (Views)**

#### **(1) 核心概念 (Concept)**

- **定义**：视图是一种机制，用于向某些用户隐藏数据。它是**虚拟关系 (Virtual Relation)** 。
- **本质**：视图**不存储**实际数据（**除非是物化**视图），它只存储**查询表达式**。当你**查询视图时，系统会把它替换为底层的 SQL 查询**（这叫 **View Expansion**）。

------

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

------

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

------

#### **(4) 物化视图 (Materialized Views) —— 概念题**

- **定义**：不仅仅存储查询逻辑，而是**真的创建了一张物理表**来存结果 。
- **特点**：
	- 查询速度快（不用每次都重算）。
	- **维护困难**：如果**底层关系更新了**，物化视图的数据就会**过时 (Out of date)** 。需要**系统定期刷新**。

------

### **💡 考试做题总结**

1. **写 SQL 题**：看到“Create a view ...”，直接套用 `create view ... as select ...` 模板。
2. **判断题**：看到“Can we insert/update this view?”，立刻拿出**4大条件**去卡它：
	- 是不是**多表**？(是 $\rightarrow$ No)
	- 有没有 **sum/avg/distinct**? (有 $\rightarrow$ No)
	- 有没有 **Group by**? (有 $\rightarrow$ No)
	- **缺的列能不能为空**? (不能 $\rightarrow$ No)

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

------

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

------

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
	- **含义**：****指定一个谓词 $P$，所有元组都必须满足该条件。****
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

------

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

------

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

------

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

------

#### **(4) 角色 (Roles) —— 权限的打包**

- **概念**：角色本质上是一个“职位”或“组”，可以将一组权限**赋给角色，再将角色赋给用户** 。
- **操作流程**：
	1. **创建角色**：`create role instructor;` 
	2. **给角色授权**：`grant select on takes to instructor;` 
	3. **给用户赋角色**：`grant instructor to Amit;` 
- **角色的继承 (Chain of roles)** ：
	- 角色**可以授予给另一个角色。**
	- *例子*：如果有角色 `teaching_assistant`，执行 `grant teaching_assistant to instructor;`，**那么 `instructor` 角色就继承了 `teaching_assistant` 的所有权限。**

------

#### **(5) 视图上的授权 (Authorization on Views) —— 逻辑判断题难点**

- **创建视图的要求**：如果你**想创建一个视图（比如 `se_instructor`）**，你必须**拥有底层表（`instructor`）的 `select` 权限**，否则系统会拒绝创建 。
- **使用视图的权限**：
	- 假设 Bob 创建了视图 `se_instructor` 并**把该视图的 `select` 权限授予给了 Alice**。
	- **关键判断**：即使 Alice **没有**底层表 `instructor` 的权限，她**依然可以查询 `se_instructor` 视图（前提是 Bob 有底层表的权限）**。这就是视图作为安全机制的作用——**向用户隐藏数据** 。

------

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

