这份讲义是基于你提供的《Java语言程序设计》（梁勇著，第10版）第16章 PPT 内容生成的。针对软件工程专业的期末复习，我将重点放在**类结构继承**、**核心控件用法**、**事件处理机制**以及**考试常考的坑点**上。

---

# JavaFX UI Controls & Multimedia 期末复习讲义

## 1. 核心概念与类层次结构 (Class Hierarchy)

理解继承关系是掌握 JavaFX 的关键，考试中常考多态特性（例如：哪些属性是共有的）。

### 1.1 继承树 (参考 Slide 4)
所有的 UI 控件最终都继承自 `Node`。
*   **Node** (基类) -> **Parent** -> **Control** (所有控件的基类)
    *   **Labeled** (带文本/图标的): `Label`, `ButtonBase` (`Button`, `CheckBox`, `RadioButton`, `ToggleButton`)
    *   **TextInputControl** (文本输入): `TextField`, `TextArea`
    *   **其他**: `ComboBox`, `ListView`, `ScrollBar`, `Slider`

### 1.2 常用前缀 (Coding Convention) (Slide 4)
书中使用以下变量命名前缀，考试写代码题时建议遵守此规范，显得专业：
*   `Label`: **lbl**
*   `Button`: **bt**
*   `CheckBox`: **chk**
*   `RadioButton`: **rb**
*   `TextField`: **tf**
*   `TextArea`: **ta**
*   `ComboBox`: **cbo**
*   `ListView`: **lv**
*   `MediaPlayer`: **mp**

---

## 2. 常用控件详解 (Controls)

### 2.1 Labeled 抽象类 (Slide 5)
`Label` 和 `Button` 的父类。
*   **特性**: 可以同时显示文本(Text)和图形(Graphic)。
*   **常用属性**:
    *   `text`: 显示的字符串。
    *   `graphic`: 显示的图标节点（如 `ImageView`）。
    *   `contentDisplay`: 控制文本和图形的相对位置 (`TOP`, `BOTTOM`, `LEFT`, `RIGHT`)。
    *   `graphicTextGap`: 间距。
    *   `textFill`: 字体颜色 (`Color.RED`)。
    *   `underline`: 下划线。

### 2.2 Button (按钮) (Slide 7-8)
*   **继承**: `Button` -> `ButtonBase` -> `Labeled`。
*   **核心动作**: `setOnAction(EventHandler<ActionEvent>)`。
*   **代码示例**:
    ```java
    Button btLeft = new Button("Left", new ImageView("image/left.gif"));
    btLeft.setOnAction(e -> {
        System.out.println("Button clicked!");
    });
    ```

### 2.3 CheckBox (复选框) (Slide 9-10)
*   **用途**: 多选（N个选项可以选M个）。
*   **核心属性**: `selected` (boolean)。
*   **检测状态**: `chk.isSelected()`。
*   **继承**: 也是继承自 `ButtonBase`，所以也有 `setOnAction`。

### 2.4 RadioButton (单选按钮) **(考试重点 Slide 11)**
*   **用途**: 互斥选择（N选1）。
*   **关键坑点**:
    *   单纯创建 `RadioButton` 它们是独立的（类似 CheckBox）。
    *   **必须**将它们添加同一个 **`ToggleGroup`** 才能实现互斥。
*   **代码示例**:
    ```java
    RadioButton rbRed = new RadioButton("Red");
    RadioButton rbBlue = new RadioButton("Blue");
    
    ToggleGroup group = new ToggleGroup(); // 必须创建组
    rbRed.setToggleGroup(group);
    rbBlue.setToggleGroup(group);
    
    // 获取被选中的项
    if (rbRed.isSelected()) { ... }
    ```

### 2.5 TextField (单行文本) vs TextArea (多行文本) (Slide 13-16)
*   **TextField**:
    *   用于输入单行数据。
    *   回车键触发 `setOnAction`。
    *   `tf.getText()` 获取内容。
*   **TextArea**:
    *   用于输入多行数据。
    *   不支持 `setOnAction`（回车是换行）。
    *   关键属性: `setWrapText(true)` (自动换行)。
    *   通常配合 `ScrollPane` 使用（JavaFX `TextArea` 自带滚动条，但放入 `ScrollPane` 更灵活）。

### 2.6 ComboBox (下拉框) vs ListView (列表框) (Slide 17-20)
两者都用于从列表中选择，但 **UI 表现不同。**
*   **数据源**: 都需要 `ObservableList`。
    ```java
    ObservableList<String> items = FXCollections.observableArrayList("US", "China", "UK");
    ComboBox<String> cbo = new ComboBox<>(items);
    ListView<String> lv = new ListView<>(items);
    ```
*   **ComboBox**: 占用空间小，类似 TextField + Dropdown。
    *   `cbo.getValue()` 获取当前选中值。
*   **ListView**: 展示所有（或部分）项，占用空间大。
    *   **多选支持**: `ListView` 支持多选模式。
    *   `lv.getSelectionModel().setSelectionMode(SelectionMode.MULTIPLE);`

### 2.7 ScrollBar vs Slider (Slide 21-26)
*   **ScrollBar**: 通常用于控制视图滚动（低级控件）。
*   **Slider**: 通常用于数值选择（如音量、亮度、进度条）。
*   **共有属性**: `min`, `max`, `value`。
*   **监听变化**: 不同于 Button 的 ActionEvent，这里通常监听 **Property** 的变化。
    ```java
    Slider sl = new Slider(0, 100, 50);
    // 监听值改变（这是JavaFX特色的属性绑定/监听机制）
    sl.valueProperty().addListener(observable -> {
        System.out.println("Value: " + sl.getValue());
    });
    ```

---

## 3. 多媒体 (Multimedia) (Slide 29-32)

JavaFX 处理媒体需要三个类的配合，**这也是一个考点（区分三者的职责）**：

1.  **Media**: **数据源**。代表媒体资源（文件路径或 URL）。
    *   `new Media("file:///C:/video.mp4")` 或者 URL。
2.  **MediaPlayer**: **控制器**。负责播放逻辑（Play, Pause, Stop, Volume, Loop）。
    *   `mp.play()`, `mp.pause()`, `mp.setCycleCount(MediaPlayer.INDEFINITE)`.
3.  **MediaView**: **视图节点**。继承自 `Node`，负责将视频画面渲染到界面上。
    *   如果是**音频**，不需要 `MediaView`。
    *   如果是**视频**，必须将 `MediaView` 添加到 Pane 中。

**代码结构**:
```java
Media media = new Media(url);
MediaPlayer mediaPlayer = new MediaPlayer(media);
MediaView mediaView = new MediaView(mediaPlayer); // 只有视频才需要这一步
pane.getChildren().add(mediaView);
mediaPlayer.play();
```

---

## 4. Java vs. C++ (GUI 开发视角的对比)

作为软工学生，理解语言差异能加深印象：

1.  **内存管理 (Memory)**:
    *   **C++ (Qt/MFC)**: 创建控件通常是 `new Button()`，在销毁窗口时需要确保内存释放（Qt通过父子对象树自动管理，MFC需要小心）。
    *   **Java**: 只要从 `Scene` 图中移除且无引用，垃圾回收器 (GC) 会自动回收。

2.  **指针 vs 引用**:
    *   **C++**: 大量使用指针 `Button* btn = new Button();`，操作用 `->`。
    *   **Java**: 一切对象皆引用 `Button btn = new Button();`，操作用 `.`。

3.  **事件处理 (Event Handling)**:
    *   **C++ (Qt)**: 信号与槽 (Signals & Slots)，例如 `connect(btn, SIGNAL(clicked()), this, SLOT(onClicked()))`。
    *   **Java**: 接口回调 (Interface/Lambda)，例如 `btn.setOnAction(e -> handle())`。Java 的 Lambda 表达式让代码比早期的匿名内部类简洁很多。

4.  **属性绑定 (Property Binding)**:
    *   **JavaFX 特有**: `label.textProperty().bind(textField.textProperty())`。这在 C++ 标准库中没有直接对应物，是 JavaFX 构建响应式 UI 的强大工具。

---

## 5. 考试易错坑点 (Pitfalls) & 重点复习建议

1.  **RadioButton 的互斥**:
    *   **坑点**: 忘了加 `ToggleGroup`。如果考试出代码找错题，看到多个 RadioButton 没有 ToggleGroup，那它们就能同时被选中（逻辑错误）。

2.  **ObservableList**:
    *   **坑点**: `ComboBox` 和 `ListView` 的构造函数不接受普通的 `ArrayList`。必须用 `FXCollections.observableArrayList(...)` 包装。

3.  **UI 线程**:
    *   虽不是本章重点，但需知晓 UI 更新必须在 JavaFX Application Thread 中。如果在后台线程直接改 Label 文字会抛异常。

4.  **Media 路径**:
    *   `Media` 类的构造函数接受的是字符串形式的 **URI**，而不是简单的文件路径。
    *   错: `new Media("C:\\music.mp3")`
    *   对: `new Media(new File("C:\\music.mp3").toURI().toString())`

5.  **布局容器 (Pane)**:
    *   创建了控件（如 Button），如果不加到 `Pane` (`pane.getChildren().add(btn)`) 或者 `Scene` 中，它是**不会显示**的。

6.  **Slider/ScrollBar 的监听**:
    *   新手常试图给 Slider 加 `setOnAction`。注意：Slider 拖动是值的连续变化，通常用 `valueProperty().addListener(...)`，而不是 ActionEvent。

## 6. 案例分析复习 (Case Studies)

*   **TicTacToe (井字棋)**:
    *   学习如何自定义控件。它定义了一个 `Cell` 类继承自 `Pane`。这展示了 **"Composition" (组合)** 和 **"Inheritance" (继承)** 在构建复杂 UI 中的应用。
    *   逻辑：点击 `Cell` -> 触发 handleMouseClick -> 更新 Token ('X' 或 'O') -> 检查胜负。

*   **Flag Anthem (国旗国歌)**:
    *   综合应用：`ComboBox` 选择国家 -> `ImageView` 更新国旗 -> `MediaPlayer` 播放对应 mp3。
    *   考点：事件触发后的多组件联动更新。

---

**总结**：复习时，请看着 Slide 4 的类图默写一遍继承关系，并针对每一个控件写一个最简单的 "Hello World" 式的 Demo（创建 -> 设置属性 -> 添加事件 -> 加入面板），重点关注 `RadioButton` 的分组和 `ComboBox` 的数据源填充。祝期末高分！