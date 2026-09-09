这是一份基于你提供的《Chapter 14 JavaFX Basics》课件，专门为软件工程专业学生定制的JavaFX期末复习讲义。

这份讲义不仅涵盖了课件中的核心知识点，还结合了代码示例、与C++的对比以及考试中容易丢分的“坑点”。

---

# 📘 JavaFX 基础复习讲义 (Chapter 14)

## 1. Java GUI 发展史与 JavaFX 的地位 (Slides 2-4)

**概念理解：**
考试中可能会考察 Java GUI 框架的演变，理解它们的关系很重要。
*   **AWT (Abstract Windows Toolkit):** Java 最早的 GUI 库。缺点是**重量级 (Heavyweight)**，依赖底层操作系统的组件，容易出现跨平台 Bug，外观在不同系统上不一致。
*   **Swing:** 建立在 AWT 之上，取代了 AWT 的大部分组件。优点是**轻量级 (Lightweight)**，组件是由 Java 代码在画布上“画”出来的，不依赖操作系统原生组件，跨平台一致性好。
*   **JavaFX:** 最新的 GUI 框架，旨在取代 Swing。
    *   **特点:** 支持富互联网应用 (RIA)，支持多媒体，硬件加速，完全面向对象设计。
    *   **架构:** Swing 是单线程画图，JavaFX 的架构更现代化（属性绑定、CSS 样式等）。

---

## 2. JavaFX 程序的基本结构 (Slides 4-5)

**核心架构：舞台隐喻 (The Theater Metaphor)**
这是 JavaFX 最重要的概念，考试必考。

1.  **Stage (舞台):** 也就是应用程序的**窗口 (Window)**。
    *   这是顶层容器。
    *   主舞台 (`primaryStage`) 由 JVM 在启动时自动创建并传入 `start()` 方法。
2.  **Scene (场景):** 舞台上的**内容 (Content)**。
    *   一个舞台同一时间只能展示一个场景（就像戏剧换幕）。
    *   `Scene` 包含所有的 GUI 组件。
3.  **Node (节点):** 场景中的**演员/道具**。
    *   所有的控件（Button）、布局（Pane）、形状（Shape）都是 `Node`。
    *   这就构成了一个树状结构：**Scene Graph (场景图)**。

**代码模板 (背诵级):**
任何 JavaFX 程序都必须继承 `Application` 类并重写 `start` 方法。

```java
import javafx.application.Application;
import javafx.scene.Scene;
import javafx.scene.control.Button;
import javafx.stage.Stage;

// 1. 继承 Application 抽象类
public class MyJavaFX extends Application {

    // 2. 重写 start 方法
    @Override
    public void start(Stage primaryStage) {
        // 创建一个节点 (Node)
        Button btOK = new Button("OK");

        // 创建一个场景 (Scene)，并将节点放入场景
        // 参数: (根节点, 宽, 高)
        Scene scene = new Scene(btOK, 200, 250);

        // 设置舞台 (Stage)
        primaryStage.setTitle("MyJavaFX"); // 设置标题
        primaryStage.setScene(scene);      // 把场景放到舞台上
        primaryStage.show();               // 显示舞台 (考试易忘点!)
    }

    // main 方法用于启动
    public static void main(String[] args) {
        launch(args);
    }
}
```

---

## 3. 坐标系统 (Slide 7)

**易错点：** JavaFX 的坐标系与数学中的笛卡尔坐标系不同，与 C++ 的某些绘图库（如 GDI+）类似。

*   **原点 (0, 0):** 位于面板的**左上角**。
*   **X 轴:** 向**右**延伸。
*   **Y 轴:** 向**下**延伸 (注意：Y 值越大，位置越靠下，这与数学直觉相反)。

---

## 4. 属性绑定 (Property Binding) (Slides 8-11)

**这是 JavaFX 区别于 Swing 和 C++ MFC/Qt 的一大特色，必考概念。**

*   **概念:** 将一个对象（目标 Target）的属性绑定到另一个对象（源 Source）。当源发生变化时，目标自动更新。
*   **应用场景:** 比如让一个圆永远保持在窗口中心，当窗口大小改变时，圆的位置自动改变。

**核心语法:**
*   `target.bind(source)`: 单向绑定 (Source 变 -> Target 变)。
*   `target.bindBidirectional(source)`: 双向绑定 (Source 变 <-> Target 变)。

**代码示例:**
```java
Pane pane = new Pane();
Circle circle = new Circle();

// 获取 pane 的宽度和高度属性 (Property)，而不仅仅是值
// 将圆心的 X 坐标绑定为 pane 宽度的一半
circle.centerXProperty().bind(pane.widthProperty().divide(2));
circle.centerYProperty().bind(pane.heightProperty().divide(2));

// 注意：这里使用的是 centerXProperty() 而不是 setCenterX()
// 所有的绑定操作都针对 Property 对象
```

**与 C++ 的区别：**
在 C++ (如 Qt) 中，通常需要通过 `Signal` 和 `Slot` (信号槽) 机制，手动编写代码在 resize 事件中更新坐标。JavaFX 的 Binding 是一种声明式编程，代码更简洁，耦合度更低。

---

## 5. 常用节点属性与样式 (Slide 11-13)

*   **Style (CSS):** JavaFX 支持类似 Web 的 CSS 样式。
    *   `node.setStyle("-fx-border-color: blue; -fx-background-color: red;");`
    *   **注意:** JavaFX 的 CSS 属性前通常有 `-fx-` 前缀。
*   **Rotate:** 旋转节点。
    *   `node.setRotate(45);` // 顺时针旋转 45 度
*   **Color 类:**
    *   `Color.RED`, `Color.BLUE` (静态常量)
    *   `new Color(r, g, b, opacity)` (0.0 到 1.0 之间的浮点数)

---

## 6. 布局面板 (Layout Panes) (Slides 16-21)

**这是考试中的重点，通常会给出一种界面布局，让你选择合适的 Pane。**

| 面板类 (Class) | 描述 (Description) | 布局行为关键词                                               |
| :------------- | :----------------- | :----------------------------------------------------------- |
| **Pane**       | 基类               | 绝对定位，需手动指定 (x,y)，不推荐用于复杂布局。             |
| **StackPane**  | 堆栈面板           | 节点**重叠**放置，后加的盖在先加的上面。默认居中。           |
| **FlowPane**   | 流式面板           | 像**文字处理**一样，一行排满自动换到下一行 (Wrap)。可水平或垂直。 |
| **GridPane**   | 网格面板           | **表格**形式 (行, 列)。最灵活，但也最复杂。                  |
| **BorderPane** | 边界面板           | 分为5个区域：**Top, Bottom, Left, Right, Center**。经典应用布局。 |
| **HBox**       | 水平盒子           | 单行，**水平**排列所有节点。                                 |
| **VBox**       | 垂直盒子           | 单列，**垂直**排列所有节点。                                 |

**记忆技巧:**
*   做计算器界面 -> `GridPane`
*   做经典的 "菜单-内容-状态栏" 界面 -> `BorderPane`
*   做简单的按钮栏 -> `HBox`

---

## 7. 图像与形状 (Slides 14-29)

### 7.1 图像 (Image vs ImageView)
这是一个典型的**数据与视图分离**的设计，也是考试常见的**概念混淆点**。

*   **Image:** 代表图片**数据** (加载文件)。它**不是** Node，不能直接加到 Scene 中。
*   **ImageView:** 是一个 Node，用于**显示** Image。它可以被加到 Pane 中。

```java
Image image = new Image("file:image.gif"); // 加载
// pane.getChildren().add(image); // ❌ 错误！Image 不是 Node
ImageView imageView = new ImageView(image); // 包装
pane.getChildren().add(imageView); // ✅ 正确
```

### 7.2 形状 (Shapes)
所有形状继承自 `Shape` 类 (进而继承自 `Node`)。
*   **Text:** `new Text(x, y, "String")`
*   **Line:** `new Line(startX, startY, endX, endY)`
*   **Rectangle:** `new Rectangle(x, y, width, height)`
*   **Circle:** `new Circle(centerX, centerY, radius)`

---

## 8. C++ (Qt/MFC) 与 JavaFX 的关键对比 (针对软工学生)

如果你有 C++ 背景，理解这些差异有助于你深入理解 JavaFX：

1.  **内存管理:**
    *   **C++:** 在 Qt 中，你需要指定 `parent`，当 parent 被销毁时，children 才会自动析构。如果忘记指定 parent，可能是内存泄漏。
    *   **JavaFX:** 依赖 Java 的 **Garbage Collection (GC)**。只要一个 Node 不再连接到 Scene Graph 根节点，且没有其他引用，它就会被回收。你不需要手动 `delete` 按钮。

2.  **指针 vs 引用:**
    *   **C++:** 大量使用 `Button* btn = new Button();` 以及 `->` 操作符。
    *   **JavaFX:** 全部是对象引用 `Button btn = new Button();` 使用 `.` 操作符。在 Java 中，除了基本类型，一切皆对象引用。

3.  **属性系统:**
    *   **C++:** 通常通过 Getter/Setter 访问成员变量。
    *   **JavaFX:** 引入了 `Property` 对象 (如 `DoubleProperty`)。这不仅仅是存一个值，它是一个包装器，支持监听 (Listener) 和绑定 (Binding)。这是实现响应式 UI 的基础。

4.  **UI 定义:**
    *   **C++ (Qt):** 常用 `.ui` 文件 (XML) 或 C++ 代码。
    *   **JavaFX:** 既可以用纯 Java 代码（本章重点），也可以用 FXML (XML格式，类似 HTML) 配合 SceneBuilder（幻灯片中提到过）。

---

## 9. ⚠️ 考试易错点 & "坑" (必看)

1.  **忘记 `primaryStage.show()`:**
    *   写了半天代码，运行程序什么都没弹出来，通常就是忘了这一行。

2.  **节点只能有一个父节点:**
    *   **错误代码:**
        ```java
        Button btn = new Button("OK");
        pane1.getChildren().add(btn);
        pane2.getChildren().add(btn); // ❌ 运行时错误！
        ```
    *   **解释:** 一个 Node 对象只能存在于 Scene Graph 的一个位置。如果想在两个地方显示，必须 `new` 两个 Button。

3.  **坐标系混淆:**
    *   画矩形或文字时，给出的 `y` 值越大，实际上在屏幕上是越**靠下**的。

4.  **Image 与 ImageView:**
    *   再次强调：`Image` 不能直接 `add` 到 pane 里，必须包在 `ImageView` 里。

5.  **GridPane 的行列索引:**
    *   `GridPane.add(node, columnIndex, rowIndex)`
    *   **坑:** 先传列 (Column/X)，后传行 (Row/Y)。这和通常的矩阵 `arr[row][col]` 顺序相反！

6.  **Binding 的数据类型:**
    *   `widthProperty()` 返回的是 `ReadOnlyDoubleProperty`。你不能直接 `set` 它，只能去 `bind` 依赖它。

---

## 10. 复习自测题

在复习结束时，试着回答以下问题：
1.  JavaFX 的入口方法是什么？参数是什么类型？
2.  如何让一个 Label 的文字始终保持和 TextField 输入的内容一致？(提示：Binding)
3.  如果我想把界面分成上、下、左、右、中五个部分，应该用什么 Pane？
4.  `Circle` 类的 `centerX` 属性是 `double` 类型吗？(提示：区分值类型和属性对象类型)

祝你期末考试顺利！如有具体代码看不懂，可以随时问我。