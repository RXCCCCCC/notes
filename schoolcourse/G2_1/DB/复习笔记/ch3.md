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

