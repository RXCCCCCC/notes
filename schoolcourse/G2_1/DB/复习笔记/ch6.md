# ch6

收到。我们严格依据 **《ch6.pdf》 (Chapter 6: Formal Relational Query Languages)** 的内容（第 8 页，第 28-39 页），为你详细总结 **“4. 扩展关系代数 (Extended Relational-Algebra)”**。

这一部分是关系代数中对应 SQL 高级功能（如算术运算、Group By、Outer Join）的关键部分。考试中，**Query Processing (查询处理)** 部分经常要求你将带有 Group By 的 SQL 语句转换为对应的关系代数表达式。

------

### **4. 扩展关系代数 (Extended Relational-Algebra)**

#### **(1) 广义投影 (Generalized Projection)**

- **对应 SQL**: `SELECT` 子句中的算术运算（如 `salary * 1.1`）。

- **定义**：扩展了标准的投影运算 ($\Pi$)，允许在投影列表中使用 **算术表达式 (Arithmetic functions)**，而**不仅仅是属性名**。

- 符号：

	$$\Pi_{F_1, F_2, ..., F_n}(E)$$

	- $E$: 关系代数表达式。
	- $F_i$: 可以是属性名，也**可以是涉及常量和属性的算术表达式。**

【完整考试实例】

题目：给定关系 instructor(ID, name, dept_name, salary)，其中 salary 是年薪。请查询每个老师的 ID、名字以及月薪 (Monthly Salary)。

- 关系代数写法 ：

	$$\Pi_{ID, name, dept\_name, salary/12} (instructor)$$

- **结果**：结果关系中，第四列的值**将是原表中 `salary` 除以 12 的结果。**

------

#### **(2) 聚合函数 (Aggregate Functions) —— ⭐ 必考：Group By 的转换**

- **对应 SQL**: `GROUP BY` 子句以及 `AVG`, `SUM`, `COUNT` 等函数。

- 符号：使用手写体 $\mathcal{G}$ (Calligraphic G) 表示 3。

	$$_{G_1, G_2...} \mathcal{G}_{F_1(A_1), F_2(A_2)...}(E)$$

- **参数详解** ：

	- **左下标 ($G_1, G_2...$)**：**分组属性列表** (List of attributes on which to group)。如果为空，则**表示对整个关系进行聚合（不分组）**。
	- **右下标 ($F_1(A_1)...$)**：**聚合函数列表**。$F_i$ 是**函数名 (avg, min, max, sum, count)**，$A_i$ 是属性名。
	- **$E$**：输入的**关系表达式**。

【完整考试实例】

题目：找出每个系的平均工资 (Find the average salary in each department)。

- **SQL 思路**：`Select dept_name, avg(salary) ... Group By dept_name`。

- 关系代数写法 5：

	$$_{dept\_name} \mathcal{G}_{avg(salary) \ as \ avg\_salary} (instructor)$$

	(注意：**可以使用 `as` 给计算出的结果列重命名，就像 SQL 一样** )。

【无分组实例】

题目：计算**全校**所有老师的总工资 (Sum of all salaries)。

- 写法：

	$$_{\emptyset} \mathcal{G}_{sum(salary)} (instructor)$$

	(左下标**为空，表示不分组**)。

------

### **💡 考试做题技巧 (Exam Tips)**

1. **Group By 怎么写**：
	- 只要看到 SQL 里的 `GROUP BY A, B`，在关系代数里直接把 A, B 写在 $\mathcal{G}$ 的**左下角**。
	- 只要看到 SQL 里的 `Select avg(C)`，直接把 `avg(C)` 写在 $\mathcal{G}$ 的**右下角**。
	- **公式**：`SQL: Group By X Select F(Y)` $\Leftrightarrow$ **RA**: $_{X} \mathcal{G}_{F(Y)}$。
2. **Null 的处理**：
	- 在关系代数中，涉及 Null 的算术运算结果总是 **Null** 12。
	- 在聚合函数中，Null 通常被**忽略** (Ignore) 13。
3. **符号规范**：
	- 考试手写时，$\mathcal{G}$ 要写得**像花体字**，以**区别于普通的 G。**
	- 广义投影仍然用 $\Pi$，只是**下标里多了加减乘除。**

收到。我们严格按照 **《ch6.pdf》 (Chapter 6: Formal Relational Query Languages)** 的第 40-41 页内容，为你详细总结 **“数据库修改 (Modification of the Database)”**。

需要特别注意的是，这一章讲的是 **关系代数 (Relational Algebra)**，所以这里的“修改”是用代数表达式（赋值操作）来表示的，而不是你之前在 SQL（第三章）里学的 `DELETE FROM` 或 `UPDATE` 语句。考试如果考这一章的修改，要求你写的是**带有箭头 ($\leftarrow$) 的代数公式**。

以下是严格依据讲义的详细总结与实例：

### **7. 数据库修改 (Modification of the Database)**

在关系代数中，我们使用 **赋值操作 (Assignment Operation, $\leftarrow$)** 来表达对数据库内容的修改 。

#### **(1) 删除 (Deletion)**

- **原理**：删除本质上是 **集合差运算 (Set Difference)**。从原关系 $r$ 中减去要删除的元组集合 $E$。

	$$r \leftarrow r - E$$

	- $r$：要修改的关系（表）。
	- $E$：一个关系代数表达式，计算出**所有需要删除的元组**。

【完整考试实例】

题目：从 instructor 表中删除所有 "Finance" 系的老师。

- **分析**：

	1. 先找出要删的人（集合 $E$）：$\sigma_{dept\_name='Finance'}(instructor)$
	2. 从原表中减去这些人。

- 关系代数写法：

	$$instructor \leftarrow instructor - \sigma_{dept\_name='Finance'}(instructor)$$

------

#### **(2) 插入 (Insertion)**

- **原理**：插入本质上是 **集合并运算 (Union)**。将原关系 $r$ 与要插入的新元组集合 $E$ 合并。

	$$r \leftarrow r \cup E$$

	- $E$：包含待插入元组的集合（通常是一个常量关系）。

【完整考试实例】

题目：向 course 表中插入一门新课：ID为 "CS-101"，名为 "Intro to CS"，系为 "Comp. Sci."，学分为 4。

- **分析**：

	1. 构造要插入的元组集合 $E$：$\{('CS-101', 'Intro to CS', 'Comp. Sci.', 4)\}$
	2. 将其并入原表。

- 关系代数写法：

	$$course \leftarrow course \cup \{('CS-101', 'Intro to CS', 'Comp. Sci.', 4)\}$$

------

#### **(3) 更新 (Updating)**

- **原理**：更新本质上是 **广义投影 (Generalized Projection)**。我们并不直接修改原来的行，而是计算出一个**新**的关系，其中某些属性的值发生了变化，然后把这个新关系赋值给原关系名。

	$$r \leftarrow \Pi_{F_1, F_2, ..., F_n}(r)$$

	- $F_i$：如果是**不需要修改**的属性，直接写属性名（第 $i$ 个属性）。
	- $F_i$：如果是**需要修改**的属性，写成涉及常量和属性的**算术表达式**。

【完整考试实例】

题目：给 instructor 表（属性为 ID, name, dept_name, salary）中的所有老师涨薪 5%。

- **分析**：

	- ID, name, dept_name 保持不变。
	- salary 变为 `salary * 1.05`。

- 关系代数写法：

	$$instructor \leftarrow \Pi_{ID, name, dept\_name, salary * 1.05}(instructor)$$

【难点：带条件的更新】

如果题目说“只给 Finance 系的老师涨薪”，在关系代数里**很难直接用一条投影写出来（因为投影会对所有行生效）。**

通常需要拆分成两步（虽然讲义没展开，但逻辑如下，供参考）：

1. 找出 Finance 系的人并涨薪，存为临时关系 $t_1$。

2. 找出非 Finance 系的人（保持原薪），存为临时关系 $t_2$。

3. $instructor \leftarrow t_1 \cup t_2$。

	(注：考试主要考查上面那种“全体更新”的简单投影形式)

------

### **💡 考试做题避坑指南**

1. **符号区别**：看到题目问 **"Relational Algebra Expression for Deletion"**，千万别写 SQL 的 `DELETE FROM ...`！一定要写 $$r \leftarrow r - ...$$。
2. **更新的逻辑**：更新不是用 update 算子，而是用 **投影 ($\Pi$)**。这很反直觉，要特别记忆。
3. **多重集 (Multiset) 的影响** 5：
	- 虽然**标准关系代数是基于集合（去重）的**，但实际数据库（SQL）是基于 **Multiset (多重集/包)** 的。
	- 在 **Multiset 模式下：**
		- **并 ($\cup$)**：$r$ 有 $m$ 个副本，$s$ 有 $n$ 个副本，**结果有 $m+n$ 个副本。**
		- **差 ($-$)**：**结果有 $max(0, m-n)$ 个副本。**
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

	$$\Pi_{A_1, A_2, ..., A_n}(\sigma_P(r_1 \times r_2 \times ... \times r_m))$$

- **转换步骤 (做题口诀)**：

	1. **From 子句** $\rightarrow$ 变成 **笛卡尔积 ($\times$)**。把所有表连起来：$r_1 \times r_2 \times ...$
	2. **Where 子句** $\rightarrow$ 变成 **选择 ($\sigma_P$)**。包裹在笛卡尔积外面。
	3. **Select 子句** $\rightarrow$ 变成 **投影 ($\Pi_{...}$)**。包裹在最外面，只保留 SQL 里要求的列。

------

#### **(2) 带聚合与分组的查询 (Aggregation and Group By)**

当 SQL 中出现 `group by` 和聚合函数（如 `sum`, `avg`）时，必须使用扩展关系代数的 **聚合算子 ($\mathcal{G}$)**。

- **SQL 模板** 3:

	SQL

	```
	select A1, A2, sum(A3)
	from r1, r2, ..., rm
	where P
	group by A1, A2
	```

- 对应的关系代数 (RA)**注意分组需要写在外面,注意执行顺序:**

	$$_{A_1, A_2}\mathcal{G}_{sum(A_3)}(\sigma_P(r_1 \times r_2 \times ... \times r_m))$$

- **转换步骤 (做题口诀)**：

	1. **From & Where** $\rightarrow$ 先处理笛卡尔积和选择：$\sigma_P(r_1 \times ...)$。
	2. **Group By 子句** $\rightarrow$ 变成 $\mathcal{G}$ 的 **左下标** (分组属性)：$_{A_1, A_2}\mathcal{G}$。
	3. **Select 中的聚合函数** $\rightarrow$ 变成 $\mathcal{G}$ 的 **右下标** (函数)：$\mathcal{G}_{sum(A_3)}$。
	4. **注意**：在这种简单情况下（Select 的非聚合列 = Group By 列），**不需要**再写最外层的 $\Pi$。

------

#### **(3) 进阶：Select 列少于 Group By 列 (General Case)**

这是考试的一个**陷阱**。如果 SQL 按 `A1, A2` 分组，但最后**只输出了 `A1` 和 `sum(A3)`（没输出 A2），关系代数里必须在聚合之后再加一个 投影 ($\Pi$)。**

- **SQL 模板** 5:

	SQL

	```
	select A1, sum(A3)      -- 注意：这里没有 A2
	from r1, r2, ..., rm
	where P
	group by A1, A2         -- 但是按 A1, A2 分组
	```

- 对应的关系代数 (RA)6:

	$$\Pi_{A_1, sumA3}( _{A_1, A_2}\mathcal{G}_{sum(A_3) \text{ as } sumA3}(\sigma_P(r_1 \times ... \times r_m)) )$$

- **关键点**：

	1. 先做聚合 $_{A_1, A_2}\mathcal{G}_{...}$，计算**出所有分组的结果。**
	2. 再在外面套一个 $\Pi_{A_1, sumA3}$，**把不需要显示的 `A2` 去掉。**
	3. **重命名 (Rename)**：为了投影方便，通常会在**聚合时给结果起个别名（`as sumA3`）**，然后在投影里引用这个名字。

------

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
	- RA 部分：$\sigma_{salary > 50000}(instructor)$

2. **识别 Group By (左下标)**：

	- 分组列：`dept_name`
	- RA 部分：$_{dept\_name}\mathcal{G}$

3. **识别 Select 聚合 (右下标)**：

	- 函数：`avg(salary)`
	- RA 部分：$\mathcal{G}_{avg(salary)}$

4. 组合：

	$$_{dept\_name}\mathcal{G}_{avg(salary)}(\sigma_{salary > 50000}(instructor))$$

(进阶变体)：

如果 SQL 是 select avg(salary) from ... group by dept_name (不显示系名，只显示一堆数字)。

- RA 写法：需要在外面加投影。

	$$\Pi_{avg\_sal}( _{dept\_name}\mathcal{G}_{avg(salary) \text{ as } avg\_sal}(\sigma_{salary > 50000}(instructor)) )$$

### **💡 考试避坑指南**

1. **执行顺序**：永远记住 **$\sigma$ (Select) 先做**，**$\Pi$ (Project) 最后做**。对应到 SQL **就是 Where 先执行，Select 最后输出。**
2. **符号规范**：
	- 普通 Select (选列) 用 **$\Pi$**。
	- 条件 Select (选行) 用 **$\sigma$**。
	- Group By 用 **$\mathcal{G}$**。
	- 这三个符号千万别搞混，写错了直接 0 分。
3. **多表连接**：如果 `From` 后面有多个表，记得在 $\sigma$ 里面写 $r_1 \times r_2$，或者直接写 $r_1 \bowtie r_2$ (如果 **Where 里有连接条件**)。讲义公式用的是 $\times$，**写 $\times$ 最保险** 。

### **复习建议**

1. **符号记忆**: 必须记准 $\sigma$ (选行) 和 $\Pi$ (选列)，这是最容易混淆的。
2. **表达式书写**: 练习。例如：“找出 Physics 系工资大于 90000 的老师名字”。
	- SQL: `Select name from instructor where dept_name='Physics' and salary > 90000`
	- RA: $\Pi_{name}(\sigma_{dept\_name='Physics' \land salary > 90000}(instructor))$
3. **除法运算**: 虽然讲义这里没详细展开“除法 (Division)”，但在关系代数完整体系中 `Query 1` (找出选修了所有Biology课程的学生) 实际上隐含了除法逻辑，可以用两重差集表示 (参考 Ch3 笔记)。