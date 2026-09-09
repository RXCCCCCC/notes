# 常用命令

为了方便你快速上手，我将 `uv` 的常用命令按照功能逻辑进行了分类。`uv` 的设计理念是“一个工具搞定一切”，所以它的命令非常直观。

------

### 1. 项目管理（现代推荐工作流）

这组命令围绕 `pyproject.toml` 和 `uv.lock` 展开，适合管理你自己的开发项目。

- **初始化项目：**

	```
	uv init my-project  # 创建一个新项目文件夹
	uv init .           # 在当前目录初始化
	```

- **添加/删除依赖：**

	```
	uv add requests          # 安装并添加到 pyproject.toml
	uv add numpy --dev       # 添加为开发依赖（仅开发环境使用）
	uv remove pandas         # 移除依赖
	```

- **同步环境：**

	```
	uv sync                  # 根据 lock 文件自动安装/更新所有依赖
	```

- **运行代码：**

	```
	uv run main.py           # 自动在虚拟环境中运行脚本（无需手动激活环境）
	uv run python            # 进入带环境的交互式 REPL
	```

------

### 2. Python 版本管理

`uv` 可以像 `pyenv` 一样管理系统中的 Python 版本，甚至能自动为你下载。

- **列出/安装 Python 版本：**

	```
	uv python list           # 查看已安装和可下载的版本
	uv python install 3.12   # 下载并安装特定版本
	```

- **在当前项目切换版本：**

	Bash

	```
	uv python pin 3.11       # 锁定当前项目使用的 Python 版本
	```

------

### 3. 工具运行 (`uvx`)

如果你只想运行一个 Python 工具（比如代码格式化工具、静态分析工具），而不想把它安装到当前项目的依赖里，用 `uvx`（等同于 `uv tool run`）。

- **临时运行工具：**

	```
	uvx ruff check .         # 运行代码检查
	uvx black .              # 运行代码格式化
	uvx cowsay hello         # 运行有趣的命令行工具
	```

------

### 4. 虚拟环境管理（传统/手动模式）

如果你更喜欢类似 `venv` 和 `pip` 的操作习惯：

- **创建虚拟环境：**

	```
	uv venv                  # 默认在 .venv 创建
	uv venv --python 3.10    # 指定 Python 版本创建
	```

- **类似 pip 的安装：**

	```
	uv pip install flask     # 极速安装
	uv pip list              # 查看已安装的包
	uv pip freeze > req.txt  # 导出依赖
	```

------

### 5. 编译与导出

处理 `requirements.txt` 相关操作：

- **编译依赖：**

	```
	uv pip compile requirements.in -o requirements.txt  # 将输入文件转为锁定版本文件
	```

- **导出项目依赖：**

	```
	uv export --format requirements-txt > requirements.txt
	```

------

### 6. 自我维护

- **更新 uv：**

	```
	uv self update
	```

- **清理缓存：**

	```
	uv cache clean           # 如果遇到奇怪的安装问题，可以尝试清理
	```

------

### 💡 核心小贴士

1. **忘记“激活”：** 尽量养成使用 `uv run` 的习惯，它可以确保你永远运行在正确的环境下，省去了 `source .venv/bin/activate` 的麻烦。
2. **极速安装：** 如果你觉得 `pip install` 慢，在任何地方直接换成 `uv pip install` 都会有惊喜。
3. **深度学习建议：** 处理大型包（如 `torch`、`tensorflow`）时，`uv` 的并行下载和硬链接（Hard Link）机制能节省大量的安装时间和磁盘空间。

你目前常用的 Python 开发场景主要是数据处理、Web 开发，还是更偏向于算法研究？