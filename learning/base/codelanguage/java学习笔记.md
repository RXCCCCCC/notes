[toc]

# java学习笔记

从互联网到企业平台，Java是应用最广泛的编程语言，原因在于：

- Java是基于JVM虚拟机的**跨平台**语言，一次编写，到处运行；
- Java程序**易于编写**，而且有**内置垃圾收集，不必考虑内存管理**；
- Java虚拟机拥有**工业级的稳定性**和**高度优化的性能**，且经过了长时期的考验；
- Java拥有**最广泛的开源社区支持**，各种高质量组件随时可用。

Java语言常年霸占着三大市场：

- **互联网和企业应用**，这是**Java EE**的长期优势和市场地位；
- **大数据平台**，主要有Hadoop、Spark、Flink等，他们都是Java或Scala（种运行于JVM的编程语言）开发的；
- **Android移动平台**。

这意味着Java拥有最广泛的就业市场。

目前教程版本是：Java 22!

# 快速入门

# 第一个Java程序

```java
public class Hello {
    public static void main(String[] args) {
        System.out.println("Hello, world!");
    }
}
```

定义被称为class（类），这里的类名是`Hello`，**大小写敏感**，`class`用来定义一个类，`public`表示这个类是公开的，`public`、`class`都是Java的关键字，必须小写，`Hello`是类的名字，按照习惯**，首字母`H`要大写**。而花括号`{}`中间则是类的定义。

注意到类的定义中，我们**定义了一个名为`main`的方法**.方法是可执行的代码块，一个方法除了**方法名`main`，还有用`()`括起来的方法参数**，这里的`main`方法有一个参数，**参数类型是`String[]`，参数名是`args`**，`public`、`static`用来修饰方法，这里表示它是一个**公开的静态方法**，`void`是方法的返回类型，而花括号`{}`中间的就是方法的代码。

方法的代码每一行用`;`结束，这里只有一行代码，就是：

Java规定，某个类定义的`public static void main(String[] args)`是Java程序的固定入口方法，因此，Java程序总是从`main`方法开始执行。

注意到Java源码的**缩进不是必须的**，但是用缩进后，格式好看，很容易看出代码块的开始和结束，缩进一般是4个空格或者一个tab。

最后，当我们把代码保存为文件时，**文件名必须是`Hello.java`，**而且文件名也要注意大小写，因为**要和我们定义的类名`Hello`完全保持一致**。

### 如何运行Java程序

Java源码**本质上是一个文本文件**，我们需要先用`javac`把`Hello.java`编译成字节码文件`Hello.class`，然后，用**`java`命令执行**这个字节码文件：

```
┌──────────────────┐
│    Hello.java    │◀── source code
└──────────────────┘
          │ compile
          ▼
┌──────────────────┐
│   Hello.class    │◀── byte code
└──────────────────┘
          │ execute
          ▼
┌──────────────────┐
│    Run on JVM    │
└──────────────────┘
```

因此，**可执行文件`javac`是编译器**，而**可执行文件`java`就是虚拟机**。

第一步，在保存`Hello.java`的**目录下执行命令**`javac Hello.java`：

```plain
$ javac Hello.java
```

如果源代码无误，上述命令不会有任何输出，而**当前目录下会产生一个`Hello.class`文件**：

```plain
$ ls
Hello.class	Hello.java
```

第二步，执行`Hello.class`，使用命令`java Hello`（注意不是`java Hello.class`）：

```plain
$ java Hello
Hello, world!
```

注意：给虚拟机传递的参数`Hello`是我们定义的类名，虚拟机自动查找对应的class文件并执行。

如果执行`java Hello`报错：

```plain
$ java Hello
Error: Could not find or load main class Hello
Caused by: java.lang.ClassNotFoundException: Hello
```

出现`ClassNotFoundException`信息，说明在**当前目录下并没有`Hello.class`这个文件**，请切换到`Hello.class`的目录，然后执行`java Hello`。

有一些童鞋可能知道，**直接运行`java Hello.java`也是可以**的：

```plain
$ java Hello.java 
Hello, world!
```

这是从Java 11开始新增的一个功能，它可以**直接运行一个单文件源码**！

需要注意的是，在实际项目中，单个不依赖第三方库的Java源码是非常罕见的，所以，绝大多数情况下，我们**无法直接运行一个Java源码文件，原因是它需要依赖其他的库**。

### 小结

一个Java源码**只能定义一个`public`类型的class，并且class名称和文件名要完全一致；**

使用`javac`可以将`.java`源码编译成**`.class`字节码**；

使用`java`可以运行一个**已编译的Java程序**，参数是**类名**。

# Java程序基本结构

我们先剖析一个完整的Java程序，它的基本结构是什么：

```java
/**
 * 可以用来自动创建文档的注释
 */
public class Hello {
    public static void main(String[] args) {
        // 向屏幕输出文本:
        System.out.println("Hello, world!");
        /* 多行注释开始
        注释内容
        注释结束 */
    }
} // class定义结束
```

因为Java是**面向对象的语言**，**一个程序的基本单位就是`class`**，`class`是关键字，这里定义的`class`名字就是`Hello`：

```java
public class Hello { // 类名是Hello
    // ...
} // class定义结束
```

类名要求：

- 类名必须以英文字母开头，后接**字母，数字和下划线**的组合
- 习惯以大写字母开头

要注意遵守命名习惯，好的类命名(**大驼峰**)：

- Hello
- NoteBook
- VRPlayer

注意到`public`是访问修饰符，表示该`class`是公开的。

不写`public`，也能正确编译，但是这个类将无法从命令行执行。

在`class`内部，可以**定义若干方法（method）**：

```java
public class Hello {
    public static void main(String[] args) { // 方法名是main
        // 方法代码...
    } // 方法定义结束
}
```

方法定义了一组执行语句，方法内部的代码将会被依次顺序执行。

这里的方法名是`main`，返回值是`void`，表示没有任何返回值。

我们注意到`public`除了可以修饰`class`外，也可以修饰方法。而关键字`static`是另一个修饰符，它表示静态方法，后面我们会讲解方法的类型，目前，我们只需要知道**，Java入口程序规定的方法必须是静态方法，方法名必须为`main`，括号内的参数必须是String数组。**

方法名也有命名规则，命名和`class`一样，**但是首字母小**

在方法内部，语句才是真正的执行代码。Java的每一行语句必须以分号结束：

```java
public class Hello {
    public static void main(String[] args) {
        System.out.println("Hello, world!"); // 语句
    }
}
```

在Java程序中，注释是一种给人阅读的文本，不是程序的一部分，所以编译器会自动忽略注释。

Java有3种注释，第一种是单行注释，以双斜线开头，直到这一行的结尾结束：

```java
// 这是注释...
```

而多行注释以`/*`星号开头，以`*/`结束，可以有多行：

```java
/*
这是注释
blablabla...
这也是注释
*/
```

还有一种特殊的多行注释，**以`/**`开头，以`*/`结束，如果有多行，每行通常以星号开头：**

```java
/**
 * 可以用来自动创建文档的注释
 * 
 * @auther liaoxuefeng
 */
public class Hello {
    public static void main(String[] args) {
        System.out.println("Hello, world!");
    }
}
```

这种**特殊的多行注释**需要写在类和方法的**定义处，可以用于自动创建文档**。

Java程序对格式没有明确的要求，多几个空格或者回车不影响程序的正确性，但是我们要养成良好的编程习惯，注意遵守Java社区约定的编码格式。

那约定的编码格式有哪些要求呢？其实我们在前面介绍的Eclipse IDE提供了快捷键`Ctrl+Shift+F`（macOS是`⌘+⇧+F`）帮助我们快速格式化代码的功能，Eclipse就是按照约定的编码格式对代码进行格式化的，所以只需要看看格式化后的代码长啥样就行了。具体的代码格式要求可以在Eclipse的设置中`Java`-`Code Style`查看。

## 变量

在Java中，变量分为两种：基本类型的变量和引用类型的变量。

我们先讨论基本类型的变量。

在Java中，变量必须先定义后使用，在定义变量的时候，可以给它一个初始值。例如：

```java
int x = 1;
```

上述语句定义了一个整型`int`类型的变量，名称为`x`，初始值为`1`。

不写初始值，就相当于给它指定了默认值。默认值总是`0`。

来看一个完整的定义变量，然后打印变量值的例子：

```java
// 定义并打印变量
public class Main {
    public static void main(String[] args) {
        int x = 100; // 定义int类型变量x，并赋予初始值100
        System.out.println(x); // 打印该变量的值
    }
}
```

变量的一个重要特点是可以重新赋值。例如，对变量`x`，先赋值`100`，再赋值`200`，观察两次打印的结果：

```java
// 重新赋值变量
public class Main {
    public static void main(String[] args) {
        int x = 100; // 定义int类型变量x，并赋予初始值100
        System.out.println(x); // 打印该变量的值，观察是否为100
        x = 200; // 重新赋值为200
        System.out.println(x); // 打印该变量的值，观察是否为200
    }
}
```

注意到第一次定义变量`x`的时候，需要指定变量类型`int`，因此使用语句`int x = 100;`。而第二次重新赋值的时候，变量`x`已经存在了，不能再重复定义，因此不能指定变量类型`int`，必须使用语句`x = 200;`。

变量不但可以重新赋值，还可以赋值给其他变量。让我们来看一个例子：

```java
// 变量之间的赋值
public class Main {
    public static void main(String[] args) {
        int n = 100; // 定义变量n，同时赋值为100
        System.out.println("n = " + n); // 打印n的值

        n = 200; // 变量n赋值为200
        System.out.println("n = " + n); // 打印n的值

        int x = n; // 变量x赋值为n（n的值为200，因此赋值后x的值也是200）
        System.out.println("x = " + x); // 打印x的值

        x = x + 100; // 变量x赋值为x+100（x的值为200，因此赋值后x的值是200+100=300）
        System.out.println("x = " + x); // 打印x的值
        System.out.println("n = " + n); // 再次打印n的值，n应该是200还是300？
   }
}
```

我们一行一行地分析代码执行流程：

执行`int n = 100;`，该语句定义了变量`n`，同时赋值为`100`，因此，JVM在内存中为变量`n`分配一个“存储单元”，填入值`100`：

```
      n
      │
      ▼
┌───┬───┬───┬───┬───┬───┬───┐
│   │100│   │   │   │   │   │
└───┴───┴───┴───┴───┴───┴───┘
```

执行`n = 200;`时，JVM把`200`写入变量`n`的存储单元，因此，原有的值被覆盖，现在`n`的值为`200`：

```
      n
      │
      ▼
┌───┬───┬───┬───┬───┬───┬───┐
│   │200│   │   │   │   │   │
└───┴───┴───┴───┴───┴───┴───┘
```

执行`int x = n;`时，定义了一个新的变量`x`，同时对`x`赋值，因此，JVM需要*新分配*一个存储单元给变量`x`，并写入和变量`n`一样的值，结果是变量`x`的值也变为`200`：

```
      n           x
      │           │
      ▼           ▼
┌───┬───┬───┬───┬───┬───┬───┐
│   │200│   │   │200│   │   │
└───┴───┴───┴───┴───┴───┴───┘
```

执行`x = x + 100;`时，JVM首先计算等式右边的值`x + 100`，结果为`300`（因为此刻`x`的值为`200`），然后，将结果`300`写入`x`的存储单元，因此，变量`x`最终的值变为`300`：

```
      n           x
      │           │
      ▼           ▼
┌───┬───┬───┬───┬───┬───┬───┐
│   │200│   │   │300│   │   │
└───┴───┴───┴───┴───┴───┴───┘
```

可见，变量可以反复赋值。注意，等号`=`是赋值语句，不是数学意义上的相等，否则无法解释`x = x + 100`。

### 基本数据类型

基本数据类型是CPU可以直接进行运算的类型。Java定义了以下几种基本数据类型：

- 整数类型：byte，short，int，long
- 浮点数类型：float，double
- 字符类型：char
- 布尔类型：boolean

Java定义的这些基本数据类型有什么区别呢？要了解这些区别，我们就必须简单了解一下计算机内存的基本结构。

计算机内存的最小存储单元是字节（byte），一个字节就是一个8位二进制数，即8个bit。它的二进制表示范围从`00000000`~`11111111`，换算成十进制是0~255，换算成十六进制是`00`~`ff`。

内存单元从0开始编号，称为内存地址。每个内存单元可以看作一间房间，内存地址就是门牌号。

```
  0   1   2   3   4   5   6  ...
┌───┬───┬───┬───┬───┬───┬───┐
│   │   │   │   │   │   │   │...
└───┴───┴───┴───┴───┴───┴───┘
```

一个字节是1byte，1024字节是1K，1024K是1M，1024M是1G，1024G是1T。一个拥有4T内存的计算机的字节数量就是：

```plain
4T = 4 x 1024G
   = 4 x 1024 x 1024M
   = 4 x 1024 x 1024 x 1024K
   = 4 x 1024 x 1024 x 1024 x 1024
   = 4398046511104
```

不同的数据类型占用的字节数不一样。我们看一下Java基本数据类型占用的字节数：

```
       ┌───┐
  byte │   │
       └───┘
       ┌───┬───┐
 short │   │   │
       └───┴───┘
       ┌───┬───┬───┬───┐
   int │   │   │   │   │
       └───┴───┴───┴───┘
       ┌───┬───┬───┬───┬───┬───┬───┬───┐
  long │   │   │   │   │   │   │   │   │
       └───┴───┴───┴───┴───┴───┴───┴───┘
       ┌───┬───┬───┬───┐
 float │   │   │   │   │
       └───┴───┴───┴───┘
       ┌───┬───┬───┬───┬───┬───┬───┬───┐
double │   │   │   │   │   │   │   │   │
       └───┴───┴───┴───┴───┴───┴───┴───┘
       ┌───┬───┐
  char │   │   │
       └───┴───┘
```

`byte`恰好就是一个字节，而`long`和`double`需要8个字节。

### 整型

对于整型类型，Java只定义了带符号的整型，因此，最高位的bit表示符号位（0表示正数，1表示负数）。各种整型能表示的最大范围如下：

- byte：-128 ~ 127
- short: -32768 ~ 32767
- int: -2147483648 ~ 2147483647
- long: -9223372036854775808 ~ 9223372036854775807

我们来看定义整型的例子：

```java
// 定义整型
public class Main {
    public static void main(String[] args) {
        int i = 2147483647;
        int i2 = -2147483648;
        int i3 = 2_000_000_000; // 加下划线更容易识别
        int i4 = 0xff0000; // 十六进制表示的16711680
        int i5 = 0b1000000000; // 二进制表示的512

        long n1 = 9000000000000000000L; // long型的结尾需要加L
        long n2 = 900; // 没有加L，此处900为int，但int类型可以赋值给long
        int i6 = 900L; // 错误：不能把long型赋值给int
    }
}
```

特别注意：同一个数的不同进制的表示是完全相同的，例如`15`=`0xf`＝`0b1111`。

### 浮点型

浮点类型的数就是小数，因为小数用科学计数法表示的时候，小数点是可以“浮动”的，如1234.5可以表示成12.345x102，也可以表示成1.2345x103，所以称为浮点数。

下面是定义浮点数的例子：

```java
float f1 = 3.14f;
float f2 = 3.14e38f; // 科学计数法表示的3.14x10^38
float f3 = 1.0; // 错误：不带f结尾的是double类型，不能赋值给float

double d = 1.79e308;
double d2 = -1.79e308;
double d3 = 4.9e-324; // 科学计数法表示的4.9x10^-324
```

对于`float`类型，需要加上`f`后缀。

浮点数可表示的范围非常大，`float`类型可最大表示3.4x1038，而`double`类型可最大表示1.79x10308。

### 布尔类型

布尔类型`boolean`只有`true`和`false`两个值，布尔类型总是关系运算的计算结果：

```java
boolean b1 = true;
boolean b2 = false;
boolean isGreater = 5 > 3; // 计算结果为true
int age = 12;
boolean isAdult = age >= 18; // 计算结果为false
```

Java语言对布尔类型的存储并没有做规定，因为理论上存储布尔类型只需要1 bit，但是通常JVM内部会把`boolean`表示为4字节整数。

### 字符类型

字符类型`char`表示一个字符。Java的`char`类型除了可表示标准的ASCII外，还可以表示一个Unicode字符：

```java
// 字符类型
public class Main {
    public static void main(String[] args) {
        char a = 'A';
        char zh = '中';
        System.out.println(a);
        System.out.println(zh);
    }
}
```

注意`char`类型使用单引号`'`，且仅有一个字符，要和双引号`"`的字符串类型区分开。

### 引用类型

除了上述基本类型的变量，剩下的都是引用类型。例如，引用类型最常用的就是`String`字符串：

```java
String s = "hello";
```

引用类型的变量类似于C语言的指针，它内部存储一个“地址”，指向某个对象在内存的位置，后续我们介绍类的概念时会详细讨论。

### 常量

定义变量的时候，如果加上`final`修饰符，这个变量就变成了常量：

```java
final double PI = 3.14; // PI是一个常量
double r = 5.0;
double area = PI * r * r;
PI = 300; // compile error!
```

常量在定义时进行初始化后就不可再次赋值，再次赋值会导致编译错误。

常量的作用是用有意义的变量名来避免魔术数字（Magic number），例如，不要在代码中到处写`3.14`，而是定义一个常量。如果将来需要提高计算精度，我们只需要在常量的定义处修改，例如，改成`3.1416`，而不必在所有地方替换`3.14`。

为了和变量区分开来，根据习惯，常量名通常**全部大写**。

### var关键字

有些时候，类型的名字太长，写起来比较麻烦。例如：

```java
StringBuilder sb = new StringBuilder();
```

这个时候，如果想省略变量类型，可以使用`var`关键字：

```java
var sb = new StringBuilder();
```

编译器会根据赋值语句自动推断出变量`sb`的类型是`StringBuilder`。对编译器来说，语句：

```java
var sb = new StringBuilder();
```

实际上会自动变成：

```java
StringBuilder sb = new StringBuilder();
```

因此，使用`var`定义变量，仅仅是少写了变量类型而已。

### 变量的作用范围

在Java中，多行语句用`{ ... }`括起来。很多控制语句，例如条件判断和循环，都以`{ ... }`作为它们自身的范围，例如：

```java
if (...) { // if开始
    ...
    while (...) { // while 开始
        ...
        if (...) { // if开始
            ...
        } // if结束
        ...
    } // while结束
    ...
} // if结束
```

只要正确地嵌套这些`{ ... }`，编译器就能识别出语句块的开始和结束。而在语句块中定义的变量，它有一个作用域，就是从定义处开始，到语句块结束。超出了作用域引用这些变量，编译器会报错。举个例子：

```java
{
    ...
    int i = 0; // 变量i从这里开始定义
    ...
    {
        ...
        int x = 1; // 变量x从这里开始定义
        ...
        {
            ...
            String s = "hello"; // 变量s从这里开始定义
            ...
        } // 变量s作用域到此结束
        ...
        // 注意，这是一个新的变量s，它和上面的变量同名，
        // 但是因为作用域不同，它们是两个不同的变量:
        String s = "hi";
        ...
    } // 变量x和s作用域到此结束
    ...
} // 变量i作用域到此结束
```

定义变量时，要遵循作用域最小化原则，尽量将变量定义在尽可能小的作用域，并且，不要重复使用变量名。

### 小结

Java提供了两种变量类型：基本类型和引用类型

基本类型包括整型，浮点型，布尔型，字符型。

变量可重新赋值，等号是赋值语句，不是数学意义的等号。

常量在初始化后不可重新赋值，使用常量便于理解程序意图。

## 整数运算

整数的除法对于除数为0时**运行时将报错，但编译不会报错**。

有一种**无符号**的右移运算，**使用`>>>`，**它的特点是不管符号位，右移后**高位总是补`0`**，因此，对一个负数进行`>>>`右移，它会变成正数，原因是最高位的`1`变成了`0`：

对`byte`和`short`类型进行移位时，会首先转换为`int`再进行位移。

Java没有单个bit的数据类型。在Java中，对两个整数进行位运算，实际上就是**按位对齐，然后依次对每一位进行运算**。例如：

在运算过程中，如果参与运算的两个数类型不一致，那么计算结果为较大类型的整型。例如，`short`和`int`计算，结果总是`int`，原因是`short`首先自动被转型为`int`：

```java
// 类型自动提升与强制转型
public class Main {
    public static void main(String[] args) {
        short s = 1234;
        int i = 123456;
        int x = s + i; // s自动转型为int
        short y = s + i; // 编译错误!不会截断
    }
}
```

## 浮点数运算

浮点数运算和整数运算相比，只能进行加减乘除这些数值计算，不能做位运算和移位运算。

在计算机中，浮点数虽然表示的**范围大**，但是，浮点数有个非常重要的特点，就是浮点数**常常无法精确表示**。

浮点数`0.1`在计算机中就**无法精确表示**，因为十进制的`0.1`换算成二进制是一个无限循环小数，很显然，无论使用`float`还是`double`，都只能存储一个`0.1`的**近似值**。但是，`0.5`这个浮点数又可以精确地表示。

Java的浮点数完全遵循[IEEE-754](https://standards.ieee.org/ieee/754/6210/)标准，这也是绝大多数计算机平台都支持的浮点数标准表示方法。

### 溢出

整数运算在除数为`0`时会报错，而浮点数运算在除数为`0`时，不会报错，但会返回几个特殊值：

- `NaN`表示Not a Number
- `Infinity`表示无穷大
- `-Infinity`表示负无穷大

例如：

```java
double d1 = 0.0 / 0; // NaN
double d2 = 1.0 / 0; // Infinity
double d3 = -1.0 / 0; // -Infinity
```

## 字符和字符串

因为Java在内存中总是使用Unicode表示字符，所以，一个英文字符和一个中文字符都用一个`char`类型表示，它们**都占用两个字节**。要显示一个字符的Unicode编码，只需将`char`类型直接赋值给`int`类型即可：

还可以直接用转义字符`\u`+Unicode编码来表示一个字符：

```java
// 注意是十六进制:
char c3 = '\u0041'; // 'A'，因为十六进制0041 = 十进制65
char c4 = '\u4e2d'; // '中'，因为十六进制4e2d = 十进制20013
```

如果用`+`连接字符串和其他数据类型，**会将其他数据类型先自动转型为字符串**，再连接：

```java
// 字符串连接
public class Main {
    public static void main(String[] args) {
        int age = 25;
        String s = "age is " + age;
        System.out.println(s); // age is 25
    }
}
```

### 多行字符串

如果我们要表示多行字符串，使用+号连接会非常不方便：

```java
String s = "first line \n"
         + "second line \n"
         + "end";
```

从Java 13开始，字符串可以用**`"""..."""`表示多行字符串（Text Blocks）**了。举个例子：

```java
// 多行字符串
public class Main {
    public static void main(String[] args) {
        String s = """
                   SELECT * FROM
                     users
                   WHERE id > 100
                   ORDER BY name DESC
                   """;
        System.out.println(s);
    }
}
```

上述多行字符串**实际上是5行**，在**最后一个`DESC`后面还有一个`\n`。**如果我们不想在字符串末尾加一个`\n`，就需要这么写：

```java
String s = """ 
           SELECT * FROM
             users
           WHERE id > 100
           ORDER BY name DESC""";
```

还需要注意到，多行字符串前面**共同的空格会被去掉**，即：

```java
String s = """
...........SELECT * FROM
...........  users
...........WHERE id > 100
...........ORDER BY name DESC
...........""";
```

用`.`标注的空格都会被去掉。

如果多行字符串的排版不规则，那么，去掉的空格就会变成这样：

```java
String s = """
.........  SELECT * FROM
.........    users
.........WHERE id > 100
.........  ORDER BY name DESC
.........  """;
```

即**总是以最短的行首空格为基准**。

### 不可变特性

Java的字符串除了是一个引用类型外，还有个重要特点，就是**字符串不可变**。考察以下代码：

```java
// 字符串不可变
public class Main {
    public static void main(String[] args) {
        String s = "hello";
        System.out.println(s); // 显示 hello
        s = "world";
        System.out.println(s); // 显示 world
    }
}
```

观察执行结果，难道字符串`s`变了吗？其实变的不是字符串，而是变量`s`的**“指向”**。

执行`String s = "hello";`时，**JVM虚拟机先创建字符串`"hello"`，**然后，把字符串变量`s`指向它：

```
      s
      │
      ▼
┌───┬───────────┬───┐
│   │  "hello"  │   │
└───┴───────────┴───┘
```

紧接着，执行`s = "world";`时，JVM虚拟机先创建字符串`"world"`，然后，把字符串变量`s`指向它：

```
      s ──────────────┐
                      │
                      ▼
┌───┬───────────┬───┬───────────┬───┐
│   │  "hello"  │   │  "world"  │   │
└───┴───────────┴───┴───────────┴───┘
```

**原来的字符串`"hello"`还在，只是我们无法通过变量`s`访问它而已**。因此，字符串的不可变是指字符串内容不可变。至于变量，可以一会指向字符串`"hello"`，一会指向字符串`"world"`。

```java
// 字符串不可变
public class Main {
    public static void main(String[] args) {
        String s = "hello";
        String t = s;//t终究需要指向一个String，不会指向一个s指针的
        s = "world";
        System.out.println(t); 
    }
}
```

## 数组类型

用数组来表示“一组”`int`类型。代码如下：

```java
// 数组
public class Main {
    public static void main(String[] args) {
        // 5位同学的成绩:
        int[] ns = new int[5];
        ns[0] = 68;
        ns[1] = 79;
        ns[2] = 91;
        ns[3] = 85;
        ns[4] = 62;
    }
}
```

定义一个数组类型的变量，使用数组类型“类型[]”，例如，`int[]`。和单个基本类型变量不同，**数组变量初始化必须使用`new int[5]`**表示创建一个可容纳5个`int`元素的数组。

Java的数组有几个特点：

- 数组所有元素**初始化为默认值**，整型都是`0`，浮点型是`0.0`，布尔型是`false`；
- 数组一旦创建后，大小就不可改变。

可以用`数组变量.length`获取数组大小：

```java
// 数组
public class Main {
    public static void main(String[] args) {
        // 5位同学的成绩:
        int[] ns = new int[5];
        System.out.println(ns.length); // 5
    }
}
```

数组是引用类型，在使用索引访问数组元素时，如果索引超出范围，运行时将报错：

也可以在定义数组时直接指定初始化的元素，这样就不必写出数组大小，而是由**编译器自动推算数组大小**。例如：

```java
// 数组
public class Main {
    public static void main(String[] args) {
        // 5位同学的成绩:
        int[] ns = new int[] { 68, 79, 91, 85, 62 };
        System.out.println(ns.length); // 编译器自动推算数组大小为5
    }
}
```

还可以进一步简写为：

```java
int[] ns = { 68, 79, 91, 85, 62 }
```

## 输入和输出

### 输出

在前面的代码中，我们总是**使用`System.out.println()`**来向屏幕输出一些内容。

**`println`是print line的缩写**，表示输出并换行。因此，如果输出后**不想换行，可以用`print()`：**

### 格式化输出

Java还提供了格式化输出的功能。为什么要格式化输出？因为计算机表示的数据不一定适合人来阅读：

```java
// 格式化输出
public class Main {
    public static void main(String[] args) {
        double d = 12900000;
        System.out.println(d); // 1.29E7
    }
}
```

如果要把数据显示成我们期望的格式，就需要使用格式化输出的功能。**格式化输出使用`System.out.printf()`，**通过使用占位符`%?`，`printf()`可以把后面的参数格式化成指定格式：

```java
// 格式化输出
public class Main {
    public static void main(String[] args) {
        double d = 3.1415926;
        System.out.printf("%.2f\n", d); // 显示两位小数3.14
        System.out.printf("%.4f\n", d); // 显示4位小数3.1416
    }
}
```

Java的格式化功能提供了多种占位符，可以把各种数据类型“格式化”成指定的字符串：

| 占位符 | 说明                             |
| ------ | -------------------------------- |
| %d     | 格式化输出整数                   |
| %x     | 格式化输出十六进制整数           |
| %f     | 格式化输出浮点数                 |
| %e     | 格式化输出科学计数法表示的浮点数 |
| %s     | 格式化字符串                     |

注意，由于`%`表示占位符，因此，**连续两个`%%`(不能用\\%)表示一个`%`字符本身。**

占位符本身还可以有更详细的格式化参数。下面的例子把一个整数格式化成十六进制，并用0补足8位：

```java
// 格式化输出
public class Main {
    public static void main(String[] args) {
        int n = 12345000;
        System.out.printf("n=%d, hex=%08x", n, n); // 注意，两个%占位符必须传入两个数
    }
}
```

### 输入

和输出相比，Java的输入就要复杂得多。

我们先看一个从控制台读取一个字符串和一个整数的例子：

```java
import java.util.Scanner;
public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in); // 创建Scanner对象
        System.out.print("Input your name: "); // 打印提示
        String name = scanner.nextLine(); // 读取一行输入并获取字符串
        System.out.print("Input your age: "); // 打印提示
        int age = scanner.nextInt(); // 读取一行输入并获取整数
        System.out.printf("Hi, %s, you are %d\n", name, age); // 格式化输出
    }
}
```

首先，我们通过`import`语句导入`java.util.Scanner`，`import`是导入某个类的语句，必须放到Java源代码的开头，后面我们在Java的`package`中会详细讲解如何使用`import`。

然后，创建`Scanner`对象并传入`System.in`。`System.out`代表标准输出流，而`System.in`代表标准输入流。直接使用`System.in`读取用户输入虽然是可以的，但需要更复杂的代码，而通过`Scanner`就可以简化后续的代码。

有了**`Scanner`对象**后，要读取用户输入的字符串，使用`scanner.nextLine()`，要读取用户输入的整数，使用`scanner.nextInt()`。`Scanner`会自动转换数据类型，因此不必手动转换。

要测试输入，必须从命令行读取用户输入，因此，**需要走编译、执行的流程：**

```plain
$ javac Main.java
```

这个程序编译时如果有警告，可以暂时忽略它，在后面学习IO的时候再详细解释。编译成功后，执行：

```plain
$ java Main
Input your name: Bob ◀── 输入 Bob
Input your age: 12   ◀── 输入 12
Hi, Bob, you are 12  ◀── 输出
```

根据提示分别输入一个字符串和整数后，我们得到了格式化的输出。

### 判断引用类型相等

在Java中，判断值类型的变量是否相等，可以使用`==`运算符。但是，判断引用类型的变量是否相等，`==`表示“引用是否相等”，或者说，**是否指向同一个对象**。例如，下面的两个String类型，它们的**内容是相同的，但是，分别指向不同的对象，用`==`判断，结果为`false`,要判断引用类型的变量内容是否相等，必须使用`equals()`方法：**

注意：执行语句`s1.equals(s2)`时，如果变量`s1`为`null`，会报`NullPointerException`：

```java
// 条件判断
public class Main {
    public static void main(String[] args) {
        String s1 = null;
        if (s1.equals("hello")) {
            System.out.println("hello");
        }
    }
}
```

要**避免`NullPointerException`错误**，可以利用短路运算符`&&`：

```java
// 条件判断
public class Main {
    public static void main(String[] args) {
        String s1 = null;
        if (s1 != null && s1.equals("hello")) {
            System.out.println("hello");
        }
    }
}
```

还可以把一定不是`null`的对象`"hello"`放到前面：例如：`if ("hello".equals(s)) { ... }`。

使用`switch`时，注意`case`语句并没有花括号`{}`，任何时候都不要忘记写`break`,当`option = 2`时，将**依次输出(先匹配到,然后一直输出)**

`switch`语句还可以匹配字符串。字符串匹配时，是**比较“内容相等”**。

## 流程控制

### switch表达式

使用`switch`时，如果遗漏了`break`，就会造成严重的逻辑错误，而且不易在源代码中发现错误。从Java 12开始，`switch`语句升级为更简洁的表达式语法，使用类似模式匹配（Pattern Matching）的方法，保证**只有一种路径会被执行，并且不需要`break`语句：**

```java
// switch
public class Main {
    public static void main(String[] args) {
        String fruit = "apple";
        switch (fruit) {
        case "apple" -> System.out.println("Selected apple");
        case "pear" -> System.out.println("Selected pear");
        case "mango" -> {
            System.out.println("Selected mango");
            System.out.println("Good choice!");
        }
        default -> System.out.println("No fruit selected");
        }
    }
}
```

**注意新语法使用`->`，如果有多条语句，需要用`{}`括起来**。不要写`break`语句，因为新语法只会执行匹配的语句，*没有*穿透效应。

很多时候，我们还可能用`switch`语句给某个变量赋值。例如：

使用新的`switch`语法，不但不需要`break`，还可以直接返回值。把上面的代码改写如下：

```java
// switch
public class Main {
    public static void main(String[] args) {
        String fruit = "apple";
        int opt = switch (fruit) {
            case "apple" -> 1;
            case "pear", "mango" -> 2;
            default -> 0;
        }; // 注意赋值语句要以;结束
        System.out.println("opt = " + opt);
    }
}
```

#### yield

大多数时候，在`switch`表达式内部，我们会返回简单的值。

但是，如果需要复杂的语句，我们也可以写很多语句，放到`{...}`里，然后，**用`yield`返回一个值作为`switch`语句的返回值**：

```java
// yield
public class Main {
    public static void main(String[] args) {
        String fruit = "orange";
        int opt = switch (fruit) {
            case "apple" -> 1;
            case "pear", "mango" -> 2;
            default -> {
                int code = fruit.hashCode();
                yield code; // switch语句返回值
            }
        };
        System.out.println("opt = " + opt);
    }
}
```

### for each循环

`for`循环经常用来遍历数组，因为通过计数器可以根据索引来访问数组的每个元素：

```java
int[] ns = { 1, 4, 9, 16, 25 };
for (int i=0; i<ns.length; i++) {
    System.out.println(ns[i]);
}
```

但是，很多时候，我们实际上真正想要访问的是数组每个元素的值。Java还提供了另一种`for each`循环，它可以更简单地遍历数组：

```java
// for each
public class Main {
    public static void main(String[] args) {
        int[] ns = { 1, 4, 9, 16, 25 };
        for (int n : ns) {
            System.out.println(n);
        }
    }
}
```

和`for`循环相比，`for each`循环的变量n不再是计数器，而是**直接对应到数组的每个元素**。`for each`循环的写法也更简洁。但是，`for each`循环**无法指定遍历顺序，也无法获取数组的索引**。

除了数组外，`for each`循环能够遍历所有“可迭代”的数据类型，包括后面会介绍的`List`、`Map`等。

`for each`循环打印也很麻烦。幸好Java标准库提供了`Arrays.toString()`，可以快速打印数组内容：

```java
// 遍历数组
import java.util.Arrays;

public class Main {
    public static void main(String[] args) {
        int[] ns = { 1, 1, 2, 3, 5, 8 };
        System.out.println(Arrays.toString(ns));
    }
}
```

Java的标准库已经**内置了排序功能**，我们只需要调用JDK提供的`Arrays.sort()`就可以排序：

```java
// 排序
import java.util.Arrays;

public class Main {
    public static void main(String[] args) {
        int[] ns = { 28, 12, 89, 73, 65, 18, 96, 50, 8, 36 };
        Arrays.sort(ns);
        System.out.println(Arrays.toString(ns));
    }
}
```

必须注意，对数组排序实际上**修改了数组本身。**

如果对一个字符串数组进行排序，例如：

```java
String[] ns = { "banana", "apple", "pear" };
```

排序前，这个数组在内存中表示如下：

```
                   ┌──────────────────────────────────┐
               ┌───┼──────────────────────┐           │
               │   │                      ▼           ▼
         ┌───┬─┴─┬─┴─┬───┬────────┬───┬───────┬───┬──────┬───┐
ns ─────▶│░░░│░░░│░░░│   │"banana"│   │"apple"│   │"pear"│   │
         └─┬─┴───┴───┴───┴────────┴───┴───────┴───┴──────┴───┘
           │                 ▲
           └─────────────────┘
```

调用`Arrays.sort(ns);`排序后，这个数组在内存中表示如下：

```
                   ┌──────────────────────────────────┐
               ┌───┼──────────┐                       │
               │   │          ▼                       ▼
         ┌───┬─┴─┬─┴─┬───┬────────┬───┬───────┬───┬──────┬───┐
ns ─────▶│░░░│░░░│░░░│   │"banana"│   │"apple"│   │"pear"│   │
         └─┬─┴───┴───┴───┴────────┴───┴───────┴───┴──────┴───┘
           │                              ▲
           └──────────────────────────────┘
```

原来的3个字符串在内存中均没有任何变化，但是**`ns`数组的每个元素指向变化了**。

## 多维数组

### 二维数组

二维数组就是数组的数组。定义一个二维数组如下：

```java
// 二维数组
public class Main {
    public static void main(String[] args) {
        int[][] ns = {
            { 1, 2, 3, 4 },
            { 5, 6, 7, 8 },
            { 9, 10, 11, 12 }
        };
        System.out.println(ns.length); // 3
    }
}
```

因为`ns`包含3个数组，因此，`ns.length`为`3`。实际上`ns`在内存中的结构如下：

```
                    ┌───┬───┬───┬───┐
         ┌───┐  ┌──▶│ 1 │ 2 │ 3 │ 4 │
ns ─────▶│░░░│──┘   └───┴───┴───┴───┘
         ├───┤      ┌───┬───┬───┬───┐
         │░░░│─────▶│ 5 │ 6 │ 7 │ 8 │
         ├───┤      └───┴───┴───┴───┘
         │░░░│──┐   ┌───┬───┬───┬───┐
         └───┘  └──▶│ 9 │10 │11 │12 │
                    └───┴───┴───┴───┘
```

如果我们定义一个普通数组`arr0`，然后把`ns[0]`赋值给它：

```java
// 二维数组
public class Main {
    public static void main(String[] args) {
        int[][] ns = {
            { 1, 2, 3, 4 },
            { 5, 6, 7, 8 },
            { 9, 10, 11, 12 }
        };
        int[] arr0 = ns[0];
        System.out.println(arr0.length); // 4
    }
}
```

实际上`arr0`就获取了`ns`数组的第0个元素。因为`ns`数组的每个元素也是一个数组，因此，**`arr0`指向的数组**就是`{ 1, 2, 3, 4 }`。在内存中，结构如下：

```
            arr0 ─────┐
                      ▼
                    ┌───┬───┬───┬───┐
         ┌───┐  ┌──▶│ 1 │ 2 │ 3 │ 4 │
ns ─────▶│░░░│──┘   └───┴───┴───┴───┘
         ├───┤      ┌───┬───┬───┬───┐
         │░░░│─────▶│ 5 │ 6 │ 7 │ 8 │
         ├───┤      └───┴───┴───┴───┘
         │░░░│──┐   ┌───┬───┬───┬───┐
         └───┘  └──▶│ 9 │10 │11 │12 │
                    └───┴───┴───┴───┘
```

二维数组的每个数组元素的长度**并不要求相同**，例如，可以这么定义`ns`数组：

```java
int[][] ns = {
    { 1, 2, 3, 4 },
    { 5, 6 },
    { 7, 8, 9 }
};
```

这个二维数组在内存中的结构如下：

```
                    ┌───┬───┬───┬───┐
         ┌───┐  ┌──▶│ 1 │ 2 │ 3 │ 4 │
ns ─────▶│░░░│──┘   └───┴───┴───┴───┘
         ├───┤      ┌───┬───┐
         │░░░│─────▶│ 5 │ 6 │
         ├───┤      └───┴───┘
         │░░░│──┐   ┌───┬───┬───┐
         └───┘  └──▶│ 7 │ 8 │ 9 │
                    └───┴───┴───┘
```

要打印一个二维数组，可以使用**两层嵌套的for循环：**

```java
for (int[] arr : ns) {
    for (int n : arr) {
        System.out.print(n);
        System.out.print(', ');
    }
    System.out.println();
}
```

或者使用Java标准库的**`Arrays.deepToString()`：**

```java
// 二维数组
import java.util.Arrays;

public class Main {
    public static void main(String[] args) {
        int[][] ns = {
            { 1, 2, 3, 4 },
            { 5, 6 },
            { 7, 8, 9 }
        };
        System.out.println(Arrays.deepToString(ns));
    }
}
```

## 命令行参数

Java程序的**入口是`main`方法，**而`main`方法可以**接受一个命令行参数，它是一个`String[]`数组。**

这个命令行参数**由JVM接收用户输入并传给`main`方法**：

```java
public class Main {
    public static void main(String[] args) {
        for (String arg : args) {
            System.out.println(arg);
        }
    }
}
```

我们可以利用接收到的命令行参数，根据不同的参数执行不同的代码。例如，实现一个`-version`参数，打印程序版本号：

```java
public class Main {
    public static void main(String[] args) {
        for (String arg : args) {
            if ("-version".equals(arg)) {
                System.out.println("v 1.0");
                break;
            }
        }
    }
}
```

上面这个程序必须在命令行执行，我们**先编译它**：

```plain
$ javac Main.java
```

然后，执行的时候，给它传递一个`-version`参数：

```plain
$ java Main -version
v 1.0
```

这样，程序就可以根据传入的命令行参数，作出不同的响应。

# 面向对象基础

### class和instance

所以，只要理解了class和instance的概念，基本上就明白了什么是面向对象编程。

class是一种**对象模版**，它定义了如何创建实例，因此，class本身就是一种**数据类型**：

![class](./java%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0/class.jpg)

而instance是**对象实例**，instance是根据class创建的实例，可以创建多个instance，每个instance**类型相同，但各自属性可能不相同**：

![instances](./java%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0/instances.jpg)

### 定义class

在Java中，创建一个类，例如，给这个类命名为`Person`，就是定义一个`class`：

```java
class Person {
    public String name;
    public int age;
}
```

一个`class`可以包含多个**字段（`field`）**，字段用来描述一个类的特征。上面的`Person`类，我们定义了两个字段，一个是`String`类型的字段，命名为`name`，一个是`int`类型的字段，命名为`age`。因此，通过`class`，把一组数据汇集到一个对象上，实现了数据封装。

`public`是用来修饰字段的，它表示这个字段可以被外部访问。

### 创建实例

定义了class，只是定义了对象模版，而要**根据对象模版创建出真正的对象实例，必须用new操作符。**

new操作符可以创建一个实例，然后，我们需要定义一个**引用类型的变量**来指向这个实例：

```java
Person ming = new Person()
```

上述代码创建了一个Person类型的实例，并**通过变量`ming`指向它。**

注意区分`Person ming`是定义`Person`类型的变量`ming`，而`new Person()`是创建`Person`实例。

有了指向这个实例的变量，我们就可以通过这个变量来操作实例。访问实例变量可以用`变量.字段`，例如：

```java
ming.name = "Xiao Ming"; // 对字段name赋值
ming.age = 12; // 对字段age赋值
System.out.println(ming.name); // 访问字段name

Person hong = new Person();
hong.name = "Xiao Hong";
hong.age = 15;
```

上述两个变量分别指向两个不同的实例，它们在内存中的结构如下：

```
            ┌──────────────────┐
ming ──────▶│Person instance   │
            ├──────────────────┤
            │name = "Xiao Ming"│
            │age = 12          │
            └──────────────────┘
            ┌──────────────────┐
hong ──────▶│Person instance   │
            ├──────────────────┤
            │name = "Xiao Hong"│
            │age = 15          │
            └──────────────────┘
```

两个`instance`拥有`class`定义的`name`和`age`字段，且各自都有一份独立的数据，互不干扰。

一个Java源文件可以**包含多个类的定义，但只能定义一个public类**，且**public类名必须与文件名一致**。**如果要定义多个public类，必须拆到多个Java源文件中。**

把`field`从`public`改成`private`，外部代码不能访问这些`field`，那我们定义这些`field`有什么用？怎么才能给它赋值？怎么才能读取它的值？

所以我们需要**使用方法（`method`）来让外部代码可以间接修改`field`：**

```java
// private field
public class Main {
    public static void main(String[] args) {
        Person ming = new Person();
        ming.setName("Xiao Ming"); // 设置name
        ming.setAge(12); // 设置age
        System.out.println(ming.getName() + ", " + ming.getAge());
    }
}

class Person {
    private String name;
    private int age;

    public String getName() {
        return this.name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return this.age;
    }

    public void setAge(int age) {
        if (age < 0 || age > 100) {
            throw new IllegalArgumentException("invalid age value");
        }
        this.age = age;
    }
}
```

虽然外部代码不能直接修改`private`字段，但是，外部代码可以调用方法`setName()`和`setAge()`来间接修改`private`字段。在方法内部，我们就**有机会检查参数对不对**。比如，`setAge()`就会检查传入的参数，参数超出了范围，直接报错。这样，外部代码就**没有任何机会把`age`设置成不合理的值。**

对`setName()`方法同样可以做检查，例如，不允许传入`null`和空字符串：

```java
public void setName(String name) {
    if (name == null || name.isBlank()) {
        throw new IllegalArgumentException("invalid name");
    }
    this.name = name.strip(); // 去掉首尾空格
}
```

所以，一个类通过定义方法，就可以**给外部代码暴露一些操作的接口，同时，内部自己保证逻辑一致性**。

### this变量

在方法内部，可以使用一个隐含的变量`this`，它始终指向当前实例。因此，通过`this.field`就可以访问当前实例的字段。

如果**没有命名冲突，可以省略`this`。**例如：

```java
class Person {
    private String name;

    public String getName() {
        return name; // 相当于this.name
    }
}
```

但是，如果有局部变量和字段重名，那么局部变量优先级更高，就必须加上`this`：

```java
class Person {
    private String name;

    public void setName(String name) {
        this.name = name; // 前面的this不可少，少了就变成局部变量name了
    }
}
```

### 方法参数

方法可以包含0个或任意个参数。方法参数用于接收传递给方法的变量值。调用方法时，必须严格按照参数的定义一一传递。例如：

```java
class Person {
    ...
    public void setNameAndAge(String name, int age) {
        ...
    }
}
```

调用这个`setNameAndAge()`方法时，必须有两个参数，且第一个参数必须为`String`，第二个参数必须为`int`：

```java
Person ming = new Person();
ming.setNameAndAge("Xiao Ming"); // 编译错误：参数个数不对
ming.setNameAndAge(12, "Xiao Ming"); // 编译错误：参数类型不对
```

### 可变参数

可变参数用`类型...`定义，可变参数相当于数组类型：

```java
class Group {
    private String[] names;

    public void setNames(String... names) {
        this.names = names;
    }
}
```

上面的`setNames()`就定义了一个可变参数。调用时，可以这么写：

```java
Group g = new Group();
g.setNames("Xiao Ming", "Xiao Hong", "Xiao Jun"); // 传入3个String
g.setNames("Xiao Ming", "Xiao Hong"); // 传入2个String
g.setNames("Xiao Ming"); // 传入1个String
g.setNames(); // 传入0个String
```

完全可以把可变参数改写为`String[]`类型：

```java
class Group {
    private String[] names;

    public void setNames(String[] names) {
        this.names = names;
    }
}
```

但是，调用方需要自己先构造`String[]`，比较麻烦。例如：

```java
Group g = new Group();
g.setNames(new String[] {"Xiao Ming", "Xiao Hong", "Xiao Jun"}); // 传入1个String[]
```

另一个问题是，调用方可以传入`null`：

```java
Group g = new Group();
g.setNames(null);
```

而可变参数**可以保证无法传入`null`，**因为传入0个参数时，接收到的实际值是一个**空数组**而不是`null`。

### 参数绑定

调用方把参数传递给实例方法时，调用时传递的值会按参数位置一一绑定。

那什么是参数绑定？

我们先观察一个基本类型参数的传递：

```java
// 基本类型参数绑定
public class Main {
    public static void main(String[] args) {
        Person p = new Person();
        int n = 15; // n的值为15
        p.setAge(n); // 传入n的值
        System.out.println(p.getAge()); // 15
        n = 20; // n的值改为20
        System.out.println(p.getAge()); // 15还是20?用的是实例里面的属性,不是n
    }
}

class Person {
    private int age;
    public int getAge() {
        return this.age;
    }
    public void setAge(int age) {
        this.age = age;
    }
}
```

运行代码，从结果可知，修改外部的局部变量`n`，不影响实例`p`的`age`字段，原因是`setAge()`方法获得的参数，复制了`n`的值，因此，`p.age`和局部变量`n`互不影响。

结论：基本类型参数的传递，是调用方值的复制。双方各自的后续修改，互不影响。

我们再看一个**传递引用参数**的例子：

```java
// 引用类型参数绑定
public class Main {
    public static void main(String[] args) {
        Person p = new Person();
        String[] fullname = new String[] { "Homer", "Simpson" };
        p.setName(fullname); // 传入fullname数组
        System.out.println(p.getName()); // "Homer Simpson"
        fullname[0] = "Bart"; // fullname数组的第一个元素修改为"Bart"
        System.out.println(p.getName()); // "Homer Simpson"还是"Bart Simpson"?
    }
}

class Person {
    private String[] name;
    public String getName() {
        return this.name[0] + " " + this.name[1];
    }
    public void setName(String[] name) {
        this.name = name;
    }
}
```

注意到`setName()`的参数现在是一个数组。一开始，把`fullname`数组传进去，然后，修改`fullname`数组的内容，结果发现，实例`p`的字段`p.name`也被修改了！

### 构造方法

构造方法是如此特殊，所以构造方法的名称就是类名。构造方法的参数没有限制，在方法内部，也可以编写任意语句。但是，和普通方法相比，**构造方法没有返回值（也没有`void`）**，**调用构造方法，必须用`new`操作符**。如果既要能使用带参数的构造方法，又想保留不带参数的构造方法，那么**只能把两个构造方法都定义出来**：没有在构造方法中初始化字段时，引用类型的字段默认是`null`，数值类型的字段用默认值，`int`类型默认值是`0`，布尔类型默认值是`false`：也可以对字段直接进行初始化：

一个构造方法可以**调用其他构造方法**，这样做的目的是**便于代码复用**。**调用其他构造方法的语法是`this(…)`：**

```java
class Person {
    private String name;
    private int age;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public Person(String name) {
        this(name, 18); // 调用另一个构造方法Person(String, int)
    }

    public Person() {
        this("Unnamed"); // 调用另一个构造方法Person(String)
    }
}
```

## 继承

不要写重复的代码,复用代码,只需要编写新增的功能

```java
Java使用extends关键字来实现继承：

class Person {
    private String name;
    private int age;

    public String getName() {...}
    public void setName(String name) {...}
    public int getAge() {...}
    public void setAge(int age) {...}
}

class Student extends Person {
    // 不要重复name和age字段/方法,
    // 只需要定义新增score字段/方法:
    private int score;

    public int getScore() { … }
    public void setScore(int score) { … }
}
```

子类自动获得了父类的所有字段，**严禁定义与父类重名的字段！**

在OOP的术语中，我们把**`Person`称为超类（super class），父类（parent class），基类（base class）**，把**`Student`称为子类（subclass），扩展类（extended class）**。

### 继承树

注意到我们在定义`Person`的时候，没有写`extends`。在Java中，没有明确写`extends`的类，编译器会自动加上`extends Object`。所以，**任何类，除了`Object`，都会继承自某个类**。下图是`Person`、`Student`的继承树：

```
┌───────────┐
│  Object   │
└───────────┘
      ▲
      │
┌───────────┐
│  Person   │
└───────────┘
      ▲
      │
┌───────────┐
│  Student  │
└───────────┘
```

Java只允许一个class继承自一个类，因此，一个类有且仅有一个父类。只有`Object`特殊，它没有父类。

类似的，如果我们定义一个继承自`Person`的`Teacher`，它们的继承树关系如下：

```java
       ┌───────────┐
       │  Object   │
       └───────────┘
             ▲
             │
       ┌───────────┐
       │  Person   │
       └───────────┘
          ▲     ▲
          │     │
          │     │
┌───────────┐ ┌───────────┐
│  Student  │ │  Teacher  │
└───────────┘ └───────────┘
```

### protected

继承有个特点，就是**子类无法访问父类的`private`字段或者`private`方法**。例如，`Student`类就无法访问`Person`类的`name`和`age`字段：

```java
class Person {
    private String name;
    private int age;
}

class Student extends Person {
    public String hello() {
        return "Hello, " + name; // 编译错误：无法访问name字段
    }
}
```

这使得继承的作用被削弱了。为了让子类可以访问父类的字段，我们需要把`private`改为`protected`。用`protected`修饰的字段**可以被子类访问**：

```java
class Person {
    protected String name;
    protected int age;
}

class Student extends Person {
    public String hello() {
        return "Hello, " + name; // OK!
    }
}
```

因此，`protected`关键字可以把字段和方法的访问权限控制在继承树内部，一个`protected`字段和方法可以被其子类，以及子类的子类所访问，后面我们还会详细讲解。

### super

`super`关键字表示父类（超类）。子类引用父类的字段时，可以用`super.fieldName`。例如：

```java
class Student extends Person {
    public String hello() {
        return "Hello, " + super.name;
    }
}
```

实际上，这里使用`super.name`，或者`this.name`，或者`name`，效果都是一样的。编译器会**自动定位到父类的`name`字段**。

但是，在**某些时候，就必须使用`super`。**

```java
// super
public class Main {
    public static void main(String[] args) {
        Student s = new Student("Xiao Ming", 12, 89);
    }
}

class Person {
    protected String name;
    protected int age;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
}

class Student extends Person {
    protected int score;

    public Student(String name, int age, int score) {
        this.score = score;
    }
}

```

运行上面的代码，会得到一个编译错误，大意是**在`Student`的构造方法中，无法调用`Person`的构造方法。**

这是因为在Java中，任何`class`的构造方法，第一行语句必须是**调用父类的构造方法**。如果没有明确地调用父类的构造方法，编译器会帮我们自动加一句`super();`，所以，`Student`类的构造方法实际上是这样：

```java
class Student extends Person {
    protected int score;

    public Student(String name, int age, int score) {
        super(); // 自动调用父类的构造方法
        this.score = score;
    }
}

```

但是，**`Person`类并没有无参数的构造方法**，因此，编译失败。

解决方法是调用`Person`类存在的某个构造方法。例如：

```java
class Student extends Person {
    protected int score;

    public Student(String name, int age, int score) {
        super(name, age); // 调用父类的构造方法Person(String, int)
        this.score = score;
    }
}

```

这样就可以正常编译了！

因此我们得出结论：如果**父类没有默认的构造方法**，**子类就必须显式调用`super()`并给出参数**以便让编译器定位到父类的一个合适的构造方法。

这里还顺带引出了另一个问题：即子类*不会继承*任何父类的构造方法。子类默认的构造方法是编译器自动生成的，不是继承的。

### 阻止继承

正常情况下，只要某个class**没有`final`修饰符，那么任何类都可以从该class继承。**

从Java 15开始，允许使用`sealed`修饰class，并通过`permits`明确写出能够从该class继承的子类名称。

例如，定义一个`Shape`类：

```java
public sealed class Shape permits Rect, Circle, Triangle {
    ...
}
```

上述`Shape`类就是一个`sealed`类，它只允许指定的3个类继承它。如果写：

```java
public final class Rect extends Shape {...}
```

是没问题的，因为`Rect`出现在`Shape`的`permits`列表中。但是，如果定义一个`Ellipse`就会报错：

```java
public final class Ellipse extends Shape {...}
// Compile error: class is not allowed to extend sealed class: Shape
```

原因是`Ellipse`并未出现在`Shape`的`permits`列表中。这种`sealed`类主要用于一些框架，防止继承被滥用。

`sealed`类在Java 15中目前是预览状态，要启用它，必须使用参数`--enable-preview`和`--source 15`。

### 向上转型

如果一个引用变量的类型是`Student`，那么它可以指向一个`Student`类型的实例：

```java
Student s = new Student();
```

如果一个引用类型的变量是`Person`，那么它可以指向一个`Person`类型的实例：

```java
Person p = new Person();
```

现在问题来了：如果`Student`是从`Person`继承下来的，那么，一个引用类型为`Person`的变量，能否指向`Student`类型的实例？

```java
Person p = new Student(); // ???
```

测试一下就可以发现，这种指向是**允许的！**

这是因为`Student`继承自`Person`，因此，它**拥有`Person`的全部功能**。`Person`类型的变量，如果指向`Student`类型的实例，对它进行操作，是没有问题的！

这种**把一个子类类型安全地变为父类类型的赋值**，被称为**向上转型（upcasting）**。

### 向下转型

和向上转型相反，如果把一个父类类型强制转型为子类类型，就是向下转型（downcasting）。例如：

```java
Person p1 = new Student(); // upcasting, ok
Person p2 = new Person();
Student s1 = (Student) p1; // ok
Student s2 = (Student) p2; // runtime error! ClassCastException!
```

如果测试上面的代码，可以发现：

`Person`类型`p1`实际指向`Student`实例，`Person`类型变量`p2`实际指向`Person`实例。在向下转型的时候，把`p1`转型为`Student`会成功，因为`p1`确实指向`Student`实例，把`p2`转型为`Student`会失败，因为`p2`的实际类型是`Person`，不能把父类变为子类，因为子类功能比父类多，**多的功能无法凭空变出来。**

因此，向下转型很可能会失败。失败的时候，Java虚拟机会报`ClassCastException`。

为了避免向下转型出错，Java**提供了`instanceof`操作符，可以先判断一个实例究竟是不是某种类型**：

```java
Person p = new Person();
System.out.println(p instanceof Person); // true
System.out.println(p instanceof Student); // false

Student s = new Student();
System.out.println(s instanceof Person); // true
System.out.println(s instanceof Student); // true

Student n = null;
System.out.println(n instanceof Student); // false
```

`instanceof`实际上判断一个变量所指向的实例是否是指定类型，或者这个类型的子类。如果一个引用变量为`null`，那么对任何`instanceof`的判断都为`false`。

利用`instanceof`，在向下转型前可以先判断：

```java
Person p = new Student();
if (p instanceof Student) {
    // 只有判断成功才会向下转型:
    Student s = (Student) p; // 一定会成功
}
```

从Java 14开始，判断`instanceof`后，可以直接转型为指定变量，避免再次强制转型。例如，对于以下代码：

```java
Object obj = "hello";
if (obj instanceof String) {
    String s = (String) obj;
    System.out.println(s.toUpperCase());
}
```

可以改写如下：

```java
// instanceof variable:
public class Main {
    public static void main(String[] args) {
        Object obj = "hello";
        if (obj instanceof String s) {
            // 可以直接使用变量s:
            System.out.println(s.toUpperCase());
        }
    }
}
```

这种使用`instanceof`的写法更加简洁。

### 区分继承和组合

在使用继承时，我们要注意逻辑一致性。

考察下面的`Book`类：

```java
class Book {
    protected String name;
    public String getName() {...}
    public void setName(String name) {...}
}
```

这个`Book`类也有`name`字段，那么，我们能不能让`Student`继承自`Book`呢？

```java
class Student extends Book {
    protected int score;
}
```

显然，从逻辑上讲，这是不合理的，`Student`不应该从`Book`继承，而应该从`Person`继承。

究其原因，是因为`Student`是`Person`的一种，它们是is关系，而`Student`并不是`Book`。实际上`Student`和`Book`的关系是has关系。

具有has关系不应该使用继承，而是使用组合，即`Student`可以持有一个`Book`实例：

```java
class Student extends Person {
    protected Book book;
    protected int score;
}
```

因此，继承是is关系，组合是has关系。

## 多态

在继承关系中，子类如果定义了一个与父类方法**签名完全相同的方法**，被称为**覆写（Override）。**

例如，在`Person`类中，我们定义了`run()`方法：

```java
class Person {
    public void run() {
        System.out.println("Person.run");
    }
}
```

在子类`Student`中，覆写这个`run()`方法：

```java
class Student extends Person {
    @Override
    public void run() {
        System.out.println("Student.run");
    }
}
```

Override和Overload不同的是，如果**方法签名不同，就是Overload**，Overload方法是**一个新方法**；如果**方法签名相同，并且返回值也相同，就是`Override`**。

方法名相同，方法**参数相同**，但方法**返回值不同，也是不同的方法**。在Java程序中，出现这种情况，编译器会**报错**。

```java
class Person {
    public void run() { … }
}

class Student extends Person {
    // 不是Override，因为参数不同:
    public void run(String s) { … }
    // 不是Override，因为返回值不同:
    public int run() { … }
}
```

加上`@Override`可以让编译器**帮助检查是否进行了正确的覆写**。希望进行覆写，但是**不小心写错了方法签名，编译器会报错**。

```java
// override
public class Main {
    public static void main(String[] args) {
    }
}

class Person {
    public void run() {}
}

public class Student extends Person {
    @Override // Compile error!
    public void run(String s) {}
}
```

但是`@Override`不是必需的。

在上一节中，我们已经知道，引用变量的声明类型可能与其实际类型不符，例如：

```java
Person p = new Student();
```

现在，我们考虑一种情况，如果**子类覆写了父类的方法**：

```java
// override
public class Main {
    public static void main(String[] args) {
        Person p = new Student();
        p.run(); // 应该打印Person.run还是Student.run?
    }
}

class Person {
    public void run() {
        System.out.println("Person.run");
    }
}

class Student extends Person {
    @Override
    public void run() {
        System.out.println("Student.run");
    }
}
```

那么，一个实际类型为`Student`，引用类型为`Person`的变量，调用其`run()`方法，调用的是`Person`还是`Student`的`run()`方法？

运行一下上面的代码就可以知道，实际上调用的方法**是`Student`的`run()`方法。**因此可得出结论：

Java的实例方法调用是**基于运行时的实际类型的动态调用**，而**非变量的声明类型**。

这个非常重要的特性在面向对象编程中称之为多态。它的英文拼写非常复杂：**Polymorphic。**

### 多态

多态是指，针对某个类型的方法调用，其**真正执行的方法取决于运行时期实际类型的方法。**这样子就可以避免额外判断类型,归一至父类简化地编写,**运行时程序根据具体类型再决定具体调用的方法.**例如：

```java
Person p = new Student();
p.run(); // 无法确定运行时究竟调用哪个run()方法
```

有同学会说，从上面的代码一看就明白，肯定调用的是`Student`的`run()`方法啊。

但是，假设我们编写这样一个方法：

```java
public void runTwice(Person p) {
    p.run();
    p.run();
}
```

它传入的参数类型是`Person`，我们是无**法知道传入的参数实际类型究竟是`Person`，还是`Student`，还是`Person`的其他子类例如`Teacher`，**因此，也无法确定调用的是不是`Person`类定义的`run()`方法。

所以，多态的特性就是，**运行期才能动态决定**调用的子类方法。对某个类型调用某个方法，执行的实际方法可能是某个子类的覆写方法。这种**不确定性的方法调用**，究竟有什么作用？

我们还是来举例子。

假设我们定义一种收入，需要给它报税，那么先定义一个`Income`类：

```java
class Income {
    protected double income;
    public double getTax() {
        return income * 0.1; // 税率10%
    }
}
```

对于工资收入，可以减去一个基数，那么我们可以从`Income`派生出`SalaryIncome`，并覆写`getTax()`：

```java
class Salary extends Income {
    @Override
    public double getTax() {
        if (income <= 5000) {
            return 0;
        }
        return (income - 5000) * 0.2;
    }
}
```

如果你享受国务院特殊津贴，那么按照规定，可以全部免税：

```java
class StateCouncilSpecialAllowance extends Income {
    @Override
    public double getTax() {
        return 0;
    }
}
```

现在，我们要编写一个报税的财务软件，对于**一个人的所有收入**进行报税，可以这么写：

```java
public double totalTax(Income... incomes) {
    double total = 0;
    for (Income income: incomes) {
        total = total + income.getTax();
    }
    return total;
}
```

来试一下：

```java
// Polymorphic
public class Main {
    public static void main(String[] args) {
        // 给一个有普通收入、工资收入和享受国务院特殊津贴的小伙伴算税:
        Income[] incomes = new Income[] {
            new Income(3000),
            new Salary(7500),
            new StateCouncilSpecialAllowance(15000)
        };
        System.out.println(totalTax(incomes));
    }

    public static double totalTax(Income... incomes) {
        double total = 0;
        for (Income income: incomes) {
            total = total + income.getTax();
        }
        return total;
    }
}

class Income {
    protected double income;

    public Income(double income) {
        this.income = income;
    }

    public double getTax() {
        return income * 0.1; // 税率10%
    }
}

class Salary extends Income {
    public Salary(double income) {
        super(income);
    }

    @Override
    public double getTax() {
        if (income <= 5000) {
            return 0;
        }
        return (income - 5000) * 0.2;
    }
}

class StateCouncilSpecialAllowance extends Income {
    public StateCouncilSpecialAllowance(double income) {
        super(income);
    }

    @Override
    public double getTax() {
        return 0;
    }
}
```

观察`totalTax()`方法：利用多态，`totalTax()`方法**只需要和`Income`打交道**，它**完全不需要知道`Salary`和`StateCouncilSpecialAllowance`的存在**，就可以正确计算出总的税。如果我们要新增一种稿费收入，**只需要从`Income`派生，然后正确覆写`getTax()`方法就可以**。把新的类型传入`totalTax()`，**不需要修改任何代码**。

可见，多态具有一个非常强大的功能，就是**允许添加更多类型的子类实现功能扩展，却不需要修改基于父类的代码,简化编写**。覆写Object方法

因为所有的`class`最终都继承自`Object`，而`Object`定义了几个重要的方法：

- `toString()`：把**instance输出为`String`；**
- `equals()`：判断**两个instance是否逻辑相等；**
- `hashCode()`：计算**一个instance的哈希值。**

在必要的情况下，我们可以覆写`Object`的这几个方法。例如：

```java
class Person {
    ...
    // 显示更有意义的字符串:
    @Override
    public String toString() {
        return "Person:name=" + name;
    }

    // 比较是否相等:
    @Override
    public boolean equals(Object o) {
        // 当且仅当o为Person类型:
        if (o instanceof Person) {
            Person p = (Person) o;
            // 并且name字段相同时，返回true:
            return this.name.equals(p.name);
        }
        return false;
    }

    // 计算hash:
    @Override
    public int hashCode() {
        return this.name.hashCode();
    }
}
```

### 调用super

在子类的覆写方法中，如果**要调用父类的被覆写的方法，可以通过`super`来调用**。例如：

```java
class Person {
    protected String name;
    public String hello() {
        return "Hello, " + name;
    }
}

class Student extends Person {
    @Override
    public String hello() {
        // 调用父类的hello()方法:
        return super.hello() + "!";
    }
}
```

### final

继承可以允许子类覆写父类的方法。如果一个父类**不允许子类对它的某个方法进行覆写，可以把该方法标记为`final`。**用`final`修饰的方法不能被`Override`：

```java
class Person {
    protected String name;
    public final String hello() {
        return "Hello, " + name;
    }
}
class Student extends Person {
    // compile error: 不允许覆写
    @Override
    public String hello() {
    }
}
```

如果一个类不希望任何其他类继承自它，那么可以把这个类本身标记为`final`。用`final`修饰的类不能被继承：

```java
final class Person {
    protected String name;
}
// compile error: 不允许继承自Person
class Student extends Person {
}
```

对于一个类的实例字段，同样可以用`final`修饰。用`final`修饰的字段在初始化后**不能被修改。**例如：

```java
class Person {
    public final String name = "Unamed";
}
```

对`final`字段重新赋值会报错：

```java
Person p = new Person();
p.name = "New Name"; // compile error!
```

**可以在构造方法中初始化final字段**：

```java
class Person {
    public final String name;
    public Person(String name) {
        this.name = name;
    }
}
```

这种方法更为常用，因为可以保证实例一旦创建，其`final`字段就不可修改。

# 抽象类

由于多态的存在，每个子类都可以覆写父类的方法，例如：

```java
class Person {
    public void run() { … }
}

class Student extends Person {
    @Override
    public void run() { … }
}

class Teacher extends Person {
    @Override
    public void run() { … }
}
```

从`Person`类派生的`Student`和`Teacher`都可以覆写`run()`方法。

如果父类`Person`的`run()`方法没有实际意义，能否去掉方法的执行语句？

```java
class Person {
    public void run(); // Compile Error!
}
```

答案是不行，会导致编译错误，因为定义方法的时候，必须实现方法的语句。

能不能去掉父类的`run()`方法？

答案还是不行，因为去掉父类的`run()`方法，就失去了多态的特性。例如，`runTwice()`就无法编译：

```java
public void runTwice(Person p) {
    p.run(); // Person没有run()方法，会导致编译错误
    p.run();
}
```

如果父类的方法**本身不需要实现任何功能，仅仅是为了定义方法签名**，目的是让子类去覆写它，那么，可以把父类的方法**声明为抽象方法**：

```java
class Person {
    public abstract void run();
}
```

把一个方法声明为`abstract`，表示它是一个抽象方法，**本身没有实现任何方法语句。因为这个抽象方法本身是无法执行的**，所以，**`Person`类也无法被实例化**。编译器会告诉我们，无法编译`Person`类，因为它**包含抽象方法**。

**必须把`Person`类本身也声明为`abstract`，才能正确编译**它：

```java
abstract class Person {
    public abstract void run();
}
```

### 抽象类

如果一个`class`定义了方法，但没有具体执行代码，这个方法就是抽象方法，抽象方法用`abstract`修饰。

因为无法执行抽象方法，因此**这个类也必须申明为抽象类（abstract class）**。

使用`abstract`修饰的类就是抽象类。我们无法实例化一个抽象类：

```java
Person p = new Person(); // 编译错误
```

无法实例化的抽象类有什么用？

因为**抽象类本身被设计成只能用于被继承**，因此，**抽象类可以强迫子类实现其定义的抽象方法**，否则编译会报错。因此，抽象方法实际上**相当于定义了“规范”**。

例如，`Person`类定义了抽象方法`run()`，那么，在实现子类`Student`的时候，就**必须覆写`run()`方法**：

```java
// abstract class
public class Main {
    public static void main(String[] args) {
        Person p = new Student();
        p.run();
    }
}

abstract class Person {
    public abstract void run();
}

class Student extends Person {
    @Override
    public void run() {
        System.out.println("Student.run");
    }
}
```

### 面向抽象编程

当我们定义了抽象类`Person`，以及具体的`Student`、`Teacher`子类的时候，我们**可以通过抽象类`Person`类型去引用**具体的子类的实例：

```java
Person s = new Student();
Person t = new Teacher();
```

这种引用抽象类的好处在于，我们对其进行方法调用，并不关心`Person`类型变量的具体子类型：

```java
// 不关心Person变量的具体子类型:
s.run();
t.run();
```

同样的代码，如果引用的是一个新的子类，我们仍然**不关心具体类型**：

```java
// 同样不关心新的子类是如何实现run()方法的：
Person e = new Employee();
e.run();
```

这种**尽量引用高层类型，避免引用实际子类型的方式，称之为面向抽象编程**。

面向抽象编程的本质就是：

- **上层代码只定义规范**（例如：`abstract class Person`）；
- **不需要子类就可以实现业务逻辑**（正常编译）；
- **具体的业务逻辑由不同的子类实现，调用者并不关心**。

# 接口

在抽象类中，**抽象方法本质上是定义接口规范**：即**规定高层类的接口**，从而**保证所有子类都有相同的接口实现**，这样，多态就能发挥出威力。

如果一个抽象类没有字段，所有方法全部都是抽象方法：

```java
abstract class Person {
    public abstract void run();
    public abstract String getName();
}
```

就**可以把该抽象类改写为接口：`interface`。**

在Java中，**使用`interface`可以声明一个接口：**

```java
interface Person {
    void run();
    String getName();
}
```

所谓`interface`，就是比抽象类还要抽象的纯抽象接口，因为它**连字段都不能有**。因为接口定义的所有方法**默认都是`public abstract`的**，所以这两个修饰符不需要写出来（写不写效果都一样）。

当**一个具体的`class`去实现一个`interface`时，需要使用`implements`关键字**。举个例子：

```java
class Student implements Person {
    private String name;

    public Student(String name) {
        this.name = name;
    }

    @Override
    public void run() {
        System.out.println(this.name + " run");
    }

    @Override
    public String getName() {
        return this.name;
    }
}
```

我们知道，在Java中，一个类只能继承自另一个类，不能从多个类继承。但是，一个类可以**实现多个`interface`，**例如：

```java
class Student implements Person, Hello { // 实现了两个interface
    ...
}
```

### 术语

注意区分术语：

**Java的接口**特指`interface`的定义，表示**一个接口类型和一组方法签名**，而**编程接口泛指接口规范**，如**方法签名，数据格式，网络协议等**。

抽象类和接口的对比如下：

|            | abstract class         | interface                   |
| ---------- | :--------------------- | :-------------------------- |
| 继承       | 只能extends一个class   | 可以implements多个interface |
| **字段**   | 可以定义实例字段       | 不能定义实例字段            |
| 抽象方法   | 可以定义抽象方法       | 可以定义抽象方法            |
| 非抽象方法 | **可以定义非抽象方法** | 可以定义default方法         |

### 接口继承

一个`interface`可以继承自另一个`interface`。`interface`继承自`interface`使用`extends`，它相当于**扩展了接口的方法**。例如：

```java
interface Hello {
    void hello();
}

interface Person extends Hello {
    void run();
    String getName();
}
```

此时，`Person`接口继承自`Hello`接口，因此，`Person`接口现在实际上有3个抽象方法签名，其中一个来自继承的`Hello`接口。

### 继承关系

合理设计`interface`和`abstract class`的继承关系，可以**充分复用代码**。一般来说，**公共逻辑适合放在`abstract class`中，具体逻辑放到各个子类**，而**接口层次代表抽象程度**。可以参考Java的集合类定义的一组接口、抽象类以及具体子类的继承关系：

```
┌───────────────┐
│   Iterable    │
└───────────────┘
        ▲                ┌───────────────────┐
        │                │      Object       │
┌───────────────┐        └───────────────────┘
│  Collection   │                  ▲
└───────────────┘                  │
        ▲     ▲          ┌───────────────────┐
        │     └──────────│AbstractCollection │
┌───────────────┐        └───────────────────┘
│     List      │                  ▲
└───────────────┘                  │
              ▲          ┌───────────────────┐
              └──────────│   AbstractList    │
                         └───────────────────┘
                                ▲     ▲
                                │     │
                                │     │
                     ┌────────────┐ ┌────────────┐
                     │ ArrayList  │ │ LinkedList │
                     └────────────┘ └────────────┘
```

在使用的时候，**实例化的对象永远只能是某个具体的子类**，但**总是通过接口去引用它，因为接口比抽象类更抽象：**

```java
List list = new ArrayList(); // 用List接口引用具体子类的实例
Collection coll = list; // 向上转型为Collection接口
Iterable it = coll; // 向上转型为Iterable接口
```

### default方法

在接口中，可以定义`default`方法。例如，把`Person`接口的`run()`方法改为`default`方法：

```java
// interface
public class Main {
    public static void main(String[] args) {
        Person p = new Student("Xiao Ming");
        p.run();
    }
}

interface Person {
    String getName();
    default void run() {
        System.out.println(getName() + " run");
    }
}

class Student implements Person {
    private String name;

    public Student(String name) {
        this.name = name;
    }

    public String getName() {
        return this.name;
    }
}
```

实现类**可以不必覆写`default`方法**。`default`方法的目的是，当我们需要给接口新增一个方法时，会涉及到修改全部子类。**如果新增的是`default`方法，那么子类就不必全部修改**，只需要在需要覆写的地方去覆写新增方法。

`default`方法和抽象类的普通方法是有所不同的。因为`interface`没有字段**，`default`方法无法访问字段**，而抽象类的普通方法可以访问实例字段。

# 静态字段和静态方法

在一个`class`中定义的字段，我们称之为**实例字段**。实例字段的特点是，每个实例都有独立的字段，各个实例的同名字段互不影响。

还有一种字段，是**用`static`修饰的字段**，**称为静态字段**：`static field`。

实例字段在每个实例中都有自己的**一个独立“空间”**，但是静态字段**只有一个共享“空间”，所有实例都会共享该字段**。举个例子：

```java
class Person {
    public String name;
    public int age;
    // 定义静态字段number:
    public static int number;
}
```

我们来看看下面的代码：

```java
// static field
public class Main {
    public static void main(String[] args) {
        Person ming = new Person("Xiao Ming", 12);
        Person hong = new Person("Xiao Hong", 15);
        ming.number = 88;
        System.out.println(hong.number);
        hong.number = 99;
        System.out.println(ming.number);
    }
}

class Person {
    public String name;
    public int age;

    public static int number;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
}
```

对于静态字段，**无论修改哪个实例的静态字段，效果都是一样的**：**所有实例的静态字段都被修改了，原因是静态字段并不属于实例**：

```
        ┌──────────────────┐
ming ──▶│Person instance   │
        ├──────────────────┤
        │name = "Xiao Ming"│
        │age = 12          │
        │number ───────────┼──┐    ┌─────────────┐
        └──────────────────┘  │    │Person class │
                              │    ├─────────────┤
                              ├───▶│number = 99  │
        ┌──────────────────┐  │    └─────────────┘
hong ──▶│Person instance   │  │
        ├──────────────────┤  │
        │name = "Xiao Hong"│  │
        │age = 15          │  │
        │number ───────────┼──┘
        └──────────────────┘
```

虽然实例可以访问静态字段，但是它们指向的其实都是`Person class`的静态字段。所以，所有实例共享一个静态字段。

因此，不推荐用`实例变量.静态字段`去访问静态字段，因为在Java程序中，**实例对象并没有静态字段。**在代码中，实例对象能访问静态字段只是因为**编译器可以根据实例类型自动转换为`类名.静态字段`来访问静态对象**。

推荐**用类名来访问静态字段**。可以**把静态字段理解为描述`class`本身的字段**。对于上面的代码，更好的写法是：

```java
Person.number = 99;
System.out.println(Person.number);
```

### 静态方法

有静态字段，就有静态方法。用`static`修饰的方法称为静态方法。

调用实例方法必须通过一个实例变量，而**调用静态方法则不需要实例变量，通过类名就可以调用**。静态方法类似其它编程语言的函数。例如：

```java
// static method
public class Main {
    public static void main(String[] args) {
        Person.setNumber(99);
        System.out.println(Person.number);
    }
}

class Person {
    public static int number;

    public static void setNumber(int value) {
        number = value;
    }
}
```

因为静态方法**属于`class`而不属于实例**，因此，静态方法**内部，无法访问`this`变量，也无法访问实例字段，它只能访问静态字段**。

通过实例变量也可以调用静态方法，但这**只是编译器自动帮我们把实例改写成类名而已。**

通常情况下，通过实例变量访问静态字段和静态方法，会得到一个编译警告。

静态方法经常用于**工具类**。例如：

- Arrays.sort()
- Math.random()

静态方法也经常**用于辅助方法**。注意到Java程序的入口**`main()`也是静态方法**。

### 接口的静态字段

**因为`interface`是一个纯抽象类，所以它不能定义实例字段**。但是，`interface`是**可以有静态字段的**，并且静态字段**必须为`final`类型**：

```java
public interface Person {
    public static final int MALE = 1;
    public static final int FEMALE = 2;
}
```

实际上，**因为`interface`的字段只能是`public static final`类型，所以我们可以把这些修饰符都去掉**，上述代码可以简写为：

```java
public interface Person {
    // 编译器会自动加上public static final:
    int MALE = 1;
    int FEMALE = 2;
}
```

编译器会自动把该字段变为`public static final`类型。

# 包

在Java中，我们**使用`package`来解决名字冲突。**

Java定义了一种**名字空间，称之为包**：`package`。一个类总是属于某个包，类名（比如`Person`）只是一个简写，真正的**完整类名是`包名.类名`。**

**JDK的`Arrays`类存放在包`java.util`下面**，因此，完整类名是`java.util.Arrays`。

在定义`class`的时候，我们需要在第一行声明这个`class`属于哪个包。

小明的`Person.java`文件：

```java
package ming; // 申明包名ming

public class Person {
}
```

小军的`Arrays.java`文件：

```java
package mr.jun; // 申明包名mr.jun

public class Arrays {
}
```

在Java虚拟机执行的时候，JVM只看完整类名，因此，只要包名不同，类就不同。

**包可以是多层结构，用`.`隔开。**例如：`java.util`。00

包没有父子关系。java.util和java.util.zip是不同的包，两者没有任何继承关系。

没有定义包名的`class`，它使用的是**默认包**，非常容易引起名字冲突，因此，不推荐不写包名的做法。

我们还需要按照包结构把上面的Java文件组织起来。假设以`package_sample`作为根目录，`src`作为源码目录，那么所有文件结构就是：

```
package_sample
└─ src
    ├─ hong
    │  └─ Person.java
    │  ming
    │  └─ Person.java
    └─ mr
       └─ jun
          └─ Arrays.java
```

即所有Java文件对应的目录层次要和包的层次一致。

**编译后的`.class`文件也需要按照包结构存放**。如果使用IDE，把编译后的`.class`文件放到`bin`目录下，那么，编译的文件结构就是：

```
package_sample
└─ bin
   ├─ hong
   │  └─ Person.class
   │  ming
   │  └─ Person.class
   └─ mr
      └─ jun
         └─ Arrays.class
```

### 包作用域

**位于同一个包的类**，可以访问**包作用域的**字段和方法。**不用`public`、`protected`、`private`修饰的字段和方法就是包作用域**。例如，`Person`类定义在`hello`包下面：

```java
package hello;

public class Person {
    // 包作用域:
    void hello() {
        System.out.println("Hello!");
    }
}
```

`Main`类也定义在`hello`包下面：

```java
package hello;

public class Main {
    public static void main(String[] args) {
        Person p = new Person();
        p.hello(); // 可以调用，因为Main和Person在同一个包
    }
}
```

### import

在一个`class`中，我们总会引用其他的`class`。例如，小明的`ming.Person`类，如果要引用小军的`mr.jun.Arrays`类，他有三种写法：

第一种，直接写出完整类名，例如：

```java
// Person.java
package ming;

public class Person {
    public void run() {
        // 写完整类名: mr.jun.Arrays
        mr.jun.Arrays arrays = new mr.jun.Arrays();
    }
}
```

很显然，每次写完整类名比较痛苦。

因此，第二种写法是用`import`语句，导入小军的`Arrays`，然后**写简单类名**：

```java
// Person.java
package ming;

// 导入完整类名:
import mr.jun.Arrays;

public class Person {
    public void run() {
        // 写简单类名: Arrays
        Arrays arrays = new Arrays();
    }
}
```

在写`import`的时候，可以使用`*`，表示把这个包下面的所有`class`都导入进来（但**不包括子包的`class**`）一般不推荐这种写法，因为在导入了多个包后，很难看出`Arrays`类属于哪个包。

还有一种`import static`的语法，它可以导入一个类的静态字段和静态方法：

```java
package main;

// 导入System类的所有静态字段和静态方法:
import static java.lang.System.*;

public class Main {
    public static void main(String[] args) {
        // 相当于调用System.out.println(…)
        out.println("Hello, world!");
    }
}
```

`import static`很少使用。

Java编译器最终编译出的`.class`文件只使用*完整类名*，因此，在代码中，当编译器遇到一个`class`名称时：

- 如果是完整类名，就直接根据完整类名查找这个`class`；
- 如果是简单类名，按**下面的顺序依次查找：**
	- **查找当前`package`是否存在这个`class`；**
	- **查找`import`的包是否包含这个`class`；**
	- **查找`java.lang`包是否包含这个`class`。**

如果按照上面的规则还无法确定类名，则编译报错。

我们来看一个例子：

```java
// Main.java
package test;

import java.text.Format;

public class Main {
    public static void main(String[] args) {
        java.util.List list; // ok，使用完整类名 -> java.util.List
        Format format = null; // ok，使用import的类 -> java.text.Format
        String s = "hi"; // ok，使用java.lang包的String -> java.lang.String
        System.out.println(s); // ok，使用java.lang包的System -> java.lang.System
        MessageFormat mf = null; // 编译错误：无法找到MessageFormat: MessageFormat cannot be resolved to a type
    }
}
```

因此，编写class的时候，编译器会自动帮我们做两个import动作：

- 默认**自动`import`当前`package`的其他`class`；**
- 默认**自动`import java.lang.*`。**

自动导入的是java.lang包，但类似java.lang.reflect这些包仍需要手动导入。

如果有两个`class`名称相同，例如，`mr.jun.Arrays`和`java.util.Arrays`，那么只能`import`其中一个，另一个必须写完整类名。

### 编译和运行

假设我们创建了如下的目录结构：

```
work
├── bin
└── src
    └── com
        └── itranswarp
            ├── sample
            │   └── Main.java
            └── world
                └── Person.java
```

其中，`bin`目录用于存放**编译后**的`class`文件，`src`目录按包结构存放Java**源码**，我们怎么一次性编译这些Java源码呢？

首先，确保当前目录是`work`目录，即存放`src`和`bin`的父目录：

```plain
$ ls
bin src
```

然后，编译`src`目录下的所有Java文件：

```plain
$ javac -d ./bin src/**/*.java
```

命令行`-d`指定输出的`class`文件存放`bin`目录，后面的参数`src/**/*.java`表示`src`目录下的所有`.java`文件，包括任意深度的子目录。

注意：Windows不支持`**`这种搜索全部子目录的做法，所以在Windows下编译必须依次列出所有`.java`文件：

```plain
C:\work> javac -d bin src\com\itranswarp\sample\Main.java src\com\itranswarp\world\Persion.java
```

使用Windows的PowerShell可以利用`Get-ChildItem`来列出指定目录下的所有`.java`文件：

```plain
PS C:\work> (Get-ChildItem -Path .\src -Recurse -Filter *.java).FullName
C:\work\src\com\itranswarp\sample\Main.java
C:\work\src\com\itranswarp\world\Person.java
```

因此，编译命令可写为：

```plain
PS C:\work> javac -d .\bin (Get-ChildItem -Path .\src -Recurse -Filter *.java).FullName
```

如果编译无误，则`javac`命令没有任何输出。可以在`bin`目录下看到如下`class`文件：

```
bin
└── com
    └── itranswarp
        ├── sample
        │   └── Main.class
        └── world
            └── Person.class
```

现在，我们就可以直接运行`class`文件了。根据当前目录的位置确定classpath，例如，当前目录仍为`work`，则classpath为`bin`或者`./bin`：

```plain
$ java -cp bin com.itranswarp.sample.Main 
Hello, world!
```

### package

最后，包作用域是指一个类允许访问**同一个`package`的没有`public`、`private`修饰的`class`，以及没有`public`、`protected`、`private`修饰的字段和方法。**

```java
package abc;
// package权限的类:
class Hello {
    // package权限的方法:
    void hi() {
    }
}
```

只要在同一个包，就可以访问`package`权限的`class`、`field`和`method`：

```java
package abc;

class Main {
    void foo() {
        // 可以访问package权限的类:
        Hello h = new Hello();
        // 可以调用package权限的方法:
        h.hi();
    }
}
```

注意，包名必须完全一致，包没有父子关系，`com.apache`和`com.apache.abc`是不同的包。如果不确定是否需要`public`，就不声明为`public`，即尽可能少地暴露对外的字段和方法。

把方法定义**为`package`权限有助于测试**，因为测试类和被测试类只要**位于同一个`package`**，测试代码就可以**访问被测试类的`package`权限方法**。

**一个`.java`文件只能包含一个`public`类，但可以包含多个非`public`类**。如果有`public`类，文件名必须和`public`类的名字相同。

# 内部类

### Inner Class

如果一个类定义在另一个类的内部，这个类就是Inner Class：Inner Class的实例不能单独存在，必须依附于一个Outer Class的实例

要实例化一个`Inner`，我们必须首先创建一个`Outer`的实例，然后，调用`Outer`实例的`new`来创建`Inner`实例：

```java
Outer.Inner inner = outer.new Inner();
```

这是因为Inner Class除了有一个`this`指向它自己，还隐含地持有一个Outer Class实例，可以用`Outer.this`访问这个实例。所以，实例化一个Inner Class**不能脱离Outer实例**。

Inner Class和普通Class相比，除了能引用Outer实例外，还有一个额外的“特权”，就是可以修改Outer Class的`private`字段，因为**Inner Class的作用域在Outer Class内部，所以能访问Outer Class的`private`字段和方法。**

观察Java编译器编译后的`.class`文件可以发现，`Outer`类被编译为`Outer.class`，**而`Inner`类被编译为`Outer$Inner.class`。**

### Anonymous Class

还有一种定义Inner Class的方法，它不需要在Outer Class中明确地定义这个Class，而是在方法内部，通过匿名类（Anonymous Class）来定义。示例代码如下：

```java
// Anonymous Class
public class Main {
    public static void main(String[] args) {
        Outer outer = new Outer("Nested");
        outer.asyncHello();
    }
}

class Outer {
    private String name;

    Outer(String name) {
        this.name = name;
    }

    void asyncHello() {
        Runnable r = new Runnable() {
            @Override
            public void run() {
                System.out.println("Hello, " + Outer.this.name);
            }
        };
        new Thread(r).start();
    }
}
```

观察`asyncHello()`方法，我们在方法内部实例化了一个`Runnable`**。`Runnable`本身是接口，接口是不能实例化的**，所以这里实际上是定义了一个**实现了`Runnable`接口的匿名类，并且通过`new`实例化该匿名类，然后转型为`Runnable`。**在定义匿名类的时候就必须实例化它，定义匿名类的写法如下：

```java
Runnable r = new Runnable() {
    // 实现必要的抽象方法...
};
```

匿名类和Inner Class一样，可以访问Outer Class的`private`字段和方法。之所以我们要定义匿名类，是因为在这里我们**通常不关心类名**，比直接定义Inner Class可以少写很多代码。

观察Java编译器编译后的`.class`文件可以发现，`Outer`类被编译为`Outer.class`，而匿名类被编译为`Outer$1.class`。如果有多个匿名类，Java编译器会将每个匿名类**依次命名为`Outer$1`、`Outer$2`、`Outer$3`……**

除了接口外，匿名类**也完全可以继承自普通类**。观察以下代码：

```java
// Anonymous Class
import java.util.HashMap;

public class Main {
    public static void main(String[] args) {
        HashMap<String, String> map1 = new HashMap<>();
        HashMap<String, String> map2 = new HashMap<>() {}; // 匿名类!
        HashMap<String, String> map3 = new HashMap<>() {
            {
                put("A", "1");
                put("B", "2");
            }
        };
        System.out.println(map3.get("A"));
    }
}
```

`map1`是一个普通的`HashMap`实例，**但`map2`是一个匿名类实例，只是该匿名类继承自`HashMap`。**`map3`也是一个**继承自`HashMap`的匿名类实例，并且添加了`static`代码块来初始化数据。**观察编译输出可发现`Main$1.class`和`Main$2.class`两个匿名类文件。

### Static Nested Class

最后一种内部类**和Inner Class类似，但是使用`static`修饰**，称为**静态内部类（Static Nested Class）：**

```java
// Static Nested Class
public class Main {
    public static void main(String[] args) {
        Outer.StaticNested sn = new Outer.StaticNested();
        sn.hello();
    }
}

class Outer {
    private static String NAME = "OUTER";

    private String name;

    Outer(String name) {
        this.name = name;
    }

    static class StaticNested {
        void hello() {
            System.out.println("Hello, " + Outer.NAME);
        }
    }
}
```

用`static`修饰的内部类和Inner Class有很大的不同，它**不再依附于`Outer`的实例，而是一个完全独立的类**，因此无法引用`Outer.this`，但它**可以访问`Outer`的`private`静态字段和静态方法**。如果把**`StaticNested`移到`Outer`之外，就失去了访问`private`的权限。**

### jar包

JVM通过环境变量`classpath`决定**搜索`class`的路径和顺序**；

如果有很多`.class`文件，散落在各层目录中，肯定不便于管理。如果能把目录打一个包，变成一个文件，就方便多了。

jar包就是用来干这个事的，它可以**把`package`组织的目录层级，以及各个目录下的所有文件（包括`.class`文件和其他文件）都打成一个jar文件**，这样一来，无论是备份，还是发给客户，就简单多了。

jar包实际上就是一个**zip格式的压缩文件，而jar包相当于目录**。如果我们要**执行一个jar包的`class`，就可以把jar包放到`classpath`中：**

```plain
java -cp ./hello.jar abc.xyz.Hello
```

这样JVM会**自动在`hello.jar`文件里去搜索某个类。**

那么问题来了：如何创建jar包？

因为jar包就是zip包，所以，直接在资源管理器中，找到正确的目录，点击右键，在弹出的快捷菜单中选择“发送到”，“压缩(zipped)文件夹”，就制作了一个zip文件。然后，**把后缀从`.zip`改为`.jar`，一个jar包就创建成功。**

假设编译输出的目录结构是这样：

```
package_sample
└─ bin
   ├─ hong
   │  └─ Person.class
   │  ming
   │  └─ Person.class
   └─ mr
      └─ jun
         └─ Arrays.class
```

这里需要特别注意的是，jar包里的第一层目录，不能是`bin`，而应该是`hong`、`ming`、`mr`。如果在Windows的资源管理器中看，应该长这样：

![hello.zip.ok](./java%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0/good-jar.jpg)

如果长这样：

![hello.zip.invalid](./java%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0/bad-jar.jpg)

上面的`hello.zip`**包含有`bin`目录，说明打包打得有问题**，JVM仍然无法从jar包中查找正确的`class`，原因是**`hong.Person`必须按`hong/Person.class`存放，而不是`bin/hong/Person.class`。**

jar包还可以包含一个特殊的`/META-INF/MANIFEST.MF`文件，`MANIFEST.MF`是纯文本，可以指定`Main-Class`和其它信息。JVM会自动读取这个`MANIFEST.MF`文件，如果存在`Main-Class`，我们就不必在命令行指定启动的类名，而是用更方便的命令：

```plain
java -jar hello.jar
```

在大型项目中，不可能手动编写`MANIFEST.MF`文件，再手动创建jar包。Java社区提供了大量的开源构建工具，例如[Maven](https://liaoxuefeng.com/books/java/maven/index.html)，可以非常方便地创建jar包。

# 模块

`.class`文件是JVM看到的最小可执行文件，而一个大型程序需要编写很多Class，并**生成一堆`.class`文件**，很不便于管理，所以，`jar`文件**就是`class`文件的容器。**

如果是自己开发的程序，**除了一个自己的`app.jar`以外，还需要一堆第三方的jar包**，运行一个Java程序，一般来说，命令行写这样：

```plain
java -cp app.jar:a.jar:b.jar:c.jar com.liaoxuefeng.sample.Ma
```

**JVM自带的标准库rt.jar不要写到classpath中，写了反而会干扰JVM的正常运行。**

如果漏写了某个运行时需要用到的jar，那么在运行期极有可能抛出`ClassNotFoundException`。

所以，jar只是用于存放class的容器，它**并不关心class之间的依赖。**

从Java 9开始引入的模块，主要是为了解决“依赖”这个问题。如果`a.jar`必须依赖另一个`b.jar`才能运行，那我们应该**给`a.jar`加点说明啥的，让程序在编译和运行的时候能自动定位到`b.jar`**，这种**自带“依赖关系”的class容器就是模块。**

为了表明Java模块化的决心，从Java 9开始，原有的Java标准库已经由一个单一巨大的`rt.jar`分拆成了几十个模块，这些模块以`.jmod`扩展名标识，可以在`$JAVA_HOME/jmods`目录下找到它们：

- java.base.jmod
- java.compiler.jmod
- java.datatransfer.jmod
- java.desktop.jmod
- ...

这些`.jmod`文件每一个都是一个模块，**模块名就是文件名**。例如：模块`java.base`对应的文件就是`java.base.jmod`。模块之间的依赖关系已经被写入到模块内的`module-info.class`文件了。所有的模块都直接或间接地依赖`java.base`模块，**只有`java.base`模块不依赖任何模块，它可以被看作是“根模块”**，好比所有的类都是从`Object`直接或间接继承而来。

把**一堆class封装为jar仅仅是一个打包的过程，而把一堆class封装为模块则不但需要打包**，还**需要写入依赖关系，并且还可以包含二进制代码（通常是JNI扩展）**。此外，模块支持多版本，即在同一个模块中可以为不同的JVM提供不同的版本。

### 编写模块

那么，我们应该如何编写模块呢？还是以具体的例子来说。首先，创建模块和原有的创建Java项目是完全一样的，以`oop-module`工程为例，它的目录结构如下：

```
oop-module
├── bin
├── build.sh
└── src
    ├── com
    │   └── itranswarp
    │       └── sample
    │           ├── Greeting.java
    │           └── Main.java
    └── module-info.java
```

其中，**`bin`目录存放编译后的class文件**，`**src`目录存放源码**，按包名的目录结构存放，仅仅在`src`目录下多了一个`module-info.java`这个文件，这就是**模块的描述文件**。在这个模块中，它长这样：

```java
module hello.world {
	requires java.base; // 可不写，任何模块都会自动引入java.base
	requires java.xml;
}
```

其中，`module`是关键字，**后面的`hello.world`是模块的名称，它的命名规范与包一致**。花括号的`requires xxx;`表示**这个模块需要引用的其他模块名**。除了`java.base`可以被自动引入外，这里我们引入了一个`java.xml`的模块。

当我们**使用模块声明了依赖关系后，才能使用引入的模块**。例如，`Main.java`代码如下：

```java
package com.itranswarp.sample;

// 必须引入java.xml模块后才能使用其中的类:
import javax.xml.XMLConstants;

public class Main {
	public static void main(String[] args) {
		Greeting g = new Greeting();
		System.out.println(g.hello(XMLConstants.XML_NS_PREFIX));
	}
}
```

如果**把`requires java.xml;`从`module-info.java`中去掉，编译将报错。**可见，**模块的重要作用就是声明依赖关系。**

下面，我们用JDK提供的命令行工具来编译并创建模块。

首先，我们把工作目录切换到`oop-module`，在当前目录下编译所有的`.java`文件，并存放到`bin`目录下，命令如下：

```plain
$ javac -d bin src/module-info.java src/com/itranswarp/sample/*.java
```

如果编译成功，现在项目结构如下：

```
oop-module
├── bin
│   ├── com
│   │   └── itranswarp
│   │       └── sample
│   │           ├── Greeting.class
│   │           └── Main.class
│   └── module-info.class
└── src
    ├── com
    │   └── itranswarp
    │       └── sample
    │           ├── Greeting.java
    │           └── Main.java
    └── module-info.java
```

注意到`src`目录下的`module-info.java`被编译到`bin`目录下的`module-info.class`。

下一步，我们需要**把bin目录下的所有class文件先打包成jar，在打包的时候，注意传入`--main-class`参数，**让**这个jar包能自己定位`main`方法所在的类：**

```plain
$ jar --create --file hello.jar --main-class com.itranswarp.sample.Main -C bin .
```



现在我们就在当前目录下得到了`hello.jar`这个jar包，它和普通jar包并无区别，可以直接使用命令`java -jar hello.jar`来运行它。但是我们的目标是创建模块，所以，继续使用JDK自带的`jmod`命令把一个jar包转换成模块：

```plain
$ jmod create --class-path hello.jar hello.jmod
```



于是，在当前目录下我们又得到了`hello.jmod`这个模块文件，这就是最后打包出来的传说中的模块！

### 运行模块

要运行一个jar，我们使用`java -jar xxx.jar`命令。要运行一个模块，我们只需要指定模块名。试试：

```plain
$ java --module-path hello.jmod --module hello.world
```



结果是一个错误：

```plain
Error occurred during initialization of boot layer
java.lang.module.FindException: JMOD format not supported at execution time: hello.jmod
```



原因**是`.jmod`不能被放入`--module-path`中。换成`.jar`就没问题了：**

```plain
$ java --module-path hello.jar --module hello.world
Hello, xml!
```



那我们辛辛苦苦创建的`hello.jmod`有什么用？答案是我们可以用它来打包JRE。

### 打包JRE

前面讲了，为了支持模块化，Java 9首先带头把自己的一个巨大无比的`rt.jar`拆成了几十个`.jmod`模块，原因就是，运行Java程序的时候，**实际上我们用到的JDK模块，并没有那么多。不需要的模块，完全可以删除。**

过去发布一个Java应用程序，要运行它，必须下载一个完整的JRE，再运行jar包。而完整的JRE块头很大，有100多M。怎么给JRE瘦身呢？

现在，JRE自身的标准库已经分拆成了模块，只需要带上程序用到的模块，其他的模块就可以被裁剪掉。怎么裁剪JRE呢？并不是说把系统安装的JRE给删掉部分模块，而是**“复制”一份JRE，但只带上用到的模块**。为此，JDK提供了`jlink`命令来干这件事。命令如下：

```plain
$ jlink --module-path hello.jmod --add-modules java.base,java.xml,hello.world --output jre/
```

我们在`--module-path`参数指定了我们自己的模块`hello.jmod`，然后，在`--add-modules`参数中指定了我们用到的3个模块`java.base`、`java.xml`和`hello.world`，用`,`分隔。最后，在`--output`参数指定输出目录。

现在，在当前目录下，我们可以找到`jre`目录，这是**一个完整的并且带有我们自己`hello.jmod`模块的JRE。**试试直接运行这个JRE：

```plain
$ jre/bin/java --module hello.world
Hello, xml!
```

要分发我们自己的Java应用程序，**只需要把这个`jre`目录打个包给对方发过去，对方直接运行上述命令即可**，既**不用下载安装JDK，也不用知道如何配置我们自己的模块**，极大地**方便了分发和部署。**

### 访问权限

前面我们讲过，Java的class访问权限分为public、protected、private和默认的包访问权限。引入模块后，这些访问权限的规则就要稍微做些调整。

确切地说，class的这些访问权限只在一个模块内有效，模块和模块之间，例如，a模块要访问b模块的某个class，必要条件是b模块明确地导出了可以访问的包。

举个例子：我们编写的模块`hello.world`**用到了模块`java.xml`的一个类`javax.xml.XMLConstants`，**我们之所以能直接使用这个类，是因为模块`java.xml`的`module-info.java`中**声明了若干导出：**

```java
module java.xml {
    exports java.xml;
    exports javax.xml.catalog;
    exports javax.xml.datatype;
    ...
}
```

**只有它声明的导出的包，外部代码才被允许访问**。换句话说，如果外部代码想要访问我们的`hello.world`模块中的`com.itranswarp.sample.Greeting`类，我们**必须将其导出：**

```java
module hello.world {
    exports com.itranswarp.sample;

    requires java.base;
	requires java.xml;
}
```

因此，**模块进一步隔离了代码的访问权限。**

### String

在Java中，`String`是一个引用类型，它本身也是一个`class`。但是，Java编译器对`String`有特殊处理，即可以直接用`"..."`来表示一个字符串：

```java
String s1 = "Hello!";
```

实际上字符串**在`String`内部是通过一个`char[]`数组表示的**，因此，按下面的写法也是可以的：

```java
String s2 = new String(new char[] {'H', 'e', 'l', 'l', 'o', '!'});
```

因为`String`太常用了，所以Java提供了`"..."`这种字符串字面量表示方法。

Java字符串的一个重要特点就是**字符串*不可变*。**这种不可变性是通过内部的`private final char[]`字段，以及没有任何修改`char[]`的方法实现的。

我们来看一个例子：

```java
// String
public class Main {
    public static void main(String[] args) {
        String s = "Hello";
        System.out.println(s);
        s = s.toUpperCase();
        System.out.println(s);
    }
}
```

根据上面代码的输出，试解释字符串内容是否改变。

根据输出结果，虽然变量 `s` 的值从 `"Hello"` 变成了 `"HELLO"`，但是**字符串的内容本身并没有改变**。

### 关键概念：字符串的不可变性 (Immutability)

在 Java 中，`String` 对象是**不可变的 (immutable)**。这意味着一旦一个 `String` 对象被创建，它的值（它所包含的字符序列）就永远不能被修改。

### 逐步分析代码

1. **`String s = "Hello";`**
	- 在内存中创建了一个 `String` 对象，其内容是 `"Hello"`。
	- 变量 `s` 引用（指向）这个对象。
2. **`System.out.println(s);`**
	- 输出：`Hello`
3. **`s = s.toUpperCase();`**
	- `s.toUpperCase()` 方法**不会**修改原始的 `"Hello"` 对象。
	- 它会在内存中创建一个**全新的** `String` 对象，其内容是 `"HELLO"`。
	- 然后，将变量 `s` 的引用从原始的 `"Hello"` 对象**重新指向**这个新的 `"HELLO"` 对象。
4. **`System.out.println(s);`**
	- 输出：`HELLO`

**总结：**

变量 `s` 看起来是“改变”了，但实际上是它**引用了一个新的、内容不同的 `String` 对象**。原始的 `"Hello"` 字符串对象**仍然存在于内存中（直到被垃圾回收）**，并且它的内容**依旧是 `"Hello"`**，没有被修改成 `"HELLO"`。

### 字符串比较

当我们想要比较两个字符串是否相同时，要特别注意，我们实际上是想比较字符串的内容是否相同。必须使用`equals()`方法而不能用`==`。

我们看下面的例子：

```java
// String
public class Main {
    public static void main(String[] args) {
        String s1 = "hello";
        String s2 = "hello";
        System.out.println(s1 == s2);
        System.out.println(s1.equals(s2));
    }
}
```



从表面上看，两个字符串用`==`和`equals()`比较都为`true`，但实际上那只是Java编译器在编译期，会自动把所有**相同的字符串当作一个对象放入常量池**，**自然`s1`和`s2`的引用就是相同的**。

所以，这种`==`比较返回`true`纯属巧合。换一种写法，`==`比较就会失败：

```java
// String
public class Main {
    public static void main(String[] args) {
        String s1 = "hello";
        String s2 = "HELLO".toLowerCase();
        System.out.println(s1 == s2);
        System.out.println(s1.equals(s2));
    }
}
```



结论：两个字符串比较，**必须总是使用`equals()`方法。**

要忽略大小写比较，使用`equalsIgnoreCase()`方法。

`String`类还提供了多种方法来搜索子串、提取子串。常用的方法有：

```java
// 是否包含子串:
"Hello".contains("ll"); // true
```

注意到`contains()`方法的参数是`CharSequence`而不是`String`，因为`CharSequence`是`String`实现的一个接口。

搜索子串的更多的例子：

```java
"Hello".indexOf("l"); // 2
"Hello".lastIndexOf("l"); // 3
"Hello".startsWith("He"); // true
"Hello".endsWith("lo"); // true
```

提取子串的例子：

```java
"Hello".substring(2); // "llo"
"Hello".substring(2, 4); "ll"
```

注意索引号是从`0`开始的。

### 去除首尾空白字符

使用`trim()`方法可以**移除字符串首尾空白字符。空白字符包括空格，`\t`，`\r`(return to the line's head)，`\n`：**

```java
"  \tHello\r\n ".trim(); // "Hello"
```



注意：`trim()`**并没有改变字符串的内容，而是返回了一个新字符串。**

另一个`strip()`方法也可以移除字符串首尾空白字符。它和`trim()`不同的是，**类似中文的空格字符`\u3000`也会被移除：**

```java
"\u3000Hello\u3000".strip(); // "Hello"
" Hello ".stripLeading(); // "Hello "
" Hello ".stripTrailing(); // " Hello"
```



`String`还提供了`isEmpty()`和`isBlank()`来判断**字符串是否为空和空白字符串**：

```java
"".isEmpty(); // true，因为字符串长度为0
"  ".isEmpty(); // false，因为字符串长度不为0
"  \n".isBlank(); // true，因为只包含空白字符
" Hello ".isBlank(); // false，因为包含非空白字符
```



### 替换子串

要在字符串中替换子串，有两种方法。一种是根据字符或字符串替换：

```java
String s = "hello";
s.replace('l', 'w'); // "hewwo"，所有字符'l'被替换为'w'
s.replace("ll", "~~"); // "he~~o"，所有子串"ll"被替换为"~~"
```

另一种是**通过正则表达式替换：**

```java
String s = "A,,B;C ,D";
s.replaceAll("[\\,\\;\\s]+", ","); // "A,B,C,D"
```

上面的代码通过正则表达式，把**匹配的子串统一替换为`","`**。关于正则表达式的用法我们会在后面详细讲解。

### 分割字符串

要分割字符串，使用`split()`方法，并且**传入的也是正则表达式**：

```java
String s = "A,B,C,D";
String[] ss = s.split("\\,"); // {"A", "B", "C", "D"}
```



### 拼接字符串

**拼接字符串使用静态方法`join()`，**它用指定的字符串连接字符串数组：

```java
String[] arr = {"A", "B", "C"};
String s = String.join("***", arr); // "A***B***C"
```



### 格式化字符串

字符串提供了`formatted()`方法和`format()`静态方法，可以**传入其他参数，替换占位符，然后生成新的字符串：**

```java
// String
public class Main {
    public static void main(String[] args) {
        String s = "Hi %s, your score is %d!";
        System.out.println(s.formatted("Alice", 80));
        System.out.println(String.format("Hi %s, your score is %.2f!", "Bob", 59.5));
    }
}
```



**有几个占位符，后面就传入几个参数。参数类型要和占位符一致。**我们经常用这个方法来格式化信息。常用的占位符有：

- `%s`：显示字符串；
- `%d`：显示整数；
- `%x`：显示十六进制整数；
- `%f`：显示浮点数。

占位符还可以带格式，例如`%.2f`表示显示两位小数。如果你不确定用啥占位符，那就始终用`%s`，因为`%s`可以显示任何数据类型。要查看完整的格式化语法，请参考[JDK文档](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/Formatter.html#syntax)。

### 类型转换

要把任意基本类型或引用类型转换为字符串，可以**使用静态方法`valueOf()`。**这是一个重载方法，编译器会根据参数自动选择合适的方法：

```java
String.valueOf(123); // "123"
String.valueOf(45.67); // "45.67"
String.valueOf(true); // "true"
String.valueOf(new Object()); // 类似java.lang.Object@636be97c
```



要把字符串转换为其他类型，就需要根据情况。例如，把字符串转换为`int`类型：

```java
int n1 = Integer.parseInt("123"); // 123
int n2 = Integer.parseInt("ff", 16); // 按十六进制转换，255
```



把字符串转换为`boolean`类型：

```java
boolean b1 = Boolean.parseBoolean("true"); // true
boolean b2 = Boolean.parseBoolean("FALSE"); // false
```



要特别注意，`Integer`有个`getInteger(String)`方法，它不是将字符串转换为`int`，而是**把该字符串对应的系统变量转换为`Integer`：**

```java
Integer.getInteger("java.version"); // 版本号，11
```



### 转换为char[]

`String`和`char[]`类型可以互相转换，方法是：

```java
char[] cs = "Hello".toCharArray(); // String -> char[]
String s = new String(cs); // char[] -> String
```



如果修改了`char[]`数组，`String`并不会改变：

```java
// String <-> char[]
public class Main {
    public static void main(String[] args) {
        char[] cs = "Hello".toCharArray();
        String s = new String(cs);
        System.out.println(s);
        cs[0] = 'X';
        System.out.println(s);
    }
}
```



这是因为通过`new String(char[])`创建新的`String`实例时，它并不会直接引用传入的`char[]`数组，而是会复制一份，所以，修改外部的`char[]`数组不会影响`String`实例内部的`char[]`数组，因为这是两个不同的数组。

从`String`的不变性设计可以看出，如果**传入的对象有可能改变，我们需要复制而不是直接引用。**

例如，下面的代码设计了一个`Score`类保存一组学生的成绩：

```java
// int[]
import java.util.Arrays;

public class Main {
    public static void main(String[] args) {
        int[] scores = new int[] { 88, 77, 51, 66 };
        Score s = new Score(scores);
        s.printScores();
        scores[2] = 99;
        s.printScores();
    }
}

class Score {
    private int[] scores;
    public Score(int[] scores) {
        this.scores = scores;//调用.clone()方法
    }

    public void printScores() {
        System.out.println(Arrays.toString(scores));
    }
}
```



观察两次输出，**由于`Score`内部直接引用了外部传入的`int[]`数组，这会造成外部代码对`int[]`数组的修改，影响到`Score`类的字段。如果外部代码不可信，这就会造成安全隐患。**

请修复`Score`的构造方法，使得外部代码对数组的修改不影响`Score`实例的`int[]`字段。

在Java中，`char`类型实际上就是两个字节的`Unicode`编码。如果我们要手动把字符串转换成其他编码，可以这样做：

```java
byte[] b1 = "Hello".getBytes(); // 按系统默认编码转换，不推荐
byte[] b2 = "Hello".getBytes("UTF-8"); // 按UTF-8编码转换
byte[] b2 = "Hello".getBytes("GBK"); // 按GBK编码转换
byte[] b3 = "Hello".getBytes(StandardCharsets.UTF_8); // 按UTF-8编码转换
```



注意：转换编码后，就不再是`char`类型，而是`byte`类型表示的数组。

如果要把已知编码的`byte[]`转换为`String`，可以这样做：

```java
byte[] b = ...
String s1 = new String(b, "GBK"); // 按GBK转换
String s2 = new String(b, StandardCharsets.UTF_8); // 按UTF-8转换
```

始终牢记：Java的`String`和`char`在内存中总是**以Unicode编码表示**。

## StringBuilder

Java编译器对`String`做了特殊处理，使得我们可以直接用`+`拼接字符串。

考察下面的循环代码：

```java
String s = "";
for (int i = 0; i < 1000; i++) {
    s = s + "," + i;
}
```



虽然可以直接拼接字符串，但是，在循环中，**每次循环都会创建新的字符串对象，然后扔掉旧的字符串。**这样，绝大部分字符串都是临时对象，不但浪费内存，还会影响GC效率。

为了能高效拼接字符串，Java标准库提供了`StringBuilder`，它是一个**可变对象，可以预分配缓冲区**，这样，往`StringBuilder`中新增字符时，不会创建新的临时对象：

```java
StringBuilder sb = new StringBuilder(1024);
for (int i = 0; i < 1000; i++) {
    sb.append(',');
    sb.append(i);
}
String s = sb.toString();
```



`StringBuilder`还可以进行链式操作：

```java
// 链式操作
public class Main {
    public static void main(String[] args) {
        var sb = new StringBuilder(1024);
        sb.append("Mr ")
          .append("Bob")
          .append("!")
          .insert(0, "Hello, ");
        System.out.println(sb.toString());
    }
}
```



如果我们查看`StringBuilder`的源码，可以发现，进行链式操作的关键是，定义的`append()`方法会返回`this`，这样，就可以不断调用自身的其他方法。

仿照`StringBuilder`，我们也可以设计支持链式操作的类。例如，一个可以不断增加的计数器：

```java
// 链式操作
public class Main {
    public static void main(String[] args) {
        Adder adder = new Adder();
        adder.add(3)
             .add(5)
             .inc()
             .add(10);
        System.out.println(adder.value());
    }
}

class Adder {
    private int sum = 0;

    public Adder add(int n) {
        sum += n;
        return this;
    }

    public Adder inc() {
        sum ++;
        return this;
    }

    public int value() {
        return sum;
    }
}
```

注意：对于**普通的字符串`+`操作**，并不需要我们将其改写为`StringBuilder`，因为Java编译器在编译时就**自动把多个连续的`+`操作编码为`StringConcatFactory`的操作。**在运行期，`StringConcatFactory`会自动把字符串连接操作优化为数组复制或者`StringBuilder`操作。

你可能还听说过`StringBuffer`，这是Java早期的一个`StringBuilder`的线程安全版本，它通过同步来保证多个线程操作`StringBuffer`也是安全的，但是同步会带来执行速度的下降。

`StringBuilder`和`StringBuffer`接口完全相同，现在完全没有必要使用`StringBuffer`。

# StringJoiner

很多时候，我们拼接的字符串像这样：

```java
// 输出: Hello Bob, Alice, Grace!
public class Main {
    public static void main(String[] args) {
        String[] names = {"Bob", "Alice", "Grace"};
        var sb = new StringBuilder();
        sb.append("Hello ");
        for (String name : names) {
            sb.append(name).append(", ");
        }
        // 注意去掉最后的", ":
        sb.delete(sb.length() - 2, sb.length());
        sb.append("!");
        System.out.println(sb.toString());
    }
}
```



类似用分隔符拼接数组的需求很常见，所以Java标准库还提供了一个`StringJoiner`来干这个事：

```java
import java.util.StringJoiner;
public class Main {
    public static void main(String[] args) {
        String[] names = {"Bob", "Alice", "Grace"};
        var sj = new StringJoiner(", ");
        for (String name : names) {
            sj.add(name);
        }
        System.out.println(sj.toString());
    }
}
```



慢着！用`StringJoiner`的结果少了前面的`"Hello "`和结尾的`"!"`！遇到这种情况，需要给`StringJoiner`指定“开头”和“结尾”：

```java
import java.util.StringJoiner;
public class Main {
    public static void main(String[] args) {
        String[] names = {"Bob", "Alice", "Grace"};
        var sj = new StringJoiner(", ", "Hello ", "!");
        for (String name : names) {
            sj.add(name);
        }
        System.out.println(sj.toString());
    }
}
```



### String.join()

`String`还提供了一个静态方法`join()`，这个方法在内部使用了`StringJoiner`来拼接字符串，**在不需要指定“开头”和“结尾”的时候，用`String.join()`更方便：**

```java
String[] names = {"Bob", "Alice", "Grace"};
var s = String.join(", ", names);
```

# 包装类型

Java的数据类型分两种：

- 基本类型：`byte`，`short`，`int`，`long`，`boolean`，`float`，`double`，`char`；
- 引用类型：所有`class`和`interface`类型。

**引用类型可以赋值为`null`，表示空**，但基本类型不能赋值为`null`：

```java
String s = null;
int n = null; // compile error!
```

那么，**如何把一个基本类型视为对象（引用类型）**？

比如，想要把`int`基本类型变成一个引用类型，我们可以**定义一个`Integer`类，它只包含一个实例字段`int`，这样，`Integer`类就可以视为`int`的包装类（Wrapper Class）**：

```java
public class Integer {
    private int value;

    public Integer(int value) {
        this.value = value;
    }

    public int intValue() {
        return this.value;
    }
}
```



定义好了`Integer`类，我们就可以把`int`和`Integer`互相转换：

```java
Integer n = null;
Integer n2 = new Integer(99);
int n3 = n2.intValue();
```



实际上，因为**包装类型非常有用**，Java核心库为每种基本类型都提供了对应的包装类型：

| 基本类型 | 对应的引用类型          |
| :------- | :---------------------- |
| boolean  | java.lang.Boolean       |
| byte     | java.lang.Byte          |
| short    | java.lang.Short         |
| int      | java.lang.**Integer**   |
| long     | java.lang.Long          |
| float    | java.lang.Float         |
| double   | java.lang.Double        |
| char     | java.lang.**Character** |

我们可以直接使用，并不需要自己去定义：

```java
// Integer:
public class Main {
    public static void main(String[] args) {
        int i = 100;
        // 通过new操作符创建Integer实例(不推荐使用,会有编译警告):
        Integer n1 = new Integer(i);
        // 通过静态方法valueOf(int)创建Integer实例:
        Integer n2 = Integer.valueOf(i);
        // 通过静态方法valueOf(String)创建Integer实例:
        Integer n3 = Integer.valueOf("100");
        System.out.println(n3.intValue());
    }
}
```

### Auto Boxing

因为`int`和`Integer`可以互相转换：

```java
int i = 100;
Integer n = Integer.valueOf(i);
int x = n.intValue();
```

所以，Java编译器可以帮助我们自动在`int`和`Integer`之间转型：

```java
Integer n = 100; // 编译器自动使用Integer.valueOf(int)
int x = n; // 编译器自动使用Integer.intValue()
```

这种直接把`int`变为`Integer`的赋值写法，称为**自动装箱（Auto Boxing）**，反过来，把`Integer`变为`int`的赋值写法，称为**自动拆箱（Auto Unboxing）**。

装箱和拆箱会影响代码的执行效率，因为编译后的`class`代码是严格区分基本类型和引用类型的。并且，自动拆箱执行时可能会报`NullPointerException`：

```java
// NullPointerException
public class Main {
    public static void main(String[] args) {
        Integer n = null;
        int i = n;
    }
}
```

### 不变类

所有的包装类型都是不变类。我们查看`Integer`的源码可知，它的核心代码如下：

```java
public final class Integer {
    private final int value;
}
```

因此，一旦创建了`Integer`对象，该**对象就是不变的。**

对两个`Integer`实例进行比较要特别注意：**绝对不能用`==`比较，因为`Integer`是引用类型，必须使用`equals()`比较：**

```java
// == or equals?
public class Main {
    public static void main(String[] args) {
        Integer x = 127;
        Integer y = 127;
        Integer m = 99999;
        Integer n = 99999;
        System.out.println("x == y: " + (x==y)); // true
        System.out.println("m == n: " + (m==n)); // false
        System.out.println("x.equals(y): " + x.equals(y)); // true
        System.out.println("m.equals(n): " + m.equals(n)); // true
    }
}
```

仔细观察结果的童鞋可以发现，`==`比较，较小的两个相同的`Integer`返回`true`，较大的两个相同的`Integer`返回`false`，这是因为`Integer`是不变类，编译器把`Integer x = 127;`自动变为`Integer x = Integer.valueOf(127);`，为了节省内存，`Integer.valueOf()`对于较小的数，始终返回相同的实例，因此，`==`比较“恰好”为`true`，但我们*绝不能*因为Java标准库的`Integer`内部有**缓存优化**就用`==`比较，必须用`equals()`方法比较两个`Integer`。

因为`Integer.valueOf()`**可能始终返回同一个`Integer`实例**，因此，在我们自己创建`Integer`的时候，以下两种方法：

- 方法1：`Integer n = new Integer(100);`
- 方法2：`Integer n = Integer.valueOf(100);`

**方法2更好，因为方法1总是创建新的`Integer`实例，方法2把内部优化留给`Integer`的实现者去做**，即使在当前版本没有优化，也有可能在下一个版本进行优化。

我们把能创建“新”对象的静态方法称为静态工厂方法。**`Integer.valueOf()`就是静态工厂方法，它尽可能地返回缓存的实例以节省内存。**

### 进制转换

`Integer`类本身还提供了大量方法，例如，最常用的静态方法`parseInt()`可以把字符串解析成一个整数：

```java
int x1 = Integer.parseInt("100"); // 100
int x2 = Integer.parseInt("100", 16); // 256,因为按16进制解析
```



`Integer`还可以把整数格式化为指定进制的字符串：

```java
// Integer:
public class Main {
    public static void main(String[] args) {
        System.out.println(Integer.toString(100)); // "100",表示为10进制
        System.out.println(Integer.toString(100, 36)); // "2s",表示为36进制
        System.out.println(Integer.toHexString(100)); // "64",表示为16进制
        System.out.println(Integer.toOctalString(100)); // "144",表示为8进制
        System.out.println(Integer.toBinaryString(100)); // "1100100",表示为2进制
    }
}
```

注意：上述方法的**输出都是`String`，**在计算机内存中，只用二进制表示，不存在十进制或十六进制的表示方法。`int n = 100`在内存中总是以4字节的二进制表示：

```
┌────────┬────────┬────────┬────────┐
│00000000│00000000│00000000│01100100│
└────────┴────────┴────────┴────────┘
```

我们经常使用的`System.out.println(n);`是依靠核心库自动把整数格式化为10进制输出并显示在屏幕上，使用`Integer.toHexString(n)`则通过核心库自动把整数格式化为16进制。

这里我们注意到程序设计的一个重要原则：数据的**存储和显示要分离**。

Java的包装类型还定义了一些有用的静态变量

```java
// boolean只有两个值true/false，其包装类型只需要引用Boolean提供的静态字段:
Boolean t = Boolean.TRUE;
Boolean f = Boolean.FALSE;
// int可表示的最大/最小值:
int max = Integer.MAX_VALUE; // 2147483647
int min = Integer.MIN_VALUE; // -2147483648
// long类型占用的bit和byte数量:
int sizeOfLong = Long.SIZE; // 64 (bits)
int bytesOfLong = Long.BYTES; // 8 (bytes)
```

最后，所有的整数和浮点数的包装类型都继承自`Number`，因此，可以非常方便地直接通过包装类型获取各种基本类型：

```java
// 向上转型为Number:
Number num = new Integer(999);
// 获取byte, int, long, float, double:
byte b = num.byteValue();
int n = num.intValue();
long ln = num.longValue();
float f = num.floatValue();
double d = num.doubleValue();
```

### 处理无符号整型

在Java中，并没有无符号整型（Unsigned）的基本数据类型。`byte`、`short`、`int`和`long`都是带符号整型，最高位是符号位。而C语言则提供了CPU支持的全部数据类型，包括无符号整型。无符号整型和有符号整型的转换在Java中就需要借助包装类型的静态方法完成。

例如，byte是有符号整型，范围是`-128`~`+127`，但如果把`byte`看作无符号整型，它的范围就是`0`~`255`。我们把一个负的`byte`按无符号整型转换为`int`：

```java
// Byte
public class Main {
    public static void main(String[] args) {
        byte x = -1;
        byte y = 127;
        System.out.println(Byte.toUnsignedInt(x)); // 255
        System.out.println(Byte.toUnsignedInt(y)); // 127
    }
}
```

因为`byte`的`-1`的二进制表示是`11111111`，以无符号整型转换后的`int`就是`255`。

类似的，可以把一个`short`按unsigned转换为`int`，把一个`int`按unsigned转换为`long`。

## JavaBean

在Java中，有很多`class`的定义都符合这样的规范：

- 若干`private`实例字段；
- **通过`public`方法来读写实例字段。**

例如：

```java
public class Person {
    private String name;
    private int age;

    public String getName() { return this.name; }
    public void setName(String name) { this.name = name; }

    public int getAge() { return this.age; }
    public void setAge(int age) { this.age = age; }
}
```

如果读写方法符合以下这种命名规范：

```java
// 读方法:
public Type getXyz()
// 写方法:
public void setXyz(Type value)
```

那么这种`class`被称为`JavaBean`：

![java-bean](./java%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0/javabean.jpg)

**上面的字段是`xyz`，那么读写方法名分别以`get`和`set`开头，并且后接大写字母开头的字段名`Xyz`，因此两个读写方法名分别是`getXyz()`和`setXyz()`。**

`boolean`字段比较特殊，它的**读方法一般命名为`isXyz()`：**

```java
// 读方法:
public boolean isChild()
// 写方法:
public void setChild(boolean value)
```

我们通常把一组对应的**读方法（`getter`）和写方法（`setter`）称为属性（`property`）**。例如，`name`属性：

- 对应的读方法是`String getName()`
- 对应的写方法是`setName(String)`

**只有`getter`的属性称为只读属性（read-only）**，例如，定义一个age只读属性：

- 对应的读方法是`int getAge()`
- 无对应的写方法`setAge(int)`

类似的，只有`setter`的属性称为只写属性（write-only）。

很明显，只读属性很常见，只写属性不常见。

属性只需要定义`getter`和`setter`方法，不一定需要对应的字段。例如，`child`只读属性定义如下：

```java
public class Person {
    private String name;
    private int age;

    public String getName() { return this.name; }
    public void setName(String name) { this.name = name; }

    public int getAge() { return this.age; }
    public void setAge(int age) { this.age = age; }

    public boolean isChild() {
        return age <= 6;
    }
}
```

可以看出，`getter`和`setter`也是一种数据封装的方法。

### JavaBean的作用

JavaBean主要用来传递数据，即把一组数据组合成一个JavaBean便于传输。此外，JavaBean可以方便地被IDE工具分析，生成读写属性的代码，主要用在图形界面的可视化设计中。

通过IDE，可以快速生成`getter`和`setter`。例如，在Eclipse中，先输入以下代码：

```java
public class Person {
    private String name;
    private int age;
}
```



然后，点击右键，在弹出的菜单中选择“Source”，“Generate Getters and Setters”，在弹出的对话框中选中需要生成`getter`和`setter`方法的字段，点击确定即可由IDE自动完成所有方法代码。

### 枚举JavaBean属性

要枚举一个JavaBean的所有属性，可以**直接使用Java核心库提供的`Introspector`：**

```java
import java.beans.*;

public class Main {
    public static void main(String[] args) throws Exception {
        BeanInfo info = Introspector.getBeanInfo(Person.class);
        for (PropertyDescriptor pd : info.getPropertyDescriptors()) {
            System.out.println(pd.getName());
            System.out.println("  " + pd.getReadMethod());
            System.out.println("  " + pd.getWriteMethod());
        }
    }
}

class Person {
    private String name;
    private int age;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }
}
```

运行上述代码，可以列出所有的属性，以及对应的读写方法。注意`class`属性是从`Object`继承的`getClass()`方法带来的。

# BigInteger

`java.math.BigInteger`就是用来表示任意大小的整数。`BigInteger`内部用一个`int[]`数组来模拟一个非常大的整数：

对`BigInteger`做运算的时候，只能使用实例方法，例如，加法运算：

```java
BigInteger i1 = new BigInteger("1234567890");
BigInteger i2 = new BigInteger("12345678901234567890");
BigInteger sum = i1.add(i2); // 12345678902469135780
```



和`long`型整数运算比，`BigInteger`不会有范围限制，但缺点是速度比较慢。

也可以把`BigInteger`转换成`long`型：

```java
BigInteger i = new BigInteger("123456789000");
System.out.println(i.longValue()); // 123456789000
System.out.println(i.multiply(i).longValueExact()); // java.lang.ArithmeticException: BigInteger out of long range
```



使用`longValueExact()`方法时，如果超出了`long`型的范围，会抛出`ArithmeticException`。

`BigInteger`和`Integer`、`Long`一样，也是不可变类，并且也继承自`Number`类。因为`Number`定义了转换为基本类型的几个方法：

- 转换为`byte`：`byteValue()`
- 转换为`short`：`shortValue()`
- 转换为`int`：`intValue()`
- 转换为`long`：`longValue()`
- 转换为`float`：`floatValue()`
- 转换为`double`：`doubleValue()`

因此，通过上述方法，可以把`BigInteger`转换成基本类型。如果`BigInteger`表示的范围超过了基本类型的范围，转换时将丢失高位信息，即结果不一定是准确的。如果需要准确地转换成基本类型，可以使用`intValueExact()`、`longValueExact()`等方法，在转换时如果超出范围，将直接抛出`ArithmeticException`异常。

## BigDecimal

`BigDecimal`可以表示一个任意大小且精度完全准确的浮点数。

### 比较BigDecimal

在比较两个`BigDecimal`的值是否相等时，要特别注意，使用`equals()`方法不但要求两个`BigDecimal`的值相等，还要求它们的`scale()`相等：

## Java的异常

调用方如何获知调用失败的信息？有两种方法：

方法一：约定返回错误码。

例如，处理一个文件，如果返回`0`，表示成功，返回其他整数，表示约定的错误码：

```java
int code = processFile("C:\\test.txt");
if (code == 0) {
    // ok:
} else {
    // error:
    switch (code) {
    case 1:
        // file not found:
    case 2:
        // no read permission:
    default:
        // unknown error:
    }
}
```

因为**使用`int`类型的错误码，想要处理就非常麻烦**。这种方式常见于底层C函数。

方法二：在**语言层面上**提供一个异常处理机制。

Java内置了一套异常处理机制，总是使用**异常来表示错误**。

**异常是一种`class`，因此它本身带有类型信息。异常可以在任何地方抛出，但只需要在上层捕获，这样就和方法调用分离了：**

```java
try {
    String s = processFile(“C:\\test.txt”);
    // ok:
} catch (FileNotFoundException e) {
    // file not found:
} catch (SecurityException e) {
    // no read permission:
} catch (IOException e) {
    // io error:
} catch (Exception e) {
    // other error:
}
```

因为Java的异常是`class`，它的继承关系如下：

```
                     ┌───────────┐
                     │  Object   │
                     └───────────┘
                           ▲
                           │
                     ┌───────────┐
                     │ Throwable │
                     └───────────┘
                           ▲
                 ┌─────────┴─────────┐
                 │                   │
           ┌───────────┐       ┌───────────┐
           │   Error   │       │ Exception │
           └───────────┘       └───────────┘
                 ▲                   ▲
         ┌───────┘              ┌────┴──────────┐
         │                      │               │
┌─────────────────┐    ┌─────────────────┐┌───────────┐
│OutOfMemoryError │... │RuntimeException ││IOException│...
└─────────────────┘    └─────────────────┘└───────────┘
                                ▲
                    ┌───────────┴─────────────┐
                    │                         │
         ┌─────────────────────┐ ┌─────────────────────────┐
         │NullPointerException │ │IllegalArgumentException │...
         └─────────────────────┘ └─────────────────────────┘
```

从继承关系可知：`Throwable`是异常体系的根，它继承自`Object`。`Throwable`有两个体系：`Error`和`Exception`，`**Error`表示严重的错误，程序对此一般无能为力**，例如：

- `OutOfMemoryError`：内存耗尽
- `NoClassDefFoundError`：无法加载某个Class
- `StackOverflowError`：栈溢出

而`Exception`则是**运行时的错误，它可以被捕获并处理。**

某些异常是应用程序逻辑处理的一部分，应该捕获并处理。例如：

- `NumberFormatException`：数值类型的格式错误
- `FileNotFoundException`：未找到文件
- `SocketException`：读取网络失败

还有一些异常是**程序逻辑编写不对造成的，应该修复程序本身**。例如：

- `NullPointerException`：对某个`null`的对象调用方法或字段
- `IndexOutOfBoundsException`：数组索引越界

`Exception`又分为两大类：

1. `RuntimeException`以及它的子类；
2. 非`RuntimeException`（包括`IOException`、`ReflectiveOperationException`等等）

Java规定：

- **必须捕获的异常，包括`Exception`及其子类，但不包括`RuntimeException`及其子类**，这种类型的异常称为**Checked Exception。**
- **不需要捕获的异常，包括`Error`及其子类，`RuntimeException`及其子类。**

### 捕获异常

捕获异常使用`try...catch`语句，**把可能发生异常的代码放到`try {...}`中，然后使用`catch`捕获对应的`Exception`及其子类**：

