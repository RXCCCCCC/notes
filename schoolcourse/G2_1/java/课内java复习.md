# 单词

## A

- anonymous 匿名者

## D

- descend 下降,遗传

## I

- invoke 调用;请求
- inheritance 继承
- implicit 隐式的
- immutable 不可变；

## L



## M

- **Modularity** 模块化

## P

- Pluggability 可插拔性
- polymorphism

## T

- trim 修剪

# 词组



# 小知识点

- case 指的是大小写相关
- **要加什么前缀**就去看**当前所处的作用域和查找顺序**
- 多态就是**父**变量引用可**指子**变量

# Chapter 1

![image-20251224133636242](./%E8%AF%BE%E5%86%85java%E5%A4%8D%E4%B9%A0/image-20251224133636242.png)

# Chapter 2



# Chapter 3

## Scanner 类

### 1. 引入与创建 (Import & Creation)

在考试的代码填空或改错题中，要注意以下两点：

- **Import Statement (导包):**

	**必须显式导入 Scanner 类**，否则编译错误。

	```
	import java.util.Scanner;
	```

- Constructor (构造方法):

	**创建 Scanner 对象以从控制台读取数据。**

	- **Syntax**: `Scanner input = new Scanner(System.in);`
	- **考点词汇**: `System.in` 代表 **Standard Input Device (标准输入设备)**，默认指键盘。

------

### 2. 读取数据的方法 (Methods for Reading Data)

讲义和真题中反复出现的是以下几种读取方式。请务必区分读取 **数值** 和 **字符串** 的不同方法：

#### A. 读取基本类型 (Primitive Types)

- **`nextInt()`**: 读取一个 `int` 整数。
- **`nextDouble()`**: 读取一个 `double` 浮点数。
- **`nextByte()`, `nextShort()`, `nextLong()`, `nextFloat()`**: (了解即可，考频较低，逻辑同上)。

#### B. 读取字符串 (String) —— **高频易混淆考点**

这也是《选择题题库》中经常用来设置陷阱的地方：

1. **`next()`**:
	- 读取**一个单词 (Token)**。
	- **Delimiter (分隔符)**: 遇到**空格 (Space)**、**Tab** 或 **换行符 (Enter)** 就会停止读取。
	- *真题特征*: 如果输入 "New York"，`next()` 只会读取 "New"。
2. **`nextLine()`**:
	- 读取**整行文本 (Entire Line)**。
	- **Ending character**: 直到按下 **Enter (换行符)** 为止。
	- *真题特征*: 如果输入 "New York"，`nextLine()` 会**读取完整的 "New York"。**

------

### 3. 考试中的易错点 (Common Pitfalls)

根据《Java 考试复习笔记》和真题中的代码逻辑，这里有一个非常重要的“坑”：

- 混合使用 nextInt() 和 nextLine() 的问题:

	如果你先调用 nextInt()，然后再调用 nextLine()，通常会导致 nextLine() 读到一个空字符串 (Empty String)。

	- **原因 (Reason)**: `nextInt()` 只读取了数字，但用户按下的 **Enter 键 (newline character)** 还留在缓冲区里。紧接着的 `nextLine()` 看到这个换行符，会认为输入结束了，直接返回空内容。
	- **复习笔记建议**: 遇到这种情况，通常建议多写一句 `input.nextLine();` 来“吃掉”那个换行符。

------

### 4. 异常处理 (Exception) - 仅限提及范围

在《选择题题库》中，若输入类型不匹配（例如要求 `nextInt()` 但用户输入了 "abc"），会抛出以下异常：

- **`InputMismatchException`** (输入不匹配异常)
	- *注：这通常在 Exception Handling 章节结合考察，但在 Scanner 部分要能认出这个异常名字。*

------

**总结表格 (Cheat Sheet):**

| **Method**     | **Reads (读取内容)** | **Stops at (停止条件)**            |
| -------------- | -------------------- | ---------------------------------- |
| `next()`       | String (Token)       | **Whitespace** (space, tab, enter) |
| `nextLine()`   | String (Line)        | **Enter key** (newline)            |
| `nextInt()`    | int                  | **Whitespace**                     |
| `nextDouble()` | double               | **Whitespace**                     |

# Chapter 4

## 随机数

![image-20251224103349804](./%E8%AF%BE%E5%86%85java%E5%A4%8D%E4%B9%A0/image-20251224103349804.png)

## 大小写

![image-20251224103428343](./%E8%AF%BE%E5%86%85java%E5%A4%8D%E4%B9%A0/image-20251224103428343.png)

## String类

![image-20251224103502685](./%E8%AF%BE%E5%86%85java%E5%A4%8D%E4%B9%A0/image-20251224103502685.png)

![image-20251224103529566](./%E8%AF%BE%E5%86%85java%E5%A4%8D%E4%B9%A0/image-20251224103529566.png)



# Chapter 5

## for-each限制

Java 中的 **for-each 循环**（也叫增强型 for 循环，Enhanced For-Loop）主要是为了**简化代码**和**提高可读性**而设计的。

它的核心理念是：“**我要遍历这一堆东西，我不关心索引是第几个，我只想拿到里面的每一个元素读一下。**”

正因为这种简化，它牺牲了很多灵活性。以下是 for-each 循环的主要限制：

### 1. 不能在遍历时删除/添加元素 (结构性修改)

这是最常见的一个“坑”。如果你在 for-each 循环中尝试删除集合中的元素，会抛出 **`ConcurrentModificationException`** 异常。

- **原因**：for-each 的底层是基于 **`Iterator`** 实现的。当你使用集合（如 `ArrayList`）的 `remove()` 方法时，集合的修改计数器会更新，但迭代器并不知道，于是下一次检查时发现数据版本不一致，为了防止数据混乱，直接报错。
- **如何解决**：必须使用传统的 `Iterator` 并调用 `iterator.remove()`，或者使用 `removeIf` (Java 8+)。

```
List<String> list = new ArrayList<>(Arrays.asList("A", "B", "C"));

// ❌ 错误做法：会抛出异常
for (String s : list) {
    if (s.equals("B")) {
        list.remove(s); // 崩溃！
    }
}

// ✅ 正确做法：使用迭代器
Iterator<String> it = list.iterator();
while (it.hasNext()) {
    String s = it.next();
    if (s.equals("B")) {
        it.remove(); // 安全删除
    }
}
```

### 2. 无法获取当前元素的索引 (Index)

for-each 循环**隐藏了迭代器和索引变量。**

- **限制**：如果你需要知道当前是第几个元素（比如打印 "第 1 名：张三"），或者需要访问“下一个”或“上一个”元素（比如 `i+1` 或 `i-1`），for-each 做不到。
- **如何解决**：只能换回传统的 `for (int i = 0; i < len; i++)` 循环。

### 3. 不能替换集合中的元素 (Reassigning)

你**不能通过赋值操作来改变集合中原有的引用。**

- 对于对象数组/集合：

	你无法将集合中的某个位置“换成”另一个对象。

	```
	String[] names = {"Alice", "Bob"};
	for (String name : names) {
	    name = "Charlie"; // ❌ 这里的 name 只是一个临时局部变量
	}
	// names 数组依然是 {"Alice", "Bob"}，没有任何改变
	```

	*注意：虽然你不能替换对象（引用），但你可以**修改对象内部的状态**（比如 `person.setName("NewName")` 是有效的）。*

- 对于基本数据类型数组 (int[], double[])：

	更加严格。循环变量只是原数组值的一个副本。修改它完全不会影响原数组。

	```
	int[] nums = {1, 2, 3};
	for (int n : nums) {
	    n = 0; // ❌ 只是改了局部变量 n 的值
	}
	// nums 依然是 {1, 2, 3}
	```

### 4. 只能正向遍历，不能反向

for-each 循环**只能从头到尾（index 0 -> end）**依次遍历。

- **限制**：如果你想从后往前遍历，或者每次跳过两个元素（步长为 2），**for-each 无法通过语法直接支持。**

------

### 总结：什么时候该用哪个？

| **场景**                     | **推荐循环方式**                 |
| ---------------------------- | -------------------------------- |
| **只是读取**集合里的每个元素 | ✅ **For-Each** (首选，最简洁)    |
| 需要**删除**或**添加**元素   | 🔹 Iterator (或 removeIf)         |
| 需要知道**索引** (第几个)    | 🔹 传统 for 循环 (`int i`)        |
| 需要**替换**数组里的元素     | 🔹 传统 for 循环 (`arr[i] = val`) |
| 需要**反向**遍历             | 🔹 传统 for 循环                  |

一句话概括：

For-each 是**“只读视角的线性遍历”，一旦涉及到修改结构**、修改引用或者非线性顺序，请退回到传统的 for 循环或迭代器。

# Chapter 6



# Chapter 7



# Chapter 9



# Chapter 10

在 **Strings (字符串)**、**Arrays (数组)** 和 **Console I/O (控制台输入输出)** 章节。

------

### 1. `compareTo` 方法 (String Comparison)

这是字符串和包装类（Wrapper Class）比较的核心方法，**必考**其返回值的含义。

- **定义 (Definition)**: 用于按字典顺序（Lexicographical Order）比较两个字符串或对象。
- **语法 (Syntax)**: `s1.compareTo(s2)`
- **返回值 (Return Value) —— 高频考点**: 它返回一个 **int (整数)**，而不是 boolean。
	- **Return > 0 (正数)**: 表示 `s1` 大于 `s2` (s1 comes **after** s2)。
	- **Return < 0 (负数)**: 表示 `s1` 小于 `s2` (s1 comes **before** s2)。
	- **Return 0**: 表示两者**相等 (Equal)** (contents are the same)。
- **真题提示**:
	- `"abc".compareTo("abe")`: 返回负数（因为 'c' - 'e' < 0）。
	- 常用于 **Sorting (排序)** 逻辑中。

------

### 2. `printf` 方法 (Formatted Output)

用于控制台的格式化输出，考察点在于**格式说明符 (Format Specifiers)**。

- **语法**: `System.out.printf(format, items...);`
- **常见格式符 (Specifiers) —— 必须背过**:
	- **`%d`**: Decimal integer (十进制整数).
	- **`%f`**: Floating-point number (浮点数).
	- **`%s`**: String (字符串).
	- **`%c`**: Character (字符).
	- **`%b`**: Boolean (布尔值).
- **精度控制 (Precision) —— 难点**:
	- **`%.2f`**: 保留两位小数 (Rounds to 2 decimal places)。
	- *示例*: `System.out.printf("%.2f", 3.14159);` 输出 `3.14`。
- **域宽 (Field Width)**:
	- **`%5d`**: 至少占用5个字符宽度，默认右对齐 (Right justified)。

------

### 3. `StringBuilder` / `StringBuffer` 方法

*注意：根据《考纲2025》和现代讲义，通常主要讲解 **StringBuilder**，因为它非线程安全但效率更高。`StringBuffer` 用法基本一致，只是它是线程安全的。复习时重点掌握以下**修改内容**的方法（String 做不到的）。*

- **特点 (Characteristics)**:
	- **Mutable (可变的)**: 可以在原对象上修改内容，不会像 String 那样创建新对象。
- **核心方法 (Key Methods)**:
	1. **`append(value)`**: 在末尾添加内容 (Add to the end)。
		- *Example*: `sb.append("Java");`
	2. **`insert(index, value)`**: 在指定位置插入内容。
	3. **`delete(startIndex, endIndex)`**: 删除一段内容。
	4. **`reverse()`**: **反转**字符串内容 (非常常考的算法辅助方法)。
	5. **`toString()`**: 将 `StringBuilder` **转换回普通的 `String` 对象。**
	6. **`setCharAt(index, ch)`**: 修改指定位置的字符。

------

### 4. `Arrays` 类方法 (The Arrays Class)

`java.util.Arrays` 类提供了一系列静态方法来操作数组，**极高频**出现在填空题和编程题中。

- **`Arrays.sort(array)`**:
	- 对数组进行**升序排序 (Sorts in ascending order)**。
	- *注意*: 修改的是原数组。
- **`Arrays.toString(array)`**:
	- **必用**: 直接打印数组**会输出哈希码（地址）**，想看数组里的内容必须用这个方法。
	- *Output format*: `[element1, element2, ...]`
- **`Arrays.equals(array1, array2)`**:
	- 比较两个数组的**内容**是否严格相同（Length same, elements same in same order）。
	- *注意*: 不能用 `==` 比较数组，`==` 比较的是引用地址。
- **`Arrays.binarySearch(array, key)`**:
	- 使用**二分查找 (Binary Search)** 寻找元素。
	- **前提 (Prerequisite)**: 数组必须**已经排序 (Sorted)**。
	- *返回值*: 找到返回索引；找不到返回负数。

## **Array (数组) vs ArrayList** 的核心对比考点

### 1. 核心区别 (Key Differences)

| **特性 (Feature)**        | **Array (数组)**                                             | **ArrayList**                                                |
| ------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| **大小 (Size)**           | **Fixed (固定的)**。创建后不能改变大小。                     | **Dynamic (动态的)**。可以自动增长 (Shrink/Grow automatically)。 |
| **存储类型 (Data Types)** | 既可以是 **Primitives (基本类型)** 也可以是 **Objects (对象)**。 | **只能存储 Objects (对象)**。不能直接存 `int`, `double` 等，必须用 **Wrapper Class (包装类)**。 |
| **访问效率 (Efficiency)** | 效率最高，直接通过内存地址访问。                             | 略低于数组，但功能更强大。                                   |

------

### 2. 语法操作对比 (Operation Syntax Comparison)

这是讲义中要求死记硬背的部分，考试中经常会给出混淆的写法（例如对 ArrayList 使用 `[]`）让你找错。

| **操作 (Operation)**  | **Array (a)**                                 | **ArrayList (list)**                              |
| --------------------- | --------------------------------------------- | ------------------------------------------------- |
| **创建 (Creation)**   | `new String[10]`                              | `new ArrayList<>()`                               |
| **获取长度 (Size)**   | **`a.length`** (注意：**没有**括号，它是属性) | **`list.size()`** (注意：**有**括号，它是方法)    |
| **读取元素 (Access)** | `a[index]`                                    | **`list.get(index)`**                             |
| **修改元素 (Update)** | `a[index] = "Val"`                            | **`list.set(index, "Val")`**                      |
| **添加元素 (Add)**    | 不支持直接添加 (需创建新数组复制)             | **`list.add("Val")`** 或 `list.add(index, "Val")` |
| **删除元素 (Remove)** | 不支持 (需手动移位)                           | **`list.remove(index)`** 或 `list.remove(Object)` |
| **打印 (Printing)**   | 需用 `Arrays.toString(a)`                     | 直接 `System.out.println(list)` 即可              |

------

### 3. ArrayList 的泛型与包装类 (Generics & Wrapper Classes)

根据《选择题题库》中的常见陷阱：

- **语法规则**: ArrayList 定义时尖括号 `< >` 里必须是**引用类型 (Reference Type)**。
- **错误写法 (Wrong)**: `ArrayList<int> list = new ArrayList<>();` (编译错误)
- **正确写法 (Correct)**: `ArrayList<Integer> list = new ArrayList<>();`
	- 这里利用了 Java 的 **Autoboxing (自动装箱)** 和 **Unboxing (自动拆箱)** 机制，讲义强调虽然写的是 `Integer`，但添加时可以直接写 `list.add(5)`。

------

### 4. ArrayList 其他常用方法 (Other Methods)

在编程题或填空题中，除了 `add`, `get`, `remove`, `size`，还需要掌握以下几个讲义中提到的方法：

- **`contains(Object o)`**: 返回 `boolean`，判断列表中是否包含某个元素。
- **`indexOf(Object o)`**: 返回 `int`，返回元素第一次出现的索引；没找到返回 `-1`。
- **`isEmpty()`**: 返回 `boolean`，判断列表是否为空 (等价于 `size() == 0`)。
- **`clear()`**: 清空所有元素。

------

### 5. 易错点总结 (Exam Pitfalls)

1. **Length vs Size**:
	- 数组用 `.length` (无括号)。
	- String 用 `.length()` (有括号)。
	- ArrayList 用 `.size()` (有括号)。
	- *真题中经常把这三个混用让你找错。*
2. **Capacity vs Size**:
	- ArrayList 在内部有一个 **Capacity (容量)**，通常比实际的 **Size (元素个数)** 大。讲义中提到 `trimToSize()` 可以把容量调整为当前大小，但考试更关注 `size()`。
3. **remove(int) vs remove(Object)**:
	- 对于 `ArrayList<Integer>`，调用 `remove(2)` 是删除**索引为2**的元素，还是删除**值为2**的元素？
	- **规则**: `remove(int index)` 优先级更高。如果想删除值为 2 的对象，必须写 `remove(Integer.valueOf(2))`

## **修改器 (Mutator)** 和 **访问器 (Accessor)**

#### **A. 语法格式 (Syntax)**

- **Accessor (Getter)**:

	- 必须有返回值（return type 与字段一致）。

	- 通常无参数。

	- **格式**:

		Java

		```
		public DataType getPropertyName() {
		    return propertyName;
		}
		```

- **Mutator (Setter)**:

	- 返回类型通常为 `void`。

	- 必须包含一个参数（新值）。

	- **格式**:

		```
		public void setPropertyName(DataType propertyName) {
		    this.propertyName = propertyName;
		}
		```

	- **注意 `this` 关键字**: 讲义特别强调，`this` 关键字用于引用被隐藏的类数据域（当参数名与字段名冲突时使用）5555.

#### **B. 考点：参数验证 (Validation)**

- **讲义示例**: 在 `setRadius` 方法中，如果传入的半径是负数，需要抛出异常。

	- **Strict Logic**: Mutator 不仅仅是赋值，还常用于**数据合法性检查**。

	- **代码示例** (基于讲义 Source 1681):

		```
		public void setRadius(double newRadius) throws IllegalArgumentException {
		    if (newRadius >= 0)
		        radius = newRadius;
		    else
		        throw new IllegalArgumentException("Radius cannot be negative");
		}
		```

	- **复习提示**: 如果真题要求 "Ensure the score is between 0 and 100"（确保成绩在0-100之间），你必须在 Setter 中加 `if` 判断，否则会被扣分。

### **3. 关联考点：不可变对象 (Immutable Objects)**

- **定义**: 如果一个对象创建后**内容不能改变**，则称为不可变对象（如 `String` 类）6.
- **如何创建一个不可变类 (How to make a class immutable)**:
	1. 将所有数据域声明为 **private**。
	2. **不提供任何修改器方法 (No mutator methods)**.
	3. 不提供返回可变数据域对象引用的访问器方法.
- **真题应用**: 2024年真题判断题第一题专门考了这个点：“如果一个类全部的域都声明为final,且不提供任何修改方法,那么这个类是不可变的” 。

### **4. 真题实战 (Exam Context)**

- **2024 真题 (StudentScore 类)**:
	- UML图中 `-studentNo: String` 前面的减号 `-` 代表 **private**。
	- 这意味着你**必须**为它编写 `public String getStudentNo()` 和 `public void setStudentNo(String studentNo)` 才能在类外部访问或修改它。
	- **UML 符号对照**:
		- `+` : public
		- `-` : private (考点核心：**配合 Accessor/Mutator 使用**)

在**写代码题（如 StudentScore）**时：

1. 看到 `-` 属性，立马反应出需要写 **private** 字段。
2. **除非题目说 "Immutable" 或 "Read-only"**，否则默认都要成对写 **public get...** 和 **public set...** 方法。
3. 在 **set** 方法中留意**题目是否有“非负”、“非空”等限制条件**，如果有，必须加 `if` 判断并**可能需要 `throw exception`**。

## ArrayList以及排序

### **1. ArrayList (数组列表)**

在编程题中，`ArrayList` 常用于替代数组来存储**数量不确定**的对象（例如存储一组 `StudentScore` 对象）。

#### **A. 核心声明与初始化 (Declaration & Initialization)**

- **考点**: 必须使用泛型 (Generics) 指定类型，且不能使用基本数据类型（如 `int`, `double`），必须用**包装类 (Wrapper Class)**。

- **代码模板**:

	```
	// 只能用 Integer, 不能用 int
	ArrayList<Integer> scores = new ArrayList<>();
	
	// 存储对象
	ArrayList<StudentScore> list = new ArrayList<>(); 
	```

- **相关考点 (Syllabus)**: 《考纲》中特别提到了“**自动装箱/拆箱 (Autoboxing/Unboxing)**”，即 `list.add(10)` 也是合法的，**虽然它是 `Integer` 类型，系统会自动转换。**

#### **B. 必背 API 方法 (Common Methods)**

根据《复习笔记》第17页的对比表，编程题中只需掌握以下 5 个核心方法：

1. **`add(E e)`**: 添加元素到末尾。
2. **`get(int index)`**: 获取指定索引的元素（**注意**: 不能像数组那样用 `[]`，必须用 `get()`）。
3. **`size()`**: 返回列表大小（**注意**: 数组是 `.length`，String是 `.length()`，ArrayList是 `.size()`，千万别混淆）。
4. **`remove(int index)`** 或 **`remove(Object o)`**: 删除元素。
5. **`set(int index, E e)`**: 修改指定索引的元素。

#### **C. 遍历 (Traversal)**

编程题中若要求“打印所有学生成绩”或“计算平均分”，建议使用 **For-Each Loop**：

```
public double getAverage(ArrayList<Integer> list) {
    double sum = 0;
    // 这里的 score 会自动拆箱为 double/int
    for (Integer score : list) {
        sum += score;
    }
    return sum / list.size();
}
```

------

### **2. Sorting (排序)**

根据《题库》中的代码片段和《考纲》中的 `Arrays` 方法及 `Comparable` 接口，排序主要考两种形式：**调用库函数** 和 **实现接口**。

#### **A. 考点一：Comparable 接口 (The Comparable Interface)**

这是**编程大题 (Writing Code)** 的高频考点。题目通常要求：“修改 `Student` 类，使其可以根据成绩进行排序”。

- **实现步骤**:

	1. **类必须实现 `Comparable<ClassName>` 接口。**
	2. **重写 `compareTo` 方法。**

- **代码模板 (严格依据真题逻辑)**:

	```java
	// 假设题目要求按 score 从小到大排序
	public class StudentScore implements Comparable<StudentScore> {
	    private double score;
	
	    // ... 其他代码 (构造器, getters) ...
	
	    @Override
	    public int compareTo(StudentScore other) {
	        if (this.score > other.score) 
	            return 1;
	        else if (this.score < other.score) 
	            return -1;
	        else 
	            return 0;
	    }
	}
	```

	- **记忆口诀**: `this` 比 `other` 大返回正数，排序结果就是升序（从小到大）。

#### **B. 考点二：使用库函数排序 (Using Library Sort)**

如果题目没有要求手写算法，而是让你排序一个列表：

- **数组 (Array)**: `java.util.Arrays.sort(myArray);`
- **列表 (ArrayList)**: `java.util.Collections.sort(myList);`
- **前提**: 列表中的对象必须已经**实现了 `Comparable` 接口（如上所示）。**

#### **C. 考点三：选择排序逻辑 (Selection Sort Logic)**

在您上传的《JAVA_选择题题库》第18章（Recursion部分）的题目中，出现了**递归版本的选择排序**代码片段。虽然这是选择题，但编程题有小概率考查其核心逻辑：

- **真题逻辑 (Selection Sort)**:
	1. 寻找数组中最大（或最小）的元素。
	2. 将其与最后一个（或第一个）元素**交换 (Swap)**。
	3. 对剩余部分重复上述步骤。
- **真题代码特征**: 如果在读程序题或填空题看到类似 `if (list[i] > max)` 紧接着 `swap` 操作，这一定是 **Selection Sort (选择排序)**。

------

### **复习总结 (Quick Review for Exam)**

1. **写代码时**:
	- 定义列表用 `ArrayList<Type>`。
	- 取值用 `.get(i)`，长度用 `.size()`。
2. **排序对象时**:
	- **必须 `implements Comparable<T>`。**
	- **必须 `@Override public int compareTo(T o)`。**
3. **读程序时**:
	- 看到 `Arrays.sort` 要反应过来它**依赖 `compareTo`**。
	- 看到找最大值并交换 (find max and swap)，那是 Selection Sort。

# Chapter 11



# Chapter 12

## Lambda 表达式

### 1. 核心定义与用途 (Definition & Usage)

- **用途 (Purpose)**: Lambda 表达式主要用于**简化代码**，特别是简化 **Anonymous Inner Class (匿名内部类)** 的写法。
	- *真题语境*: 通常用于 JavaFX 中的**事件注册，比如 `bt.setOnAction(e -> ...)`。**
- **前提条件 (Prerequisite)**: Lambda 表达式只能用于 **Functional Interface (函数式接口)**。
	- **考点**: 什么是 **Functional Interface**？
		- 定义: An interface that contains **exactly one abstract method** (只包含**一个**抽象方法的接口)。
		- 例如: `EventHandler<ActionEvent>` 就是一个典型的函数式接口，因为它只有一个 `handle` 方法。

------

### 2. 基础语法 (Basic Syntax)

Lambda 表达式由三部分组成： `(parameters) -> expression` 或 `(parameters) -> { statements; }`

1. **Parameters (参数)**: 对应接口中方法的参数。
2. **The Arrow (箭头 `->`)**: 分隔参数和执行体。
3. **Body (执行体)**: 具体的代码逻辑。

------

### 3. 语法简写规则 (Simplification Rules) - **高频考点**

考试常考察如何将代码写得最简（Rewrite code），或者判断哪种写法是合法的。请记住以下**省略规则**：

1. **省略参数类型 (Omit Parameter Type)**: 编译器可以自动推断类型，所以不需要写类型。
	- `(ActionEvent e) -> ...` 可以简化为 `(e) -> ...`
2. **省略小括号 (Omit Parentheses)**: 如果**只有一个参数 (Only one parameter)**，小括号可以省略。
	- `(e) -> ...` 可以简化为 `e -> ...`
	- *注意*: 如果没有参数 `()` 或者有多个参数 `(x, y)`，括号**不能**省。
3. **省略大括号 (Omit Braces)**: 如果执行体里**只有一条语句 (Single statement)**，大括号 `{}` 可以省略。
	- `e -> { System.out.println("Hi"); }` 可以简化为 `e -> System.out.println("Hi")`

------

### 4. 考试实战对比 (Comparison)

这是讲义中经典的对比案例，必须掌握：

**Before (使用匿名内部类 Anonymous Inner Class):**

```
btEnlarge.setOnAction(new EventHandler<ActionEvent>() {
    @Override
    public void handle(ActionEvent e) {
        System.out.println("Click");
    }
});
```

**After (使用 Lambda 表达式 Lambda Expression):**

```
// 标准写法
btEnlarge.setOnAction((ActionEvent e) -> {
    System.out.println("Click");
});

// 考试中最常见的简化写法
btEnlarge.setOnAction(e -> System.out.println("Click"));
```

------

### 5. 易错点提示 (Common Pitfalls)

根据《选择题题库》中的干扰项：

- **Q:** Can Lambda be used for an interface with two abstract methods? (Lambda 能用于有两个抽象方法的接口吗？)
- **A:** **No**. 它只能用于 **Functional Interface** (One abstract method)。

## Singleton Pattern (单例模式)

只有 **Singleton Pattern (单例模式)**，其次是理解 **MVC** 和 **Observer (观察者)** 的概念（通常结合 GUI 章节考察）。

以下是严格基于讲义和真题的精简总结：

### 1. 单例模式 (Singleton Pattern) —— **核心必考**

这是考试中唯一可能让你**手写代码**或者**填空**的设计模式。

- 定义 (Definition):

	**确保一个类只有一个实例 (Only one instance)**，并提供一个全局访问点来获取这个实例。

- **代码实现三要素 (3 Key Elements)** —— **背诵重点**:

	1. **Private Constructor (私有构造方法)**:
		- **必须用 `private` 修饰构造方法**，防止外部使用 `new` 关键字创建新对象。
		- *Code*: `private MyClass() { ... }`
	2. **Private Static Data Field (私有静态数据域)**:
		- 在类**内部存储这唯一的实例**。
		- *Code*: `private static MyClass instance;`
	3. **Public Static Method (公共静态方法)**:
		- **提供给外部获取实例的方法**，通常命名为 `getInstance()`。
		- *Code*: `public static MyClass getInstance() { ... }`

- **标准代码示例 (Exam Standard Code)**:

	```java
	public class Bank {
	    // 1. 存储唯一实例的静态变量
	    private static Bank instance = new Bank();
	
	    // 2. 私有构造方法，防止外部 new
	    private Bank() {
	    }
	
	    // 3. 公共静态方法，返回实例
	    public static Bank getInstance() {
	        return instance;
	    }
	}
	```

	*真题考法*: 往往会挖空 `private` 关键字，或者挖空 `static`，让你填进去。

------

### 2. 观察者模式 (Observer Pattern)

这一模式通常不单独考代码实现，而是结合 **JavaFX Event Handling (事件处理)** 考察概念。

- **概念 (Concept)**:
	- 定义了对象之间的一对多依赖 (One-to-many dependency)。当一个对象状态改变时，所有依赖者都会收到通知。
- **考试中的对应关系**:
	- **Subject / Observable (被观察者)**: 对应 **Source Object** (如 `Button`).
	- **Observer (观察者)**: 对应 **Listener / Event Handler** (如 `EventHandler` 接口的实现类).
- **关键动作**:
	- `bt.setOnAction(handler)` 本质上就是在**注册 (Registering)** 一个观察者。

------

### 3. MVC 架构 (Model-View-Controller)

这一部分主要出现在 JavaFX 或 GUI 编程的理论选择题中。

- **定义**: 一种将应用程序分为三个部分的架构模式。
- **组成 (Components)**:
	1. **Model (模型)**: 负责**数据和业务逻辑 (Data and Business Logic)。**
	2. **View (视图)**: 负责**数据的可视化展示 (Visualization)。**
	3. **Controller (控制器)**: 负责**处理用户交互，连接 Model 和 View (Handle events/interactions)。**
- **考试考点**:
	- 题目可能**会描述一个功能（例如“存储学生成绩的数组”），问你这属于 MVC 的哪一部分？**
	- **答案**: 属于 **Model**。
	- 题目描述“显示按钮的界面”，属于 **View**。

------

### 总结 (Cheat Sheet)

| **模式**       | **英文名**                | **核心特征 (Key Feature)**               | **典型考点**        |
| -------------- | ------------------------- | ---------------------------------------- | ------------------- |
| **单例模式**   | **Singleton**             | `private` Constructor, `static` Instance | 代码填空 (必背结构) |
| **观察者模式** | **Observer**              | Event Source -> Event Listener           | 理解事件处理机制    |
| **MVC**        | **Model-View-Controller** | Data / View / Logic separation           | 选择题概念辨析      |

## 

# Chapter 13

## 异常分类

### 1. 异常层次结构 (The Hierarchy)

根据复习笔记中的结构图，所有的异常和错误都继承自同一个根类：

- **Root Class (根类)**: **`java.lang.Throwable`**
	- *注意：考试中如果问**异常的父类**是谁，**必须选 `Throwable`，而不是 `Exception`。***

`Throwable` 下面有两个主要的子类分支：

1. **`Error` (错误)**:
	- **定义**: 表示严重的**系统错误 (System Errors)**，通常**由 JVM (Java Virtual Machine) 抛出。**
	- **特征**: 这种错误通常是程序**无法恢复 (Irrecoverable)** 的，程序**不应该尝试去捕获它 (App should not try to catch)。**
	- **讲义示例**: `LinkageError`, `VirtualMachineError` (如内存溢出)。
2. **`Exception` (异常)**:
	- **定义**: 表示程序运行时出现的错误，通常**可以被程序捕获并处理 (Recoverable)。**
	- **包含两类**:
		- `RuntimeException` (运行时异常)
		- Other Exceptions (其他异常，即 Checked Exceptions)

------

### 2. 核心考点：Unchecked vs Checked (必考分类)

这是异常章节中**最重要的分类标准**，真题中经常要求你辨别某个异常属于哪一类。

#### A. Unchecked Exceptions (免检异常)

- **定义**: 编译器**不强制**要求处理的异常 (Compiler does **not** force to check)。
- **包含内容**:
	1. **`RuntimeException`** 及其**所有子类。**
	2. **`Error`** 及其所有子类。
- **原因**: 这些通常反映了**编程逻辑错误 (Programming Logic Errors)**，是可以修正代码来避免的。
- **真题常考示例 (必须记住)**:
	- `ArithmeticException` (例如：整数除以零 `/ 0`)
	- `NullPointerException` (访问了空对象)
	- `ArrayIndexOutOfBoundsException` (数组越界)
	- `InputMismatchException` (Scanner 输入类型错误)
	- `NumberFormatException` (字符串转数字失败)

#### B. Checked Exceptions (必检异常)

- **定义**: 编译器**强制**要求处理的异常 (Compiler **forces** to check)。如果你不处理（不写 `try-catch` 也不写 `throws`），代码将**无法编译 (Compilation Error)**。
- **包含内容**: 除了 `RuntimeException` 和 `Error` 之外的所有 `Exception`。
- **原因**: 这些通常是由**外部环境 (External factors)** 导致的，程序无法完全控制（如文件不存在、网络中断）。
- **真题常考示例**:
	- `IOException` (输入输出异常)
	- `FileNotFoundException` (文件未找到)
	- `ClassNotFoundException` (类未找到)

------

### 3. 考试中的判断技巧 (Cheat Sheet)

在做《选择题题库》中的题目时，使用以下逻辑进行快速判断：

1. **看到 `Error` 结尾** -> 属于 **Unchecked** (System error, don't catch).
2. **看到 `RuntimeException` 及其子类** (如 `Arithmetic...`, `NullPointer...`) -> 属于 **Unchecked** (Programming error).
3. **其他的 `Exception`** (特别是涉及 IO, SQL, ClassNotFound) -> 属于 **Checked** (Must be handled).

------

### 4. 声明异常 (Declaring Exceptions) - 关联考点

虽然主要讲分类，但考纲中通常会结合 **`throws`** 关键字考察：

- 如果一个方法可能会抛出 **Checked Exception**，那么这个方法**必须 (Must)** 在方法头使用 `throws` 关键字声明它，或者在方法内部用 `try-catch` 处理掉。
- 如果抛出的是 **Unchecked Exception**，则**不需要 (Not required)** 显式声明。

## 读程序写结果题会涉及到的异常（类型转换异常处理）知识这一部分

### 1. 核心异常：ClassCastException

在读代码题中，如果你看到强制类型转换 `(ClassName)object`，首先要警惕是否会抛出这个异常。

- **定义**: 当你试图将一个对象强制转换为它**不是**的子类类型时，JVM 在**运行时 (Runtime)** 会抛出 `ClassCastException`。
- **属于分类**: **Unchecked Exception** (**RuntimeException 的子类**)。这意味着**编译器不会报错，只有运行到了才会挂掉。**

------

### 2. 读题逻辑：判断是否会抛出异常

在分析代码执行结果时，遵循以下步骤判断：

#### 情况 A：合法的向下转型 (Valid Downcasting)

如果父类引用变量指向的是**真正的子类对象**，那么转换是安全的。

Java

```
Object o = new String("Hello"); // 父类引用指向 String 对象
String s = (String)o;           // ✅ 安全。o 本质上就是 String
System.out.println("Success");
```

- **结果**: 正常执行，打印 "Success"。

#### 情况 B：非法的向下转型 (Invalid Downcasting) —— **考点**

如果父类引用变量指向的是**父类对象**或者**其他子类对象**，转换会失败。

Java

```
Object o = new Object();        // o 本质上是 Object
String s = (String)o;           // ❌ 运行时抛出 ClassCastException
```

或者：

Java

```
Object o = new Integer(5);      // o 本质上是 Integer
String s = (String)o;           // ❌ 运行时抛出 ClassCastException
```

- **结果**: 程序中断（如果没有 try-catch），抛出异常。

------

### 3. 结合 Try-Catch 的代码执行流 (Execution Flow)

这是“读程序写结果”题最喜欢的模式：**转换失败 -> 捕获异常 -> 继续执行**。

**真题模拟结构**:

Java

```
public class Test {
    public static void main(String[] args) {
        try {
            Object x = new Integer(0);
            System.out.println((String)x); // [1] 这里尝试强转
        }
        catch (ClassCastException e) {
            System.out.println("Error 1"); // [2] 捕获特定的转换异常
        }
        catch (RuntimeException e) {
            System.out.println("Error 2");
        }
        System.out.println("Done");        // [3] 异常处理后的后续代码
    }
}
```

**分析步骤**:

1. **Line [1]**: `x` 是 `Integer` 对象，试图强转为 `String`。-> **抛出 `ClassCastException`**。
2. **Line [1]**: 这一行本身不会打印任何东西，直接跳出 try 块。
3. **Catch 匹配**: 寻找匹配 `ClassCastException` 的 catch 块。
	- 第一个 catch 匹配成功。
4. **执行 Catch**: 打印 **"Error 1"**。
	- *注意*: 如果没有第一个 catch，它也会被 `RuntimeException` (父类) 捕获。
5. **后续代码**: `try-catch` 结束后，程序**继续执行**。
	- 打印 **"Done"**。

**最终输出**:

```
Error 1
Done
```

------

### 4. 如何避免：`instanceof` 关键字

在改错题或填空题中，为了**防止 `ClassCastException`，讲义要求使用 `instanceof` 进行检查。**

- **语法**: `objectRef instanceof ClassName`

- **返回值**: `boolean` (true/false)

- **读题技巧**:

	Java

	```
	Object o = new Integer(1);
	if (o instanceof String) {
	    System.out.println((String)o);
	} else {
	    System.out.println("Not a String");
	}
	```

	- **结果**: 打印 "Not a String"。程序**不会崩溃**。

------

### 总结 (Cheat Sheet)

在做“读程序写结果”时，看到 `(Type)obj` 这种代码：

1. **看本质**: 盯着 `new` 后面的那个类是什么。
2. **比类型**:
	- 如果 `new` 的是 `Dog`，转成 `(Dog)` -> **OK**。
	- 如果 `new` 的是 `Animal` (父类)，转成 `(Dog)` -> **Exception**。
	- 如果 `new` 的是 `Cat` (兄弟类)，转成 `(Dog)` -> **Exception**。
3. **看环境**:
	- 如果在 `try` 块里 -> 跳到 `catch` 打印错误信息，然后继续跑。
	- 如果没在 `try` 块里 -> 程序直接结束 (Terminate)。

**Next Step:** 这部分是否清楚？至此，所有核心考点的大纲复习已完成！

接下来建议：

1. **实战模拟**: 我为你出一道包含 `try-catch` 和 `ClassCastException` 的代码阅读题，你来写输出结果？
2. **多线程复习**: 如果你觉得还没看够，我们可以快速过一遍多线程的 `start()` vs `run()`。

## 编程题会涉及的异常的知识点

主要出现在两个场景：**数据验证 (Data Validation)** 和 **文件输入输出 (File I/O)**。

### **1. 核心概念与分类 (Concepts & Classification)**

- **Checked Exceptions (必检异常)**:
	- **定义**: 编译器强制要求处理的异常（Checked by the compiler）。如果不处理（不try-catch也不throws），编译无法通过。
	- **常见例子**: `FileNotFoundException`, `IOException`.
	- **复习重点**: 在做 **File I/O** 题目时，必须处理这类异常。
- **Unchecked Exceptions / Runtime Exceptions (免检异常 / 运行时异常)**:
	- **定义**: 编译器不强制要求处理。通常反映编程逻辑错误。
	- **常见例子**: `NullPointerException`, `ArrayIndexOutOfBoundsException`, `IllegalArgumentException` (重点).
	- **复习重点**: 在 **Mutator (Setter)** 方法中进行参数验证时使用。

------

### **2. 编程题场景一：数据验证 (Data Validation)**

这是在类设计题（如 `StudentScore` 或 `Circle`）中最常见的考法。如果题目要求“若输入不合法则提示错误”或“抛出异常”，请使用 `IllegalArgumentException`。

- **场景**: 在 `set` 方法或构造方法中检查参数。

- **关键字**: `throw` (注意没有 `s`)。

- **代码模板 (Code Pattern)**:

	```java
	public void setRadius(double newRadius) {
	    if (newRadius >= 0) {
	        radius = newRadius;
	    } else {
	        // 抛出一个免检异常，提示参数不合法
	        throw new IllegalArgumentException("Radius cannot be negative");
	    }
	}
	```

	- **注意**: 这里使用的是 `throw new ExceptionType("Message");`，这是 **主动抛出**。

------

### **3. 编程题场景二：文件读取 (File I/O)**

根据《Java 考试复习笔记》第23页的 `ScoreAverageCalculatorWithScanner` 示例，文件读取是编程大题的高频考点。

- **场景**: 使用 `Scanner` 读取文件内容。

- **痛点**: `new Scanner(new File("filename"))` 会**抛出 `FileNotFoundException`，**这是一个 **Checked Exception**，必须处理。

- **两种处理方式 (严格根据讲义)**:

	- **方式 A: 使用 try-catch 捕获 (讲义推荐)**

		```java
		import java.io.File;
		import java.io.FileNotFoundException;
		import java.util.Scanner;
		
		public class ReadFileDemo {
		    public static void main(String[] args) {
		        File file = new File("score.txt");
		        try {//属于必检异常,不给会报语法错误
		            Scanner input = new Scanner(file);
		            while (input.hasNext()) {
		                // 读取逻辑
		                String data = input.next();
		            }
		            input.close(); // 别忘了关闭
		        } catch (FileNotFoundException e) {
		            // 处理文件未找到的情况
		            System.out.println("File not found");
		        }
		    }
		}
		```

	- 方式 B: **在方法头声明 throws (简单粗暴)**

		如果题目没有强制要求用 try-catch，可以在 main 方法后面加 throws。

		Java

		```
		public static void main(String[] args) throws FileNotFoundException {
		    File file = new File("score.txt");
		    Scanner input = new Scanner(file); // 这样就不需要 try-catch 了
		    // ...
		}
		```

### **4. 关键字辨析 (Keywords Distinction)**

这是选择题和改错题的重灾区，但在写代码时要确保用对：

- **`throw`**:
	- **位置**: 在方法体**内部** (Inside the method).
	- **作用**: 抛出一个异常对象。
	- **写法**: `throw new IllegalArgumentException(...)`
- **`throws`**:
	- **位置**: 在方法**头** (Method header).
	- **作用**: 声明该方法**可能会抛出异常**，**告知调用者去处理。**
	- **写法**: `public void method() throws IOException`

### **5. 总结 (Summary for Review)**

在面对编程题时：

1. **看到 Set 方法 / 构造函数**: 检查是否有“非负”、“范围限制”的要求。如果有 $\rightarrow$ `if (fail) throw new IllegalArgumentException("...");`
2. **看到 File / IO**: 必须意识到有 `FileNotFoundException`。
	- 要么写 `try { ... } catch (FileNotFoundException e) { ... }`
	- 要么在方法签名后加 `throws FileNotFoundException`。

# Chapter 17

## Scanner和PrintWriter行为

### 1. `File` 类 (The File Class)

在进行文件读写之前，必须先**了解 `java.io.File` 类。**

- **用途**: 它只代表**文件属性/路径 (File properties/path)**，并不直接包含读写数据的方法。
- **构造 (Constructor)**: `File file = new File("scores.txt");`
- **常用检查方法 (Check Methods)**:
	- `exists()`: 检查文件是否存在。
	- `canRead()`, `canWrite()`: 检查读写权限。
	- `getAbsolutePath()`: 获取绝对路径。
	- `isDirectory()`, `isFile()`: 判断是目录还是文件。

------

### 2. 使用 `PrintWriter` 写入数据 (Writing Data Using PrintWriter)

这是考试中考察文件输出的核心类。

- **构造方法 (Constructor) —— 易错点**:
	- 可以直接传入文件名字符串：`PrintWriter output = new PrintWriter("scores.txt");`
	- 也可以传入 File 对象：`PrintWriter output = new PrintWriter(new File("scores.txt"));`
- **核心行为 (Key Behaviors) —— 必考逻辑**:
	1. **Create (创建)**: 如果文件**不存在，`PrintWriter` 会自动创建它。**
	2. **Overwrite/Truncate (覆盖/清空)**: 如果文件**已经存在 (Already exists)**，`PrintWriter` 会在没有警告的情况下**清空 (Destroys/Truncates)** 原文件的内容，然后从头开始写入。
	3. **No Append (无追加)**: 基础的 `PrintWriter` 构造方法**默认不支持追加模式。**
- **写入方法 (Methods)**:
	- `print()`, `println()`, `printf()`: 用法与 `System.out` 完全一致。
- **关闭文件 (Closing the file) —— 致命错误**:
	- **规则**: 必须调用 **`output.close()`**。
	- **原因**: 写入的数据可能会被暂时保存在 **Buffer (缓冲区)** 中。如果不调用 `close()`，数据可能无法真正保存到磁盘文件中 (Data may not be saved)。

------

### 3. 使用 `Scanner` 读取数据 (Reading Data Using Scanner)

你之前复习过 `Scanner` 读取控制台，这里重点是读取文件，注意两者的语法区别。

- **构造方法 (Constructor) —— 高频陷阱**:
	- **正确写法**: `Scanner input = new Scanner(new File("scores.txt"));`
	- **错误写法**: `Scanner input = new Scanner("scores.txt");`
		- *解释*: 如果直接传字符串，`Scanner` 会把这个字符串（即 "scores.txt" 这几个字符）当作读取内容，而不是去读文件。必须用 `new File(...)` 包装。
- **异常处理 (Exception Handling)**:
	- 构造 `Scanner` 读取文件时，必须处理 **`java.io.FileNotFoundException`**。这也是讲义中提到的 **Checked Exception**，必须使用 `try-catch` 或在方法头声明 `throws`。
- **读取逻辑**:
	- `hasNext()`: 检查是否还有数据。
	- `next()`, `nextInt()`, `nextLine()`: 读取数据（行为同控制台输入）。
	- **注意**: 文件读取通常配合 `while(input.hasNext())` 循环使用。

------

### 4. 资源自动关闭 (Try-with-resources)

- **语法**:

	```
	try (
	    // 在这里声明并创建资源
	    PrintWriter output = new PrintWriter("file.txt");
	) {
	    output.println("Data");
	} // 这里会自动调用 close()
	```

- **考点**: 如果考题中出现这种写在 `try (...)` 括号里的代码，要知道这表示**自动关闭资源 (Automatically close resources)**。

------

### 5. 总结对比表 (Cheat Sheet)

| 特性             | **Scanner (Read)**           | **PrintWriter (Write)**            |
| ---------------- | ---------------------------- | ---------------------------------- |
| **主要用途**     | 从文件读取数据               | 向文件写入文本数据                 |
| **构造参数**     | **必须是 `new File(...)`**   | 可以是 `String` 或 `new File(...)` |
| **文件存在时**   | 正常读取                     | **覆盖 (Overwrites)** 原内容       |
| **文件不存在时** | 抛出 `FileNotFoundException` | **自动创建 (Creates)** 新文件      |
| **必须操作**     | 处理异常                     | **调用 `close()`**                 |

## Text I/O

这一部分在编程题中主要考察 **读写文件** 的能力，核心围绕两个类：**Scanner (读)** 和 **PrintWriter (写)**。

------

### **1. 核心类与对象 (Core Classes)**

根据讲义，处理文本文件必须掌握以下三个类：

1. **`java.io.File`**:
	- **作用**: 代表文件系统中的**一个文件路径（并不直接读写内容）**。
	- **关键用法**: `new File("filename.txt")`。
	- **常考方法**: `exists()` (判断文件是否存在)。
2. **`java.util.Scanner`** (用于 **Input/Reading**):
	- **作用**: 从文件中读取文本数据。
	- **注意**: 构造函数不能直接传字符串文件名，必须传 `File` 对象。
3. **`java.io.PrintWriter`** (用于 **Output/Writing**):
	- **作用**: 向文本文件写入数据。
	- **考点**: 如果文件不存在，它会自动创建；如果存在，它会**清空 (truncate)** 原文件内容。

------

### **2. 编程题考点一：读取文件 (Reading Data)**

这是《复习笔记》第23页明确给出的编程题示例，**一定要背熟这个模板**。

- **语法陷阱 (Syntax Trap)**:

	- 错误: `Scanner input = new Scanner("score.txt");` (这**会读取字符串本身，而不是文件!)**
	- 正确: `Scanner input = new Scanner(new File("score.txt"));`

- **代码模板**:

	```java
	// 1. 创建 File 对象
	File file = new File("scores.txt");
	
	// 2. 必须处理 FileNotFoundException (使用 try-catch 或 throws)
	try {
	    Scanner input = new Scanner(file);
	
	    // 3. 循环读取 (Looping)
	    while (input.hasNext()) {
	        String firstName = input.next(); // 读取字符串
	        int score = input.nextInt();     // 读取整数
	        // 处理逻辑...
	    }
	
	    // 4. 关闭资源 (必须!)
	    input.close();
	} catch (FileNotFoundException e) {
	    System.out.println("File not found");
	}
	```

------

### **3. 编程题考点二：写入文件 (Writing Data)**

根据《考纲》中提到的 `PrintWriter` 行为，这是另一半考点。

- **核心动作**: 创建、写入、关闭。

- **致命考点 (Critical Point)**:

	- **必须调用 `close()`**: 如果不调用 `output.close()`，数据可能只停留在缓冲区，**不会真正写入文件**。

- **代码模板**:

	```java
	File file = new File("output.txt");
	
	// 检查文件是否存在 (讲义中常见的 defensive programming)
	if (file.exists()) {
	    System.out.println("File already exists");
	    System.exit(1); // 退出程序
	}
	
	// 创建 PrintWriter (必须处理 FileNotFoundException)
	// 同样可以使用 try-catch，这里为了简洁展示逻辑用 throws 演示
	public static void main(String[] args) throws FileNotFoundException {
	    PrintWriter output = new PrintWriter(file);
	
	    // 写入数据
	    output.print("John Doe ");
	    output.println(90); // 写入并换行
	
	    // 必须关闭!
	    output.close();
	}
	```

------

### **4. 易错点总结 (Exam Tips)**

1. **构造函数区别**:
	- `Scanner` 必须包一层 `File`: `new Scanner(new File("name"))`
	- `PrintWriter` 可以直接传文件名: `new PrintWriter("name")` (但也推荐传 `File` 对象)。
2. **异常处理**:
	- 凡是涉及 `new Scanner(File)` 或 `new PrintWriter(File)`，都必须处理 **`FileNotFoundException`**。
3. **资源关闭**:
	- 编程题中写完读写逻辑后，千万别忘了 `input.close()` 或 `output.close()`，这是常见扣分点。
4. **读取方法**:
	- **`next()`: 读取一个单词 (以空白符分隔)。**
	- **`nextLine()`: 读取整行。**
	- **`nextInt()`, `nextDouble()`: 读取数值。**

一句话总结复习策略:

编程题如果考 I/O，大概率是**“从文件读取数据并计算”或“将结果写入文件”**。请死记 new Scanner(new File(...)) 这一搭配，并**时刻记得写 close() 和 try-catch (或 throws)。**

## BinaryI/O

### **1. 核心概念与区别 (Text vs. Binary)**

- **Text I/O (文本 I/O)**:
	- **类: `Scanner`, `PrintWriter`.**
	- 特点: 人类可读 (Human readable)，由字符编码 (Encoding, 如 UTF-8) 解释。
	- 数据: `199` 存储为三个字符 '1', '9', '9'。
- **Binary I/O (二进制 I/O)直接根据文件路径创造数据流**:
	- **类: `InputStream`, `OutputStream` 及其子类。**
	- 特点: 机器效率高 (Efficient for machine)，直接存储内存中的二进制形式。
	- 数据: `199` (int) 存储为 4 个字节 (00 00 00 C7)。
	- **讲义原话**: "Binary I/O does not involve encoding or decoding and thus is more efficient than text I/O."

------

### **2. 关键类及其层级 (Key Classes)**

请记住这三组对应的类，考试中常考察配对使用：

#### **A. 基础字节流 (File Streams)**

- **`FileInputStream` / `FileOutputStream`**
- **作用**: **读写最基本的字节 (byte)。**
- **构造**: `new FileOutputStream("file.dat")` (如果**文件不存在会创建；如果存在会覆盖，除非使用 `append` 参数 `true`)。**

#### **B. 数据流 (Data Streams) - 重点**

- **`DataInputStream` / `DataOutputStream`**

- **作用**: **读写基本数据类型 (primitives) 和字符串 (String)。**

- **常用方法**:

	- **`writeInt(int v)`, `readInt()`**
	- **`writeDouble(double v)`, `readDouble()`**
	- **`writeUTF(String s)`**, **`readUTF()`** (专门用于读写二进制格式的字符串)。

- **代码套路**: **必须包裹在 `FileOutputStream` 之上。**

	```
	DataOutputStream output = new DataOutputStream(new FileOutputStream("file.dat"));
	output.writeInt(100);
	```

#### **C. 对象流 (Object Streams) - 重点**

- **`ObjectInputStream` / `ObjectOutputStream`**
- **作用**: 读写整个对象 (Objects)。
- **考点**: **Serialization (序列化)**。

------

### **3. 核心考点：序列化 (Serialization)**

这是 Binary I/O 这一章最爱考的概念题。

- **`Serializable` 接口**:
	- **定义**: 一个**标记接口 (Marker Interface**)，**没有任何方法 (No methods)**，只是告诉 JVM **这个类的对象可以被序列化**。
	- **考点**: 如果一个类**想被 `ObjectOutputStream.writeObject()` 写入文件，**它**必须**实现 `implements Serializable` 接口，**否则会抛出 `NotSerializableException`。**
- **`transient` 关键字**:
	- **定义**: 用于**修饰不需要序列化的字段。**
	- **效果**: 被 `transient` 修饰的变量在序列化时**会被忽略 (Ignored during serialization)，读取时恢复为默认值 (如 0 或 null)。**

------

### **4. 编程题/读程序题难点：EOFException**

与 Text I/O (`Scanner.hasNext()`) 不同，`DataInputStream` **没有** `hasNext()` 这样的方法来判断是否读完。

- **如何判断文件读完?**

	- **必须使用 `try-catch` 捕获 `EOFException` (End of File Exception)。**
	- 这是 Binary I/O 读取数据的标准逻辑，**非常容易考读程序题**。

- **代码逻辑 (Code Logic)**:

	```java
	try {
	    DataInputStream input = new DataInputStream(new FileInputStream("data.dat"));
	    while (true) { // 无限循环
	        int value = input.readInt(); // 不断读取
	        System.out.println(value);
	    }
	} catch (EOFException e) {
	    // 捕获到这个异常，说明文件读完了，循环结束
	    System.out.println("End of file reached");
	} catch (IOException e) {
	    e.printStackTrace();
	}
	```

### **5. 随机访问文件 (RandomAccessFile)**

《考纲2025》的判断题部分特别提到了这个类。

- **特点**: **既可以读也可以写 (Both read and write)。**
- **关键方法**:
	- **`seek(long pos)`: 移动文件指针 (File pointer) 到任意位置。**
	- **`length()`: 获取文件长度。**
- **不是 Stream**: 注意，**`RandomAccessFile` 不是 `InputStream` 或 `OutputStream` 的子类，它直接继承自 `Object`。**

------

### **复习总结 (Quick Review)**

1. **Text vs Binary**: 人类可读 vs 机器高效。
2. **写数据: `DataOutputStream` 用 `writeInt`, `writeUTF`。**
3. **读数据**: 这是一个**无限循环 `while(true)`**，靠捕获 **`EOFException`** 来停止。
4. **序列化**: 必须实现 `Serializable` 接口 (空接口)，不想存的字段加 `transient`。
5. **RandomAccessFile**: 只有**它能 `seek()` (随机跳转)。**

## Command-Line Arguments (命令行参数)

考察 **`main` 方法的签名**、**参数数组的索引**以及**数据类型转换**。

### **1. 核心定义 (Core Definition)**

- **Main 方法签名**: 必须背诵，一个字都不能错。

	```java
	public static void main(String[] args)
	```

	- **`args`**: 是一个 **字符串数组 (`String[]`)**。
	- **含义**: 它存储了你在运行程序时，在命令行输入的参数。

### **2. 运行与传递参数 (Execution & Passing Arguments)**

- **命令行指令**: `java ClassName arg0 arg1 arg2 ...`
- **对应关系 (关键考点)**:
	- `args[0]` 是 **第一个用户参数**（例如 `arg0`）。
	- `args[1]` 是 第二个参数。
	- **区别**: 不同于 C/C++，Java 的 `args[0]` **不是** 类名/程序名。
- **示例**: 假设运行命令：`java TestMain 10 20`
	- `args.length` 是 `2`。
	- `args[0]` 是字符串 `"10"`。
	- `args[1]` 是字符串 `"20"`。

### **3. 编程题核心考法 (Coding Exam Patterns)**

#### **考点 A: 参数长度检查 (Length Check)**

在访问 `args` 之前，**必须**检查数组长度，否则会抛出 `ArrayIndexOutOfBoundsException`（这是编程题常见的扣分点）。

```java
public static void main(String[] args) {
    // 检查是否有足够的参数
    if (args.length != 2) {
        System.out.println("Usage: java TestMain arg1 arg2");
        System.exit(1); // 退出程序
    }
    // ...
}
```

#### **考点 B: 数据类型转换 (Parsing Strings)**

`args` 中的元素永远是 **`String`**。如果题目要求计算数字（如计算器），你**必须**进行转换。

- **转整数**: `int n = Integer.parseInt(args[0]);`
- **转浮点**: `double d = Double.parseDouble(args[1]);`

#### **考点 C: 处理带空格的参数 (Arguments with Spaces)**

（虽然这是操作系统特性，但常作为“陷阱”出现）

- 命令: `java Test "New York" Boston`
- 结果:
	- `args[0]` 是 `"New York"` (**作为一个整体**)。
	- `args[1]` 是 `"Boston"`。
	- `args.length` 是 `2`。

------

### **4. 经典编程题示例：计算器 (Calculator)**

这是最经典的 Command-Line Arguments 题目，请务必掌握逻辑：

**题目**: 编写一个程序，通过命令行接收两个整数和一个操作符（如 `+`），输出运算结果。 **运行**: `java Calculator 2 + 3`

**代码模板**:

```java
public class Calculator {
    public static void main(String[] args) {
        // 1. 检查参数个数
        if (args.length != 3) {
            System.out.println("Usage: java Calculator operand1 operator operand2");
            System.exit(1);
        }

        // 2. 提取并转换参数
        int num1 = Integer.parseInt(args[0]);
        String operator = args[1]; // 操作符本身就是 String
        int num2 = Integer.parseInt(args[2]);

        // 3. 执行逻辑
        int result = 0;
        switch (operator) {
            case "+": result = num1 + num2; break;
            case "-": result = num1 - num2; break;
            // ...
        }
        System.out.println(result);
    }
}
```

*(注意：在命令行运行 `\*` 时通常需要加引号 `"\*"`，否则会被 shell 识别为通配符，但在代码逻辑中只需处理字符串 `"\*"` 即可)*

### **复习总结 (Quick Review)**

1. **类型**: `args` 是 `String[]`，**存的全是字符串**。
2. **索引**: `args[0]` 是第一个参数，**不是类名**。
3. **转换**: 用 **`Integer.parseInt()` 处理数字**。
4. **安全**: **永远先检查 `args.length` 再访问**。

# Chapter 19

## 泛型

### 1. 核心概念与优势 (Concept & Benefits)

- **定义 (Definition)**: 泛型就是**参数化类型 (Parameterized Types)**。这就好比把类型（Type）当成一个参数传进去。
	- *例子*: `ArrayList<String>`，这里的 `String` 就是传给 `ArrayList` 的类型参数。
- **为什么要用泛型 (Why Generics?) —— 选择题必考**:
	1. **Detect errors at Compile Time (在编译时检测错误)**: 这是最重要的优点。如果没有泛型，类型错误要等到程序跑起来（Runtime）才会崩溃；有了泛型，编译器在写代码时就会报错。
	2. **No Casting needed (无需强制类型转换)**: 取出元素时不需要显式地写 `(String)list.get(0)`。

------

### 2. 泛型类与方法 (Generic Class & Method)

#### A. 泛型类 (Generic Class)

- **语法**: `public class GenericStack<E> { ... }`
- **术语**:
	- `E`: **Formal Generic Type (形式泛型类型)** —— 只是个占位符。
	- `String` (在 `new GenericStack<String>()` 中): **Actual Concrete Type (实际具体类型)**。

#### B. 泛型方法 (Generic Method) —— 易错语法

- **位置**: 泛型参数 `<E>` 必须放在**返回值类型 (Return Type) 之前**。
	- *正确*: `public static <E> void print(E[] list)`
	- *错误*: `public static void <E> print(E[] list)`

------

### 3. 原始类型 (Raw Type) —— 常见干扰项

- **定义**: 如果使用泛型类但**不指定**具体类型，就叫 Raw Type。
	- *例子*: `ArrayList list = new ArrayList();` (没有 `<String>`)
- **后果**:
	- 编译器会把类型视为 `Object`。
	- **Unsafe (不安全)**: 可能会导致运行时的 `ClassCastException`。
	- **Backward Compatibility (向后兼容性)**: Java 允许这样做主要是为了兼容旧版本代码，但新代码**不推荐**使用。

------

### 4. 泛型通配符 (Wildcards) —— 难点与重点

考试中经常出现带有 `?` 的代码，问你这是什么意思或者是否合法。

| **符号**          | **名称 (Name)**                        | **含义 (Meaning)**                        |
| ----------------- | -------------------------------------- | ----------------------------------------- |
| **`?`**           | **Unbounded Wildcard (无界通配符)**    | 等同于 `? extends Object`。接受任何类型。 |
| **`? extends T`** | **Bounded Wildcard (受限/上界通配符)** | 接受 `T` 或 `T` 的**子类 (Subclass)**。   |
| **`? super T`**   | **Lower Bound Wildcard (下界通配符)**  | 接受 `T` 或 `T` 的**父类 (Superclass)**。 |

- *真题场景*: `public void add(GenericStack<? extends Number> stack)`
	- 这就意味着你可以传入 `GenericStack<Integer>` 或 `GenericStack<Double>`，因为它们都继承自 `Number`。

------

### 5. 类型擦除 (Type Erasure) —— 理论考点

- **概念**: Java 的泛型是**伪泛型**。编译器在编译完代码后，会**Erasure (擦除)** 掉所有的泛型信息。
- **结果**: 在 **Runtime (运行时)**，JVM 根本不知道 `<String>` 的存在，它只知道 `Object`（或者泛型的上界）。
- **考试意义**: 这解释了为什么泛型有那么多限制（见下一点）。

------

### 6. 泛型的限制 (Restrictions) —— 找错题/判断题核心

根据讲义，以下操作在泛型中是**严格禁止 (Forbidden)** 的，看到直接判错：

1. **Cannot create instance of E (不能创建 E 的实例)**:
	- `E object = new E();` ❌ (因为运行时不知道 E 是谁)
2. **Cannot create generic array (不能创建泛型数组)**:
	- `E[] list = new E[10];` ❌
3. **Static context restrictions (静态上下文限制)**:
	- 类的泛型参数 `<E>` 不能用于 **static** 变量或 **static** 方法中。
	- *原因*: 静态成员是**类共享的**，而 `<E>` 是**属于对象的。**
4. **Exception limitation (异常限制)**:
	- **自定义异常类不能是泛型的**。`class MyException<T> extends Exception` ❌

## Lab1

收到。根据你上传的《实验一.docx》，这一实验的“思考与回答”要点主要集中在**命令行工具的高级选项**以及**Java 包机制**的理解上。

以下是该部分的核心思考点总结（附带标准答案）：

### **Lab 1: Thinking & Answering (思考与回答要点)**

这部分要求你不仅仅会输命令，还要理解命令后面参数的真实含义。

#### **1. 关于编译命令 `javac` 的思考**

- **思考点**：`javac` 命令中**的 `-d` 参数有什么作用？**
	- **回答 (Answer)**：`-d` (directory) 用于指定**生成的字节码文件 (.class) 存放的目录**。
	- **示例**：`javac -d . MyClass.java` 会**在当前目录 (`.`) 下，根据包名自动生成对应的文件夹结构（例如生成 `banking/domain/` 目录）**，并**将 `.class` 文件放进去。**
- **思考点**：`-sourcepath` 和 `-classpath` 有什么区别？
	- **回答 (Answer)**：
		- `-sourcepath`：**告诉编译器去哪里**找**源代码文件 (.java)**。
		- `-classpath` (或 `-cp`)：告诉编译器**去哪里找已经编译好的类文件 (.class) 或库文件（.jar）**，以便在编译当前代码时引用它们。

#### **2. 关于运行命令 `java` 的思考**

- **思考点**：为什么运行程序时也需要 `-classpath`？
	- **回答 (Answer)**：启动 **JVM** 时，它**需要知道去哪里加载你的类**。如果**不指定 `-classpath`，JVM 默认只在当前目录找**；如果**你的类引用了其他目录的类（或 jar 包），必须显式告诉 JVM 路径。**

#### **3. 关于包机制 (Package & Import) 的思考**

- **思考点**：Java 的 `package` **到底是什么？**
	- **回答 (Answer)**：包是 Java 提供的一种**有效管理类 (Class Management)** 的机制。
	- **本质**：它**在操作系统层面直接对应目录/文件夹 (Directory Structure)。**例如 `package banking;` 对应文件夹 `banking`。
- **思考点**：什么时候需要用 `import`？
	- **回答 (Answer)**：当你需要使用**另一个包**中的类时，**必须使用 `import`** 语句导入。**如果两个类在同一个包中，则不需要导入。**

------

### 🧠 核心考点自测 (Self-Check)

Question:

如果在源代码 Test.java 的第一行写了 package com.test;，但你编译时直接运行 javac Test.java（**没有加 -d 参数），生成的 Test.class 文件会出现在哪里？**它能直接运行吗？

针对刚才那个问题的**直接答案**如下：

**答案 (Answer):**

1. 生成的 .class 文件在哪里？

	它会出现在 **当前目录 (Current Directory) 下（即和 .java 文件在同一个文件夹里）**，而不会自动生成 com/test 文件夹。

2. **它能直接运行吗？**

	**不能 (No)。**

3. 为什么？(Reason)

	因为你在代码里声明了 package com.test;，JVM **在运行时会严格要求物理目录结构必须与包名一致。**

	- **错误状态**：如果 `.class` 文件**直接躺在当前目录下，JVM 会报错说找不到类。**
	- **正确做法**：必须使用 `javac -d . Test.java`。这样编译器会自**动创建 `com/test/` 目录结构并将 `.class` 文件放进去**，程序才能正常运行。

## Lab2

#### **1. 关于 ArrayList 泛型警告的思考**

- **思考点**：当直接使用 `new ArrayList()` 创建集合时，为什么编译器会报 *"uses unchecked or unsafe operations"* （使用了**未经检查或不安全的操作**）警告？
	- **回答 (Answer)**：
		- **原因**：这是 **JDK 1.5** 以后版本引入的检查机制。老式的 `ArrayList` **默认存储 `Object` 类型，缺乏类型检查。**
		- **解决方式 (Solution)**：虽然可以忽略该警告（不影响运行），但**标准做法是使用 泛型 (Generics)。**
		- **代码示例**：**将 `ArrayList` 改写为 `ArrayList<Account>`，明确告诉编译器这个列表只存 `Account` 对象**。这**类似于 C++ 的模板 (Template)。**

#### **2. 关于单例模式 (Singleton Pattern) 的思考**

- **思考点**：如何修改一个类（如 `Bank`），使其成为单例类？即**如何保证一个类仅有一个实例？**
	- **回答 (Answer)**：需要满足以下三个结构要素（参照讲义中的 `ServicesFactory` 结构）：
		1. **私有构造方法 (Private Constructor)**：**防止外部使用 `new Bank()` 随意创建对象。**
		2. **私有静态变量 (Private Static Field)**：在**类内部持有一个唯一的自身实例（例如 `private static Bank instance`）。**
		3. **公有静态方法 (Public Static Method)**：**提供一个全局访问点（例如 `public static Bank getBanking()`）**，**供外部获取这个唯一的实例。**

#### **3. 关于继承中 super 关键字的思考**

- **思考点**：在子类（如 `SavingsAccount`）的构造方法中，`super()` 的作用是什么？如果我不写它会怎样？
	- **回答 (Answer)**：
		- **作用**：`super()` 用于**调用父类 (`Account`) 的构造方法，以初始化继承来的属性（如 `balance`）。**
		- **规则**：它**必须是子类构造方法中的第一行语句。**
		- *(补充：如果你不写，编译器**会默认插入** `super()` 调用父类的**无参构造方法**。**如果父类没有无参构造方法，编译会报错**。)*

## Lab3

#### **1. 关于接口编译产物的思考**

- **思考点**：实验中的 `TestInterface.java` 程序（包含接口 `Speakable`, `Runner` 和实现类 `Dog`, `Person` 以及主类 `TestInterface`）编译后，一共会生成几个 `.class` 文件？
	- **回答 (Answer)**：**5 个**。
	- **详细列表**：
		1. `Speakable.class` **(接口也会生成 class 文件！)**
		2. `Runner.class` **(接口)**
		3. `Dog.class` (实现类)
		4. `Person.class` (实现类)
		5. `TestInterface.class` (包含 main 方法的主类)
	- **核心结论**：**接口 (Interface)** 在编译后和类一样，**也会生成独立的字节码文件。**

#### **2. 关于接口与抽象类转换的思考**

- **思考点**：如果不用接口，如何用 **抽象类 (Abstract Class)** 来实现同样的功能（让狗既能叫也能跑）？试比较它们的不同。

	- **回答 (Answer)**：

		- **代码改造**：定义一个抽象类（讲义建议命名为 `BaseAction`），将 `speak()` 和 `run()` 定义为 **抽象方法 (abstract methods)**。

		- **代码示例**：

			Java

			```
			public abstract class BaseAction {
			    public abstract void speak();
			    public abstract void run();
			}
			public class Dog extends BaseAction { ... }
			```

	- **本质区别 (Difference)**：

		- **抽象类**：是 **"Is-a"** 关系（狗**是**一种基础动作的集合体，这在逻辑上有点牵强），且 Java 只支持 **单继承**。
		- **接口**：是 **"Can-do"** 关系（狗**会**叫，也**会**跑），Java 支持 **多实现**（`implements Speakable, Runner`），设计上更灵活。

## **UML Class Diagram (类图)**，要求你“Write the code for the following class” (根据下图编写类代码)。

![Image of UML class diagram structure](https://encrypted-tbn2.gstatic.com/licensed-image?q=tbn:ANd9GcRR48xg6URi-Fdgo0N_lg9xRAi41xY4y9OIPI0lkHoNwkmYA8IMO2XEkUCo7P_HWCTLTwd7I9vBFLdLRTU2aH_CSCTwEzFSqfVcPfmFX-cuUKEWY7I)

你需要能够从图中精确提取出**类名**、**变量**、**方法**以及**修饰符**。以下是严格的对应规则总结：

### 1. 类图的基本结构 (Structure of Class Box)

UML 类图通常是一个矩形，被横线分为三部分：

1. **Top Section (顶部)**: **Class Name (类名)**。
	- 如果是**斜体** (*Italic*)，表示这是一个 **Abstract Class (抽象类)**。
	- 如果是正常字体，表示这是一个普通类。
2. **Middle Section (中间)**: **Data Fields (数据域/成员变量)**。
3. **Bottom Section (底部)**: **Constructors and Methods (构造方法与普通方法)**。

------

### 2. 符号与修饰符的对应 (Visibility Modifiers) —— **最易扣分点**

在变量或方法名前面的符号，决定了你在代码中必须写 `public`, `private` 还是 `protected`。

| **符号 (Symbol)** | **关键字 (Keyword)** | **含义 (Meaning)**                    |
| ----------------- | -------------------- | ------------------------------------- |
| **`+`**           | `public**`**         | **公共的，**任何地方都能访问。        |
| **`-`**           | **`private`**        | **私有的**，只能在本类内部访问。      |
| **`#`**           | **`protected`**      | **受保护的，**子类和同包类可访问。    |
| (无符号)          | (缺省/package)       | 只能**在同一个包 (package) 内访问**。 |

- **真题提示**: 绝大多数数据域（Data Fields）在真题中都是 **`-` (private)**，而构造方法和普通方法通常是 **`+` (public)**。一定要看清符号！

------

### 3. 特殊格式的含义 (Special Formatting)

这是区分高级特性的关键：

1. **下划线 (Underlined)**: 表示 **`static` (静态)**。
	- *UML*: `<u>numberOfObjects</u> : int`
	- *Code*: `private static int numberOfObjects;` (假设前面有 `-`)
	- *UML*: `<u>getNumberOfObjects()</u> : int`
	- *Code*: `public static int getNumberOfObjects() { ... }`
2. **斜体 (Italic)**: 表示 **`abstract` (抽象)**。
	- *UML*: *`getArea() : double`*
	- *Code*: `public abstract double getArea();`
	- *注意*: 如果**类名是斜体**，类定义必须写 `public abstract class Name ...`。

------

### 4. 数据域与方法的写法 (Fields & Methods Syntax)

UML 中的写法与 Java 代码的顺序通常是**反的**（**名字在前，类型在后**）。

#### A. 数据域 (Data Fields)

- *UML*: `radius : double`
- *Code*: `private double radius;` (假设**前缀是 `-`)**
- *UML*: `name : String = "Unknown"`
- *Code*: `private String name = "Unknown";` (带有默认值)

#### B. 构造方法 (Constructors)

- *特征*: 名字与类名相同，且**没有返回类型**。

- *UML*: `Circle(newRadius : double)`

- *Code*:

	```
	public Circle(double newRadius) {
	    this.radius = newRadius;
	}
	```

#### C. 普通方法 (Methods)

- *UML*: `getArea() : double`
- *Code*: `public double getArea() { return ...; }`
- *UML*: `setRadius(newRadius : double) : void`
- *Code*: `public void setRadius(double newRadius) { ... }`

------

### 5. 类之间的关系 (Relationships) —— **箭头含义**

在编程题中，你必须关注连接两个类方框的**连线**：

1. **实线 + 空心三角形 (Solid line + Hollow Triangle)**:
	- **含义**: **Inheritance (继承)** -> **`extends`**
	- *Code*: `public class Circle extends GeometricObject { ... }`
	- *方向*: 箭头指向父类 (Superclass)。
2. **虚线 + 空心三角形 (Dashed line + Hollow Triangle)**:
	- **含义**: **Implementation (实现接口)** -> **`implements`**
	- *Code*: `public class MyHandler implements EventHandler<ActionEvent> { ... }`

------

### 6. 实战演练 (Exam Simulation)

假设你在试卷上看到这样一个框：

| **Circle**                          |
| ----------------------------------- |
| - radius : double                   |
| <u>- numberOfObjects : int</u>      |
| + Circle()                          |
| + Circle(radius : double)           |
| + getRadius() : double              |
| <u>+ getNumberOfObjects() : int</u> |

你需要写出的代码结构如下：

```java
public class Circle {
    // 1. 变量 (注意 private 和 static)
    private double radius;
    private static int numberOfObjects;

    // 2. 构造方法 (注意无返回值)
    public Circle() {
        // ... logic
    }

    public Circle(double radius) {
        this.radius = radius;
        // ... logic
    }

    // 3. 方法 (注意 public 和 static)
    public double getRadius() {
        return radius;
    }

    public static int getNumberOfObjects() {
        return numberOfObjects;
    }
}
```

# 疑问

## 再为我详细解释一下UML图中类之间的箭头指向关系 (Relationships) 含义,用谁指谁

### 1. 继承 (Generalization / Inheritance) —— **最核心**

这是考试中出现频率最高的箭头，决定了类的层级关系。

- **符号**: **实线 + 空心三角形 (Solid line + Hollow Triangle)**

- **含义**: **"Is-A"** 关系（是一种）。表示子类继承父类。

- **方向**: 三角形**指向 Parent Class (父类)。**

- **代码对应关键字**: **`extends`**

- **真题示例**:

	- 如果 `Circle` 发出一个**实线空心三角**指向 `GeometricObject`。

	- **代码**:

		Java

		```
		public class Circle extends GeometricObject {
		    // ...
		}
		```

------

### 2. 实现 (Realization / Implementation) —— **接口必考**

通常出现在涉及 `Comparable`、`Runnable` 或 `EventHandler` 的题目中。

- **符号**: **虚线 + 空心三角形 (Dashed line + Hollow Triangle)**

- **含义**: **"Implements"** 关系。表示一个类实现了一个接口。

- **方向**: 三角形**指向 Interface (接口)。**

- **代码对应关键字**: **`implements`**

- **真题示例**:

	- 如果 `MyTask` 发出一个虚线空心三角指向 `Runnable`。

	- **代码**:

		Java

		```
		public class MyTask implements Runnable {
		    @Override
		    public void run() { ... }
		}
		```

------

### 3. 关联 (Association) —— **变成成员变量**

这是最基础的**“拥有”关系。**

- **符号**: **实线 + 普通箭头 (Solid line + Open Arrowhead)**

- **含义**: **"Has-A"** 关系。表示一个类知道另一个类的存在，或者持有另一个类的引用。

- **方向**: 箭头**指向 被引用的类 (The class being used)。**

- **代码对应**: 变成类中的 **Data Field (数据域/成员变量)**。

- **真题示例**:

	- 如果在 `Student` 类和 `Course` 类之间有一条实线，箭头指向 `Course`。

	- **代码**:

		Java

		```
		public class Student {
		    // 箭头指向谁，谁就是这里的变量类型
		    private Course course;
		}
		```

------

### 4. 聚合与组合 (Aggregation & Composition) —— **菱形符号**

这两种在代码实现上非常相似，通常都体现为成员变量，但在逻辑强弱上有区别。讲义中通常会有区分，但在“写代码”这类题目中，处理方式基本一致。

#### A. 聚合 (Aggregation) - 弱关系

- **符号**: **实线 + 空心菱形 (Hollow Diamond)**
- **含义**: **"Has-A"** 关系，但是是松散的。部分可以脱离整体存在。
	- *例子*: 学生 (Student) 和 社团 (Club)。学生退社了，学生还在。
- **方向**: 菱形**画在 Container (整体/容器) 那一端。**
- **代码**: 同样是作为成员变量。

#### B. 组合 (Composition) - 强关系

- **符号**: **实线 + 实心菱形 (Solid Diamond)**
- **含义**: **"Contains"** 关系，是**独占**的。部分不能脱离整体存在。
	- *例子*: 书 (Book) 和 页 (Page)。书销毁了，页也就没了。
- **方向**: 菱形**画在 Owner (拥有者) 那一端。**
- **代码**: 作为成员变量。
	- *注意*: 在组合关系中，通常会在构造方法中直接 `new` 那个对象，表示严格的生命周期绑定。

------

### 总结对照表 (Cheat Sheet for Exam)

你在考试看图写代码时，只需要按照下表机械转换即可：

| 图形符号 (Symbol)           | 含义 (Meaning)                              | **代码关键字/写法 (Code)**               |
| --------------------------- | ------------------------------------------- | ---------------------------------------- |
| **实线 + 空心三角** `────▷` | 继承 (Inheritance)                          | **`extends`**                            |
| **虚线 + 空心三角** `----▷` | 实现 (Implementation)                       | **`implements`**                         |
| **实线 + 普通箭头** `────>` | **关联 (Association)**                      | 定义为 **成员变量** (private Type name;) |
| **实线 + 菱形** `◇────`     | **聚合/组合 (Aggregation)(看哪边可否脱离)** | 定义为 **成员变量** (private Type name;) |

Export to Sheets

**考试重要提示**: 千万不要搞混 **空心三角 (extends)** 和 **普通箭头 (变量)**。

- 三角意味着“它是...” (父子关系)。
- 箭头意味着“它有...” (持有关系)。

## print(), println(), printf()则三个的区别是什么

通常会在 **"Show the output" (写出运行结果)** 的题目中考察它们的细微区别，特别是**换行 (Newline)** 和 **格式化 (Formatting)** 的行为。

以下是严格基于讲义的对比总结：

### 1. `print(item)`

- **行为 (Behavior)**:

	- 打印内容到控制台。
	- **不换行 (No Newline)**: 打印结束后，光标停留在当前行的**末尾**。下一次打印会紧接着这一行继续。

- **示例**

	```
	System.out.print("Java");
	System.out.print("Programming");
	// 输出: JavaProgramming (连在一起，没有空格，没有换行)
	```

### 2. `println(item)`

- **行为 (Behavior)**:

	- 打印内容到控制台。
	- **自动换行 (With Newline)**: 打印结束后，光标移动到**下一行的开头** (Moves cursor to the next line)。
	- *特例*: `System.out.println();` 不带参数时，仅用于输出一个换行符。

- **示例**:

	```
	System.out.println("Java");
	System.out.println("Programming");
	// 输出:
	// Java
	// Programming
	```

### 3. `printf(format, items...)`

- **行为 (Behavior)**:

	- **格式化输出 (Formatted Output)**: 允许你控制数字的小数位、对其方式等。
	- **不自动换行 (No Newline by default)**: 除非你在格式字符串中显式加上 `\n` 或 `%n`，否则它和 `print()` 一样，光标停留在末尾。

- **语法**: `printf(formatString, argumentList)`

- **关键格式符 (Format Specifiers)** (考试必背):

	- `%d`: 整数 (int)
	- `%f`: 浮点数 (float/double) -> **常考**: `%.2f` (保留两位小数)
	- `%s`: 字符串 (String)

- **示例**:

	```
	double price = 12.3456;
	System.out.printf("Price is %.2f", price);
	System.out.print(" dollars");
	// 输出: Price is 12.35 dollars (注意 12.35 紧接着 dollars，且四舍五入)
	```

------

### 总结对比表 (Cheat Sheet)

| **方法**        | **全称**        | **是否自动换行?**    | **主要用途**                 |
| --------------- | --------------- | -------------------- | ---------------------------- |
| **`print()`**   | Print           | **No**               | 最基础的连续输出             |
| **`println()`** | Print Line      | **Yes**              | 输出完一行后结束             |
| **`printf()`**  | Print Formatted | **No** (除非加 `\n`) | 控制小数位数、对齐、特定格式 |

### 考试常见陷阱 (Exam Trap)

在阅读代码题中，如果看到这样的组合：

Java

```
System.out.print("A");
System.out.println("B");
System.out.print("C");
```

**正确输出是**:

Plaintext

```
AB
C
```

(因为 A 后面没换行，直接接 B；B 打完换行了，所以 C 在下一行)。

## 解释一下这个图中的代码,并告诉我编译后的结果

![image-20251224101615388](./%E8%AF%BE%E5%86%85java%E5%A4%8D%E4%B9%A0/image-20251224101615388.png)

通过**匿名内部类（Anonymous Inner Class）** 的方式为一个按钮注册了事件监听器。这通常用于 JavaFX 或 Swing 等 GUI（图形用户界面）编程中。

以下是对这段代码的详细解释以及它编译后的结果：

### 1. 代码逐行解释

这段代码的目的是告诉程序：“当用户点击 `btEnlarge` 这个按钮时，执行大括号里的代码。”

- **`btEnlarge.setOnAction(...)`**:
	- `btEnlarge` 是一个按钮对象（Button）。
	- `setOnAction` 是一个**方法**，用于给这个按钮**设置点击时要触发的动作。**
- **`new EventHandler<ActionEvent>() { ... }`**:
	- 这里定义并实例化了一个**匿名内部类**。
	- 它实现了 `EventHandler` 接口（专门用于处理 `ActionEvent` 类型的时间）。
	- 之所以叫“匿名”，是因为这个类没有名字，它是在创建对象的那一刻临时定义的。
- **`@Override`**:
	- 这是一个注解，表示下面的 `handle` 方法**是覆盖（重写）了接口中的方法**。这是一种良好的编程习惯，可以**帮助编译器检查错误。**
- **`public void handle(ActionEvent e) { ... }`**:
	- **这是 `EventHandler` 接口中必须实现的方法。**
	- 当按钮被点击时，Java 系统会自动调用这个 `handle` 方法。
	- `// Code for processing event e`: 这里是注释位置，你需要在这里填入真正想要执行的逻辑（比如放大图片的具体代码）。

------

### 2. 编译后的结果（重点）

这是很多初学者容易忽略的地方。虽然这段代码写在另一个类（外部类）的内部，但在 Java 编译（javac）之后，它会生成一个独立的字节码文件。

假设这段代码写在一个名为 `MainFrame` 的类中：

1. **外部类文件**：你会得到一个 `MainFrame.class` 文件。
2. **匿名内部类文件**：你会得到一个名为 **`MainFrame$1.class`** 的文件。

**命名规则解释：**

- **`MainFrame`**：包含这段代码的外部类的名字。
- **`$`**：分隔符。
- **`1`**：数字。因为匿名内部类没有名字，编译器会**按照它们在代码中出现的顺序，从 1 开始自动编号**。如果有第二个匿名内部类，就会生成 `MainFrame$2.class`。

### 3. 补充知识：现代写法 (Lambda 表达式)

这段代码展示的是 Java 8 之前的经典写法（现在依然有效但显得啰嗦）。在现代 Java（Java 8 及以上）中，通常会使用 **Lambda 表达式** 来简化这种代码：

```
btEnlarge.setOnAction(e -> {
    // Code for processing event e
});
```

Lambda 写法更加简洁，但理解图片中的匿名内部类原理对于掌握 Java 的底层机制（如作用域、编译原理）非常重要。

### 总结

这张图演示了**如何使用匿名内部类来处理按钮点击事件**。编译后，这个“匿名”的类会变成一个**带有美元符号和数字后缀的独立 `.class` 文件**（例如 `OuterClass$1.class`）。

------

**你可以让我继续：**

- 为你解释如何将这段代码改写为 Lambda 表达式？
- 解释为什么匿名内部类中使用的外部变量必须是 final 或 effectively final？

### 1. 为什么这里没有 `implements` 或 `extends` 关键字？

你可能会觉得奇怪，通常我们实现接口都要写 `class MyHandler implements EventHandler`，但这里直接就是一个 `new`。

**原因：** 这是 Java 提供的**语法糖（Syntactic Sugar）**。 **在 `new EventHandler<ActionEvent>() { ... }` 这行代码中，Java 编译器帮你在幕后做了两件事的结合体：**

1. **定义了一个类**：这个类没有名字，但编译器知道它**实现（implements）** 了 `EventHandler` 接口。
2. **创建了对象**：立即创建了这个无名类的一个实例。

**规则是这样的：**

- 如果你 `new` 的是一个**接口**（如本例），匿名内部类其实**默认隐式地 `implements` 了这个接口。**
- 如果你 `new` 的是一个**类**（比如 `new Thread() { ... }`），匿名内部类**其实默认隐式地 `extends` 了这个父类。**

因为语法要求**匿名类必须在定义的同时实例化**，所以**没有地方让你写 `extends` 或 `implements` 关键字**，它是**通过 `new` 后面的类型自动推断的。**

------

### 2. 分隔符都是用的 `$` 吗？

**是的，在标准的 Java 编译器（javac）中，内部类的编译产物文件名分隔符就是 `$`。**

这是 Java 虚拟机（JVM）识别内部类的一种约定俗成的方式。

- **具名内部类（Named Inner Class）**： 如果你在 `Main` 类里写了一个名叫 `MyHelper` 的内部类，编译后文件名为：`Main$MyHelper.class`。
- **匿名内部类（Anonymous Inner Class）**： 因为它没有名字，编译器就给它分配数字编号，按出现顺序排列。
	- 第一个出现的匿名类：`Main$1.class`
	- 第二个出现的：`Main$2.class`

**注意**：正因为 Java 编译器使用 `$` 作为分隔符，我们在给普通的变量或类命名时，虽然语法允许使用 `$`（比如 `int money$ = 10;`），但**强烈不建议**在代码中主动使用 `$`，以免引起混淆或工具解析错误。

------

### 3. "要是匿名类而不是匿名内部类呢?"

这是一个非常敏锐的概念辨析，但实际上在 Java 中：**所有的匿名类（Anonymous Class），本质上一定都是内部类（Inner Class）。**

为什么这么说？

**根本原因：** Java **不允许在文件级别（Top-level）定义“没有名字的类”**。一个 `.java` 文件里，**顶层类必须有名字（public class X...）**。既然匿名类没有名字，它就**必须**寄生在另一个类的方法里、或者是作为成员变量存在。只要是寄生在别的类里面的，它就是“内部类”。

不过，你可能想问的是下面这两种情况的区别：

#### 情况 A：你指的是“基于类的匿名类” vs “基于接口的匿名类”？

- **图片中的情况（基于接口）**： `new EventHandler() { ... }` -> 这是一个实现了接口的匿名内部类。

- **基于类的匿名类**： 假设你有一个普通的类 `Person`，你可以这样写：

	Java

	```
	// 这里的匿名类继承(extends)了 Person 类，而不是实现接口
	Person p = new Person() {
	    @Override
	    public void speak() {
	        System.out.println("我是匿名子类");
	    }
	};
	```

	**编译结果**：依然是 `OuterClass$1.class`。对编译器来说，无论是继承类还是实现接口，**只要是匿名的，处理方式都是一样的**。

#### 情况 B：你指的是“静态嵌套类（Static Nested Class）”？

虽然匿名类总是内部类，但它们确实持有外部类的引用（导致内存泄漏的常见原因）。如果你的意思是“有没有一种匿名类是不依赖外部类实例的？”，答案是：**没有**。

- **匿名类总是非静态的**。它们总是**隐式地持有外部对象的一个引用（`this$0`）。**
- 如果你想切断这个联系，必须显式地定义一个 `static class`（静态内部类），但这就不叫“匿名”了，因为定义 `static class` 必须要有名字。

### 总结

1. **没有 extends/implements**：是因为 `new Type() { ... }` 语法本身就包含了“继承”或“实现”的含义。
2. **分隔符 $**：是的，`外部类$1.class\` 是标准命名，数字代表它是第几个匿名类。
3. **匿名类 vs 匿名内部类**：在 Java 中它们是同一个东西。只要是匿名的，它就一定是内部类（因为它必须依附于某个外部作用域存在）。

## 改为new Object()呢?为什么这里没有extend接口?分隔符都是用的$吗?要是匿名类而不是匿名内部类呢?

![image-20251217144453757](./%E8%AF%BE%E5%86%85java%E5%A4%8D%E4%B9%A0/image-20251217144453757.png)

首先这里**一定要强转**才能到**实际运行时对象**

o instanceof Circle` 返回 `**false**,此时 `((Circle) o).getDiameter()`运行时会抛出 `ClassCastException`。

## 详细解释一下多态是什么意思

**多态 (Polymorphism)** 意味着“**同一个类型的变量，在指向不同的对象时，调用同一个方法(完全一致)会表现出不同的行为**”,因为**父类可以指向多个子类**。

### 1. 多态的三大要素（核心公式）

要实现多态，必须同时满足这三个条件：

1. **继承 (Inheritance)**：必须有**父类和子类**（比如 `Dog` 继承 `Animal`）。

2. **重写 (Overriding)**：**子类必须重新定义父类**的方法（比如 `Dog` 重写了 `makeSound()`）。

3. **父类引用指向子类对象 (Upcasting)**：这是最关键的一步。

	```
	Animal a = new Dog(); // 重点！
	```

### 2. 核心机制：编译看左，运行看右

这是做题时的**黄金法则**，请务必背下来：

当使用多态方式调用方法时（例如 `a.makeSound()`）：

- **编译时 (Compile-time)**：**编译器看左边**（引用类型 `Animal`）。**如果 `Animal` 类里没有 `makeSound` 方法，编译直接报错。**
- **运行时 (Runtime)**：**JVM 看右边（实际对象 `Dog`）**。真正执行的是 `Dog` 类里重写过的 `makeSound` 方法。这就是**动态绑定 (Dynamic Binding)**。

------

### 3. 代码实例解析

我们来看一个经典的考试模型：

```
// 1. 父类
class Animal {
    public void shout() {
        System.out.println("动物叫");
    }
}

// 2. 子类 Cat
class Cat extends Animal {
    @Override // 重写
    public void shout() {
        System.out.println("喵喵喵");
    }
}

// 3. 子类 Dog
class Dog extends Animal {
    @Override // 重写
    public void shout() {
        System.out.println("汪汪汪");
    }
}

public class TestPolymorphism {
    public static void main(String[] args) {
        // 父类引用指向子类对象
        Animal a1 = new Cat();
        Animal a2 = new Dog();

        a1.shout(); // 输出：喵喵喵 (执行的是 Cat 的方法)
        a2.shout(); // 输出：汪汪汪 (执行的是 Dog 的方法)
    }
}
```

**为什么要这样写？** 你可能会问：“为什么不直接 `Dog d = new Dog();`？” **好处**：灵活性。如果我写一个方法 `public void feed(Animal a)`，那么**我可以传 `Dog` 进去，也可以传 `Cat` 进去。**这个方法不**需要修改任何代码就能处理所有 `Animal` 的子类**。这符合**软件工程的“开闭原则”。**

------

### 4. 考试中的“致命陷阱”

多态**只适用于实例方法 (Instance Methods)**。

- **成员变量 (Fields)**：**不**具备多态性。**编译运行都看左边**。
- **静态方法 (Static Methods)**：**不**具备多态性。**编译运行都看左边**。

**陷阱题演示：**

```
class Father {
    int money = 100;
    static void say() { System.out.println("Father says"); }
}

class Son extends Father {
    int money = 200;
    static void say() { System.out.println("Son says"); }
}

public class Test {
    public static void main(String[] args) {
        Father f = new Son(); // 多态
        
        // 问：输出什么？
        System.out.println(f.money); 
        f.say(); 
    }
}
```

- `f.money` 输出 **100** (看左边 Father)。
- `f.say()` 输出 **"Father says"** (静态方法看左边 Father)。
- 只有普通实例方法，才会看右边 Son。

## java中为什么要分interface和abstract class这两个东西

既然你熟悉 C++，你会发现 **C++ 允许“多重继承”**（即一个类**可以同时继承 A 和 B**），但这会带来著名的**“菱形继承问题”（Diamond Problem，即如果 A 和 B 都有 `run()` 方法，子类该听谁的？**）。

Java 为了避开这个坑，**强制规定类只能单继承**。但现实世界中，**一个物体往往有多种身份**。为了**弥补单继承的缺陷并提供更灵活的设计**，Java 引入了 `interface`。

为什么要分这两个？核心区别在于：**设计意图的不同**。

### 1. 核心区别：身份 (Is-a) vs. 能力 (Can-do)

- **Abstract Class (抽象类) —— 定义“它是谁”**
	- **概念**：它是对一类事物的**抽象**。它代表了**“Is-a”（是一个）**的关系。
	- **作用**：**代码复用**。它不仅定义了**“哪怕我不懂怎么做，但你必须有这个功能”（抽象方法）**，还可以包含子类通用的**属性**和**实现代码**（**非抽象方法**）。
	- **例子**：`Dog` **is an** `Animal`。`Animal` 是抽象类，因为**所有动物都有 `age`（属性）和 `eat()`（通用行为）**，但 **`move()`（抽象方法）具体怎么动，由狗自己定**。
- **Interface (接口) —— 定义“它能干什么”**
	- **概念**：它是对行为的**契约**。它代表了**“Can-do”（能做）**的关系。
	- **作用**：**解耦**。它不管你是谁，只要**你签了这个“合同”（实现了接口），我就能用你。**
	- **例子**：`Dog` **can** `Swim`，`Submarine` **can** `Swim`。狗和潜水艇风马牛不相及，**不能强行让它们继承同一个父类，但它们都可以实现 `Swimmable` 接口。**

------

### 2. 为什么不能只用其中一个？

- **为什么不能只用抽象类？**
	- 因为 Java 是**单继承**的。如果你让 `SmartPhone` 继承了 `Phone` 类，它就不能再继承 `Camera` 类了。但你**可以让它继承 `Phone` (身份)，同时实现 `Camera` 和 `MusicPlayer` 接口 (能力)**。
- **为什么不能只用接口？**
	- 接口（在 Java 8 之前）**不能有普通成员变量（只能有 `static final` 常量），也不能有构造方法**。如果你有一堆子类都需要共享同一个变量（比如 `name`）或者同一段复杂的逻辑代码，**用接口的话，每个子类都得把这代码复制粘贴一遍**，**违背了 DRY (Don't Repeat Yourself)** 原则。

------

### 3. 一张表总结（考试必考）

| **特性**     | **Abstract Class (抽象类)**               | **Interface (接口)**               |
| ------------ | ----------------------------------------- | ---------------------------------- |
| **继承关系** | 单继承 (`extends`)                        | **多实现** (`implements`)          |
| **成员变量** | 可以有各种类型的变量 (int, String...)     | 只能是常量 (`public static final`) |
| **构造方法** | **有** (用于**初始化子类继承的通用属性**) | **无** (接口没有“实例”的概念)      |
| **设计语义** | 模板设计，强调**家族血统**                | 插件式设计，强调**功能扩展**       |

> **注意：** Java 8 **引入了接口的 `default method`（默认方法），让接口也能写代码实现**，这让两者界限稍微模糊了一点，但**“接口不能存状态（没有非静态变量）”**依然是死穴。

------

### 4. 场景实战

假设我们要设计一个**报警门 (AlarmDoor)**。

- **方案 A (错误)**：让 `AlarmDoor` 同时继承 `Door` 和 `Alarm`。
	- *Java 报错：不能同时继承两个类！*
- **方案 B (正确)**：
	- `Door` 是本质身份（它是一扇门），所以用 **abstract class Door**（里面可以写 `open()` 和 `close()` 的通用代码）。
	- `Alarm` 是附加功能（它能报警），所以用 **interface Alarm**（里面定义 `alarm()` 方法）。
	- 定义：`class AlarmDoor extends Door implements Alarm { ... }`

## 为什么java中的error是定义在方法后面的?什么时候才会被抛出?为我详细解释一下

“定义在方法后面”，在 Java 术语中叫做 **`throws` 声明**。

这涉及 Java 独有（且常被吐槽）的一个机制：**检查型异常 (Checked Exceptions)**。这也是 Java 和 C++ 在异常处理上最大的不同。

### 1. 为什么写在方法后面？（Warning Label）

**一句话解释：** 这是一个**“免责声明”**或**“预警标签”**。

在 Java 中，编译器非常“婆婆妈妈”。如果一个方法**可能会出事（比如读文件可能会找不到文件）**，编译器强制要求你**必须**告诉调用者：“嘿，用我的时候小心点，我**可能会爆这个错！**”

这就是写在方法后面的原因——它是**方法签名（Contract）**的一部分。

```
// 这里的 throws IOException 就是“预警标签”
public void readFile(String path) throws IOException {
    // ... 可能出事的代码
}
```

- **对于调用者 (Caller)：看到这个标签，就知道必须用 `try-catch` 包住它，或者继续往上抛，否则代码编译不过。**
- **对比 C++**：**C++** 也有 `throw(...)` 语法，但通常是**可选**的。而 **Java 对于某些异常是强制的**。

------

### 2. 什么时候才会被抛出？(The "Boom" Moment)

这里要区分两个长得很像的关键字：**`throws`** (名词，挂牌示众) 和 **`throw`** (动词，扔手雷)。

异常会在以下两种时刻被抛出：

#### **情况 A：JVM 自动抛出（被动触发）**

当代码干了非法的事情，**JVM 也就无法执行**下去了，它**会自动生成一个异常对象并抛出。**

- **例子**：你写了 `1 / 0`，JVM 马上抛出 `ArithmeticException`。
- **例子**：你访问了 `null.method()`，JVM 马上抛出 `NullPointerException`。

#### **情况 B：代码主动抛出（主动触发）**

你在代码里**显式地使用 `throw` 关键字**。这通常用于业务逻辑校验。

```java
public void setAge(int age) throws IllegalArgumentException {
    if (age < 0) {
        // 这里的 throw 才是真正的“动作”！
        // 就像你发现数据不对，手动扔出一个炸弹
        throw new IllegalArgumentException("年龄不能为负数");//还是在这个里面实现的
    }
    this.age = age;
}
```

------

### 3. 核心概念：必检 vs 免检 (Checked vs Unchecked)

为了防止你被绕晕，必须记住这张图的分类，因为**只有一类异常**需要写在方法后面：

1. **免检异常 (Unchecked / RuntimeException)**
	- **特点**：通常是**你代码写得烂**导致的（如数组越界、空指针）。
	- **规则**：**不需要写在方法后面 `throws`**。编译器假装没看见，你自己负责。
	- *例子*：`NullPointerException`, `ArrayIndexOutOfBoundsException`.
2. **必检异常 (Checked Exception)**
	- **特点**：通常是**外部环境**导致的，你**控制不了**（如文件找不到、网络断了）。
	- **规则**：***必须写在方法后面 `throws`，或者当场 `try-catch` 消化掉。(以防止因为这个就跑不起来了)**
	- *例子*：`IOException`, `SQLException`.

------

### ⚡️ 避坑指南（C++ 转 Java 注意）

在 C++ 中，你可能习惯了哪里都能抛异常。但在 Java 中：

- **语法区别**：**`throw` 是动作（在方法体内），`throws` 是声明（在方法头）。**
- **编译强制性**：如果你调用了一个声明了 `throws IOException` 的方法，你**必须处理它,如用try-catch块，否则代码会飘红（编译错误）,再改代码也好。**

## 为我详细解释匿名内部类 anonymous inner class 及其特征？

### 一、匿名内部类（Anonymous Inner Class）的定义

匿名内部类是 Java 中一种**没有显式类名**的特殊内部类，它将「类的定义」和「对象的实例化」合并为一步完成，属于局部内部类的子集（绝大多数场景下定义在方法 / 代码块中）。

它的核心目的是**简化代码**—— 当需要快速实现一个简单的接口、继承抽象类 / 普通类，且该类仅需使用一次时，无需单独定义类文件，直接通过匿名内部类完成。典型场景：线程创建、事件监听、回调函数等。

### 二、匿名内部类的核心特征

| 特征                                        | 详细解释 & 语法约束                                          |
| ------------------------------------------- | ------------------------------------------------------------ |
| 1. 无显式类名，编译器自动生成类名           | ✅ 没有 `class 类名` 的定义语法，编译后编译器会生成形如 `外部类名+$数字` 的类文件（如 `Outer$1.class`）；❌ 无法通过类名引用，只能在创建时直接使用。 |
| 2. 必须继承一个类 OR 实现一个接口（二选一） | 匿名内部类的创建必须基于一个「父类型」：✅ 要么实现**一个接口**（如 `Runnable`）；✅ 要么继承**一个类**（普通类 / 抽象类，如 `Thread`）；❌ 不能同时继承类 + 实现接口（语法不支持）。 |
| 3. 定义即实例化，一次性使用                 | 语法本身是 `new 父类型() { 类体 }`，执行这行代码时：✅ 同时完成「类的定义」+「对象创建」；❌ 无法单独定义类而不实例化，也无法多次实例化（无类名）。 |
| 4. 无法定义构造方法                         | 构造方法名必须与类名一致，但匿名内部类无类名，因此：❌ 不能显式定义构造方法；✅ 替代方案：用「实例初始化块 `{}`」完成构造逻辑（初始化块在对象创建时执行，效果等价于构造方法）。 |
| 5. 可访问外部类 / 局部变量（有约束）        | - 访问外部类成员：能直接访问外部类的所有成员（包括 `private`），若需区分匿名类自身和外部类的 `this`，需用 `外部类名.this`；- 访问局部变量：仅能访问「`final` 或有效 final」的局部变量（Java 8+ 隐式识别「有效 final」，即变量赋值后不再修改）。 |
| 6. 作用域仅限创建语句，无法复用             | 匿名内部类的对象只能在创建它的语句中使用（如赋值给父类型引用、作为方法参数传递），无法在其他位置复用。 |
| 7. 不能包含非 final 的静态成员              | ✅ 仅允许定义 `static final` 常量（编译期确定值）；❌ 不能定义普通静态变量 / 静态方法（匿名类是实例级，静态成员无意义）。 |

### 三、典型使用场景（代码示例）

#### 场景 1：实现接口（如 Runnable 创建线程）

java



运行









```java
public class AnonymousInterfaceDemo {
    public static void main(String[] args) {
        // 匿名内部类实现 Runnable 接口
        Runnable runnable = new Runnable() {
            // 必须实现接口的抽象方法
            @Override
            public void run() {
                System.out.println("匿名内部类实现Runnable，执行线程逻辑");
            }
        };
        new Thread(runnable).start();

        // 简化：直接作为方法参数传递（更常用）
        new Thread(new Runnable() {
            @Override
            public void run() {
                System.out.println("匿名内部类直接作为参数");
            }
        }).start();
    }
}
```

#### 场景 2：继承抽象类

```java
// 定义抽象类
abstract class Animal {
    String name = "动物";
    abstract void makeSound(); // 抽象方法
}

public class AnonymousAbstractDemo {
    public static void main(String[] args) {
        // 匿名内部类继承抽象类，实现抽象方法
        Animal cat = new Animal() {
            // 实现抽象方法
            @Override
            void makeSound() {
                System.out.println(name + "：喵喵喵");
            }

            // 可添加自定义方法（但只能通过向下转型调用，不推荐）
            void scratch() {
                System.out.println("猫抓东西");
            }
        };
        cat.makeSound(); // 输出：动物：喵喵喵
        // cat.scratch(); // 编译报错：Animal 无该方法，需转型：((AnonymousAbstractDemo$1)cat).scratch();
    }
}
```

#### 场景 3：继承普通类 + 初始化块替代构造方法

```java
// 普通父类
class Person {
    String name;
    // 父类构造方法
    Person(String name) {
        this.name = name;
    }
    void sayHello() {
        System.out.println("你好，我是" + name);
    }
}

public class AnonymousNormalClassDemo {
    public static void main(String[] args) {
        // 匿名内部类继承 Person，调用父类构造方法
        Person student = new Person("张三") {
            // 实例初始化块（替代构造方法，执行初始化逻辑）
            {
                this.age = 18; // 初始化自定义成员
                System.out.println("初始化块：年龄=" + age);
            }

            // 自定义成员变量
            int age;

            // 重写父类方法
            @Override
            void sayHello() {
                System.out.println("学生" + name + "（" + age + "岁）：大家好！");
            }
        };
        student.sayHello(); // 输出：学生张三（18岁）：大家好！
    }
}
```

#### 场景 4：访问外部类 / 局部变量



```java
public class AnonymousVarDemo {
    // 外部类私有成员
    private String outerField = "外部类字段";

    public void test() {
        // 局部变量（Java 8+ 隐式 final，无需显式声明）
        String localVar = "局部变量";
        // localVar = "修改"; // 若修改，编译报错：不是有效 final

        // 匿名内部类
        Runnable runnable = new Runnable() {
            @Override
            public void run() {
                // 访问外部类字段
                System.out.println("外部类字段：" + outerField);
                // 访问外部类的 this（区分匿名类自身的 this）
                System.out.println("外部类实例：" + AnonymousVarDemo.this);
                // 访问局部变量（有效 final）
                System.out.println("局部变量：" + localVar);
            }
        };
        runnable.run();
    }

    public static void main(String[] args) {
        new AnonymousVarDemo().test();
    }
}
```

### 四、注意事项 & 易错点

1. **构造方法替代方案**：匿名内部类无构造方法，需用「实例初始化块 `{}`」处理初始化逻辑（如示例 3）。
2. **局部变量的 final 约束**：匿名内部类访问的局部变量必须是 `final` 或「有效 final」（赋值后不修改）。原因：匿名内部类对象可能在局部变量生命周期结束后仍存在（如线程），Java 会复制局部变量的值到匿名类实例中，保证不可变。
3. **this 关键字的区分**：匿名类中 `this` 指向自身实例，若要访问外部类的实例，需写 `外部类名.this`（如示例 4）。
4. **复用性限制**：仅适用于「一次性、简单逻辑」，若逻辑复杂 / 需多次使用，应定义普通内部类或独立类。
5. **静态成员约束**：仅允许 `static final` 常量（编译期确定值），不能定义普通静态变量 / 方法（如 `static int num = 10` 编译报错）。

### 五、核心价值总结

匿名内部类的本质是「语法糖」，核心价值是**简化一次性类的实现成本**，避免为简单逻辑创建独立的类文件。但因「无类名、无法复用、不能定义构造方法」等特征，仅适合轻量级、一次性的使用场景。

## 这道题的完整 Java 实现

```java
import java.util.Arrays;

// 1. 定义StudentScore类，并实现Comparable接口（满足要求3）
public class StudentScore implements Comparable<StudentScore> {
    // 私有成员变量（类图定义）
    private String studentNo;
    private int usualScore;
    private int experimentScore;
    private int examScore;

    // 构造方法（类图定义）
    public StudentScore(String studentNo) {
        this.studentNo = studentNo;
        // 初始分数默认设为0
        this.usualScore = 0;
        this.experimentScore = 0;
        this.examScore = 0;
    }

    // 设置分数的方法（类图定义）
    public void setStudentScore(int usualScore, int experimentScore, int examScore) {
        this.usualScore = usualScore;
        this.experimentScore = experimentScore;
        this.examScore = examScore;
    }

    // 获取最终成绩（修正题目中参数冗余的问题，逻辑更合理）
    public int getFinalScore() {
        // 按公式计算：20%平时 + 20%实验 + 60%考试（用整数运算避免浮点误差）
        return (20 * usualScore + 20 * experimentScore + 60 * examScore) / 100;
    }

    // 获取学号的getter（辅助toString和测试）
    public String getStudentNo() {
        return studentNo;
    }

    // 2. 实现toString方法（满足要求2）
    @Override
    public String toString() {
        return studentNo + ":(" + usualScore + "," + experimentScore + "," + examScore + ")";
    }

    // 3. 实现Comparable接口的compareTo方法（按finalScore比较）
    @Override
    public int compareTo(StudentScore other) {
        // 升序排列：当前对象finalScore - 目标对象finalScore
        int thisFinal = this.getFinalScore();
        int otherFinal = other.getFinalScore();
        return thisFinal - otherFinal;
    }

    // 4. 测试：对StudentScore数组排序（满足要求4）
    public static void main(String[] args) {
        // 创建StudentScore数组
        StudentScore[] scoreArray = new StudentScore[3];

        // 初始化学生1
        scoreArray[0] = new StudentScore("S001");
        scoreArray[0].setStudentScore(80, 90, 85);

        // 初始化学生2
        scoreArray[1] = new StudentScore("S002");
        scoreArray[1].setStudentScore(75, 85, 90);

        // 初始化学生3
        scoreArray[2] = new StudentScore("S003");
        scoreArray[2].setStudentScore(90, 80, 75);

        // 排序前输出
        System.out.println("排序前：");
        for (StudentScore student : scoreArray) {
            System.out.println(student + " —— 最终成绩：" + student.getFinalScore());
        }

        // 调用Arrays.sort排序（依赖Comparable接口实现）
        Arrays.sort(scoreArray);

        // 排序后输出
        System.out.println("\n排序后（按最终成绩升序）：");
        for (StudentScore student : scoreArray) {
            System.out.println(student + " —— 最终成绩：" + student.getFinalScore());
        }
    }
}
```

### 代码说明：

1. **StudentScore 类实现（要求 1）**：
	- 包含类图定义的私有成员变量（`studentNo`、`usualScore`等）。
	- 实现了指定的构造方法和`setStudentScore`方法。
	- `getFinalScore`按题目公式计算最终成绩（用整数运算避免浮点精度误差）。
2. **toString 方法（要求 2）**：
	- 输出格式严格匹配题目要求：`studentNo:(usualScore,experimentScore,examScore)`（例如：`S001:(80,90,85)`）。
3. **Comparable 接口实现（要求 3）**：
	- 类实现`Comparable<StudentScore>`接口，`compareTo`方法通过`getFinalScore`获取最终成绩并比较，实现按最终成绩升序排列。
4. **数组排序（要求 4）**：
	- 在`main`方法中创建`StudentScore`数组，初始化数据后，调用`Arrays.sort`（该方法会自动调用`compareTo`实现排序），并输出排序前后的结果。

### 运行结果示例：

```plaintext
排序前：
S001:(80,90,85) —— 最终成绩：85
S002:(75,85,90) —— 最终成绩：86
S003:(90,80,75) —— 最终成绩：79

排序后（按最终成绩升序）：
S003:(90,80,75) —— 最终成绩：79
S001:(80,90,85) —— 最终成绩：85
S002:(75,85,90) —— 最终成绩：86
```

## 定义一个 EncryptFile 类，用于加密 sourceFile 文件中的digit，并存储到 newFile 中。

命令行为 java EncryptFile sourceFile newFile

```java
//import java.io.*;
//import java.util.*;
import java.io.File;
import java.io.FileNotFoundException;
import java.io.PrintWriter;
import java.util.Scanner;

public class ScoreAverageCalculatorWithScanner {
    public static void main(String[] args) {
        // 校验命令行参数
        if (args.length != 2) {
            System.out.println("Usage: java ScoreAverageCalculatorWithScanner sourceFile newFile");
            return;
        }
        String sourceFileName = args[0];
        String newFileName = args[1];
        
        File sourceFile = new File(sourceFileName);
        File newFile = new File(newFileName);
        Scanner scanner = null;
        PrintWriter printWriter = null; // 替换FileWriter为PrintWriter
        
        try {
            scanner = new Scanner(sourceFile);
            // 初始化PrintWriter（支持直接写入File对象，自动处理字符编码）
            printWriter = new PrintWriter(newFile); 
            
            // 逐行读取源文件（与原示例逻辑一致）
            while (scanner.hasNextLine()) {
                String line = scanner.nextLine();
                StringBuilder encryptedLine = new StringBuilder();
                
                // 遍历字符加密数字
                for (char c : line.toCharArray()) {
                    if (Character.isDigit(c)) {
                        int originalNum = c - '0';
                        int encryptedNum = (originalNum + 4) % 10;
                        encryptedLine.append(encryptedNum);
                    } else {
                        encryptedLine.append(c);
                    }
                }
                
                // PrintWriter核心写入方法：println自动补充换行符（跨平台兼容）
                printWriter.println(encryptedLine.toString());
            }
            
            System.out.println("Encryption completed successfully!");
        } catch (FileNotFoundException e) {
            // PrintWriter初始化时若文件无法创建/读取，也会抛FileNotFoundException
            System.out.println("File not found or cannot be created: " + e.getMessage());
        } finally {
            // 手动关闭资源（与原示例风格一致）
            if (scanner != null) {
                scanner.close();
            }
            if (printWriter != null) {
                printWriter.close(); // PrintWriter.close()无检查异常，无需嵌套try-catch
            }
        }
    }
}
```

