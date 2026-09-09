# TIPS:

- **RTFM** **read the fucking manual**
- **RTFSC** **read the fucking source code**

## 一种推荐的提问方式如下:

```md
我在xxx的时候遇到了xxx的错误. 这个错误可以通过以下步骤重现: (描述具体的现象)
1. 我的系统版本是xxx, 相关的工具版本是xxx
2. 我做了xxx (必要的时候贴个图)
3. 然后xxx (必要的时候贴个图)
...
为了排查这个错误, 我进行了以下尝试: (说明我很希望可以解决问题, 真的没办法才提问的)
1. 我做了xxx, 出现了xxx的结果 (必要的时候贴个图)
2. 我还做了xxx, 出现了xxx的结果 (必要的时候贴个图)
...
最后问题还没有解决, 请问我还需要做哪些事情?
```

### 坚持了好久还是搞不定, 我想放弃了

也许是你坚持的姿势不对, 来跟yzh或者老师助教聊聊吧.

### PA是不是组成原理/体系结构实验

都不是, PA不要求大家使用硬件描述语言进行编码, 而是**通过C语言开发一款模拟器以及上层的软件(裸机运行时环境, 简易操作系统等)**. 与侧重于硬件层次的传统组成原理/体系结构实验相比, PA更**注重计算机系统抽象层次的认识和理解**.

### PA是不是很难

PA是一个**超硬核的编程实验**, 其中的硬核体现在, 只有同时满足以下条件, 你才有可能比较顺利地完成PA:

- 端正心态, 学会STFW/RTFM独立解决问题
- 掌握工具, 知道**用什么样的工具快速解决什么样的问题**
- **了解细节, 明白每一个文件每一行代码的行为**
- 系统编程, 熟悉计算机系统中**每个模块之间的联系**

### 为什么要用这么变态的PA来折磨我

因为大家普遍都很菜, 按照目前的状态, 你毕业之后很可能无法在社会的毒打下存活. 以下例子展示了将来你可能会面对什么样的毒打:

```
你的导师/老板: 你来做一个xxx, 目标是达到yyy的效果.
```

讲义? 框架代码? 不存在的. 如果我们不提供讲义和框架代码, 让你写一个能跑仙剑的NEMU, 这就和你毕业后面临的任务很类似了.

这就是社会毒打你的第一招必杀技: **充满不确定性的未知问题**. 如果你现在连链表都调不好, 显然你几乎无法胜任这些不确定的任务. **作为算是扛得住社会毒打的人, 我们知道你缺什么, 知道应该给你挖什么样的坑, 设置什么样的技能训炼, 你就会得到什么程度的成长.**

所以我们设计PA来把现在的你毒打一顿, 让你受尽折磨, 吃尽苦头, 归根到底是为了让将来的你**在求职时候更具有竞争力, 经受得起社会的毒打.**

### 我将来不从事系统相关的工作, 就不会那么痛苦了

太天真了. 就算是应用程序, 几千个模块之间通过几万个API交互的场景都是家常便饭, 复杂性是社会毒打你的第二招必杀技.

###  现在是开源的时代, 开源在《十四五规划和2035年远景目标纲要》中首次被列入国家发展规划, 那我为什么不能参考别人的代码/心得/攻略来学习

你来上课来做实验, 就应该接受我们设置的训练, 而独立完成实验是训练最基本的要求之一, 参考别人代码就是违反[学术诚信](http://integrity.mit.edu/)的行为, 针对这一点你没有任何理由讨价还价.

就算退一万步来讲, 你说参考别人是为了学习, 那**你能写出比别人更好的代码作为学习的成果吗?** 你要是真有这样的学习能力, 相信你也能够在遵守[学术诚信](http://integrity.mit.edu/)的前提下自己独立完成实验.

同理, 阅读网上流传的心得和攻略, 也是属于违反[学术诚信](http://integrity.mit.edu/)的行为, 因为你**没有通过自己的努力和预期的训练来获得正确的答案.**

### 为什么不能把代码/心得/攻略上传到公开的地方让大家学习

因为这违反了[学术诚信](http://integrity.mit.edu/). PA和其它项目不一样, 它**本质上是一个课程作业**, 

### 能不能公布阶段性的参考答案, 这样即使bug调不出来, 也能往后做

不能. 把代码写对是对码农最基本的要求, **代码还没写对, 就不应该往后做**. 以后你进入社会, 如果**你交付的代码给客户造成经济损失, 是要赔钱的.** 所以要学会对自己写的代码负责.

### 我总觉得讲义写得不清楚

你毕业后进入公司/课题组, 不会再有讲义具体地告诉你应该做什么, **总有一天你需要在脱离讲义的情况下完成任务. 我们希望你现在就放弃"讲义和框架代码会把我应该做的一切细节清楚地告诉我"的幻想, 为自己的成长负起责任:**

- 不知道在说什么, 说明你对**知识点的理解还不够清楚, 这时候你应该去看书/看手册**
- 不知道要做什么/怎么做, 说明你的**系统观还是零碎的, 理解不了系统中各个模块之间的联系**, 这时候你应该RTFSC, 尽自己最大努力梳理并理解系统中的一切细节
- **bug调不出来, 说明你不清楚程序正确的预期行为,** 你需要RTFSC理解程序应该如何运行; 此外也说明你不重视工具和方法的使用, 你需要花时间去体验和总结它们

如果**你发现自己有以上情况, 你还是少抱怨, 多吃苦吧.**

老师助教不是你的保姆, 你需要先搞清楚哪些问题是你可以问的, 哪些问题是你应该自己解决的: 我们鼓励你提出关于方法层面的疑问以及对各种问题的探索性想法, 但不要直接让别人提供你想要的答案, 因为学会独立分析问题也是PA训练的一个重要环节.

一个我们不愿意回答的提问如下:

```
Q: 我的程序段错误了, 怎么办?
A: 机器永远是对的.
```

如果你确实需要一些调试相关的帮助, 那怎么办? 你**需要展示你为了解决问题而付出过的努力**, 当我们看到了你的努力, 也会认可你受到的训练. 例如:

```
Q: 我的程序在xxx的情况下段错误了, 我进行了以下尝试
* 首先我做了aaa, 现象是AAA, 我得到的结论是XXX
* 然后我做了bbb, 现象是BBB, 我得到的结论是YYY
* 我还做了ccc, 现象是CCC, 我得到的结论是ZZZ

综上, 我觉得问题可能出在yyy, 但我接下来没有思路了, 我的分析和理解是否正确? 或者我忽略了什么吗?
```

### 我觉得PA对像我一样的菜鸡很不友好

程序设计实验对菜鸡友好, 但**为什么你做完之后还是菜鸡? 做一个对菜鸡友好的实验, 能力并不会得到提升.**

如果你是菜鸡, 说明你**之前该吃苦的时候没吃够, 现在比别人多吃苦就是应该的.**

### 我感觉大部分同学都无法在一学期内做完PA的全部内容

很正常, **PA从一开始设计的时候就没有想过会让所有同学完成所有内容. PA的设计理念是把目标定得很高, 强迫大家往上跳**, 所以你会明显感觉到PA和你做过的其它实验都不太一样: 只要你愿意付出足够的努力, 即使你没有完成所有的内容, 你也会感到自己各个方面的技能都有了明显的提升.

每年确实会有同学直接躺倒放弃而挂科, 但也总有为数不少一批同学能坚持到最后, **锻炼出专业程序员该有的素质**. 大家的编程水平有高有低, 如果你自己都觉得编程能力不行, 那你就应该去把编程能力补上来, 在那之前就**不要抱有"完成PA所有内容"的幻**想了. 一个比较合适的目标是**"独立完成PA1和PA2"**, 最后你仍然有机会通过课程.

### 我觉得应该搜集大家踩过的坑, 这样后面做的同学就会顺利一些

这种想法是不对的, **掉坑里然后自己爬出来是训练重要的一环, 只有吃苦头才会让你成长**. 如果你**不想以后掉坑里, 正确的做法是现在花时间踩坑吃苦, 让自己变强大, 而不是通过投机取巧的方式绕过那些你本应该接受的训练.**

### PA和OSlab有什么区别和联系

1. PA独有的内容: **上至运行时库函数和真实应用程序(仙剑, ONScripter等)**, **下至ISA模拟器的实现(寄存器, 指令, 分页机制等)**
2. OSlab独有的内容: **并发(锁, 同步等), 持久化(可靠性, 崩溃一致性等)**
3. PA和OSlab少量重合的部分: **上下文切换, 系统调用, 文件系统, 内存管理等**. 但PA在涉及这些内容的时候是**以广度优先为目标**, 目的是通过设计一个最简单的模块, 把程序和计算机之前的关系串起来; 而OSlab会深入地展开这些内容, 比如**ext文件系统, mmap等.**

PA是OSlab的前导实验, 完成PA就会对操作系统有一个基本但不算深入的认识, 这时候再做OSlab, 上手就会更加顺利.

### 和大班的PA相比有什么区别, 哪个更难

大班PA是主线PA 2016版本的一个分支, 并且根据授课教师的想法进行调整. 随着主线PA的演进, 和大班PA的区别也逐渐增大. 鉴于"难"这个说法比较主观, 我们从另一个角度来展示两者的区别: 对于一个难点, **大班PA的设计理念是通过降低要求(提供提示甚至是代码)的方式来弱化它, 而主线PA的设计理念是呈现科学的方法来引导大家解决它.**

所以从某种程度上来说, 大班PA降低要求的做法会对菜鸡来说会更友好, 但相对地, 全体学生在大班PA中受到的训练都会少很多. "计算机系统综合实验"课程对大班学生的少量抽样可以证明上述结论. 我们认为不应该为了照顾部分菜鸡而牺牲所有学生成长(吃苦)的机会, 所以主线PA没有选择降低要求, 而是让大家根据自身情况设置一个合适目标去努力.

# PA0

the terminal is completely with **CLI (Command Line Interface)**

**PRACTICE IS VERY IMPORTANT. You can not learn anything by only reading the tutorials.**

##  Configuring vim

- **:**

	Enter VIM Command line mode. For a full command list, type ':help :' (without the quotes)

- **j**

	**Move cursor down**

- **k**

	**Move cursor up**

- :q

	Close file

- :q!

	Close file, **don't save changes**

- :w

	**Save changes to** file

- :wq or :x or ZZ

	**Save changes and close file**

- x

	**Delete character at cursor**

- i

	**Insert** at cursor

- I

	**Insert at beginning of line**

- **a**

	**Append at cursor**

- **A**

	Append **at end of line**

- **escape or ctrl+[**

	**Exit insert mode**

where `<ESC>` means the ESC key, and `<C-a>` means "Ctrl + a" here. You only press no more than 15 keys to generate this file. Is it amazing? What about a file with 1000 lines? What you do is just to press one more key:

```
i1<ESC>q1yyp<C-a>q998@1
```

The magic behind this example is recording and replaying. You **initial the file with the first line**. Then **record the generation of the second**. After that, you **replay the generation for 998 times to obtain the file.**

The second example is to modify a file. Suppose you have such a file:

The second example is to modify a file. Suppose you have such a file:

```
aaaaaaaaaaaaaaaaaaaaaaaaabbbbbbbbbbbbbbbbbbbbbbbbb
cccccccccccccccccccccccccddddddddddddddddddddddddd
eeeeeeeeeeeeeeeeeeeeeeeeefffffffffffffffffffffffff
ggggggggggggggggggggggggghhhhhhhhhhhhhhhhhhhhhhhhh
iiiiiiiiiiiiiiiiiiiiiiiiijjjjjjjjjjjjjjjjjjjjjjjjj
```

You want to modify it into:

```
bbbbbbbbbbbbbbbbbbbbbbbbbaaaaaaaaaaaaaaaaaaaaaaaaa
dddddddddddddddddddddddddccccccccccccccccccccccccc
fffffffffffffffffffffffffeeeeeeeeeeeeeeeeeeeeeeeee
hhhhhhhhhhhhhhhhhhhhhhhhhggggggggggggggggggggggggg
jjjjjjjjjjjjjjjjjjjjjjjjjiiiiiiiiiiiiiiiiiiiiiiiii
```

What will you do? In `vim`, this is a piece of cake, too. **First locate the cursor to first "a" in the first line**. And **change `vim` into normal state**, then press the following keys sequentially:

```
<C-v>24l4jd$p
```

where `<C-v>` means "Ctrl + v" here. What about a file with 100 such lines? What you do is just to press one more key:

```
<C-v>24l99jd$p
```

Although these two examples are artificial, they display the powerful functionality of `vim`, comparing with other editors you have used.

If you use `ls` to list files, you **will not see the `.vimrc` you just copied**. This is because a file whose name **starts with a `.` is a hidden file in GNU/Linux.** To show hidden files, **use `ls` with `-a` option:**

```
ls -a
```

After you learn some basic operations in `vim` (such as moving, inserting text, deleting text), you can try to modify the `.vimrc` file as following:

```diff
--- before modification
+++ after modification
@@ -17,3 +17,3 @@//标明了位置
 " Vim5 and later versions support syntax highlighting. Uncommenting the next
 " line enables syntax highlighting by default.
-"syntax on
+syntax on
```

We present the modification **with [GNU diff format](http://www.gnu.org/software/diffutils/manual/html_node/Unified-Format.html).** If you do not understand the diff format, please search the Internet for more information.

**The unified output format** is **a variation on the context format** that is **more compac**t because it **omits redundant context lines**. To select this output format, **use the --unified[=lines] (-U lines), or -u option**. The argument lines is **the number of lines of context to show.** When it is not given, it **defaults to three.**
统一输出格式是上下文格式的一种变体，它更紧凑，因为它省略了冗余的上下文行。要选择这种输出格式，请使用 --unified[=lines] （ -U lines ）或 -u 选项。参数 lines 是要显示的上下文行数。如果没有给出，默认为三行。

In the early 1990s, only GNU `diff` could produce this format and only GNU `patch` could automatically apply diffs in this format. **For proper operation, `patch` typically needs at least three lines of context.**
在 20 世纪 90 年代初，**只有 GNU `diff` 能够生成这种格式，只有 GNU `patch` 能够自动应用这种格式的差异**。为了正常运行， `patch` 通常至少需要三行上下文。

##### 为什么不要用百度?

相信大家都用过百度来搜索一些非技术问题, 而且一般很容易找到答案. 但随着问题技术含量的提高, 百度的搜索结果会变得越来越不靠谱. 坚持使用百度搜索技术问题, 你将很有可能会碰到以下情况之一:

- 搜不到相关结果, 你感到挫败
- **搜到看似相关的结果, 但无法解决问题, 你在感到挫败之余, 也发现自己浪费了不少时间**
- 你搜到了解决问题的方案, 但**没有发现原因分析, 结果你不知道这个问题背后的细节**

你可能会觉得"可以解决问题就行, 不需要了解问题背后的细节". 但对于一些问题(例如编程问题), 你了解这些细节就相当于学到了新的知识, 所以你应该去了解这些细节, 让自己懂得更多.

如果谷歌能以更高的概率提供可以解决问题的方案, 并且带有原因分析, 你应该没有理由使用百度来搜索技术问题. 如果你仍然坚持使用百度, 原因就只有一个: 你不想主动去成长.

You can append the following content at the end of the `.vimrc` file to enable more features. Note that contents after a double quotation mark `"` are comments, and you do not need to include them. Of course, you can inspect every features to determine to enable or not.

```
setlocal noswapfile " 不要生成swap文件
set bufhidden=hide " 当buffer被丢弃的时候隐藏它
colorscheme evening " 设定配色方案
set number " 显示行号
set cursorline " 突出显示当前行
set ruler " 打开状态栏标尺
set shiftwidth=2 " 设定 << 和 >> 命令移动时的宽度为 2
set softtabstop=2 " 使得按退格键时可以一次删掉 2 个空格
set tabstop=2 " 设定 tab 长度为 2
set nobackup " 覆盖文件时不备份
set autochdir " 自动切换当前目录为当前文件所在的目录
set backupcopy=yes " 设置备份时的行为为覆盖
set hlsearch " 搜索时高亮显示被找到的文本
set noerrorbells " 关闭错误信息响铃
set novisualbell " 关闭使用可视响铃代替呼叫
set t_vb= " 置空错误铃声的终端代码
set matchtime=2 " 短暂跳转到匹配括号的时间
set magic " 设置魔术
set smartindent " 开启新行时使用智能自动缩进
set backspace=indent,eol,start " 不设定在插入状态无法用退格键和 Delete 键删除回车符
set cmdheight=1 " 设定命令行的行数为 1
set laststatus=2 " 显示状态栏 (默认值为 1, 无法显示状态栏)
set statusline=\ %<%F[%1*%M%*%n%R%H]%=\ %y\ %0(%{&fileformat}\ %{&encoding}\ Ln\ %l,\ Col\ %c/%L%) " 设置在状态行显示的信息
set foldenable " 开始折叠
set foldmethod=syntax " 设置语法折叠
set foldcolumn=0 " 设置折叠区域的宽度
setlocal foldlevel=1 " 设置折叠层数为 1
nnoremap <space> @=((foldclosed(line('.')) < 0) ? 'zc' : 'zo')<CR> " 用空格键来开关折叠
```

#####  提高开发效率的编辑器

程序设计课上你学会了使用Visual Studio, 然后你可能会认为, 程序员就是这样写代码的了. 其实并不是, 程序员会**追求那些提高效率的方法**. 不是GUI不好, 而是你只是用记事本的操作方式来写代码. 所以你需要改变, 去尝试一些可以帮助你提高开发效率的工具.

**在GNU/Linux中,** 与记事本的操作方式相比, 学会`vim`的基本操作就已经可以大大提高开发效率. 还有各种插件来增强`vim`的功能, 比如可以在代码中变量跳转的`ctags`等等. 你可以花点时间去配置一下`vim`, 具体配置方式请STFW. 总之, "编辑器之神"可不是浪得虚名的.

## QA

## 什么是GNU

**GNU**（发音为 /gnuː/）是一个递归缩写，代表 **"GNU's Not Unix!"**。

精简来说，你可以从以下两个维度来理解它：

1. **理念与系统 (Philosophy & System)**：它始于 1983 年，是一个旨在构建完全由**自由软件 (Free Software)** 组成的操作系统项目。我们现在常用的 "Linux" 操作系统，更准确的叫法其实是 **GNU/Linux**——因为 Linux 只是核心的**内核 (Kernel)**，而**系统外围的大量基础命令和工具（比如 `ls`, `grep`）都来自 GNU 项目。**
2. **开发工具 (Development Tools)**：这也是在你的 PA 学习中最重要的部分。GNU 产出了强大的 **GNU 工具链 (GNU Toolchain)**，这是一套**用于编写、编译和调试代码的基础设施。**

在 PA 的底层实验环境中，你将极度依赖以下几个 GNU 核心工具：

- **GCC (GNU Compiler Collection)**：**编译器**。它负责将你写的 C 语言人类可读代码，翻译成计算机能理解的**机器码 (Machine Code)**。
- **GNU Make**：**构建自动化工具 (Build Automation Tool)**。PA 项目包含很多文件，**`make` 通过读取 `Makefile`，自动帮你执行繁琐的编译命令。**
- **GDB (GNU Debugger)**：**调试器**。当你的程序出现 Bug 时，它**允许你暂停程序、查看内存 (Memory) 和寄存器 (Registers)** 的状态。你在 PA1 中要实现的 `sdb` (Simple Debugger)，其实就是一个微缩版的 GDB。

------

> **[Q]** 假设你修改了 NEMU 项目中的一个 C 源代码文件，然后你在终端输入了 `make` 命令。在这个过程中，`make` 和 `gcc` 是如何协同工作的？
>
> **[A]** `make` 作为构建工具，**首先会读取项目中的 `Makefile` 文件。它会检查文件的修改时间，发现你刚刚修改了某个 C 源文件 (Source File)。**接着，`make` 会根据 `Makefile` 中**定义的规则，自动调用 `gcc` 这个编译器，仅仅将修改过的 C 文件重新编译成目标文件 (Object File)**，最后再**链接 (Link) 生成新的可执行程序 (Executable)**。这样避免了每次都全量编译，极大节省了时间。

##  More Exploration

### Learning to use basic tools

##### TFM

The most important command in GNU/Linux is **`man` - the on-line manual pager.** This is because **`man` can tell you how to use other commands**. [Here](https://nju-projectn.github.io/ics-pa-gitbook/ics2025/man.html) is a small tutorial for `man`. Remember, learn to use `man`, learn to use everything. Therefore, if you want to know something about GNU/Linux (such as shell commands, system calls, library functions, device files, configuration files...), [RTFM](http://en.wikipedia.org/wiki/RTFM).

##### 为什么要RTFM?

RTFM是STFW的长辈, 在互联网还不是很流行的年代, RTFM是解决问题的一种有效方法. 这是因为手册包含了查找对象的所有信息, 关于查找对象的一切问题都可以在手册中找到答案.

**你或许会觉得翻阅手册太麻烦了, 所以可能会在百度上随便搜一篇博客来尝试寻找解决方案.** 但是, 你需要明确以下几点:

- 你搜到的博客可能也是转载别人的, 有可能有坑
- 博主只是分享了他的经历, 有些说法也不一定准确
- 搜到了相关内容, 也不一定会有全面的描述

最重要的是, 当你**尝试了上述方法而又无法解决问题的时候, 你需要明确"我刚才只是在尝试走捷径, 看来我需要试试RTFM了".**

##### Write a "Hello World" program under GNU/Linux

Write a "Hello World" program, compile it, then run it under GNU/Linux. If you do not know what to do, refer to the GNU/Linux tutorial above.

```
vim hello.c
gcc hello.c -o hello
./hello
```

`-o hello` 是一个选项 (Option)，告诉 `gcc` 将生成的**可执行文件 (Executable File)** 命名为 `hello`。如果不加 `-o`，`gcc` 默认会生成**一个名为 `a.out` 的文件。**

**[Q]** 在第三步运行程序时，为什么我们必须要在文件名前面加上 `./`（即输入 `./hello`），而不是像使用 `ls` 或 `gcc` 那样直接输入 `hello` 呢？

**[A]** 这是由 Linux 的环境变量机制决定的。当你在终端输入一个命令时，系统**会在一个叫做 `PATH` 的环境变量 (Environment Variable) 所指定的几个系统目录中去寻找这个程序。** 为了安全起见，你所在的**当前目录 (Current Directory)**（在 Linux 中用 `.` 表示）**默认并不在 `PATH` 列表中**。因此，加上 `./` 是明确告诉操作系统 (Operating System)："请**不要去系统目录里找了**，直接执行**当前目录**下的这个 `hello` 文件。" 这能防止你意外运行与系统命令同名的恶意程序。

##### Write a Makefile to compile the "Hello World" program

Write a Makefile to compile the "Hello World" program above. If you do not know what to do, refer to the GNU/Linux tutorial above.

**自动化构建**是**大型 C 语言项目（如 NEMU）的核心**。为了编译之前的 "Hello World" 程序，我们需要编写一个 `Makefile`。

`make` 工具的工作原理是**读取项目中名为 `Makefile` 的文本文件，并根据里面定义的规则 (Rules) 来执行命令。**

### Makefile 的核心结构 📜

一条基本的 `Makefile` 规则由三个部分组成：

```makefile
target: dependencies
<Tab> command
```

 **目标 (Target)**：你**想要生成的文件名（例如可执行程序 `hello`）**，或者是你**想执行的一个动作名称**。

**📦 依赖 (Dependencies)**：**生成该目标所需要的文件（例如源代码 `hello.c`）**。

**⚙️ 命令 (Command)**：为了从依赖生成目标，**系统需要执行的具体终端命令**。

### 编写你的 Makefile

在包含 `hello.c` 的同一个目录下，创建一个名为 `Makefile` （注意**首字母大写，不要加任何后缀**）的文件，并将以下内容写入其中：

```
hello: hello.c
	gcc hello.c -o hello

clean:
	rm -f hello
```

**保存后，你只需要在终端中输入：**

```
make
```

系统就会**自动寻找 `Makefile` 中的第一个目标（也就是 `hello`），检查依赖文件 `hello.c`，然后执行下面那行 `gcc` 编译命令。**

------

> **[Q: 问题]** 在编写和使用上述的 Makefile 时，有两个非常关键的细节：
>
> 1. `gcc` 和 `rm` 这两行命令前面的空白部分，有什么极其严格的格式要求？
> 2. 我们在**文件底部写了一个 `clean` 目标，如果我们想在终端里执行它，应该输入什么命令？它在软件开发中有什么实际意义？**
>
> **[A: 答案]**
>
> 1. 命令前的缩进**必须使用 Tab 键 (Tabulator key)**，**绝对不能使用空格 (Spaces)。**如果你用了空格，`make` 工具会直接报错终止，通常**提示为 "missing separator"（缺失分隔符）**。这是 Makefile 最著名的语法陷阱。
> 2. 需要在终端输入 `make clean`。它的实际意义是**清理构建产物 (Clean up build artifacts)**。在反复修改代码和编译的过程中，或者在提交代码到 Git 仓库之前，我们需要**一个快捷的方式来删除所有由编译器生成的中间文件（比如 `.o` 文件）和最终的可执行文件**，让项目恢复到**最干净的源代码状态。**

##### GDB

GDB is the most common used debugger under GNU/Linux. If you have not used a debugger yet (even in Visual Studio), **blame the 程序设计基础 course first, then blame yourself, and finally, read the tutorial to learn to use GDB.**

## GDB debugging tutorial for beginners

GDB is a **long-standing and comprehensive** Linux debugging utility, which would take many years to learn if you wanted to know the tool well. However, even for beginners, the tool can be very powerful and useful when it comes to debugging C or C++.

you can use GDB to obtain a backtrace (a stack list of functions called – like a tree – which eventually led to the crash). Or, if you are a C or C++ developer and you just introduced a bug into your code, then you can use GDB to debug variables, code and more! Let’s dive in!
例如，如果你是一名质量保证工程师，想要调试你的团队正在处理的 C 程序和二进制文件，并且它崩溃了，你可以使用 GDB 来**获取一个回溯（一个函数调用列表，像树一样，最终导致了崩溃）**。或者，如果你是一名 C 或 C++开发者，并且刚刚在你的代码中引入了一个错误，那么你可以使用 GDB 来调试变量、代码等等！让我们开始吧！

**In this tutorial you will learn**:
在本教程中，你将学习：

- How to install and use the GDB utility from the command line in Bash
	如何从命令行在 Bash 中安装和使用 GDB 工具
- **How to do basic GDB debugging using the GDB console and prompt**
	**如何使用 GDB 控制台和提示符进行基本的 GDB 调试**
- **Learn more about the detailed output GDB produces**
	**了解 GDB 生成的详细输出**

The `build-essential` and `gcc` are going to help you compile the `test.c` C program on your system.
`build-essential` 和 `gcc` 将帮助你编译 `test.c` C 程序。

Next, let us define the `test.c` script as follows (you can copy and paste the following into your favorite editor and save the file as `test.c`):
接下来，让我们定义 `test.c` 脚本如下（你可以将以下内容复制并粘贴到最喜欢的编辑器中，并将文件保存为 `test.c` ）：

A few notes about this script: You can see that when the `main` function will be started (the `main` function is the always the main and first function called when you start the compiled binary, this is part of the C standard), it immediately calls the function `calc`, which in turn calls `atual_calc` after setting a few variables `a` and `b` to `13` and `0` respectively.

A few notes about this script: You can see that when the `main` function will be started (the `main` function is the always the main and first function called **when you start the compiled binary, this is part of the C standard**), it **immediately calls the function `calc`, which in turn calls `atual_calc` after setting a few variables `a` and `b` to `13` and `0` respectively.**
关于这个脚本有几点说明：你可以看到，当`主函数启动`时（` 主`函数总是主函数，也是你**开始编译后的二进制时调用的第一个函数**，这是 **C 标准的一部分**），它会立即调用函数 `calc，函数 calc` 在将几个变量 `a 和 ``b` 分别设为 `13` 和 `0` 后调用 `atual_calc`。

## Executing our script and configuring core dumps 

## 执行脚本并配置核心转储

```bash
$ gcc -ggdb test.c -o test.out
$ ./test.out
Floating point exception (core dumped)
```

The `-ggdb` option to `gcc` will **ensure that our debugging session using GDB will be a friendly one**; it **adds GDB specific debugging information to the `test.out` binary**. We **name this output binary file using the `-o` option to `gcc`**, and **as input we have our script `test.c`.**
gcc 的 `-ggdb` 选项可以``确保我们使用 GDB 的调试会话是友好的;它在 `test.out` 二进制文件中添加了 GDB 特定的调试信息。我们用 `-o` `选项来命名`这个输出二进制文件，作为输入是`脚本 test.c`。

当我们执行脚本时，会立即收到一条神秘的信息 `Floating point exception (core dumped)` 。我们目前感兴趣的是`核心倾倒`的信息。如果你没有看到这个消息（或者你看到了但找不到核心文件），你可以**按照以下方式设置更好的核心转储：**

```
if ! grep -qi 'kernel.core_pattern' /etc/sysctl.conf; then
  sudo sh -c 'echo "kernel.core_pattern=core.%p.%u.%s.%e.%t" >> /etc/sysctl.conf'
  sudo sysctl -p
fi
ulimit -c unlimited
```

Here we are first making sure **there is no Linux Kernel core pattern (`kernel.core_pattern`) setting made yet in `/etc/sysctl.conf`** (the configuration file for setting system variables on Ubuntu and other operating systems), and – **provided no existing core pattern was found** – **add a handy core file name pattern (`core.%p.%u.%s.%e.%t`) to the same file.**
这里我们首先要确保 `/etc/sysctl.conf`（Ubuntu 及其他操作系统用于设置系统变量的配置文件）中尚未创建 Linux 内核核心模式（`kernel.core_pattern`）设置，并且——如果找不到现有的核心模式——就在同一文件中添加一个方便的核心文件名模式（`core.%p.%u.%s.%e.%t`）。

The `sysctl -p` command (to be executed as root, hence the `sudo`) next **ensures the file is immediately reloaded without requiring a reboot**. For more information on the core pattern, you can see the **Naming of core dump files** section which **can be accessed by using the `man core` command.**
`sysctl -p` 命令（**作为 root 执行，因此使用 `sudo`**）接着**确保文件立即重新加载，无需重启**。想了解更多核心模式的信息，可以查看“ **核心转储文件命名** ”部分，可以通过 `man core` 命令访问。

Finally, the `ulimit -c unlimited` command **simply sets the core file size maximum to `unlimited` for this session.** This setting is *not* persistent across restarts. To make it permanent, you can do:
最后，`ulimit -c unlimited` 命令**只是将该会话的核心文件大小设置为`无限 `**。这个设置在**重启后*不会*持续**。要让它永久，你可以做：

```c
sudo bash -c "cat << EOF > /etc/security/limits.conf
* soft core unlimited
* hard core unlimited
EOF
```

Which will **add `* soft core unlimited` and `* hard core unlimited` to `/etc/security/limits.conf`**, ensuring there are **no limits for core dumps.**
这样就会在 `/etc/security/limits.conf` 中添加 `* soft core unlimited` 和 `* hard core unlimited` ，确保**核心转储没有限制。**

When you now re-execute the `test.out` file you should **see the `core dumped` message** and you s**hould be able to see a core file (with the specified core pattern), as follows:**
当你现在重新执行 `test.out` 文件时，应该会看到`核心转储`信息，并且你应该能看到**一个核心文件（带有指定的核心模式）**，具体如下：

```bash
$ ls
core.1341870.1000.8.test.out.1598867712  test.c  test.out
```

Let’s next examine the metadata of the core file:
接下来让我们看看核心文件的元数据：

```bash
$ file core.1341870.1000.8.test.out.1598867712
core.1341870.1000.8.test.out.1598867712: ELF 64-bit LSB core file, x86-64, version 1 (SYSV), SVR4-style, from './test.out', real uid: 1000, effective uid: 1000, real gid: 1000, effective gid: 1000, execfn: './test.out', platform: 'x86_64'
```

We can see that this is **a 64-Bit core file**, which **user ID was in use**, **what the platform was**, and finally **what executable was used**. We can also see from **the filename (`.8.`) that it was a signal 8 which terminated the program.** Signal 8 **is SIGFPE, a Floating point exception.** GDB will **later show us that this is a arithmetic exception.**
我们可以看到这是一个 64 位核心文件，包含使用的用户 ID、平台，以及使用的执行文件。从文件名（`.8.`）中我们也能看出，**终止程序的是一个信号 8。信号 8 是 SIGFPE，浮点例外。**GDB 后来会向我们证明这是一个算术例外。

## Using GDB to analyze the core dump 使用 GDB 分析核心转储

Let’s **open the core file with GDB** and assume for a second we do not know what happened (if you’re a seasoned developer, you may have already seen the actual bug in the source!):
让我们打开带有 GDB 的核心文件，假设我们不知道发生了什么（如果你是经验丰富的开发者，可能已经在源代码中见过真正的 bug！）：

```bash
root@LAPTOP-C38OJTN9:~/workspace# gdb ./test.out ./core.7538.0.8.test.out.1773886581
GNU gdb (Ubuntu 12.1-0ubuntu1~22.04.2) 12.1
Copyright (C) 2022 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.
Type "show copying" and "show warranty" for details.
This GDB was configured as "x86_64-linux-gnu".
Type "show configuration" for configuration details.
For bug reporting instructions, please see:
<https://www.gnu.org/software/gdb/bugs/>.
Find the GDB manual and other documentation resources online at:
--Type <RET> for more, q to quit, c to continue without paging--
    <http://www.gnu.org/software/gdb/documentation/>.

For help, type "help".
Type "apropos word" to search for commands related to "word"...
Reading symbols from ./test.out...
[New LWP 7538]

warning: Section `.reg-xstate/7538' in core file too small.
[Thread debugging using libthread_db enabled]
Using host libthread_db library "/lib/x86_64-linux-gnu/libthread_db.so.1".
Core was generated by `./test.out'.
Program terminated with signal SIGFPE, Arithmetic exception.

warning: Section `.reg-xstate/7538' in core file too small.
#0  0x000061a4a277213b in actual_calc (a=13, b=0) at test.c:3
--Type <RET> for more, q to quit, c to continue without paging--
```

As you can see, **on the first line we called `gdb`** with **as first option our binary and as second option the core file**. Simply remember **binary and core**. Next we see GDB initialize, and we are presented with some information. You can **use the [apropos](https://linuxconfig.org/apropos) command to search for related GDB commands** if you need help.
如你所见，第一行我们调用`了 gdb`，**第一选项是我们的二进制文件，第二选项是核心文件。**只要记住**二进制和核心** 。接着我们**看到 GDB 初始化，并呈现了一些信息**。如果需要帮助，可以用 [apropos](https://linuxconfig.org/apropos) 命令搜索相关的 GDB 命令。

If you see a `warning: Unexpected size of section`.reg-xstate/1341870′ in core file.` or similar message, you may ignore it for the time being.
如果你在核心文件中看到 `warning: Unexpected size of section` .reg-xstate/1341870'或类似的提示，**暂时可以忽略它。**

We see that **the core dump was generated by `test.out`** and were are told that the signal was an **SIGFPE**, arithmetic exception. Great; we already know something is amiss with our mathematics, and **perhaps not with our code!**
我们看到核心转储是由 `test.out` 生成的，并被告知该信号是 SIGFPE，算术异常。太好了;我们已经知道我们的数学存在问题，也许我们的代码还没有！

Next we see the frame (please think about a `frame` like a `procedure` in code for the time being) on which the program terminated: frame `#0`. GDB adds all sorts of handy information to this: the memory address, the procedure name `actual_calc`, what our variable values were, and even at one line (`3`) of which file (`test.c`) the issue happened.
接下来我们看到程序终止的帧（暂时将`帧`想象为代码中的`过程 `）：帧 `#0`。GDB 会为这些添加各种实用信息：内存地址、过程名 `actual_calc`、变量值，甚至在文件（`test.c`）的第 `3` 行出现问题。

Next we see the line of code (line `3`) again, this time with the actual code (`c=a/b;`) from that line included. Finally we are presented with a GDB prompt.
接下来我们再次看到代码行（` 第 3` 行），这次包含了该行的实际代码（`c=a/b;）。` 最后，我们看到一个 GDB 提示。

The issue is likely very clear by now; we did `c=a/b`, or with variables filled in `c=13/0`. But human cannot divide by zero, and a computer can’t therefore either. As no-one told a computer how to divide by zero, an exception occurred, an arithmetic exception, a floating point exception / error.
问题现在应该非常明确;我们做了 `c=a/b`，或者变量填充为 `c=13/0`。但人类无法除以零，计算机也无法。由于没有人告诉计算机如何除以零，因此出现了一个异常，算术异常，浮点异常/错误。

## Backtracing 回溯追踪

So let’s see what else we can discover about GDB. Let’s look at a few basic commands. The fist one is the one you are most likely to use **most often: `bt`:**
那么，让我们看看还能发现关于 GDB 的其他信息。让我们来看几个基本指令。第一个是你最常用的：`bt`：

```
(gdb) bt
#0  0x000061a4a277213b in actual_calc (a=13, 
    b=0) at test.c:3
#1  0x000061a4a2772171 in calc () at test.c:12
#2  0x000061a4a277218a in main () at test.c:17
(gdb) 
```

This command is a shorthand for `backtrace` and basically **gives us a trace of the current state** (**procedure after procedure called**) of the program. Think about it like a reverse order of things that happened; **frame `#0` (the first frame) is the last function** which was being **executed by the program when it crashed**, and **frame `#2` was the very first frame called when the program was started**.
这个命令是`回溯`的简写，基本上给我们一个**程序**调用的当前状态的痕迹。把**它想象成发生的事情的倒序**;**帧 `#0`（第一帧）是程序崩溃时执行的最后一个函数**，帧 `#2` 是**程序启动时调用的第一个帧。**

We can thus analyze what happened: the program started, and `main()` was automatically called. Next, `main()` called `calc()` (and we can confirm this in the source code above), and **finally `calc()` called `actual_calc` and there things went wrong.**
因此我们可以分析发生了什么：程序启动，`main（）` 自动被调用。接着，`main（）` 调用 `calc（）（` 我们可以在上面的源代码中确认这一点），最后是 `calc（）` 调用 `actual_calc`，但这里出现了问题。

Nicely, we can see each line at which something happened. For example, **the `actual_calc()` function was called from line 12 in `test.c`**. Note that it is **not `calc()` which was called from line 12 but rather `actual_calc()` which makes sense**; test.c ended up **executing to line 12 as far as the `calc()` function is concerned**, as this is where the `calc()` function called `actual_calc()`.
很方便地，我们可以看到每一条发生过某事的线条。例如，`actual_calc（）` 函数是从 `test.c` 的第 12 行调用的。注意，从第 12 行调用的不是 `calc（），` 而是从 `actual_calc（）` 调用，这才有意义;对于 `calc（）` 函数来说，test.c 最终执行到第 12 行，因为 `calc（）` 函数叫`做 actual_calc（）`）。

Power user tip: if you **use multiple threads**, you can **use the command `thread apply all bt` to obtain a backtrace for all threads** which were running as the program crashed!
高级用户提示：如果你使用多个线程，可以用命令“` 应用所有线程 `”来获取所有在程序崩溃时运行的线程的回溯！

## Frame inspection 车架检查

If we want, we can **inspect each frame**, **the matching source code (if it is available)**, and **each variable step by step:**
如果需要，我们可以逐步检查每个帧、匹配的源代码（如果有的话）和每个变量：

```bash
(gdb) f 2
#2  0x000055fa2323318a in main () at test.c:17
17    calc();
(gdb) list
12    actual_calc(a, b);
13    return 0;
14  }
15  
16  int main(){
17    calc();
18    return 0;
19  }
(gdb) p a
No symbol "a" in current context.
```

Here we **‘jump into’ frame 2** by **using the `f 2` command**. `f` is a **short hand for the `frame` command**. Next we **list the source code by using the `list` command**, and **finally try to print (using the `p` shorthand command)** the value of the `a` variable, which fails, as at **this point `a` was not defined yet at this point in the code**; note we are **working at line 17 in the function `main()`**, and **the actual context it existed in within the bounds of this function/frame.**
这里我们通过使用 `f 2` 命令“跳入”第 2 帧。`f` 是 `frame` 命令的简写。接下来我们用 `list` 命令列出源代码，最后尝试用 `p` 个简写命令打印 `a` 变量的值，但失败了，因为此时代码中尚未定义 `a`;注意，我们**正在操作函数 `main（）` 的第 17 行，以及它在该函数/帧范围内实际存在的上下文。**

Note that the source code display function, including some of the source code displayed in the previous outputs above, is only available if the actual source code is available.
请注意，源代码显示功能（包括上述输出中显示的一些源代码）只有在实际源代码可用时才可用。

Here we immediately also see a **gotcha**; if the source code is different then the code which the binary was compiled from, one can be easily misled; the output **may show non-applicable / changed source**. GDB **does *not* check if there is a source code revision match**! It is thus of **paramount importance that you use the exact same source code revision** as the one from which your binary was compiled.
这里我们立刻看到了一个陷阱;如果源代码与二进制文件编译的代码不同，很容易被误导;输出可能显示不适用/更改的来源。GDB *不会*检查是否有源代码版本匹配！因此，**使用与编译二进制文件完全相同的源代码版本**至关重要。

An alternative is to not use the source code at all, and simply debug a particular situation in a particular function, using a newer revision of the source code. This often happens for advanced developers and debuggers who likely do not need too many clues about where the issue may be in a given function and with provided variable values.
另一种选择是完全不使用源代码，**直接在特定函数中调试特定情境**，使用较新的源代码版本。这通常发生在高级开发者和调试员身上，他们通常不需要太多线索来判断函数和变量值中问题可能存在的位置。

Let’s next examine frame 1:
接下来我们来看第一帧：

```
(gdb) f 1
#1  0x000055fa23233171 in calc () at test.c:12
12    actual_calc(a, b);
(gdb) list
7   int calc(){
8     int a;
9     int b;
10    a=13;
11    b=0;
12    actual_calc(a, b);
13    return 0;
14  }
15  
16  int main(){
```

Here we can again see plenty of information being output by GDB which will **aid the developer in debugging the issue at hand**. Since we are now in `calc` (on line 12), and we **have already initialized and subsequently set the variables `a` and `b` to `13` and `0` respectively**, we can now print their values:
这里我们再次可以看到 GDB 输出大量信息，这将帮助开发者调试当前问题。由于我们现在处于`微积分`阶段（第 12 行），并且已经初始化并随后将变量 `a 和 ``b` 分别设置为 `13` 和 `0`，现在我们可以打印它们的值：

```
(gdb) p a
$1 = 13
(gdb) p b
$2 = 0
(gdb) p c
No symbol "c" in current context.
(gdb) p a/b
Division by zero
```

------

------

Note that when we try and **print the value of `c`, it still fails as again `c` is not defined up** to this point (developers may speak about ‘in this context’) yet.
注意当我们尝试打印 `c` 的值时，仍然失败，因为 `c` 在此之前尚未定义（开发者可能会说“在此上下文中”）。

Finally, we l**ook into frame `#0`, our crashing frame**:
最后，我们看看`帧#0`，我们的崩溃帧：

```
(gdb) f 0
#0  0x000055fa2323313b in actual_calc (a=13, b=0) at test.c:3
3     c=a/b;
(gdb) p a
$3 = 13
(gdb) p b
$4 = 0
(gdb) p c
$5 = 22010
```

All self evident, except for the value reported for `c`. Note that we **had defined the variable `c`, but had not given it an initial value yet**. As such `c` is **really undefined (and it was not filled by the equation `c=a/b` yet as that one failed)** and the resulting **value was likely read from some address space to which the variable `c` was assigned** (and that memory space **was not initialized**/cleared yet).
除了 c 的值外，其他都是显而易`见的。` 注意我们已经定义了变量 `c`，但还没有给出初始值。因此，`c` 实际上是未定义的（而且尚未被方程 `c=a/b` 填补，因为该方程失败了），所得值很可能是**从变量 `c` 分配到的某个地址空间读取的（而该内存空间尚未初始化或清除）**。

## Conclusion 结论

Great. We were able to debug a core dump for a C program, and we leaned the basics of GDB debugging in the meantime. If you are a QA engineer, or a junior developer, and you have understood and learned everything in this tutorial well, you are already quite a bit ahead of most QA engineers, and potentially other developers around you.
太好了。我们成功调试了一个 C 程序的核心转储，同时学习了 GDB 调试的基础知识。如果你是 QA 工程师或初级开发者，并且已经很好地理解并学到了这个教程里的所有内容，你已经比大多数 QA 工程师，甚至可能比你周围的其他开发者领先很多。

And the next time you watch Star Trek and Captain Janeway or Captain Picard want to ‘dump the core’, you’ll make a broader smile for sure. Enjoy debugging your next dumped core, and leave us a comment below with your debugging adventures.
下次你看《星际迷航》时，简威舰长或皮卡德舰长想“抛弃核心”时，你肯定会露出更灿烂的笑容。祝你调试下一个导出的核心，欢迎在下方留言分享你的调试体验。

# man快速入门

## 初识man

你是一只Linux菜鸟. 因为课程实验所迫, 你**不得不使用Linux, 不得不使用十分落后的命令行**. 实验内容大多数都要在命令行里进行, 面对着一大堆陌生的命令和参数, [这个链接](http://tieba.baidu.com/p/1410532522)中的饼图完美地表达了你的心情.

不行! 还是得认真做实验, 不然以后连码农都当不上了! 这样的想法鞭策着你, 因为你知道, **就算是码农, 也要有适应新环境和掌握新工具的能力.** "还是先去找man吧." 于是你在终端里输入`man`, 敲了回车. 只见屏幕上输出了一行信息:

> What manual page do you want?

噢, 原来**命令行也会说人话**! 你明白这句话的意思, `man`在询问你要查询什么内容. 你能查询什么内容呢? 既然`man`会说人话, 还是先多了解`man`吧. 为了告诉`man`你想更了解ta, 你输入

```
man man
```

敲了回车之后, `man`把你带到了一个全新的世界. 这时候, 你又看到了一句人话了, 那是`man`的独白, **ta告诉你, ta的真实身份其实是**

> an interface to the on-line reference manuals

接下来, ta忽然说了一大堆你听不懂的话, 似乎是想告诉你ta的使用方法. **可是你还没做好心理准备啊, 于是你无视了这些话.**

## 寻找帮助

很快, 你已经看到"最后一行"了. 难道man的世界就这么狭小? 你仔细一看, "最后一行"里面含有一些信息:

> Manual page man(1) line 1 (press h for help or q to quit)

原来可以通过**按`q`来离开这个世界啊,** 不过你现在并不想这么做, 因为你想多了解`man`, 以后可能会经常需要`man`的帮助. 为了更了解ta, 你按了`h`.

这时你又被带到了新的世界, 世界的起点是**"SUMMARY OF LESS COMMANDS",** 你马上知道, 这个世界要告诉你如何使用`man`, 你十分激动. 于是你往下看, 这句话说**"带有'\*'标记的命令可以在前面跟一个数, 这时命令的行为在括号里给出"**. 这是什么意思? 你没看懂, 还是找个带'*'的命令试试吧. 你继续往下看, 看到了两个功能和相应的命令:

- 第一个是展示帮助, 原来除了`h`之外, **`H`也可以看到帮助**, 而且这里把帮助的命令放在第一个, 也许`man`想暗示你, 找到帮助是十分重要的.
- 第二个命令是退出. "哈哈, 知道怎么退出之后, 就不用通过重启来退出一个命令行程序啦", 你心想. 但你现在还是不想退出, 还是再看看其它的吧.

继续往下看, 你看到了**用于移动的命令**. 果然, 你还是可以在这个世界里面移动的. **第一个用于移动的功能是往下移动一行, 你看到有5种方法可以实现:**

```
e  ^E  j  ^N  CR
```

`e`和`j`你看懂了, 就是按`e`或者`j`. 但`^E`是什么意思呢? 你尝试找到`^`的含义, 但是你没找到, 还是让我告诉你吧. **在上下文和按键有关的时候, `^`是Linux中的一个传统记号, 它表示`ctrl+`.** 还记得Windows下`ctrl+c`代表复制的例子吗? 这里的`^E`表示`ctrl+E`. **`CR`代表回车键**, 其实**`CR`是控制字符(ASCII码小于32的字符)**的一个, [这里](http://stackoverflow.com/questions/3091524/what-are-carriage-return-linefeed-and-form-feed)有一段关于控制字符的问答.

What is the meaning of the following control characters:

1. **Carriage return**
2. **Line feed**
3. **Form feed**

**Carriage return** means to **return to the beginning of the current line without advancing downward**. The name comes from a printer's carriage, as monitors were rare when the name was coined. This is **commonly escaped as "\r", abbreviated CR, and has ASCII value 13 or 0xD.**

**回车符**是指**返回当前行的开头，而不向下移动行数**。这个名称来源于打印机的托架，因为在它被创造出来的时候**显示器还很罕见。回车符通常用“\r”转义**，**缩写为 CR**，其 **ASCII 值为 13 或 0xD**。

**Linefeed** means to **advance downward to the next line**; however, it has been repurposed and renamed. Used as "newline", it *terminates* lines (commonly confused with *separating* lines). This is **commonly escaped as "\n", abbreviated LF or NL**, and has ASCII value 10 or 0xA. **CRLF (but not CRNL) is used for the pair "\r\n".**
**换行符（Linefeed）** **原本是指向下移动到下一行**；然而，它的功能已被重新定义并更名。**用作“换行符”时，它*用于结束*行（常与*分隔*行混淆）**。它通常用“\n”转义，**缩写为 LF 或 NL，ASCII 值为 10 或 0xA。**CRLF（而非 CRNL）用于表示“\r\n”这对换行符。

**Form feed** means **advance downward to the next "page"**. It was commonly used as page separators, but now is also used as section separators. **Text editors can use this character when you "insert a page break"**. This is **commonly escaped as "\f"**, **abbreviated FF, and has ASCII value 12 or 0xC.**
换页**符（Form Feed）** 表示向下移动到下一页。它过去常用作页面分隔符，但现在也**用作节分隔符**。文本编辑器在“插入分页符”时可以使用此字符。它通常用“\f”转义，缩写为 FF，ASCII 值为 12 或 0xC。

------

As control characters, they may be interpreted in various ways.
作为控制字符，它们可以有多种解释方式。

The most important interpretation is how these characters delimit lines. **Lines end with NL on Unix (including OS X)**, **CRLF on Windows**, and **CR on older Macs**. Note the **shift in meaning from LF to NL**, for **the exact same character, gives the differences between Windows and Unix**, which is also why many Windows programs **use CRLF to *separate* instead of *terminate* lines**. Many text editors can read files in any of these three formats and convert between them, but not all utilities can.
最重要的解释是**这些字符如何分隔行**。在 Unix 系统（包括 OS X）中，**行以 NL 结尾**；在 Windows 系统中，**行以 CRLF 结尾**；而在较旧的 Mac 系统中，**行以 CR 结尾**。请注意，**同一个字符在 Windows 和 Unix 系统中的含义差异**，从 LF 到 NL 正是 Windows 和 Unix 系统之间的区别所在。这也是为什么**许多 Windows 程序使用 CRLF 来*分隔行*而不是*终止*行的原因**。许多文本编辑器可以**读取这三种格式的文件并进行格式转换，但并非所有工具都支持此功能。**

**Form feed is much less commonly used**. **As page separator, it can only come between lines** or **at the start or end of the file.**
换行符的使用频率要低得多。作为**页面分隔符**，它**只能出现在行之间，或者文件的开头或结尾**。

你决定使用`j`, 因为它像一个向下的箭头, 而且它是右手食指所按下的键. 其实这点**和`vim`的使用是类似的**, 如果你不能理解为什么`vim`中使用`h`, `j`, `k`, `l`作为方向键, 这里有一个[初学者的提问](http://stackoverflow.com/questions/7665246/do-i-save-time-using-the-h-j-k-l-keys). 事实上, 这是**一种[touch typing](http://en.wikipedia.org/wiki/Touch_typing).**

你按下了`j`, 发现画面上的信息向下滚动了一行. 你看到了`*`, 想起了**`*`标记的命令可以在前面跟一个数**. 于是你试着输入`10j`, 发现画面向下滚动了10行, 你第一次感觉到**在这个"丑陋"的世界中也有比GUI方便的地方**. 你继续阅读帮助, 并且尝试每一个命令. 于是你掌握了如何**通过移动来探索**`man`所在的世界.

继续往下翻, 你看到了用于搜索的命令. 你十分感动, 因为**使用关键字可以快速定位到你关心的内容**. 帮助的内容告诉你, 通过**按`/`激活前向搜索模式, 然后输入关键字(可以使用正则表达式), 按下回车**就可以看到匹配的内容了. 帮助中还**列出了后向搜索, 跳到下一匹配处等功能**. 于是你掌握了如何使用搜索.

## 探索man

你一边阅读帮助, 一边尝试新的命令, 就这样探索着这个陌生的世界. 你虽然记不住这么多命令, 但你知道你可以随时来查看帮助. 掌握了一些基本的命令之后, 你按`q`离开了帮助, 回到了`man`的世界. 现在你可以自由探索`man`的世界了. 你向下翻, 跳过了看不懂的`SYNOPSIS`小节, **在`DESCRIPTION`小节看到了人话**, 于是你阅读这些人话. 在这里, 你看到整个manual分成9大类, 每个manual page都属于其中的某一类; 你看到了一个manual page主要包含以下的小节:

- NAME - 命令名
- SYNOPSIS - 使用方法大纲
- CONFIGURATION - 配置
- DESCRIPTION - 功能说明
- OPTIONS - 可选参数说明
- EXIT STATUS - 退出状态, 这是一个返回给父进程的值
- RETURN VALUE - 返回值
- ERRORS - 可能出现的错误类型
- ENVIRONMENT - 环境变量
- FILES - 相关配置文件
- VERSIONS - 版本
- CONFORMING TO - 符合的规范
- NOTES - 使用注意事项
- BUGS - 已经发现的bug
- EXAMPLE - 一些例子
- AUTHORS - 作者
- SEE ALSO - 功能或操作对象相近的其它命令

你还看到了对`SYNOPSIS`小节中记号的解释, 现在你可以回过头来看`SYNOPSIS`的内容了. 但为了弄明白每个参数的含义, 你需要查看`OPTIONS`小节中的内容.

你想起了搜索的功能, 为了弄清楚参数`-k`的含义, 你输入`/-k`, 按下回车, 并通过`n`跳过了那些`OPTIONS`小节之外的`-k`, 最后大约在第254行找到了`-k`的解释: 通过关键字来搜索相关功能的manual page. 在`EXAMPLES`小节中有一个使用`-k`的例子:

```
man -k printf
```

你阅读这个例子的解释: 搜索和`printf`相关的manual page. 你还是不太明白这是什么意思, 于是你退出`man`, 在命令行中输入

```
man -k printf
```

并运行, 发现输出了很多和`printf`相关的命令或库函数, 括号里面的数字代表相应的条目属于manual的哪一个大类. **例如`printf (1)`是一个shell命令, 而`printf (3)`是一个库函数**. 要访问库函数`printf`的manual page, 你需要在命令行中输入

```
man 3 printf
```

当你想做一件事的而不知道用什么命令的时候, `man`的`-k`参数可以用来列出候选的命令, 然后再通过查看这些命令的manual page来学习怎么使用它们.

接下来, 你又开始学习`man`的其它功能...

# Linux入门教程

## 探索命令行

Linux命令行中的命令使用格式都是相同的:

```bash
命令名称 参数1 参数2 参数3 ...
```

参数之间用**任意数量的空白字符分开**. 关于命令行, 可以先阅读[一些基本常识](https://linux.cn/article-6160-1.html). 然后我们介绍最常用的一些命令:

- `ls`用于列出当前目录(即"文件夹")下的所有文件(或目录). 目录会用蓝色显示. `ls -l`可以显示详细信息.
- `pwd`能够列出**当前所在的目录.**
- `cd DIR`可以切换到`DIR`目录. 在Linux中, 每个目录中都至少包含两个目录: `.`指向该目录自身, `..`指向它的上级目录. 文件系统的根是`/`.
- `touch NEWFILE`可以**创建一个内容为空的新文件`NEWFILE`, 若`NEWFILE`已存在, 其内容不会丢失.**
- `cp SOURCE DEST`可以**将`SOURCE`文件复制为`DEST`文件;** 如果**`DEST`是一个目录, 则将`SOURCE`文件复制到该目录下.**
- `mv SOURCE DEST`可以**将`SOURCE`文件重命名为`DEST`文件; 如果`DEST`是一个目录, 则将`SOURCE`文件移动到该目录下.**
- `mkdir DIR`能够**创建一个`DIR`目录.**
- `rm FILE`能够删除`FILE`文件; 如果**使用`-r`选项则可以递归删除一个目录**. 删除后的文件无法恢复, 使用时请谨慎!
- `man`可以**查看命令的帮助. 例如`man ls`可以查看`ls`命令的使用方法.** 灵活应用`man`和互联网搜索, 可以快速学习新的命令.

`man`的功能不仅限于此. `man`后**可以跟两个参数, 可以查看不同类型的帮助**(请在互联网上搜索). 例如当你不知道C标准库函数`freopen`如何使用时, 可以键入命令

```
man 3 freopen
```

下面给出一些常用命令使用的例子, 你可以键入每条命令之后使用`ls`查看命令执行的结果:

```bash
$ mkdir temp      # 创建一个目录temp
$ cd temp         # 切换到目录temp
$ touch newfile   # 创建一个空文件newfile
$ mkdir newdir    # 创建一个目录newdir
$ cd newdir       # 切换到目录newdir
$ cp ../newfile . # 将上级目录中的文件newfile复制到当前目录下
$ cp newfile aaa  # 将文件newfile复制为新文件aaa
$ mv aaa bbb      # 将文件aaa重命名为bbb
$ mv bbb ..       # 将文件bbb移动到上级目录
$ cd ..           # 切换到上级目录
$ rm bbb          # 删除文件bbb
$ cd ..           # 切换到上级目录
$ rm -r temp      # 递归删除目录temp
```

## 统计代码行数

第一个例子是统计一个目录中(包含子目录)中的代码行数. 如果想知道当前目录下究竟有多少行的代码, 就可以在命令行中键入如下命令:

```
find . | grep '\.c$\|\.h$' | xargs wc -l
```

如果**用`man find`查看`find`操作的功能, 可以看到`find`是搜索目录中的文件. Linux中一个点**`.`始终**表示Shell当前所在的目录**, 因此`find .`实际能够**列出当前目录下的所有文件**. 如果在文件很多的地方键**入`find .`, 将会看到过多的文件, 此时可以按`CTRL + c`退出.**

同样, 用`man`查看`grep`的功能——"**print lines matching a pattern**". `grep`**实现了输入的过滤**, 我们**的`grep`有一个参数, 它能够匹配以`.c`或`.h`结束的文件**. 正则表达式是处理字符串非常强大的工具之一, 每一个程序员都应该掌握其相关的知识. 有兴趣的同学可以首先阅读一个[基础的教程](https://linux.vbird.org/linux_basic/centos7/0330regularex.php), 然后看一个有趣的小例子: [如何用正则表达式判定素数](http://coolshell.cn/articles/2704.html). 正则表达式还可以用来编写一个30行的java表达式求值程序(传统方法几乎不可能), 聪明的你能想到是怎么完成的吗? **上述的`grep`命令能够提取所有`.c`和`.h`结尾的文件.**

刚才的`find`和`grep`命令, 都**从标准输入中读取数据, 并输出到标准输出.** 关于什么是标准输入输出, 请参考[这里](http://en.wikipedia.org/wiki/Standard_streams). **连接起这两个命令的关键就是管道符号`|`.** 这一符号的**左右都是Shell命令, `A | B`的含义是创建两个进程`A`和`B`,** 并**将`A`进程的标准输出连接到`B`进程的标准输入.** 这样, 将`find`和`grep`连接起来就能够筛选出当前目录(`.`)下**所有以`.c`或`.h`结尾的文件.**

我们最后的任务是**统计这些文件所占用的总行数**, 此时**可以用`man`查看`wc`命令**. **`wc`命令的`-l`选项能够计算代码的行数.** **`xargs`命令十分特殊, 它能够将标准输入转换为参数, 传送给第一个参数所指定的程序**. 所以, 代码中**的`xargs wc -l`就等价于执行`wc -l aaa.c bbb.c include/ccc.h ...`, 最终完成代码行数统计.**

## 统计磁盘使用情况

以下命令**统计`/usr/share`目录下各个目录所占用的磁盘空间**:

```
du -sc /usr/share/* | sort -nr
```

**`du`是磁盘空间分析工具,** `du -sc`将**目录的大小顺次输出到标准输出**, 继而**通过管道传送给`sort`.** `sort`是数据排序工具, 其中的**选项`-n`表示按照数值进行排序, 而`-r`则表示从大到小输出.** `sort`可以将这些参数连写在一起.

然而我们发现, `/usr/share`中的目录过多, **无法在一个屏幕内显示. 此时, 我们可以再使用一个命令: `more`或`less`.**

```
du -sc /usr/share/* | sort -nr | more
```

此时将会看到输出的前几行结果. `more`工具使用**空格翻页, 并可以用`q`键在中途退出**. `less`工具则更为强大, **不仅可以向下翻页, 还可以向上翻页**, 同样使用`q`键退出. 这里还有一个[关于less的小故事](http://en.wikipedia.org/wiki/Less_(Unix)).

## 在Linux下编写Hello World程序

Linux中**用户的主目录是`/home/用户名称`**, 如果你的用户名是`user`, 你的主目录就是`/home/user`. **用户的`home`目录可以用波浪符号`~`替代**, 例如临时文件目录`/home/user/Templates`可以**简写为`~/Templates`**. 现在我们就可以进入主目录并编辑文件了. 如果`Templates`目录不存在, 可以通过`mkdir`命令创建它:

```
cd ~
mkdir Templates
```

创建成功后, 键入

```
cd Templates
```

可以完成目录的切换. 注意在输入目录名时, **`tab`键可以提供联想.**

进入正确的目录后就可以编辑文件了, 开源世界中主流的**两大编辑器是`vi(m)`和`emacs`,** 你可以使用其中的任何一种. 如果你打算使用`emacs`, 你还需要安装它

```
apt-get install emacs
```

`vi`和`emacs`这两款编辑器都需要**一定的时间才能上手**, 它们共同的特点是**需要花较多的时间才能适应基本操作方式(命令或快捷键),** 但**一旦熟练运用, 编辑效率就比传统的编辑器快很多.**

进入了正确的目录后, 输入相应的命令就能够开始编辑文件. 例如输入

```
vi hello.c
或emacs hello.c
```

就能开启一个文件编辑. 例如可以键入如下代码(对于首次使用`vi`或`emacs`的同学, 键入代码可能会花去一些时间, 在编辑的同时要大量查看网络上的资料):

```c
#include <stdio.h>
int main(void) {
  printf("Hello, Linux World!\n");
  return 0;
}
```

保存后就能够看到`hello.c`的内容了. 终端中可以用`cat hello.c`查看代码的内容. 如果要将它编译, 可以使用`gcc`命令:

```
gcc hello.c -o hello
```

`gcc`的`-o`选项指定了输出文件的名称, **如果将`-o hello`改为`-o hi`, 将会生成名为`hi`的可执行文件. 如果不使用`-o`选项, 则会默认生成名为`a.out`的文件**, 它的含义是[assembler output](http://en.wikipedia.org/wiki/A.out). 在命令行输入

```
./hello
```

就能够运行该程序. 命令中的`./`是不能少的, 点代表了当前目录, **而`./hello`则表示当前目录下的`hello`文件**. 与Windows不同, **Linux系统默认情况下并不查找当前目录, 这是因为Linux下有大量的标准工具**(如`test`等), **很容易与用户自己编写的程序重名**, 不搜索当前目录消除了命令访问的歧义.

## 使用重定向

有时我们希望**将程序的输出信息保存到文件中**, 方便以后查看. 例如你编译了一个程序`myprog`, 你可以**使用以下命令对`myprog`进行反汇编, 并将反汇编的结果保存到`output`文件中:**

```
objdump -d myprog > output
```

`>`是**标准输出重定向符号**, 可以将**前一命令的输出重定向到文件`output`**中. 这样, 你就可以使用文本编辑工具查看`output`了.

但你会发现, 使用了输出重定向之后, 屏幕上就不会显示`myprog`输出的任何信息. 如果你**希望输出到文件的同时也输出到屏幕上**, 你可以使用`tee`命令:

```
objdump -d myprog | tee output
```

使用输出重定向还**能很方便地实现一些常用的功能**, 例如

```
> empty                  # 创建一个名为empty的空文件
cat old_file > new_file  # 将文件old_file复制一份, 新文件名为new_file
```

如果`myprog`需要**从键盘上读入大量数据(例如一个图的拓扑结构**), 当你需要**反复对`myprog`进行测试的时候, 你需要多次键入大量相同的数据.** 为了避免这种无意义的重复键入, 你可以使用以下命令:

```
./myprog < data
```

`<`是标准输入重定向符号, 可以将前一命令的输入重定向到文件`data`中. 这样, 你只需要将`myprog`读入的数据一次性输入到文件`data`中, `myprog`就会从文件`data`中读入数据, 节省了大量的时间.

下面给出了一个综合使用重定向的例子:

```
time ./myprog < data | tee output
```

这个命令在运行`myprog`的同时, 指定其从文件`data`中读入数据, **并将其输出信息打印到屏幕和文件`output`中**. **`time`工具记录了这一过程所消耗的时间**, 最后你会在屏幕上看到`myprog`运行所需要的时间. 如果你**只关心`myprog`的运行时间, 你可以使用以下命令将`myprog`的输出过滤掉:**

```
time ./myprog < data > /dev/null
```

**`/dev/null`是一个特殊的文件**, 任何**试图输出到它的信息都会被丢弃**, 你能想到这是怎么实现的吗? 总之, 上面的命令将`myprog`的输出过滤掉, 保留了`time`的计时结果, 方便又整洁.

## 使用Makefile管理工程

大规模的工程中通常含有几十甚至成百上千个源文件(Linux内核源码有25000+的源文件), 分别键入命令对它们进行编译是十分低效的. Linux提供了**一个高效管理工程文件的工具: GNU Make.** 我们首先从一个简单的例子开始, 考虑上文提到的Hello World的例子, 在`hello.c`所在目录下新建一个文件`Makefile`, 输入以下内容并保存:

```makefile
hello:hello.c
    gcc hello.c -o hello    # 注意开头的tab, 而不是空格

.PHONY: clean

clean:
    rm hello    # 注意开头的tab, 而不是空格
```

返回命令行, 键入`make`, 你会发现`make`程序调用了`gcc`进行编译. `Makefile`文件由若干规则组成, 规则的格式一般如下:

```
目标文件名:依赖文件列表
    用于生成目标文件的命令序列   # 注意开头的tab, 而不是空格
```

我们来解释一下上文中的`hello`规则. 这条规则告诉`make`程序, 需要**生成的目标文件是`hello`, 它依赖于文件`hello.c`,** 通过**执行命令**`gcc hello.c -o hello`来生成`hello`文件.

如果你连续多次执行`make`, 你会得到**"文件已经是最新版本"的提示信息,** 这是`make`程序智能管理的功能. 如果**目标文件已经存在, 并且它比所有依赖文件都要"新", 用于生成目标的命令就不会被执行**. 你能想到`make`程序是如何进行"新"和"旧"的判断的吗?

上面例子中的`clean`规则比较特殊, 它**并不是用来生成一个名为`clean`的文件**, 而是**用于清除编译结果**, 并且它不依赖于其它任何文件. `make`程序总是希望通过执行命令来生成目标, 但我们给出的命令`rm hello`并不是用来生成`clean`文件, 因此这样的命令总是会被执行. 你**需要键入`make clean`命令**来告诉`make`程序执行`clean`规则, 这是**因为`make`默认执行在`Makefile`中文本序排在最前面的规则.** 但如果很不幸地, 目录下**已经存在了一个名为`clean`的文件, 执行`make clean`会得到"文件已经是最新版本"的提示**. 解决这个问题的方法是在`Makefile`中**加入一行`PHONY: clean`, 用于指示"`clean`是一个伪目标".** 这样以后, `make`程序**就不会判断目标文件的新旧**, **伪目标相应的命令序列总是会被执行.**

对于一个规模稍大一点的工程, `Makefile`文件还会使用变量, 函数, 调用Shell命令, 隐含规则等功能. 如果你希望学习如何更好地编写一个`Makefile`, 请到互联网上搜索相关资料.

## 综合示例: 教务刷分脚本

使用编辑器编辑文件`jw.sh`为如下内容(另外由于教务网站的升级改版, 目前此脚本可能不能实现正确的功能):

```sh
#!/bin/bash
save_file="score" # 临时文件
semester=20102 # 刷分的学期, 20102代表2010年第二学期
jw_home="http://jwas3.nju.edu.cn:8080/jiaowu" # 教务网站首页地址
jw_login="http://jwas3.nju.edu.cn:8080/jiaowu/login.do" # 登录页面地址
jw_query="http://jwas3.nju.edu.cn:8080/jiaowu/student/studentinfo/achievementinfo.do?method=searchTermList&termCode=$semester" # 分数查询页面地址

name="09xxxxxxx" # 你的学号
passwd="xxxxxxxx" # 你的密码

# 请求jw_home地址, 并从中找到返回的cookie. cookie信息在http头中的JSESSIONID字段中
cookie=`wget -q -O - $jw_home --save-headers | \
    sed -n 's/Set-Cookie: JSESSIONID=\([0-9A-Z]\+\);.*$/\1/p'`
# 用户登录, 使用POST方法请求jw_login地址, 并在POST请求中加入userName和password
wget -q -O - --header="Cookie:JSESSIONID=$cookie" --post-data \
    "userName=${name}&password=${passwd}" "$jw_login" &> /dev/null
# 登录完毕后, 请求分数查询页面. 此时会返回html页面并输出到标准输出. 我们将输出重定向到文件"tmp"中.
wget -q -O - --header="Cookie:JSESSIONID=$cookie" "$jw_query" > tmp
# 获取分数列表. 因为教务网站的代码实在是实现得不太规整, 我们又想保留shell的风味, 所以用了比较繁琐的sed和awk处理. list变量中会包含课程名称的列表.
list=`cat tmp | sed -n '/<table.*TABLE_BODY.*>/,/<\/table>/p' \
        | sed '/<--/,/-->/d' | grep td \
        | awk 'NR%11==3' | sed 's/^.*>\(.*\)<.*$/\1/g'`
# 对list中的每一门课程, 都得到它的分数
for item in $list; do
    score=`cat tmp | grep -A 20 $item | awk "NR==18" | sed -n '/^.*\..*$/p'`
    score=`echo $score`
    if [[ ${#score} != 0 ]]; then # 如果存在成绩
        grep $item $save_file &>/dev/null # 查找分数是否显示过
        if [[ $? != 0 ]]; then # 如果没有显示过
        # 考虑到尝试的同学可能没有安装notify-send工具, 这里改成echo  -- yzh
            # notify-send "新成绩：$item $score" # 弹出窗口显示新成绩
            echo "新成绩：$item $score" # 在终端里输出新成绩
            echo $item >> $save_file # 将课程标记为已显示
        fi
    fi
done
```

运行这个例子需要在命令行中输入`bash jw.sh`, **用bash解释器执行这一脚本**. 如果**希望定期运行这一脚本**, 可以使用**Linux的标准工具之一: `cron`.** 将命令**添加到crontab**就能实现定期自动刷新.

为了理解这个例子, 首先需要一些HTTP协议的基础知识. HTTP请求实际就是**来回传送的文本流**——**浏览器**(或我们例子中的爬虫)生成一个文本格式的HTTP请求, **包括header和content**, 以**文本的形式**通过网络传送给服务器. 服务器根据请求内容(header中包含请求的URL以及浏览器等其他信息), 生成页面并返回.

**用户登录的实现, 就是通过HTTP头中header中的cookie实现的**. 当浏览器第一次请求页面时, 服务器会返回一串字符, 用来**标识浏览器的这次访问**. 从此以后, **所有与该网站交互时, 浏览器都会在HTTP请求的header中加入这个字符串**, 这样服务器就"记住"了浏览器的访问. 当完成登录操作(将用户名和密码发送到服务器)后, 服务器就知道**这个cookie隐含了一个合法登录的帐号, 从而能够根据帐号信息发送成绩.**

得到包含了成绩信息的html文档之后, 剩下的事情就是解析它了. 我们用了大量的`sed`和`awk`完成这件事情, 同学们不用去深究其中的细节, 只需知道我们从文本中提取出了课程名和成绩, 并且将没有显示过的成绩显示出来.

我们讲解这个例子主要是为了说明新环境下的工作方式, 以及实践Unix哲学:

- **每个程序只做一件事, 但做到极致**
- 用**程序之间的相互协作来解决复杂**问题
- 每个**程序都采用文本作为输入和输出**, 这会使程序更易于使用

一个Linux老手可以用脚本完成各式各样的任务: 在日志中筛选想要的内容, 搭建一个临时HTTP服务器(核心是使用`nc`工具)等等. 功能齐全的标准工具使Linux成为工程师, 研究员和科学家的最佳搭档.

### Installing tmux

`tmux` is **a terminal multiplexer**. With it, you can **create multiple terminals in a single screen**. It is **very convenient when you are working with a high resolution monitor**. To install `tmux`, just issue the following command:

```
apt-get install tmux
```

Now you can run `tmux`, but let's **do some configuration first.** Go back to the home directory:

```
cd ~
```

**New a file** called `.tmux.conf`:

```
vim .tmux.conf
```

Append the following content to the file:

```
bind-key c new-window -c "#{pane_current_path}"
bind-key % split-window -h -c "#{pane_current_path}"
bind-key '"' split-window -c "#{pane_current_path}"
```

These three lines of settings **make `tmux` "remember" the current working directory of the current pane while creating new window/pane.**

**Maximize the terminal windows size**, then use `tmux` to **create multiple normal-size terminals** within single screen. For example, you may **edit different files in different directories simultaneously**. You **can edit them in different terminals**, **compile them or execute other commands in another terminal**, without opening and closing source files back and forth. You **can scroll the content in a `tmux` terminal up and down.** For how to use `tmux`, please STFW.

##### 为什么要使用tmux?

这其实是一个"使用正确的工具做事情"的例子.

计算机天生就是为用户服务的, 只要你**有任何需求**, 你都可以想, "**有没有工具能帮我实现**?". 我们希望每个终端做不同的事情, 能够在屏幕上一览无余的同时, 还能在终端之间快速切换. 事实上, 通过STFW和RTFM你就可以掌握如何使用一款正确的工具: 你只要在搜索引擎上搜索"Linux 终端 分屏", 就可以搜到`tmux`这个工具; 然后再**搜索"tmux 使用教程", 就可以学习到`tmux`的基本使用方法;** 在终端中**输入`man tmux`, 就可以查阅关于`tmux`的任何疑问.**

当然, 学习不是零成本的. 往届有学长提出一种零学习成本的分屏方式: 打开4个终端, 并将它们分别拖动到屏幕的4个角落, 发现用Alt+Tab快捷键不方便选择窗口(因为4个窗口的外貌都差不多), 就使用鼠标点击的方式来切换. 然后形容安装学习`tmux`是"脱裤子放屁 -- 多此一举".

`tmux`的初衷就是为用户**节省上述的操作成本**. 如果你抱着不愿意付出任何学习成本的心态, 就无法享受到工具带来的便利.

#####  Things behind scrolling

You should have used scroll bars in GUI. You may take this for granted. So you may consider **the original un-scrollable terminal (the one you use when you just log in) the hell**. But think of these: **why the original terminal can not be scrolled**? **How does `tmux` make the terminals scrollable**? And last, do you know how to implement a scroll bar?

GUI is **not something mysterious**. Remember, **behind every elements in GUI, there is a story about it**. Learn the story, and you will learn a lot. You may say "I just use GUI, and it is unnecessary to learn the story." Yes, you are right. The appearance of GUI is to hide the story for users. But almost everyone uses GUI in the world, and **that is why you can not tell the difference between you and them**.

## tmux使用教程

在 NJU PA 的 `PA0: More Exploration`（更多探索）部分，官方讲义强烈推荐安装并使用 **tmux**。作为一个 **Terminal Multiplexer (终端复用器)**，它是你在 Linux 环境下进行系统级编程的神兵利器。

在写 PA 时，你会频繁遇到这种场景：在一个终端里用 Vim 编辑代码，在另一个终端里运行 `make` 编译，同时还要看 GDB 的调试信息或查阅讲义。如果**没有 tmux，你需要在终端之间疲于奔命地来回切换**；而有了 tmux，你可以**在一个屏幕内高效管理所有工作流。**

为了帮助你应对英文考试，接下来的讲解将采用中英文双语的形式，重点标注常用技术术语（Technical Terms）。

------

### 1. 核心概念 (Core Concepts)

要掌握 tmux，首先需要理解它的三层级架构（Architecture）：

- **Session (会话):** **最高级别的容器**。它包含**一个或多个窗口**。即使你断开了 SSH 连接（或关闭了本地终端），**Session 仍然会在 Background (后台) 运行**，保护你的工作进度不丢失。
- **Window (窗口):** **类似于浏览器中的标签页（Tab）**。一个 Session 可以有多个 Window，每个 Window 占据整个屏幕。
- **Pane (窗格):** 一个 Window 可以**被分割成多个 Pane**。这是 PA 中最常用的功能，允许你在左边放 Vim，右边放 GDB。

------

### 2. PA 专属环境配置 (PA-Specific Configuration)

根据 NJU PA 讲义的要求，安装 tmux 后，我们首先需要配置它，以解决一个痛点：默认情况下，新建 Pane 会跳回主目录（Home directory），而我们希望它留在 **Current Working Directory (当前工作目录)**。

**步骤 (Steps):**

1. 在终端中安装 tmux：`sudo apt-get install tmux`

2. 回到主目录并创建（或编辑）**Configuration file (配置文件)**：`vim ~/.tmux.conf`

3. 将讲义中要求的以下三行代码追加到文件中：

	Code snippet

	```
	bind-key c new-window -c "#{pane_current_path}"
	bind-key % split-window -h -c "#{pane_current_path}"
	bind-key '"' split-window -c "#{pane_current_path}"
	```

4. 保存退出后，在终端运行 `tmux source ~/.tmux.conf` 使其生效。

------

### 3. PA 核心操作指南 (Essential PA Workflow)

在 tmux 中，**所有的快捷键指令**都需要先按下 **Prefix Key (前缀键)**。**默认的前缀键是 `Ctrl + b`（通常简写为 `C-b`）。**

也就是说，当你看到 `C-b %` 时，意思是：**先按下 `Ctrl` 和 `b`，松开，然后再按 `%`。**

#### A. 分割窗格 (Splitting Panes) —— 一屏多用

- `C-b %`：进行 **Vertical split (垂直分割)**。屏幕左右一分为二。（适合左边写代码，右边看讲义或运行结果）。
- `C-b "`：进行 **Horizontal split (水平分割)**。屏幕上下一分为二。（适合下方留一个小窗口跑 `make run`）。

#### B. 窗格导航与管理 (Navigating & Managing Panes)

- `C-b + 方向键 (Arrow keys)`：**在不同的 Pane 之间移动光标。**
- `C-b z`：**Zoom (放大/还原) 窗格**。这是 PA 中的高频操作！当你遇到满屏的编译报错（Compilation errors）或需要仔细查看 GDB 的长段内存数据时，按 `C-b z` 可以**让当前 Pane 全屏，看完后再按一次即可还原。**
- `C-b x`：**关闭当前 Pane（需按 `y` 确认）。或者直接在当前终端输入 `exit`。**

#### C. 会话的挂起与恢复 (Detaching and Attaching) —— 防断连神器

- `C-b d`：**Detach (挂起会话)**。你的**所有 Vim 进度、编译状态都会被安全保存在后台**。
- `tmux attach` (或简写为 `tmux a`)：在普通终端中输入此命令，**即可 Attach (恢复) 之前挂起的会话，**瞬间回到你离开时的开发状态。

------

### 💡 自检问题 (Self-Check Question)

为了检验你对上述概念和 PA 配置的理解，请思考以下问题（答案已附上，请先遮挡自检）：

**问题 (Question):** In the context of terminal multiplexers, what is the primary purpose of the `.tmux.conf` configuration provided in the NJU PA manual? Why is it crucial for your development workflow? (请结合**当前工作目录 / Current Working Directory** 的概念进行解释)

**答案 (Answer):**

> **English:** The primary purpose of this configuration is to ensure that whenever we create a new **Window** or split a **Pane** (using `c`, `%`, or `"`), the newly created terminal inherits the **Current Working Directory (CWD)** of the active pane. **中文解释:** 在 PA 的开发流中，我们**经常处于深层目录（**例如 `nemu/src/cpu/`）。如果不加这三行配置，**每次分割出新窗格准备执行 `make` 或查看同一目录下的其他文件时，新终端都会默认回到 `~` (Home) 目录**，导致我们需要不厌其烦地重新 `cd`。**这个配置让 tmux "记住"当前路径**，极大提高了我们在特定模块下并行编写代码和调试的效率。

### Why GNU/Linux and How to

#####  为什么要使用Linux?

我们先来看两个例子.

**如何比较两个文件是否完全相同?** 这个例子看上去非常简单, 在**Linux下使用`diff`命令就可以实现. 如果文件很大, 那不妨用`md5sum`来计算并比较它们的MD5.** 对一个Linux用户来说, 键入这些命令只需要花费大约3秒的时间. 但在Windows下, 这件事要做起来就不那么容易了. 也许你下载了一个MD5计算工具, 但你需要点击多少次鼠标才能完成比较呢? 也许你觉得一次好像也省不了多少时间, 然而真相是, **你的开发效率就是这样一点点降低的.**

**如何列出一个C语言项目中所有被包含过的头文件?** 这个例子比刚才的稍微复杂一些, 但在Windows下你几乎无法通过GUI工具高效地做到它. 在Linux中, 我们只需要通过一行命令就可以做到了:

```
find . -name "*.[ch]" | xargs grep "#include" | sort | uniq
```

通过查阅`man`, 你应该不难理解上述命令是如何实现所需功能的. 这个例子再次体现了**Unix哲学:**

- **每个工具只做一件事情, 但做到极致**
- **工具采用文本方式进行输入输出, 从而易于使用**
- **通过工具之间的组合来解决复杂问题**

Unix哲学的最后一点最能体现Linux和Windows的区别: 编程创造. 如果把工具比作代码中的函数, 工具之间的组合就是一种编程. 而对初学者来说, Windows的GUI工具之间几乎无法组合, 因为面向普通用户的Windows需要强调易用性.

所以, 你应该使用Linux的原因非常简单: 作为一个码农, Windows一直在阻碍你思想, 能力和效率的提升.

#####  如何用好Linux?

1. 卸载Windows, 解放思想, 摆脱Windows对你的阻碍. **与其默认"没办法, 也只能这样了", 你应该去尝试"看看能不能把这件事做好"(尤其是当这件事明明有解法的时候).**
	- Linux下也有相应的常用软件, 如Chrome, WPS, 中文输入法, mplayer...
	- 没有Windows你也可以活下去
	- 实在不行可以装个Windows虚拟机备用
2. **熟悉一些常用的命令行工具, 并强迫自己在日常操作中使用它们**
	- **文件管理 -** `cd`, `pwd`, `mkdir`, `rmdir`, `ls`, `cp`, `rm`, `mv`, `tar`
	- 文件检索 - `cat`, `more`, `less`, `head`, `tail`, `file`, `find`
	- 输入输出控制 - 重定向, 管道, `tee`, `xargs`
	- 文本处理 - `vim`, `grep`, `awk`, `sed`, `sort`, `wc`, `uniq`, `cut`, `tr`
	- 正则表达式
	- 系统监控 - `jobs`, `ps`, `top`, `kill`, `free`, `dmesg`, `lsof`
	- **上述工具覆盖了程序员绝大部分的需求**
		- 可以先从简单的尝试开始, 用得多就记住了, 记不住就`man`
3. RTFM + STFW
4. 坚持.
	- 心态上, 相信总有对的工具能帮助我做得更好
	- 行动上, 愿意付出时间去找到它, 学它, 用它

#####  墙裂推荐: The Missing Semester of Your CS Education

[The Missing Semester of Your CS Education](https://missing.csail.mit.edu/)是jyy墙裂推荐的Linux工具系列教程, 教你如何使用各种工具来帮助你在计算机上高效地完成各种任务, 让你终身收益.

这套教程有中文版, 去看看吧.

#####  克服恐惧, 累积最初的信心

事实上, 学习使用Linux是一个低成本, 高成功率的锻炼机会. 只要你愿意STFW和RTFM, 就能解决绝大部分的问题. 相比较而言, 你之后(后续PA中/后续课程中/工作中)遇到的问题只会更加困难. 因此, 独立解决这些简单的小问题, 你就会开始积累最初的信心, 从而也慢慢相信自己有能力解决更难的问题.

## Getting Source Code for PAs

### Getting Source Code

Go back to the home directory by

```
cd ~
```

Usually, **all works unrelated to system should be performed under the home directory**. Other directories **under the root of file system (`/`) are related to system**. Therefore, **do NOT finish your PAs and Labs under these directories by `sudo`.**

#####  不要使用root账户做实验!!!

使用root账户进行实验, 会改变实验相关文件的权限属性, 可能会导致开发跟踪系统无法正常工作; 更严重的, 你的误操作可能会**无意中损坏系统文件, 导致系统无法启动**! 往届有若干学长因此而影响了实验进度, 甚至由于损坏了实验相关的文件而影响了分数. 请大家引以为鉴, 不要贪图方便, 否则后果自负!

如果你仍然不理解为什么要这样做, 你可以阅读这个页面: [Why is it bad to login as root?](http://askubuntu.com/questions/16178/why-is-it-bad-to-login-as-root) 正确的做法是: 永远使用你的普通账号做那些安分守己的事情(例如写代码), **当你需要进行一些需要root权限才能进行的操作时, 使用`sudo`.**

#####  在github上添加ssh key

在获取框架代码之前, 首先请你在github上添加一个ssh key, 具体操作请STFW.

Now get the source code for PA by the following command:

```
git clone -b 2025 git@github.com:NJU-ProjectN/ics-pa.git ics2025
```

**A directory called `ics2025` will be created**. This is the project directory for PAs. Details will be explained in PA1.

Then issue the following commands to perform `git` configuration:

```
git config --global user.name "20242081007-Rao Xiong Chao" # your student ID and name
git config --global user.email "2994425466@qq.com"   # your email
git config --global core.editor vim                 # your favorite editor
git config --global color.ui true
```

You should configure `git` with your student ID, name, and email. Before continuing, please read [this](https://nju-projectn.github.io/ics-pa-gitbook/ics2025/git.html) `git` tutorial to learn some basics of `git`. Another material recommended by jyy is [Visualizing Git Concepts with D3](http://onlywei.github.io/explain-git-with-d3). You can learn some `git` commands with the help of visualization.

Enter the project directory `ics2025`, then run

```
git branch -m master
bash init.sh nemu
bash init.sh abstract-machine
```

to **initialize some subprojects**. The script **will pull some subprojects from github**. We will explain them later.

Besides, the script will also add some environment variables into the bash configuration file `~/.bashrc`. These variables are defined by absolute path to support the compilation of the subprojects. Therefore, **DO NOT move** your project to another directory once finishing the initialization, else these variables will become invalid. Particularly, if you use shell other than `bash`, please set these variables in the corresponding configuration file manually.

To let the environment variables take effect, run

```
source ~/.bashrc
```

Then try

```
echo $NEMU_HOME
echo $AM_HOME
cd $NEMU_HOME
cd $AM_HOME
```

to check **whether these environment variables get the right paths.** If both the `echo` commands report the right paths, and both the `cd` command change to the target paths without errors, we are done. If not, please **double check the steps** above and the shell you are using.

### Git usage

We will use the `branch` feature of `git` to manage the process of development. A branch is **an ordered list of commits**, where a commit refers to some modifications in the project.

You can list all branches by

```
git branch
```

You will see there is only one branch called "master" now.

```
* master
```

To create a new branch, use `git checkout` command:

```
git checkout -b pa0
```

This command will create a branch called `pa0`, and check out to it. Now list all branches again, and you will see we are **now at branch `pa0`:**

```
  master
* pa0
```

From now on, all modifications of files in the project **will be recorded in the branch `pa0`.**

Now have a try! Modify the `STUID` and `STUNAME` variables in `ics2025/Makefile`:

```
STUID = 241220000  # your student ID
STUNAME = 张三     # your Chinese name
```

Run

```
git status
```

to see those files **modified from the last commit:**

```
On branch pa0
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

    modified:   Makefile

no changes added to commit (use "git add" and/or "git commit -a")
```

Run

```
git diff
```

to list modifications from the last commit:

```diff
diff --git a/Makefile b/Makefile
index c9b1708..b7b2e02 100644
--- a/Makefile
+++ b/Makefile
@@ -1,4 +1,4 @@
-STUID = 241220000
-STUNAME = 张三
+STUID = 241221234
+STUNAME = 李四

  # DO NOT modify the following code!!!
```

You should see `STUID` and `STUNAME` are modified. Now add the changes to commit by `git add`, and issue `git commit`:

```
git add .
git commit
```

The `git commit` command **will call the text editor.** **Type `modified my info` in the first line**, and **keep the remaining contents unchanged**. **Save and exit** the editor, and this **finishes a commit**. Now you should **see a log labeled with your student ID and name by**

```
git log
```

Now **switch back to the `master` branch** by

```
git checkout master
```

Open `ics2025/Makefile`, and you will find that `STUID` and `STUNAME` are still unchanged! By **issuing `git log`, you will find that the commit log you just created has disappeared**!

Don't worry! This is **a feature of branches in `git`.** Modifications in different branches **are isolated**, which means modifying files in one branch will not affect other branches. Switch back to `pa0` branch by

```
git checkout pa0
```

You will find that everything comes back! At the beginning of PA1, you will **merge all changes in branch `pa0` into `master`.**

The workflow above shows how you will use branch in PAs:

- before **starting a new PA**, **new a branch `pa?` and check out to it**
- coding in the branch `pa?` (this will introduce lot of modifications)
- after finish the PA, **merge the branch `pa?` into `master`, and check out back to `master`**

### Compiling and Running NEMU

Now **enter `nemu/` directory**. Before the first time to compile NEMU, a configuration file should be generated by

```
make menuconfig
```

error:

```
root@LAPTOP-C38OJTN9:~/workspace/project/learning/ics2025/nemu# make menuconfig
/root/workspace/project/learning/ics2025/nemu/scripts/config.mk:20: \033[1;31mWarning: .config does not exist!\033[0m
/root/workspace/project/learning/ics2025/nemu/scripts/config.mk:21: \033[1;31mTo build the project, first run 'make menuconfig'.\033[0m
+ CC confdata.c
+ CC expr.c
+ CC preprocess.c
+ CC symbol.c
+ CC util.c
+ YACC build/parser.tab.h
make[1]: bison: No such file or directory
make[1]: *** [Makefile:32: build/parser.tab.h] Error 127
make: *** [/root/workspace/project/learning/ics2025/nemu/scripts/config.mk:39: /root/workspace/project/learning/ics2025/nemu/tools/kconfig/build/mconf] Error 2
```

lack of tool:

```
apt install -y bison flex libncurses-dev
cd ~/workspace/project/learning/ics2025/nemu
make menuconfig
```

**A menu will pop up.**

![image-20260329163219303](./NJUPA%E9%9A%8F%E8%AE%B01/image-20260329163219303.png) DO NOT modify anything. Just **choose `Exit` and `Yes` to save the new configuration.** After that, compile the project by `make`:

```
make
```

If nothing goes wrong, NEMU **will be compiled successfully.**

To **perform a fresh compilation,** type

```
make clean
```

to **remove the old compilation result, then `make` again.**

To **run NEMU, type**

```
make run
```

However, you will **see an error message:**

```
[src/monitor/monitor.c:35 welcome] Exercise: Please remove me in the source code and compile NEMU again.
riscv32-nemu-interpreter: src/monitor/monitor.c:36: welcome: Assertion `0' failed.
```

This message tells you that the program **has triggered an assertion fail at line 36 of the file `nemu/src/monitor/monitor.c`**. If you do not know what is assertion, blame the 程序设计基础 course. But just ignore it now, and you **will fix it in PA1.**

To **debug NEMU with gdb**, type

```
make gdb
```

### Development Tracing

Once the compilation succeeds, the change of source code will be traced by `git`. Type

```
git log
```

If you see something like

```
commit 4072d39e5b6c6b6837077f2d673cb0b5014e6ef9
Author: tracer-ics2025 <tracer@njuics.org>
Date:   Sun Jul 26 14:30:31 2025 +0800

    >  run NEMU
    241220000 张三
    Linux 9900k 5.10.0-10-amd64 #1 SMP Debian 5.10.84-1 (2021-12-08) x86_64 GNU/Linux
    15:57:01 up 22 days,  6:01, 16 users,  load average: 0.00, 0.00, 0.00
```

this means the **change is traced successfully.**

If you see the following message while executing make, this means the tracing fails.

```
fatal: Unable to create '/home/user/ics2025/.git/index.lock': File exists.

If no other git process is currently running, this probably means a
git process crashed in this repository earlier. Make sure no other git
process is running and remove the file manually to continue.
```

Try to clean the compilation result and compile again:

```
make clean
make
```

If the error message above always appears, please contact us as soon as possible.

#####  开发跟踪

我们使用`git`对你的实验过程进行跟踪, 不合理的跟踪记录会影响你的成绩. 往届有学长"完成"了某部分实验内容, 但我们找不到相应的git log, 最终该部分内容被视为没有完成. **git log是独立完成实验的最有力证据**, 完成了实验内容却缺少合理的git log, 不仅会损失大量分数, 还会给抄袭判定提供最有力的证据. 因此, 请你注意以下事项:

- 请你不定期查看自己的git log, 检查是否与自己的开发过程相符.
- 提交往届代码将被视为没有提交.
- 不要把你的代码上传到公开的地方(现在github个人账号也可以创建私有仓库了).
- 总是在工程目录下进行开发, 不要在其它地方进行开发, 然后一次性将代码复制到工程目录下, 这样`git`将不能正确记录你的开发过程.
- 不要修改`Makefile`中与开发跟踪相关的内容.
- 不要删除我们要求创建的分支, 否则会影响我们的脚本运行, 从而影响你的成绩
- 不要清除git log

偶然的跟踪失败不会影响你的成绩. 如果上文中的错误信息总是出现, 请尽快联系我们.

#####  我不是修读本课程的学生, 是否能够关闭开发跟踪?

可进行如下修改关闭开发跟踪:

```diff
diff --git a/Makefile b/Makefile
index c9b1708..b7b2e02 100644
--- a/Makefile
+++ b/Makefile
@@ -9,6 +9,6 @@
 define git_commit
-  -@git add .. -A --ignore-errors
-  -@while (test -e .git/index.lock); do sleep 0.1; done
-  -@(echo "> $(1)" && echo $(STUID) $(STUNAME) && uname -a && uptime) | git commit -F - $(GITFLAGS)
-  -@sync
+# -@git add .. -A --ignore-errors
+# -@while (test -e .git/index.lock); do sleep 0.1; done
+# -@(echo "> $(1)" && echo $(STUID) $(STUNAME) && uname -a && uptime) | git commit -F - $(GITFLAGS)
+# -@sync
 endef
```

### Local Commit

Although the development tracing system will trace the change of your code after every successful compilation, the trace record is **not suitable for your development**. This is because **the code is still buggy at most of the time.** Also, it is **not easy for you to identify those bug-free traces**. Therefore, you should **trace your bug-free code manually.**

**When you want to commit the change, type**

```
git add .
git commit --allow-empty
```

**The `--allow-empty` option is necessary**, because usually the **change is already committed by development tracing system**. **Without this option, `git` will reject no-change commits**. If the commit succeeds, you can **see a log labeled with your student ID and name by**

```
git log
```

To filter out the commit logs **corresponding to your manual commit, use `--author` option with `git log`.** For details about how to use this option, RTFM.

### Submission

Finally, you should submit your project to the submission website (具体提交方式请咨询ICS实验课程老师). To submit PA0, put your report file (ONLY `.pdf` file is accepted) under the project directory.

```
ics2025
├── 241220000.pdf   # put your report file here
├── abstract-machine
├── fceux-am
├── init.sh
├── Makefile
├── nemu
└── README.md
```

Double check whether everything is fine. In particular, you should check whether your `.pdf` file can be opened with a PDF reader.

## RTFSC and Enjoy

If you are new to GNU/Linux and finish this tutorial by yourself, congratulations! You have learned a lot! The most important, you have learned STFW and RTFM for using new tools and trouble-shooting. (反思一下, 你真的做到了吗?) With these skills, you can solve lots of troubles by yourself during PAs, as well as in the future.

In PA1, the first thing you will do is to [RTFSC](http://i.linuxtoy.org/docs/guide/ch48s06.html). If you have troubles during reading the source code, go to RTFM:

- If you **can not find the definition of a function**, it is probably **a library function**. **Read `man` for more information** about that function.
- If you can not understand the code related to hardware details, refer to the manual.

By the way, you will use C language for programming in all PAs. [Here](http://akaedu.github.io/book/) is an excellent tutorial about C language. It contains not only C language (such as how to use `printf()` and `scanf()`), but also other elements in a computer system (data structure, computer architecture, assembly language, linking, operating system, network...). It covers most parts of this course. You are strongly recommended to read this tutorial.

Finally, enjoy the journey of PAs, and you will find hardware is not mysterious, so does the computer system! But remember:

- STFW
- RTFM
- RTFSC

#####  必答题

独立解决问题是作为码农的一项十分重要的生存技能. 往届有同学在群里提出如下问题:

- su认证失败是怎么回事?
- grep提示no such file or directory是什么意思?
- 请问怎么卸载Ubuntu?
- C语言的xxx语法是什么意思?
- ignoring return vaule of 'scanf'是什么意思?
- 出现curl: not found该怎么办?
- 为什么strtok返回NULL?
- 为什么会有Segmentation fault这个错误?
- 什么是busybox?

请仔细阅读[提问的智慧](https://github.com/ryanhanwu/How-To-Ask-Questions-The-Smart-Way/blob/master/README-zh_CN.md)和[别像弱智一样提问](https://github.com/tangx/Stop-Ask-Questions-The-Stupid-Ways/blob/master/README.md) (这篇文章很短, 1分钟就能看完)这两篇文章, 结合自己在大一时提问和被提问, 以及完成PA0的经历, 写一篇不少于800字的读后感, 谈谈你对"好的提问"以及"通过STFW和RTFM独立解决问题"的看法.

Hint: 我们设置这道题并不是为了故意浪费大家的时间, 也不是为了禁止大家提出任何问题, 而是为了让大家知道"什么是正确的". 当你愿意为这些"正确的做法"去努力, 并且尝试用专业的方式提出问题的时候, 你就已经迈出了成为"成为专业人士"的第一步.

# PA1 - 开天辟地的篇章: 最简单的计算机

在进行本PA前, 请在工程目录下执行以下命令进行分支整理, 否则将影响你的成绩:

```
git commit --allow-empty -am "before starting pa1"
git checkout master
git merge pa0
git checkout -b pa1
```

##### PA是一种全新的训练

我们从以下方面对不同的作业/实验/问题进行比较:

| 基本原理 | 做事方案     | 正确性风险   | 代表例子             |
| -------- | ------------ | ------------ | -------------------- |
| 阐述     | 明确         | 基本正确     | 高中物理实验         |
| 阐述     | 明确         | 可能出错     | 程序设计作业         |
| 阐述     | 需要思考     | 基本正确     | 数学证明/算法设计题  |
| **阐述** | **需要思考** | **可能出错** | **PA, OSlab**        |
| 需要探索 | 需要思考     | 可能出错     | 业界和科研的真实问题 |

做PA的终极目标是**通过构建一个简单完整的计算机系统, 来深入理解程序如何在计算机上运行**. 和那些"用递归实现汉诺塔"的程序设计作业不同, 计算机系统比汉诺塔要复杂得多. 这意味着, 通过程序设计作业的训练方式是不足以完成PA的, 只有去尝试理解并掌握计算机系统的每一处细节, 才能一步步完成PA.

所以, 不要再用程序设计作业的风格来抱怨PA讲义写得不清楚, 之所以**讲义的描述点到即止, 是为了强迫大家去理清计算机系统的每一处细节, 去推敲每一个模块之间的关系,** 也是为了让大家积累对系统足够的了解来面对未知的bug.

这对你来说也许是一种前所未有的训练方式, 所以你也需要拿出全新的态度来接受全新的挑战.

##### 做PA的正确姿势 - 从今天开始, 不要偷懒了 (这是一碗鸡汤, 当你将来觉得迷茫的时候, 回来这里看看吧)

~~我们先列举一些错误做法:~~

- ~~遇到问题了, 随便改改试试, 说不定就过了~~
- 随便改改过不了, 赶紧找大腿/助教/老师来搞定
	- ~~也不想多花时间精力按照[提问的智慧](https://github.com/ryanhanwu/How-To-Ask-Questions-The-Smart-Way/blob/master/README-zh_CN.md)和[别像弱智一样提问](https://github.com/tangx/Stop-Ask-Questions-The-Stupid-Ways/blob/master/README.md)中建议的方式来提问~~
- ~~这个函数/文件/命令看不懂, 反正也不是我写的, 算了就这样吧~~
- ~~宁愿在百度中舒服地浪费生命, 也不想用谷歌快速解决问题~~
- ~~蓝框题不算分, 不看也没关系~~
- ~~反正大阶段有一个月的时间, 最后一周开始做, 应该还能赶上~~

~~如果你采用了以上做法, 你也许真的能很快完成前期的实验内容, 但这是以放弃训练机会为代价的. 随着实验进度的推进, 你会感觉PA对你来说越来越吃力.~~

正确的做法是:

- 多思考为什么
	- 从问题开始着手理解系统也是个不错的方法
- 独立解决问题
	- 即使是调一个很弱智的bug, **"顺带"能学到的东西也比你想象中多得多**
	- 换句话说, 如果你选择抱大腿, 你失去的机会也比你想象中多得多
- 尝试尽可能理解每一处细节
	- 将来调bug的时候, 这些细节就是你手中强有力的工具
	- 换句话说, 当你在调bug的时候感到无从下手, 一定是你不了解其中的细节
- 用正确的工具做事情
	- 这才是节省时间的科学方法, 而不是偷懒
- 多读讲义, 彩蛋很多
	- 讲义中特地设置了不少"不合时宜"的提示, 有的彩蛋要多次阅读才能明白其中的奥妙
	- **多看一道蓝框题, 也许能少调几天bug**
- 按时完成, 拒绝拖延
	- 这样你才有时间做到上面几点

事实上, 这些做法就是PA中的最本质的能力训练, 而这样的训练, 在PA0就已经开始了: PA0之所以让大家白手装机, 就是希望让大家在解决小问题的过程中收获经验, 用来解决更大的问题; 同时也给大家传播"我可以通过STFW和RTFM独立解决问题"的最初原的信念, 这种信念可以帮助大家驱散对未知的恐惧.

你用来应付程序设计作业的心态, 在PA这里是混不过去的, 问题暴露的速度比你想象中快得多. 所以, 从今天开始, 不要偷懒了.

### NEMU是什么?

PA的目的是要实现NEMU, 一款经过简化的全系统模拟器. 但什么是模拟器呢?

你小时候应该玩过红白机, 超级玛丽, 坦克大战, 魂斗罗... 它们的画面是否让你记忆犹新? (希望我们之间没有代沟...) 随着时代的发展, 你已经很难在市场上看到红白机的身影了. 当你正在为此感到苦恼的时候, 模拟器的横空出世唤醒了你心中尘封已久的童年回忆. 红白机模拟器可以为你模拟出红白机的所有功能. 有了它, 你就好像有了一个真正的红白机, 可以玩你最喜欢的红白机游戏. 我们移植了一个[红白机模拟器项目FCEUX](https://github.com/NJU-ProjectN/fceux-am), 你在PA0中已经克隆了它. 你可以在如今这个红白机难以寻觅的时代, 再次回味你儿时的快乐时光, 这实在是太神奇了!

我们在[这里](https://jyywiki.cn/ICS/2021/labs/PA1.html)(可能需要在校园网内部访问)提供了一些游戏的ROM用于测试, 阅读并根据`fceux-am/README.md`中的内容进行操作, 即可在弹出的新窗口中运行超级玛丽.

你也可以将自己STFW获得的其它ROM文件放进来, 这样就可以运行其它游戏了.

在运行游戏的过程中, 你需要顺便检查一下是否可以看到画面, 响应按键并听到声音. 超级玛丽**在初始界面中不会播放声音, 但会在正式进入关卡时播放声音**. 如果没有声音, 会影响PA的部分选做内容, 但不会影响成绩; 但如果画面不能正常显示, 可能会影响PA必做部分的实验内容, 请自行搜索解决方案.

为了检查按键, 你需要克隆一个新的子项目`am-kernels`, 里面包含了一些测试程序:

```bash
cd ics2025
bash init.sh am-kernels
```

然后运行其中的按键测试程序:

```bash
cd am-kernels/tests/am-tests
make ARCH=native mainargs=k run
```

运行后会弹出一个新窗口, 在新窗口中按下按键, 你将会看到程序在终端输出相应的按键信息, 包括按键名, 键盘码, 以及按键状态. 如果你发现输出的按键信息与按下的按键不符, 请自行搜索解决方案(可采用关键字"SDL keystroke"等). 有网友提示问题可能与[中文输入法兼容性问题](https://github.com/NJU-ProjectN/fceux-am/issues/1)相关, 供参考.

#####  觉得编译有点慢?

`make`程序默认使用单线程来顺序地编译所有文件, 而FCEUX的源文件又非常多, 你可能需要等待十几秒来完成编译. 但现在的CPU都是多核多线程了, 不把这些计算能力用起来也是白白浪费. 为了加快编译的过程, 我们可以**让`make`创建多个线程来并行地编译文件.**

具体地, 首先你需要通过`lscpu`命令来查询你的系统中有多少个CPU. 然后在运行`make`的时候添加一个`-j?`的参数, 其中`?`为你**查询到的CPU数量**. 例如`-j4`表示创建4个线程来并行编译, 如果系统中CPU的数量大于等于4, 那么操作系统就可以将这4个线程调度到4个CPU上同时执行, **达到加速的效果**; 但**如果系统中只有2个CPU, 那操作系统最多能将2个线程调度到2个CPU上同时执行**, 这时候的加速效果就和`-j2`差不多了.

为了查看编译加速的效果, 你可以在**编译的命令前面添加`time`命令**, 它将会对紧跟在其后的命令的执行时间进行统计, 你只需要关注`total`一栏的时间即可. 你可以**通过`make clean`清除所有的编译结果, 然后重新编译并统计时间**, 对比单线程编译和多线程编译的编译时间; 你也可以尝试不同的线程数量进行编译, 并对比加速比.

#####  还是觉得编译有点慢?

我们清除所有编译结果之后重新编译, **源文件并没有发生任何变化**, 按道理编译出来的目标文件也应该和上一次编译结果完全相同. 既然这样, 那我们能不能把这些目标文件以某种方式存起来, **下次编译的时候如果发现源文件没有变化, 就直接取出之前的目标文件作为编译结果**, 从而跳过编译的步骤呢?

还真有工具专门做这件事! 这个工具叫`ccache`:

```bash
apt-get install ccache
```

如果你通过`man`阅读`ccache`的手册, 你会**发现`ccache`是一个`compiler cache`.** `cache`是计算机领域中的一个术语, 你将会在后续的ICS课程中学习相关的内容.

为了使用`ccache`, 你还需要进行一些配置的工作. 首先运行如下命令来查看一个命令的所在路径:

```bash
which gcc
```

它默认会输出`/usr/bin/gcc`, 表示当你执行`gcc`命令时, **实际执行的是`/usr/bin/gcc`.** 作为一个RTFM的练习, 接下来你需要阅读`man ccache`中的内容, 并根据手册的说明, 在`.bashrc`文件中对**某个环境变量进行正确的设置**. 如果你的设置正确且生效, **重新运行`which gcc`, 你将会看到输出变成了`/usr/lib/ccache/gcc`. 如果你不了解环境变量和`.bashrc`, STFW.**

现在就可以来体验`ccache`的效果了. 首先先清除编译结果, 然后重新编译并统计时间. 你会发现这次编译时间反而比之前要更长一些, 这是因为除了需要开展正常的编译工作之外, `ccache`还需要花时间把目标文件存起来. 接下来再次清除编辑结果, 重新编译并统计时间, 你会发现**第二次编译的速度有了非常明显的提升**! 这说明`ccache`确实跳过了完全重复的编译过程, 发挥了加速的作用. **如果和多线程编译共同使用, 编译速度还能进一步加快!**

在开发项目的过程中, 有时确实会需要在清除编译结果后进行全新的编译(fresh build). 到了PA的后期, 你可能会多次编译一些包含数百个文件的库, 在这些场合下, `ccache`能够极大地节省编译的时间, 从而提高项目开发的效率.

你被计算机强大的能力征服了, 你不禁思考, 这到底是怎么做到的? 你学习完程序设计基础课程, 但仍然找不到你想要的答案. 但你可以肯定的是, 红白机模拟器只是一个普通的程序, 因为你还是需要像运行Hello World程序那样运行它. 但同时你又觉得, 红白机模拟器又不像一个普通的程序, 它究竟是怎么模拟出一个红白机的世界, 让红白机游戏在这个世界中运行的呢?

事实上, NEMU就是在做类似的事情! 它**模拟了一个硬件的世界, 你可以在这个硬件世界中执行程序**. 换句话说, 你将要在PA中编写一个用来执行其它程序的程序! 为了更好地理解NEMU的功能, 下面将

- 在GNU/Linux中运行Hello World程序
- 在GNU/Linux中通过红白机模拟器玩超级玛丽
- 在GNU/Linux中通过NEMU运行Hello World程序

这三种情况进行比较.

```
                         +---------------------+  +---------------------+
                         |     Super Mario     |  |    "Hello World"    |
                         +---------------------+  +---------------------+
                         |    Simulated NES    |  |      Simulated      |
                         |       hardware      |  |       hardware      |
+---------------------+  +---------------------+  +---------------------+
|    "Hello World"    |  |     NES Emulator    |  |        NEMU         |
+---------------------+  +---------------------+  +---------------------+
|      GNU/Linux      |  |      GNU/Linux      |  |      GNU/Linux      |
+---------------------+  +---------------------+  +---------------------+
|    Real hardware    |  |    Real hardware    |  |    Real hardware    |
+---------------------+  +---------------------+  +---------------------+
          (a)                      (b)                     (c)
```

图中(a)展示了"在GNU/Linux中运行Hello World"的情况. GNU/Linux操作系统**直接运行在真实的计算机硬件上, 对计算机底层硬件进行了抽象, 同时向上层的用户程序提供接口和服务**. Hello World程序输出信息的时候, 需要用到操作系统提供的接口, 因此Hello World程序**并不是直接运行在真实的计算机硬件上, 而是运行在操作系统(在这里是GNU/Linux)上.**

图中(b)展示了"在GNU/Linux中通过红白机模拟器玩超级玛丽"的情况. 在GNU/Linux看来, 运行在其上的红白机模拟器NES Emulator和上面提到的Hello World程序一样, 都**只不过是一个用户程序而已.** 神奇的是, 红白机模拟器的功能是负责模拟出一套完整的红白机硬件, 让超级玛丽可以在其上运行. 事实上, 对于超级玛丽来说, 它并不能区分自己是运行在真实的红白机硬件之上, 还是运行在模拟出来的红白机硬件之上, 这正是**"模拟"的障眼法**.

图中(c)展示了"在GNU/Linux中通过NEMU执行Hello World"的情况. 在GNU/Linux看来, 运行在其上的NEMU和上面提到的Hello World程序一样, 都只不过是一个用户程序而已. 但NEMU的功能是负责模拟出一套计算机硬件, 让程序可以在其上运行. 事实上, 上图只是给出了对NEMU的一个基本理解, 更多细节会在后续PA中逐渐补充.

##### NEMU是什么?

上述描述对你来说也许还有些晦涩难懂, 让我们来看一个ATM机的例子.

ATM机是一个物理上存在的机器, 它的功能需要由物理电路和机械模块来支撑. 例如我们在ATM机上进行存款操作的时候, ATM机都会吭哧吭哧地响, 让我们相信确实是一台真实的机器. 另一方面, 现在第三方支付平台也非常流行, 例如支付宝. 事实上, 我们可以把支付宝APP看成一个模拟的ATM机, 在这个模拟的ATM机里面, 真实ATM机具备的所有功能, 包括存款, 取款, 查询余额, 转账等等, 都通过支付宝APP这个程序来实现.

同样地, NEMU就是一个模拟出来的计算机系统, 物理计算机中的基本功能, 在NEMU中都是通过程序来实现的. 要模拟出一个计算机系统并没有你想象中的那么困难. 我们可以把计算机看成由若干个硬件部件组成, 这些部件之间相互协助, 完成"运行程序"这件事情. 在NEMU中, 每一个硬件部件都由一个程序相关的数据对象来模拟, 例如变量, 数组, 结构体等; 而对这些部件的操作则通过对相应数据对象的操作来模拟. 例如NEMU中使用数组来模拟内存, 那么对这个数组进行读写则相当于对内存进行读写.

我们可以把实现NEMU的过程看成是开发一个支付宝APP. 不同的是, 支付宝具备的是真实ATM机的功能, 是用来交易的; 而NEMU具备的是物理计算机系统的功能, 是用来执行程序的. 因此我们说, NEMU是一个用来执行其它程序的程序.

##### 什么是ISA?

大部分课本上都会有类似"**ISA是软件和硬件之间的接口**"这种诠释, 但对于还不了解软件和硬件之间如何协同工作的你来说, "接口"这个词还是太抽象了.

为了理解ISA, 我们可以用现实生活中的例子来比喻: 螺钉和螺母是生活中两种常见的物品, 它们一般需要配对来使用. 给定一个螺钉, 那就要找到一个符合相同尺寸规范的螺母才能配合使用, 反之亦然.

在计算机世界中也是类似的: **不同架构的计算机**(或者说硬件)好比不同尺寸的螺钉, 不同架构的程序(或者说软件)就相当于是不同尺寸的螺母, 如果**一个程序要在特定架构的计算机上运行, 那么这个程序和计算机就必须是符合同一套规范才行.**

因此, ISA的本质就是类似这样的规范. 所以**ISA的存在形式既不是硬件电路, 也不是软件代码, 而是一本规范手册**.

和螺钉螺母的生产过程类似, 计算机硬件是按照ISA规范手册构造出来的, 而程序也是按照ISA规范手册编写(或生成)出来的, 至于ISA规范里面都有哪些内容, 我们应该如何构造一个符合规范的计算机, 程序应该如何遵守这些规范来在计算机上运行, 回答这些问题正是做PA的一个目标.

为了方便叙述, 讲义将用`$ISA`来表示**你选择的ISA,** 例如对于`nemu/src/isa/$ISA/reg.c`, 若你选择的是x86, 它将表示`nemu/src/isa/x86/reg.c`; 若你选择的是riscv32, 它将表示`nemu/src/isa/riscv32/reg.c`. 除非讲义明确说明, 否则`$ISA`总是表示你选择的ISA, 而不是`$ISA`这四个字符.

NEMU的框架代码会**把riscv32作为默认的ISA**, 如果你希望选择其它ISA, 你需要在NEMU的工程目录下执行`make menuconfig`, 然后在`Base ISA`一栏中切换到你选择的ISA, 然后保存配置并退出菜单.

# 开天辟地的篇章

### 最简单的计算机

为了执行程序, 首先要解决的第一个问题, 就是要**把程序放在哪里**. 显然, 我们不希望自己创造的计算机只能执行小程序. 因此, 我们需要一个足够大容量的部件, 来放下各种各样的程序, 这个部件就是存储器. 于是, 先驱创造了存储器, 并把程序放在存储器中, 等待着CPU去执行.

等等, CPU是谁? 你也许很早就听说过它了, 不过现在还是让我们来重新介绍一下它吧. CPU是先驱最伟大的创造, 从它的中文名字"中央处理器"就看得出它被赋予了至高无上的荣耀: CPU是负责处理数据的核心电路单元, 也就是说, **程序的执行全靠它了**. 但只有存储器的计算机还是不能进行计算. 自然地, CPU需要肩负起计算的重任, 先驱为CPU创造了运算器, 这样就可以对数据进行各种处理了. 如果觉得运算器太复杂, 那就先来考虑一个加法器吧.

先驱发现, 有时候程序需要对同一个数据进行连续的处理. 例如要计算`1+2+...+100`, 就要对部分和`sum`进行累加, 如果每完成一次累加都需要把它写回存储器, 然后又把它从存储器中读出来继续加, 这样就太不方便了. 同时天下也没有免费的午餐, 存储器的大容量也是需要付出相应的代价的, 那就是速度慢, 这是先驱也无法违背的材料特性规律. 于是先驱**为CPU创造了寄存器**, 可以让CPU**把正在处理中的数据暂时存放在其中**.

寄存器的速度很快, 但容量却很小, 和存储器的特性正好互补, 它们之间也许会交织出新的故事呢, 不过目前我们还是顺其自然吧.

#####  计算机可以没有寄存器吗? (建议二周目思考)

如果没有寄存器, 计算机还可以工作吗? 如果可以, 这会对硬件提供的编程模型有什么影响呢?

就算你是二周目来思考这个问题, 你也有可能是第一次听到"编程模型"这个概念. 不过如果一周目的时候你已经仔细地阅读过ISA手册, 你会记得确实有这么个概念. 所以, 如果想知道什么是编程模型, RTFM吧.

## QA

### 编程模型

在计算机体系结构（Computer Architecture）中，**编程模型（Programming Model）**，或者更准确地说是 **ISA 编程模型**，指的是：**硬件展现给程序员（或编译器）的、能够改变系统状态的所有可见资源和规则。**

简单来说，编程模型就是**一张“说明书”**，它告诉软件：“如果你想指挥这台机器，你可以用哪些东西，以及怎么用它们。”

------

### 1. 编程模型包含什么？

一个典型的编程模型主要由以下几个核心部分组成：

- **寄存器集合（Registers）：** 比如通用寄存器（`eax`, `rbx`, `x0`）、程序计数器（`PC` 或 `IP`）、状态寄存器（`EFLAGS`）。
- **指令集（Instruction Set）：** 硬件支持哪些操作？加法、跳转、访存等。
- **寻址方式（Addressing Modes）：** 软件如何通过偏移量或基址找到内存数据。
- **内存布局模型（Memory Layout）：** 硬件眼中的内存是平坦的还是分段的？
- **异常与中断处理机制：** 当发生除以零或按下键盘时，状态如何保存和跳转。

### 2. 回到 PA 的思考题：如果没有寄存器会怎样？

**答案是：可以工作，但编程模型会发生剧变。**

如果 CPU 内部**没有任何寄存器（连 `PC` 指针都没有的话就彻底没法跑了，假设我们只去掉通用寄存器）**，那么硬件提供的编程模型就会从**“寄存器-内存”架构**转变为**“内存-内存”架构**或**“栈”架构**。

#### 对编程模型的影响：

1. **指令格式变长：**

	- 在**有寄存器的模型下，一条加法指令是 `add eax, ebx`（通常几个字节）。**
	- 在**没有寄存器的模型下，你可能需要写成 `add [0x1000], [0x1004]`**。指令**必须包含完整的内存地址**，导致指令变得非常冗长，读取指令本身的开销会剧增。

	1. **执行效率暴跌：**

	- 寄存器是 CPU 内部的极高速缓存。如果没有寄存器，每一次最简单的运算（如 `i++`）都要进行 3 次访存（读 `i`，读 `1`，写 `i`）。内存的速度远慢于 CPU，这会产生巨大的**冯·诺依曼瓶颈**。

2. **编译器实现变得异常复杂：**

	- 编译器最核心的任务之一是**寄存器分配（Register Allocation）**。如果没有寄存器，编译器就**需要不断地管理内存地址的重用**，且无法利用寄存器的临时存储特性来优化循环和函数调用。

为了让强大的CPU成为忠诚的奴仆, 先驱还设计了"指令", 用来指示CPU对数据进行何种处理. 这样, 我们就可以通过指令来控制CPU, 让它做我们想做的事情了.

有了指令以后, 先驱提出了一个划时代的设想: 能否让程序来自动控制计算机的执行? 为了实现这个设想, 先驱和CPU作了一个简单的约定: 当**执行完一条指令之后, 就继续执行下一条指令**. 但CPU怎么知道现在执行到哪一条指令呢? 为此, 先驱为CPU创造了**一个特殊的计数器, 叫"程序计数器"(Program Counter, PC).** 在**x86中, 它有一个特殊的名字, 叫`EIP`(Extended Instruction Pointer).**

从此以后, 计算机就只需要做一件事情:

```c
while (1) {
  从PC指示的存储器位置取出指令;
  执行指令;
  更新PC;
}
```

这样, 我们就有了一个足够简单的计算机了. 我们只要**将一段指令序列放置在存储器中, 然后让PC指向第一条指令**, 计算机就会**自动执行这一段指令序列, 永不停止.**

例如, 下面的指令序列可以计算`1+2+...+100`, 其中`r1`和`r2`是两个寄存器, 还有一个隐含的程序计数器`PC`, 它的初值是`0`. 为了帮助大家理解, 我们把指令的语义翻译成C代码放在右侧, 其中每一行C代码前都添加了一个语句标号:

```
// PC: instruction    | // label: statement
0: mov  r1, 0         |  pc0: r1 = 0;
1: mov  r2, 0         |  pc1: r2 = 0;
2: addi r2, r2, 1     |  pc2: r2 = r2 + 1;
3: add  r1, r1, r2    |  pc3: r1 = r1 + r2;
4: blt  r2, 100, 2    |  pc4: if (r2 < 100) goto pc2;   // branch if less than
5: jmp 5              |  pc5: goto pc5;
```

##### 尝试理解计算机如何计算

在看到上述例子之前, 你可能会觉得指令是一个既神秘又难以理解的概念. 不过当你看到对应的C代码时, 你就会发现指令做的事情竟然这么简单! 而且看上去还有点蠢, 你随手写一个for循环都要比这段C代码看上去更高级.

不过你也不妨站在计算机的角度来理解一下, 计算机究竟是怎么通过这种既简单又笨拙的方式来计算`1+2+...+100`的. 这种理解会使你建立"程序如何在计算机上运行"的最初原的认识.

这个全自动的执行过程实在是太美妙了! 事实上, 开拓者图灵在1936年就已经提出[类似的核心思想](https://en.wikipedia.org/wiki/Universal_Turing_machine), "计算机之父"可谓名不虚传. 而这个流传至今的核心思想, 就是"存储程序". 为了表达对图灵的敬仰, 我们也把**上面这个最简单的计算机称为"图灵机"(Turing Machine, TRM)**. 或许你已经听说过"图灵机"这个作为计算模型时的概念, 不过在这里我们只强调**作为一个最简单的真实计算机需要满足哪些条件:**

- 结构上, **TRM有存储器, 有PC, 有寄存器, 有加法器**
- 工作方式上, TRM不断地重复以下过程: **从PC指示的存储器位置取出指令, 执行指令, 然后更新PC**

咦? 存储器, 计数器, 寄存器, 加法器, 这些不都是数字电路课上学习过的部件吗? 也许你会觉得难以置信, 但先驱说, 你正在面对着的那台无所不能的计算机, 就是由数字电路组成的! 不过, 我们在程序设计课上写的程序是C代码. 但如果计算机真的是个只能懂0和1的巨大数字电路, 这个冷冰冰的电路又是如何理解凝结了人类智慧结晶的C代码的呢? 先驱说, 计算机诞生的那些年还没有C语言, 大家都是直接编写对人类来说晦涩难懂的机器指令, 那是他所见过的最早的对电子计算机的编程方式了. 后来人们发明了**高级语言和编译器, 能把我们写的高级语言代码进行各种处理, 最后生成功能等价的, CPU能理解的指令**. **CPU执行这些指令, 就相当于是执行了我们写的代码**. 今天的计算机本质上还是"存储程序"这种天然愚钝的工作方式, 是经过了无数计算机科学家们的努力, 我们今天才可以轻松地使用计算机.

##### 计算机是个状态机

既然计算机是一个数组逻辑电路, 那么我们可以把计算机划分成两部分, 一部分由所有时序逻辑部件(存储器, 计数器, 寄存器)构成, 另一部分则是剩余的组合逻辑部件(如加法器等). 这样以后, 我们就可以从状态机模型的视角来理解计算机的工作过程了: 在每个时钟周期到来的时候, 计算机根据当前时序逻辑部件的状态, 在组合逻辑部件的作用下, 计算出并转移到下一时钟周期的新状态.

计算机的这个视角有什么用呢? 好像除了让你明白计算机硬件不再那么神秘之外, 也没什么特别的用处. 毕竟ICS课不要求大家用硬件描述语言来实现计算机硬件, 大家只要相信这件事能做成就可以了.

不过对于程序来说, 这个视角的作用会超乎你的想象.

### 重新认识程序: 程序是个状态机

如果把计算机看成一个状态机, 那么运行在计算机上面的程序又是什么呢?

我们知道程序是由指令构成的, 那么我们先看看一条指令在状态机的模型里面是什么. 不难理解, 计算机正是**通过执行指令的方式来改变自身状态的**, 比如执行一条加法指令, 就可以把两个寄存器的值相加, 然后把结果更新到第三个寄存器中; 如果执行一条跳转指令, **就会直接修改PC的值, 使得计算机从新PC的位置开始执行新的指令.** 所以在状态机模型里面, 指令可以看成是计算机**进行一次状态转移的输入激励**.

ICS课本的1.1.3小节中介绍了一个很简单的计算机. 这个计算机有4个8位的寄存器, 一个4位PC, 以及一段16字节的内存(也就是存储器), 那么这个计算机可以表示比特总数为`B = 4*8 + 4 + 16*8 = 164`, 因此这个计算机总共可以有`N = 2^B = 2^164`种不同的状态. 假设这个在这个计算机中, 所有指令的行为都是确定的, 那么给定`N`个状态中的任意一个, 其转移之后的新状态也是唯一确定的. 一般来说`N`非常大, 下图展示了`N=50`时某计算机的状态转移图.

![state-machine](./NJUPA%E9%9A%8F%E8%AE%B01/state-machine.png)

现在我们就可以通过状态机的视角来解释"程序在计算机上运行"的本质了: 给定一个程序, **把它放到计算机的内存中, 就相当于在状态数量为`N`的状态转移图中指定了一个初始状态**, 程序**运行的过程就是从这个初始状态开始, 每执行完一条指令, 就会进行一次确定的状态转移**. 也就是说, **程序也可以看成一个状态机**! 这个状态机是上文提到的**大状态机(状态数量为`N`)的子集.**

例如, 假设某程序在上图所示的计算机中运行, 其初始状态为左上角的8号状态, 那么这个程序对应的状态机为

```
8->1->32->31->32->31->...
```

这个程序可能是:

```
// PC: instruction    | // label: statement
0: addi r1, r2, 2     |  pc0: r1 = r2 + 2;
1: subi r2, r1, 1     |  pc1: r2 = r1 - 1;
2: nop                |  pc2: ;  // no operation
3: jmp 2              |  pc3: goto pc2;
```

#####  从状态机视角理解程序运行

以上一小节中`1+2+...+100`的指令序列为例, 尝试画出这个程序的状态机.

这个程序比较简单, 需要更新的状态只包括`PC`和`r1`, `r2`这两个寄存器, 因此我们用一个三元组`(PC, r1, r2)`就可以表示程序的所有状态, 而无需画出内存的具体状态. 初始状态是`(0, x, x)`, 此处的`x`表示未初始化. 程序`PC=0`处的指令是`mov r1, 0`, **执行完之后`PC`会指向下一条指令, 因此下一个状态是`(1, 0, x)`**. 如此类推, 我们可以画出执行前3条指令的状态转移过程:

```
(0, x, x) -> (1, 0, x) -> (2, 0, 0) -> (3, 0, 1)
```

请你尝试继续画出这个状态机, 其中程序中的循环只需要画出前两次循环和最后两次循环即可.

通过上面必做题的例子, 你应该更进一步体会到"程序是如何在计算机上运行"了. 我们其实可以从两个互补的视角来看待同一个程序:

- 一个是以代码(或指令序列)为表现形式的静态视角, 大家经常说的"写程序"/"看代码", 其实说的都是这个**静态视角**. 这个视角的一个好处是描述精简, 分支, 循环和函数调用的组合使得我们可以通过少量代码实现出很复杂的功能. 但这也可能会使得我们对程序行为的理解造成困难.
- 另一个是以状态机的状态转移为运行效果的动态视角, 它直接刻画了"程序在计算机上运行"的本质. 但这一视角的状态数量非常巨大, 程序代码中的所有循环和函数调用都以指令的粒度被完全展开, 使得我们**难以掌握程序的整体语义**. 但对于程序的局部行为, 尤其是从静态视角来看难以理解的行为, 状态机视角可以让我们清楚地了解相应的细节.

##### 程序的状态机视角有什么好处?

有一些程序**看上去很简单, 但行为却不那么直观, 比如递归**. 要很好地理解递归程序在计算机上如何运行, 从状态机视角来看程序行为才是最有效的做法, 因为这一视角可以帮助你理清每一条指令究竟如何修改计算机的状态, 从而实现宏观上的递归语义.

#####  "程序在计算机上运行"的微观视角: 程序是个状态机

"程序是个状态机"这一视角对ICS和PA来说都是非常重要的, 因为"理解程序如何在计算机上运行"就是ICS和PA的根本目标. 至于这个问题的宏观视角, 我们将会在PA的中期来介绍.

这一小节的文字对你来说应该不难理解, 但如果你将来没有养成**从状态机视角理解程序行为**的意识, 你可能会感到PA非常困难, 因为在PA中你需要不断地和代码打交道. 如果你不能从微观视角理解某些关键代码的行为, 你也无法从宏观视角完全弄清楚程序究竟是如何运行的.

## RTFSC

事实上, TRM的实现是如此的简单, 以至于框架代码已经实现它了. 接下来让我们看看, 构成TRM的那些数字电路, 在NEMU的C代码中都是何方神圣. 为了方便叙述, 我们将在NEMU中模拟的计算机称为"客户(guest)计算机", 在NEMU中运行的程序称为"客户程序".

### 框架代码初探

框架代码内容众多, 其中包含了很多在后续阶段中才使用的代码. 随着实验进度的推进, 我们会逐**渐解释所有的代码**. 因此在**阅读代码的时候, 你只需要关心和当前进度相关的模块**就可以了, 不要纠缠于和当前进度无关的代码, 否则将会给你的心灵带来不必要的恐惧.

```
ics2025
├── abstract-machine   # 抽象计算机
├── am-kernels         # 基于抽象计算机开发的应用程序
├── fceux-am           # 红白机模拟器
├── init.sh            # 初始化脚本
├── Makefile           # 用于工程打包提交
├── nemu               # NEMU
└── README.md
```

目前我们只需要关心NEMU子项目中的内容, 其它子项目会在将来进行介绍. **NEMU主要由4个模块构成: monitor, CPU, memory, 设备**. 我们已经在上一小节简单介绍了CPU和memory的功能, 设备会在PA2中介绍, 目前不必关心.

Monitor(监视器)模块是为了**方便地监控客户计算机的运行状态而引入的**. 它除了负责**与GNU/Linux进行交互(例如读入客户程序)之外, 还带有调试器的功能**, 为NEMU的调试提供了方便的途径. **从概念上来说, monitor并不属于一个计算机的必要组成部分**, 但对NEMU来说, 它是必要的基础设施. 如果缺少monitor模块, 对NEMU的调试将会变得十分困难.

代码中`nemu/`目录下的源文件组织如下(并未列出所有文件):

```
nemu
├── configs                    # 预先提供的一些配置文件
├── include                    # 存放全局使用的头文件
│   ├── common.h               # 公用的头文件
│   ├── config                 # 配置系统生成的头文件, 用于维护配置选项更新的时间戳
│   ├── cpu
│   │   ├── cpu.h
│   │   ├── decode.h           # 译码相关
│   │   ├── difftest.h
│   │   └── ifetch.h           # 取指相关
│   ├── debug.h                # 一些方便调试用的宏
│   ├── device                 # 设备相关
│   ├── difftest-def.h
│   ├── generated
│   │   └── autoconf.h         # 配置系统生成的头文件, 用于根据配置信息定义相关的宏
│   ├── isa.h                  # ISA相关
│   ├── macro.h                # 一些方便的宏定义
│   ├── memory                 # 访问内存相关
│   └── utils.h
├── Kconfig                    # 配置信息管理的规则
├── Makefile                   # Makefile构建脚本
├── README.md
├── resource                   # 一些辅助资源
├── scripts                    # Makefile构建脚本
│   ├── build.mk
│   ├── config.mk
│   ├── git.mk                 # git版本控制相关
│   └── native.mk
├── src                        # 源文件
│   ├── cpu
│   │   └── cpu-exec.c         # 指令执行的主循环
│   ├── device                 # 设备相关
│   ├── engine
│   │   └── interpreter        # 解释器的实现
│   ├── filelist.mk
│   ├── isa                    # ISA相关的实现
│   │   ├── mips32
│   │   ├── riscv32
│   │   ├── riscv64
│   │   └── x86
│   ├── memory                 # 内存访问的实现
│   ├── monitor
│   │   ├── monitor.c
│   │   └── sdb                # 简易调试器
│   │       ├── expr.c         # 表达式求值的实现
│   │       ├── sdb.c          # 简易调试器的命令处理
│   │       └── watchpoint.c   # 监视点的实现
│   ├── nemu-main.c            # 你知道的...
│   └── utils                  # 一些公共的功能
│       ├── log.c              # 日志文件相关
│       ├── rand.c
│       ├── state.c
│       └── timer.c
└── tools                      # 一些工具
    ├── fixdep                 # 依赖修复, 配合配置系统进行使用
    ├── gen-expr
    ├── kconfig                # 配置系统
    ├── kvm-diff
    ├── qemu-diff
    └── spike-diff
```

为了支持不同的ISA, 框架代码把NEMU分成两部分: **ISA无关的基本框架和ISA相关的具体实现**. NEMU把**ISA相关的代码专门放在`nemu/src/isa/`目录**下, 并通过`nemu/include/isa.h`**提供ISA相关API的声明**. 这样以后, `nemu/src/isa/`之外的其它代码就展示了NEMU的基本框架. 这样做有两点好处:

- 有助于我们认识不同ISA的共同点: 无论是哪种ISA的客户计算机, 它们**都具有相同的基本框架**
- 体现抽象的思想: 框架代码**将ISA之间的差异抽象成API**, 基本框架会**调用这些API, 从而无需关心ISA的具体细节**. 如果你将来打算选择一个不同的ISA来进行二周目的攻略, 你就能明显体会到**抽象的好处了: 基本框架的代码完全不用修改!**

[这个页面](https://nju-projectn.github.io/ics-pa-gitbook/ics2025/nemu-isa-api.html)对上述API进行了整理, 供将来查阅使用, 目前你无需完全明白它们的作用. "抽象"是计算机系统中一个非常重要的概念, 如果你现在不明白抽象的意义, 不必担心, 在PA的后续内容中, 你会一次又一次地遇到它.

大致了解上述的目录树之后, 你就可以开始阅读代码了. 至于从哪里开始, 就不用多费口舌了吧.