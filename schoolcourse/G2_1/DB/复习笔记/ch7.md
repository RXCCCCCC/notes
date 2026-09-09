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

------

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

------

#### **(3) 联系与联系集 (Relationship and Relationship Sets)**

- **联系 (Relationship)**:

	- 多个实体之间的**关联 (association)**。
	- *例子*: 老师 `Peltier` 是学生 `Hayes` 的 `advisor` (导师)。

- **联系集 (Relationship Set)**:

	- **同类联系的数学关系**。如果 $E_1, E_2, ..., E_n$ 是实体集，**联系集 $R$ 是它们的笛卡尔积的一个子集：**

		$$R \subseteq \{(e_1, e_2, ... e_n) | e_1 \in E_1, e_2 \in E_2, ..., e_n \in E_n\}$$

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

------

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
	- **简单属性/派生属性** $\rightarrow$ **一列**（**派生属性通常不存**，或者**存计算值**）。
	- **复合属性** $\rightarrow$ **打散成多个列**（如 `name_first`, `name_last`），**不要把 `name` 自己当一列。**
	- **多值属性** $\rightarrow$ **必须新建一张表**，**不能直接放在原表里**（这是 Part IV 转换题的考点）。

### **2. 约束 (Constraints)**

约束主要分为两类：**映射基数 (Mapping Cardinalities)** 和 **参与约束 (Participation Constraints)**。

#### **(1) 映射基数 (Mapping Cardinalities) —— ⭐ 决定箭头画在哪**

- **定义**: 表示**一个实体通过联系集能关联多少个其他实体。**
- **符号规则 (必背)**:
	- **箭头 ($\rightarrow$)**: 代表 **“一” (One)**。即“至**多一个**”。
	- **直线 (—)**: 代表 **“多” (Many)**。即**“任意数量（0个或多个）”**。
	- *记忆口诀*: **箭头指向谁，谁就是“唯一的那个” (Point to the "One")**。

【四种类型与完整实例】

假设我们有**实体集 instructor (老师) 和 student (学生)，联系集是 advisor (指导)。**

1. **一对一 (One-to-One, 1:1)**:
	- **语义**: 一个老师**至多指导一个学生，一个学生至多有一个导师。**
	- **画法**: `instructor` $\leftarrow$ `advisor` $\rightarrow$ `student`。
	- *(两边都是箭头)*。
2. **一对多 (One-to-Many, 1:N)**:
	- **语义**: 一个老师可以指导**多个**学生，但一个学生**至多有一个导师。**
	- **画法**: `instructor` $\leftarrow$ `advisor` —— `student`。
	- *(箭头指向老师，表示老**师是“1”的一方**；**学生那边是直线**，**表示“多”**)*。
3. **多对一 (Many-to-One, N:1)**:
	- **语义**: 一个老师至多指导一个学生，但一个学生可以有**多个**导师。
	- **画法**: `instructor` —— `advisor` $\rightarrow$ `student`。
	- *(箭头指向学生，表示学生是“1”的一方)*。
4. **多对多 (Many-to-Many, M:N)**:
	- **语义**: 一个老师指导多个学生，一个学生有多个导师。
	- **画法**: `instructor` —— `advisor` —— `student`。
	- *(**两边都是直线**)*。

------

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

------

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
		- *理由*: 题目说了“学生 **必须** 属于一个系” $\rightarrow$ **全部参与**。
	- `Department` 到 `Major_in`：**单线** (Single line)。
		- *理由*: 题目说了“系 **不一定** 有学生” $\rightarrow$ **部分参与**。

**💡 考试做题技巧**：

- **找关键词**：
	- 看到 **"At most one"** $\rightarrow$ 画 **箭头**。
	- 看到 **"Must", "Every", "Required"** $\rightarrow$ 画 **双线**。
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

------

#### **(2) 联系集的码 (Keys for Relationship Sets) —— ⭐ 难点与考点**

联系集（菱形）本身**没有像实体那样的“自带属性”作为码**，它的码是由 **参与该联系的实体集的主码** 组合而成的。

我们要根据 **映射基数 (Mapping Cardinalities)** 来**决定联系集的主码是什么**。假设联系集 $R$ 关联了实体集 $E_1$ 和 $E_2$，它们的**主码分别是 $PK_1$ 和 $PK_2$。**

**规则总结 (必背)**：

1. **多对多 (Many-to-Many, M:N)**:
	- **主码**: **双方实体集主码的并集 (Union of primary keys)**。
	- ***公式*: $Primary\_Key(R) = PK_1 \cup PK_2$**
	- *逻辑*: **既然是多对多**，你需要**知道“哪一个老师”和“哪一个学生”配对**，才能唯一确定这个联系。
2. **多对一 (Many-to-One, N:1)**:
	- **主码**: **“多” (Many) 那一方的主码**。
	- *逻辑*: 比如 `student` (多) —— `advisor` $\rightarrow$ `instructor` (一)。**一个学生只能有一个导师**。所以，**只要给出一个 `student_ID`，我就能唯一确定这个指导关系（因为这个学生不可能对应两个导师）。**
	- *注意*: 在 1:N 或 N:1 中，**箭头指向“一”，直线连接“多”**。主码选的是 **直线那头 (Many side)** 的主码。
3. **一对一 (One-to-One, 1:1)**:
	- **主码: 任意一方 的主码**都可以作为联系集的主码。
	- *逻辑*: 因为两边都是唯一的，选谁都能唯一确定这个关系。

------

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
- **ER 图**: `student` —— `advisor` $\rightarrow$ `instructor`。
- **联系集的码**: `{s_id}`。
	- *解释*: **因为 `s_id` 是“多”的一方。**只要定了学生，导师就定了。

**情况 C: 一对一 (1:1)**

- **语义**: 导师只带一个学生，学生只有一个导师。
- **ER 图**: `student` $\leftarrow$ `advisor` $\rightarrow$ `instructor` (全是箭头)。
- **联系集的码**: **可以是 `{s_id}`，也可以是 `{i_id}`。**

------

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

------

#### **(2) 属性的特殊符号 (Attribute Variations)**

- **实下划线 (Solid Underline)**: 代表 **主码 (Primary Key)**。
	- *必须画*: 任何**强实体集**都必须有**至少一个属性带下划线。**
- **双椭圆 (Double Ellipse)**: 代表 **多值属性 (Multivalued Attribute)**。
	- *例子*: `{phone_number}` (一个人有多个电话)。
- **虚线椭圆 (Dashed Ellipse)**: 代表 **派生属性 (Derived Attribute)**。
	- *例子*: `age` (由**生日算出，不直接存储**)。

------

#### **(3) 联系的约束符号 (Relationship Constraints) —— ⭐ 易错点**

- **箭头 ($\rightarrow$)**: 代表基数中的 **“一” (One)**。
	- *用法*: 从联系集指向实体集，表示该实体集中的实体至多参与一次。
- **直线 (—)**: 代表基数中的 **“多” (Many)**。
	- *用法*: 没有任何箭头的普通线段。
- **双线 (Double Line)**: 代表 **全部参与 (Total Participation)**。
	- *含义*: 实体集中的**每一个**实体都必须参与该联系。
	- *画法*: 在实体集和联系集之间画两条平行的线。

------

#### **(4) 弱实体集符号 (Weak Entity Sets) —— ⭐ 高级考点**

- **双边框矩形 (Double Rectangle)**: 代表 **弱实体集 (Weak Entity Set)**。
	- *例子*: `Section` (课程的**开课班次，离了课程就不存在**)。
- **双边框菱形 (Double Diamond)**: 代表 **标识性联系 (Identifying Relationship)**。
	- *含义*: 连**接弱实体集和它依赖的强实体集的那个联系**。
- **虚下划线 (Dashed Underline)**: 代表弱实体集的 **分辨符 (Discriminator)**。
	- *含义*: 弱实体集自己**没有主码**，**只有“部分码”（分辨符）**。

------

#### **(5) 继承符号 (Inheritance)**

- **三角形 (Triangle)**: 标有 **ISA** 字样，代表 **特化/概化 (Specialization/Generalization)**。
	- *画法*: **超类 (Superclass) 在上，子类 (Subclass) 在下**，中间**用 ISA 三角形连接。**

------

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
		- 题目说“学生必须有导师” $\rightarrow$ 画 **双线 (Double Line)** 连接 `Student` 和 `Advisor`。
		- 题目说“学生只能有一个导师” $\rightarrow$ 这是“多对一”关系中的“多”方（因为多个学生对应一个老师，通常 ER 图箭头指向“一”方，直线连接“多”方，但在 Slide 32 的标准中，**直线 (Line)** 代表 Many，**箭头 (Arrow)** 代表 One）。
		- *修正*: 按照 Slide 35 的 N:1 画法（Instructor 是 1，Student 是 N）：
			- `Advisor` 指向 `Instructor` 画 **箭头 ($\rightarrow$)**。
			- `Advisor` 连接 `Student` 画 **直线 (—)**。
	- **Instructor 侧**：
		- 题目**说“老师可以不带学生” $\rightarrow$ 单线 (Single Line) (普通线)。**
		- **箭头已在上面画过。**

最终图形描述：

Student (双线, 直线) ———— <Advisor> ———— $\rightarrow$ (单线, 箭头) Instructor

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

$$\text{弱实体集的主码} = \text{强实体集的主码 (PK)} + \text{弱实体集的分辨符 (Discriminator)}$$

- *注意*: 这意味着弱实体集的主码是一个 **组合属性**。

#### **(4) 约束与符号 (Constraints & Symbols) —— ⭐ 作图必考**

1. **弱实体集**: 画 **双边框矩形 (Double Rectangle)**。
2. **标识性联系**: 画 **双边框菱形 (Double Diamond)**。
3. **参与度**: 弱实体集对标识性联系的参与**永远是 全部参与 (Total Participation)（即画 双线）。**
	- *理由*: 弱实体集依赖强实体集存在，**没有强实体集，弱实体集就不存在。**
4. **基数**: 通常是 **一对多 (1:N)**，**强实体集是“1”，弱实体集是“N”。**

------

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
	- **属性 `sec_id`, `semester`, `year` 下画 虚下划线 (Dashed Underline)。*(注意：这些是分辨符)***
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

------

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

------

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

------

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

------

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

------

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

------

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
	- (修正后的 Student): `Student (s_id, name, tot_cred, dept_name)`