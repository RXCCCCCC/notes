这是一份为你精心整理的 **Java 事件驱动编程与 Lambda 表达式** 期末复习讲义。这份讲义基于你提供的第15章课件，结合了软件工程专业考试常见的考点、代码实现以及与C++的对比。

---

# ☕️ Java 期末复习讲义：事件驱动与 Lambda 表达式

**适用范围**：JavaFX 事件处理、内部类、Lambda 表达式、Observable 对象
**核心思想**：从“顺序执行”转变为“等待响应”。

---

## 1. 核心概念：过程式 vs. 事件驱动

*   **过程式编程 (Procedural)**：
    *   程序按照代码编写的顺序（顺序、循环、分支）一步步执行。
    *   控制权在程序手中。
*   **事件驱动编程 (Event-Driven)**：
    *   程序启动后进入“等待”状态。
    *   代码的执行由**外部事件**（Events）触发（如鼠标点击、键盘按下）。
    *   **核心机制**：信号（Signal）-> 响应（Response）。

---

## 2. Java 事件委托模型 (The Delegation Model)

这是考试中关于架构设计的核心考点。Java 采用“委托”机制，即组件本身不处理事件，而是委托给监听器处理。

### 三大要素 (必背)
1.  **事件源 (Source Object)**：谁产生了事件？
    *   例如：`Button`, `TextField`。
    *   方法：`getSource()` 可获取源对象。
2.  **事件对象 (Event Object)**：发生了什么事？
    *   封装了事件的详细信息（如鼠标点击的坐标、按下的键位）。
    *   继承自 `java.util.EventObject`。
    *   分类：
        *   `ActionEvent` (按钮点击、输入框回车) —— **最常用**
        *   `MouseEvent` (鼠标移动、点击)
        *   `KeyEvent` (键盘按键)
3.  **事件监听器 (Event Listener)**：谁来处理？
    *   必须实现特定的**接口**（如 `EventHandler<T>`）。
    *   必须注册到事件源上（`source.setOnAction(listener)`）。

### 代码流程 (标准模板)

```java
public class HandleEvent extends Application {
    @Override // Override the start method in the Application class
    public void start(Stage primaryStage) {
        // Create a pane and set its properties
        HBox pane = new HBox(10);
        pane.setAlignment(Pos.CENTER);
        Button btOK = new Button("OK");
        Button btCancel = new Button("Cancel");
        OKHandlerClass handler1 = new OKHandlerClass();
        btOK.setOnAction(handler1);
        CancelHandlerClass handler2 = new CancelHandlerClass();
        btCancel.setOnAction(handler2);
        pane.getChildren().addAll(btOK, btCancel);

        // Create a scene and place it in the stage
        Scene scene = new Scene(pane);
        primaryStage.setTitle("HandleEvent"); // Set the stage title
        primaryStage.setScene(scene); // Place the scene in the stage
        primaryStage.show(); // Display the stage
    }

    class OKHandlerClass implements EventHandler<ActionEvent> {
        @Override
        public void handle(ActionEvent e) {
            System.out.println("OK button clicked");
        }
    }

    class CancelHandlerClass implements EventHandler<ActionEvent> {
        @Override
        public void handle(ActionEvent e) {
            System.out.println("Cancel button clicked");
        }
    }
}

```



```java
// 1. 创建源
Button btOK = new Button("OK");

// 2. 创建监听器 (这里假设 OKHandlerClass 实现了 EventHandler 接口)
OKHandlerClass handler = new OKHandlerClass();

// 3. 注册 (委托)
btOK.setOnAction(handler); 
```

---

## 3. 实现监听器的四种方式 (进化史)

这是编程题和代码填空题的重点，请务必掌握**从 2 到 4 的演变**。

### 方式 1：外部类 (Separate Class)
*   **做法**：单独写一个 `.java` 文件实现 `EventHandler`。
*   **缺点**：难以访问主界面的私有组件（TextField 等），实际开发极少使用。

### 方式 2：内部类 (Inner Class) —— 重点
*   **定义**：定义在另一个类（外部类）内部的类。
*   **优点**：
    *   可以直接访问外部类的**私有成员**（这是最大优势）。
    *   代码组织更紧凑。
*   **编译结果**：`OuterClass$InnerClass.class` (注意美元符号 `$`，选择题常考)。

```java
public class MainFrame extends Application {
    private int count = 0; // 私有变量

    // 内部类
    class MyHandler implements EventHandler<ActionEvent> {
        @Override
        public void handle(ActionEvent e) {
            count++; // 直接访问外部类私有变量
            System.out.println("Clicked " + count);
        }
    }
    // 使用： button.setOnAction(new MyHandler());
}
```

### 方式 3：匿名内部类 (Anonymous Inner Class) —— 重点
*   **定义**：没有名字的内部类，声明和创建实例一步完成。
*   **语法**：`new 父类/接口() { // 重写方法 }`。
*   **特点**：
    *   必须继承一个类或实现一个接口。
    *   使用父类的无参构造器。
    *   **编译结果**：`OuterClass$1.class` (按顺序编号 $1, $2...)。

```java
button.setOnAction(new EventHandler<ActionEvent>() {
    @Override
    public void handle(ActionEvent e) {
        System.out.println("Anonymous Clicked");
    }
});
```

### 方式 4：Lambda 表达式 (Java 8+) —— 必考
*   **本质**：匿名方法的简写，用于简化**函数式接口**（SAM）。
*   **函数式接口 (SAM)**：Single Abstract Method，接口中**只有**一个抽象方法。

```java
// 极其简洁
button.setOnAction(e -> {
    System.out.println("Lambda Clicked");
});
```

---

## 4. Lambda 表达式详解 (核心语法)

### 基础语法
`(参数类型 参数名, ...) -> { 代码块; }`

### 简写规则 (课件重点 Slide 26)
这是容易扣分的地方，请记住什么时候可以省略什么：

1.  **省略参数类型**：
    *   `(ActionEvent e) -> ...` 可写成 `(e) -> ...`
    *   **坑点**：要么所有参数都写类型，要么都不写，不能混用。
2.  **省略参数小括号**：
    *   **只有 1 个参数**且不写类型时：`e -> ...`
    *   无参或多参必须保留括号：`() -> ...` 或 `(x, y) -> ...`
3.  **省略方法体大括号**：
    *   **只有 1 条语句**时：`e -> System.out.println("Hi")`
4.  **省略 return**：
    *   如果只有 1 条语句且是返回值，**必须同时省略** `return` 关键字和大括号 `{}`。
    *   *错误写法*：`x -> return x * x;`
    *   *正确写法*：`x -> x * x;` 或者 `x -> { return x * x; }`

### Java 内置函数式接口 (Slide 27)
考试可能会让你根据 Lambda 表达式推断它属于哪个接口，或者反过来。

| 接口名           | 参数 | 返回值  | 描述 (用途)                     |
| :--------------- | :--- | :------ | :------------------------------ |
| `Predicate<T>`   | T    | boolean | **断言**，用于判断 (test)       |
| `Consumer<T>`    | T    | void    | **消费**，只进不出 (accept)     |
| `Supplier<T>`    | 无   | T       | **供给**，只出不进 (get)        |
| `Function<T, R>` | T    | R       | **转换**，输入 T 输出 R (apply) |

---

## 5. Observable 对象 (监听属性变化)

JavaFX 特有的数据绑定和属性监听机制。

*   **Observable 接口**：允许对象被监听。
*   **监听器接口**：`InvalidationListener`。
*   **方法**：`property.addListener(listener)`。
*   **触发时机**：当属性值发生**变化**时。

```java
DoubleProperty balance = new SimpleDoubleProperty(100);
balance.addListener(o -> {
    System.out.println("余额变了！");
});
balance.set(200); // 触发监听器
```

---

## 6. Java vs. C++ (高分对比点)

如果考卷中有简答题涉及语言特性对比，这是得分点：

1.  **内部类 (Inner Class)**：
    *   **Java**：非静态内部类**隐式持有**外部类实例的引用 (`Outer.this`)，因此可以直接访问外部类的实例成员。
    *   **C++**：嵌套类 (Nested Class) 只是名字空间上的包含，**没有**隐含的外部类指针，不能直接访问外部类的非静态成员。
2.  **事件处理机制**：
    *   **Java**：使用接口 (Interface) 和对象 (Object) 或 Lambda。强类型检查。
    *   **C++**：传统上使用函数指针 (Function Pointers) 或回调函数。现代 C++ (C++11+) 也有 Lambda 和 `std::function`，但 Java 的基于接口的委托模型更统一。
3.  **内存管理**：
    *   **Java**：匿名内部类捕获局部变量时，该变量实际上被视为 `final`。垃圾回收器 (GC) 会管理监听器的生命周期（虽然后台可能存在内存泄漏风险，如果监听器持有强引用）。
    *   **C++**：Lambda 捕获变量需要显式指定是按值 (`=`) 还是按引用 (`&`) 捕获。容易出现**悬空指针** (Dangling Pointer)，即监听器还存在，但引用的对象已经被销毁了。

---

## 7. 考试避坑指南 (易错点)

1.  **编译文件名**：
    *   问：`Test` 类中有两个匿名内部类，编译出什么文件？
    *   答：`Test.class`, `Test$1.class`, `Test$2.class`。
2.  **Lambda 的变量捕获**：
    *   在 Lambda 表达式或匿名内部类中使用的局部变量，必须是 **final** 或 **effectively final** (即赋值后不再改变)。如果试图在 Lambda 内部修改外部局部变量（如 `i++`），编译器会报错。
3.  **注册监听器**：
    *   写了 Handler 类，也实例化了，但**忘了调用 `button.setOnAction(handler)`**。这是手写代码题最常见的低级错误。
4.  **区分 `Action` 与 `Mouse` 事件**：
    *   `Button` 点击通常用 `ActionEvent`。
    *   如果题目要求检测鼠标按下(Pressed)和释放(Released)的区别，必须用 `MouseEvent`，不能用 `ActionEvent`。
5.  **Lambda 语法题**：
    *   判断：`() -> { return "Hello"; }` (正确)
    *   判断：`() -> "Hello"` (正确)
    *   判断：`() -> return "Hello";` (**错误**，有 return 必须有大括号)

---

## 8. 代码填空模拟题

**题目**：使用 Lambda 表达式补全代码，使得点击按钮时，控制台输出 "Java is fun"。

```java
Button bt = new Button("Print");
// _______________ (1) __________________
bt.setOnAction( e -> System.out.println("Java is fun") ); 
// 或者
bt.setOnAction( e -> {
    System.out.println("Java is fun");
});
```

**题目**：将以下匿名内部类改写为 Lambda。

```java
// 原代码
btn.setOnAction(new EventHandler<ActionEvent>() {
    public void handle(ActionEvent event) {
        process();
    }
});

// 改写后
btn.setOnAction( event -> process() );
```

---

祝你复习顺利，Java 期末考试拿高分！

***

Java 中的 **Lambda 表达式**（Lambda Expression）是 Java 8 引入的一项重要特性，它允许我们将**函数作为方法参数**进行传递，或者将代码视为数据。
简单来说，Lambda 表达式让 Java 这种面向对象的语言拥有了部分**函数式编程**的能力，从而使代码更加简洁、灵活。
以下是对 Lambda 表达式的详细介绍，包括语法、用法、核心概念以及注意事项。
---
### 1. 为什么要用 Lambda？（背景）
在 Java 8 之前，如果想要使用一个匿名内部类（例如为了实现一个线程或者点击事件），你需要写很多“样板代码”。
**传统写法（匿名内部类）：**
```java
Runnable runnable = new Runnable() {
    @Override
    public void run() {
        System.out.println("Hello World");
    }
};
```
*问题：真正的核心逻辑只有 `System.out.println(...)` 这一行，但我们却写了 5 行额外的格式代码。*
**Lambda 写法：**
```java
Runnable runnable = () -> System.out.println("Hello World");
```
*优势：简洁，直击核心逻辑。*
---
### 2. Lambda 语法结构
Lambda 表达式的基本语法如下：
```java
(parameters) -> { body }
```
它由三个部分组成：
1.  **参数列表**：类似于方法中定义的参数列表。
2.  **箭头符号**：`->`，把参数列表和代码体连接起来。
3.  **代码体**：要执行的逻辑，可以是表达式或代码块。
**语法变体示例：**
1.  **无参数，无返回值：**
    ```java
    () -> System.out.println("执行")
    ```
2.  **有一个参数，无返回值（参数小括号可省略）：**
    ```java
    x -> System.out.println(x)
    // 或者
    (x) -> System.out.println(x)
    ```
3.  **有多个参数：**
    ```java
    (int a, int b) -> a + b
    ```
4.  **代码体有多条语句（必须用大括号，且有 return）：**
    ```java
    (x, y) -> {
        System.out.println("计算中...");
        return x + y;
    }
    ```
5.  **参数类型可推断（可省略不写）：**
    ```java
    // 编译器能推断出 a 和 b 是 int
    (a, b) -> a + b 
    ```
---
### 3. 核心概念：函数式接口
这是理解 Lambda 表达式的关键：**Lambda 表达式并不对应于某个特定的类，而是对应于一个“函数式接口”。**
*   **定义**：一个接口中，**只有一个抽象方法**（Abstract Method），这样的接口就是函数式接口。
*   **注解**：可以使用 `@FunctionalInterface` 注解来强制检查该接口是否为函数式接口（如果不满足条件，编译会报错）。
**Lambda 就是对函数式接口中那个唯一抽象方法的实现。**
**示例：**
```java
@FunctionalInterface
interface Calculator {
    int add(int a, int b);
}
// 使用 Lambda 实现该接口
Calculator calc = (a, b) -> a + b;
System.out.println(calc.add(1, 2)); // 输出 3
```
**Java 内置常用的函数式接口（都在 `java.util.function` 包下）：**
1.  **`Consumer`**：接收一个参数，无返回值（消费型）。
    *   `void accept(T t)`
2.  **`Supplier`**：无参数，返回一个结果（供给型）。
    *   `T get()`
3.  **`Function<T,R>`**：接收一个参数，返回一个结果（函数型）。
    *   `R apply(T t)`
4.  **`Predicate`**：接收一个参数，返回布尔值（断言型）。
    *   `boolean test(T t)`
---
### 4. 实际应用场景
#### 场景一：集合遍历
配合 `Iterable` 的 `forEach` 方法。
```java
List<String> list = Arrays.asList("Java", "Python", "C++");
// 传统 for 循环略过...
// Lambda 方式
list.forEach(str -> System.out.println("语言: " + str));
```
#### 场景二：Stream API（流式编程）
这是 Lambda 最强大的应用场景，用于对集合进行过滤、排序、映射等操作。
```java
List<Student> students = ...;
// 筛选分数大于 80 的学生，并按名字排序
List<Student> filtered = students.stream()
    .filter(s -> s.getScore() > 80)  // Predicate<Student>
    .sorted((s1, s2) -> s1.getName().compareTo(s2.getName())) // Comparator<Student>
    .collect(Collectors.toList());
```
#### 场景三：新开线程
```java
// new Thread(() -> { ... }).start();
new Thread(() -> System.out.println("线程正在运行")).start();
```
---
### 5. 变量捕获（重要限制）
在 Lambda 表达式中访问外部变量时，有一条严格的规则：**局部变量必须隐式具备 final 效果（Effectively Final）。**
这意味着你可以在 Lambda 中使用外部定义的局部变量，但是**不能在 Lambda 内部修改它**。
**示例：**
```java
int num = 10; // 这里的 num 相当于 final
// 正确：读取
Consumer<Integer> consumer = x -> System.out.println(x + num);
// 错误：修改
// Consumer<Integer> consumer = x -> { num = x; }; // 编译报错
```
*原因：为了线程安全。Lambda 可能会在另一个线程中执行，如果允许修改局部变量，会导致数据不一致。*
---
### 6. 方法引用
如果 Lambda 体中的内容仅仅是调用一个已有的方法，那么可以使用“方法引用”来进一步简化代码。这是 Lambda 的简写形式。
语法：`类名::方法名` 或 `对象::方法名`
**示例对比：**
*   **Lambda 写法：** `s -> System.out.println(s)`
*   **方法引用写法：** `System.out::println`
*   **Lambda 写法：** `(x, y) -> Math.max(x, y)`
*   **方法引用写法：** `Math::max`
---
### 7. 总结
**Lambda 表达式的优点：**
1.  **代码简洁**：减少了匿名内部类的样板代码。
2.  **并行处理友好**：配合 Stream API，可以非常方便地编写多线程代码。
3.  **将函数作为一等公民**：代码可以像数据一样传递。
**Lambda 表达式的缺点：**
1.  **可读性**：对于复杂的逻辑，过长的 Lambda 可能会降低代码可读性。
2.  **调试困难**：Lambda 的堆栈跟踪可能不像传统类那样清晰。
**一句话总结：**
Lambda 表达式就是**匿名函数**，它是实现**函数式接口**的简写方式，是 Java 迈向函数式编程的第一步，也是 Stream API 的基础。