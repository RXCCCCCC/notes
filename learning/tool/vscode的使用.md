# vscode的使用

## ps:

- 在设置,右上角有**打开设置json文件**的按钮
- 左栏即有在当前文件夹下搜索所有文件内容的功能

## 我的快捷键

- **ctrl+shift+p 打开命令面板**
- **Ctrl + Shift + ` 新建终端**
- ctrl+shift+w 关闭当前vscode窗口
- ctrl+p 快速搜索文件
- **ctrl+alt+i 唤出gpt**
- ctrl+f 查找
- **shift+alt+f 格式化代码**
- **Shift + Alt + 向下箭头 ** **快速复制到下一行**
- **shift+alt+a 多行注释**
- ctrl+g 转到行号
- Ctrl + Shift + E 显示资源管理器
- **Ctrl + Shift + B 切换侧边栏可见性** 

## 关于活动栏

### 搜索（Search）

搜索功能允许你在整个项目中查找特定的文本或代码片段,点击搜索图标（放大镜）后，你可以输入关键词，VS Code 会在项目中匹配并显示所有相关结果。

![img](./vscode%E7%9A%84%E4%BD%BF%E7%94%A8/972768c9-ca4d-4473-a084-d2722ac832cb.png)

### 源代码管理（Source Control）

源代码管理功能集成了 Git，允许你在 VS Code 中直接进行版本控制操作，我们可以查看文件的更改状态、提交代码、创建分支等。

## 自定义活动栏

VS Code 允许用户自定义活动栏的显示内容，可以右击状态栏，控制功能图标是否显示，打勾☑️的表示显示，反之，不显示：

![img](./vscode%E7%9A%84%E4%BD%BF%E7%94%A8/dfd659a4-1b03-434b-b724-da26151516e0.png)

**终端快捷键：**

| 功能         | Windows/Linux          | macOS                 |
| :----------- | :--------------------- | :-------------------- |
| 显示集成终端 | Ctrl + `               | Ctrl + `              |
| 新建终端     | Ctrl+Shift+`           | Cmd+Shift+`           |
| 切换终端     | `Ctrl+PageUp/PageDown` | `Cmd+PageUp/PageDown` |
| 关闭终端     | `Ctrl+Shift+W`         | `Cmd+Shift+W`         |

根据操作系统的不同，我们可以选择不同的终端环境，例如 PowerShell、命令提示符（Command Prompt）、或 Bash。

![img](./vscode%E7%9A%84%E4%BD%BF%E7%94%A8/vscode-panel.png)

## VSCode 命令面板

VSCode（Visual Studio Code）的命令面板是一个非常强大的工具，它允许用户快速访问和执行 VSCode中 的各种命令和功能



## 关于插件

### copliot

您有多种选项可以运行提示文件

- 在聊天视图中，在聊天输入字段中键入 `/`，然后是提示文件名。

	此选项使您可以在聊天输入字段中传递附加信息。例如，`/create-react-form` 或 `/create-react-form: formName=MyForm`。

- 从命令面板（Ctrl+Shift+P）运行 **Chat: Run Prompt** 命令，并从快速选择中选择一个提示文件。

- 在编辑器中打开提示文件，然后按编辑器标题区域中的播放按钮。您可以选择在当前聊天会话中运行提示或打开新的聊天会话。

	此选项对于快速测试和迭代提示文件非常有用。

	此处有多种生成提示辅助功能,产生提示可一键分析当前仓库

![Screenshot showing the Chat view, and Configure Chat menu, highlighting the Configure Chat button.](./vscode%E7%9A%84%E4%BD%BF%E7%94%A8/configure-chat-instructions.png)