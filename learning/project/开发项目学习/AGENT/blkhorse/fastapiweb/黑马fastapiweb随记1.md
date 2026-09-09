# FastAPI 七份示例代码讲解

本文档按文件顺序讲解 `code` 目录下的 7 份示例代码，重点说明：

- 每段代码在做什么
- 关键参数的含义
- 参数约束在运行时如何生效

---

## 1）`01-路由.py`：最基础的路由定义

### 代码作用

该文件演示 FastAPI 的基础用法：

- 创建应用实例
- **声明两个 GET 接口**
- 返回 JSON 响应

### 关键代码与参数说明

1. `app = FastAPI()`

- `FastAPI()`：创建**一个 FastAPI 应用对象。**
- `app`：后续**用于注册路由（如 `@app.get(...)`）。**

2. `@app.get("/")`

- `@app.get`：**声明**一个 **GET** 请求接口。
- `"/"`：路由路径，表示根路径。

`@` 是 Python 的**装饰器语法（decorator）**。装饰器**本质上是一个可调用对象**，**接收一个函数并返回一个函数（可以是原函数或包装后的函数）**，用于**给函数“增加行为”或登记元信息。**

在你看到的 [@app.get("/")](vscode-file://vscode-app/e:/Microsoft VS Code/10c8e557c8/resources/app/out/vs/code/electron-browser/workbench/workbench.html) 中，执行顺序相当于先**调用 [app.get("/")](vscode-file://vscode-app/e:/Microsoft VS Code/10c8e557c8/resources/app/out/vs/code/electron-browser/workbench/workbench.html) 得到一个装饰器**，然后**把下面定义的函数传给它**，等价于：
[root = app.get("/")(root)](vscode-file://vscode-app/e:/Microsoft VS Code/10c8e557c8/resources/app/out/vs/code/electron-browser/workbench/workbench.html)

在 FastAPI 里，[app.get("/")](vscode-file://vscode-app/e:/Microsoft VS Code/10c8e557c8/resources/app/out/vs/code/electron-browser/workbench/workbench.html) 返回的就是**一个用于注册路由的装饰器**——它会**把该函数注册为处理 GET 请求、路径为 `/` 的端点**，并把**相应的元信息加入到应用和 OpenAPI 文档中。**

**再为我详细解释一下这里面的逻辑传递**

1. 装饰器（`@`）在定义时发生什么

- 语法行为：Python 在遇到
	@app.get("/")
	async def root(): ...
	时，**等价于：**
	**def root(...): ...**
	**root = app.get("/")(root)**
- 也就是说先调用 app.get("/")（**得到一个“装饰器函数”**），然**后把定义好的函数对象作为参数传入该装饰器**。FastAPI 的 app.get(...) **返回的装饰器会把这个函数注册为处理 GET / 的端点**，然后**通常返回原函数（函数引用不变）**。

1. 定义阶段（模块加载）— 注册路由的具体动作（简化）

- 当你的模块被 Python 解释器执行（导入或作为主程序运行）时，装饰器被执行：FastAPI 内部会**调用类似 APIRouter.add_api_route(path, endpoint, methods=["GET"], ...) 的逻辑，把路由信息写入应用的路由表（例如 app.router.routes）。**
- 同时，FastAPI 会**记录函数签名、类型注解、docstring、response_model 等元信息，**用于**后续参数解析、响应校验和 OpenAPI 文档生成。**

1. 请求到达时的总体执行顺序（简化步骤）

- 1. **HTTP 服务器（如 uvicorn）接收请求并构造** ASGI scope，然后调用 FastAPI 应用的 ASGI callable。

- 1. **路由匹配（Starlette 的 Router）：根据 path + method 找到对应的 Route，得到 endpoint（你的 root）和 path_params。**

- 1. **中间件（middleware）按注册顺序被运行**（在**调用 endpoint 之前/之后可做处理**，如 **CORS、日志、请求体压缩**等）。

- 1. 参数解析与依赖注入：FastAPI **根据 endpoint 的函数签名决定如何获取每个参数（来自 path、query、body、header、cookie、Depends、Request 等）**，并对类型进行转换与校验（若有 Path/Query/Body 等约束会生效）。若校验失败，会直接返回 422 错误并终止调用。

- 1. 调用 endpoint：

	- 若 endpoint **是 async def，则直接 await endpoint(...)；**
	- 若是**同步 def，则 FastAPI 会把它提交到线程池（run_in_threadpool）执行**，**避免阻塞事件循环。**

- 1. endpoint 返回值：若返回 Starlette/FastAPI 的 Response 实例（如 JSONResponse、HTMLResponse、FileResponse），则直接使用；若返回普通 dict/list/Pydantic model，FastAPI 会把它**序列化为 JSONResponse（并在需要时根据 response_model 进行校验/过滤）**。

- 1. Response **通过 ASGI send 发回客户端；中间件的“后置”逻辑随后运行（如记录响应时间）。**

1. 针对 [01-路由.py](vscode-file://vscode-app/e:/Microsoft VS Code/10c8e557c8/resources/app/out/vs/code/electron-browser/workbench/workbench.html) 的具体逻辑传递（最简路径）

- 请求 GET / 到达 → 路由**匹配到你注册的根路由** → **无需参数解析（函数没有参数）** → **await root() 执行 → root 返回 Python dict** → FastAPI **把 dict 转为 JSONResponse（Content-Type: application/json）**并**返回 HTTP 200 与对应 body {"message":"Hello World888"}。**

1. 参数解析与校验要点（虽此文件未用到，但**通用逻辑**）

- Path/Query/Body 等声明会被 FastAPI 在**调用前解析并验证（类型转换、长度、范围等）；失败返回 422。**
- Pydantic 用于复杂请求体与 response_model 的校验与序列化。
- Depends 提供可组合的依赖注入（可嵌套、可声明 scope、可用作权限/上下文注入）。

1. 异步 vs 同步函数差异（实际影响）

- async def：在事件循环中**以协程方式**运行，适合**非阻塞 I/O（数据库异步客户端、httpx async 等）。**
- def（同步）：若直接在请求处理线程运行会阻塞事件循环，FastAPI 会把其放到线程池运行以保护事件循环。
- 结论：IO 密集优先使用 async，CPU 密集任务考虑后台任务或专用 worker。

1. 返回值处理细节

- 返回 dict/list/Pydantic → **自动序列化成 JSONResponse。**
- 返回 Response 子类（如 HTMLResponse, FileResponse）→ FastAPI 不再二次转换，**直接使用用户构造的 Response（因此可以控制 headers、content-type、status_code）。**
- 若使用 response_model，FastAPI 会对返回数据进行校验与字段过滤，保证对外契约一致。

1. 与 OpenAPI/Swagger 的关系

- 装饰器注册时（基于函数签名和注解）会让 FastAPI 收集元信息用于生成 OpenAPI 文档，开发者可在浏览器访问 /docs 或 /redoc 查看。

1. 简单可观察/调试方法

- 查看已经注册的路由：在交互或启动脚本里打印 `print(app.routes)` 或 `for r in app.routes: print(r.path, r.methods)`。
- 启动示例（假设你的模块能被正确作为 Python 模块导入）：

- 测试请求（启动后）：

总结一句话：[@app.get("/")](vscode-file://vscode-app/e:/Microsoft VS Code/10c8e557c8/resources/app/out/vs/code/electron-browser/workbench/workbench.html) 在文件加载时**把 [root](vscode-file://vscode-app/e:/Microsoft VS Code/10c8e557c8/resources/app/out/vs/code/electron-browser/workbench/workbench.html) 函数“登记”为处理 GET / 的端点**；在**请求到达时，FastAPI/Starlette 会根据路由表匹配、解析参数/依赖、执行函数（await 或线程池）、然后将返回值转换成 HTTP 响应发回客户端。**

3. `async def root():`

- `async`：异步函数，**适合 I/O 场景（如数据库、网络请求）**。
- `root`：函数名（可自定义），对应处理逻辑。

4. `return {"message": "Hello World888"}`

- **返回 Python 字典，FastAPI 会自动序列化成 JSON。**
- 响应体示例：`{"message": "Hello World888"}`。

5. `@app.get("/hello")` 与 `get_hello`

- 定义第二个 GET 接口，路径为 `/hello`。
- 返回 `{"msg": "你好 FastAPI"}`。

### 总结

本文件核心是：**路由装饰器（`@app.get`） + 处理函数 + JSON 返回值**。

---

## 2）`02-路径参数.py`：路径参数与约束

### 代码作用

该文件演示如何在 URL 路径中接收参数，并通过 `Path(...)` 添加校验规则。

### 关键代码与参数说明

1. `from fastapi import FastAPI, Path`

- `Path`：用于**声明“路径参数”的校验与元信息。**

2. `@app.get("/book/{id}")`

- `{id}`：路径参数**占位符**，请求如 `/book/10`。
- FastAPI 会将 **URL 中的值映射到函数参数 `id`**。

3. `id: int = Path(..., gt=0, lt=101, description="书籍id，取值范围1-100")`

- `id: int`：将参数声明为整数；若传入非整数会触发校验错误。
- `Path(...)`：
    - `...`：表示该参数是**必填（路径参数本质上也必填，这里是显式声明）**。
    - `gt=0`：**必须大于 0**。
    - `lt=101`：**必须小于 101。**
    - `description`：用于 OpenAPI 文档说明（Swagger UI 可见）。

4. `@app.get("/author/{name}")`

- 定义作者信息接口，**路径参数为 `name`。**

5. `name: str = Path(..., min_length=2, max_length=10)`

- `name: str`：参数类**型为字符串。**
- `min_length=2`：**最短 2 个字符。**
- `max_length=10`：**最长 10 个字符。**

### 总结

本文件核心是：**路径参数类型声明 + `Path` 约束校验**。当参数不满足约束时，FastAPI 会自动返回 422 错误并给出原因。

---

## 3）`03-查询参数.py`：查询参数（Query）分页示例

### 代码作用

该文件演示通过 URL 查询字符串接收分页参数，例如：

- `/news/news_list?skip=0&limit=10`

### 关键代码与参数说明

1. `from fastapi import FastAPI, Query`

- `Query`：用于声明**查询参数的默认值、校验规则、文档信息。**

2. `@app.get("/news/news_list")`

- 定义新闻列表**接口（GET）。**

3. `skip: int = Query(0, description="跳过的记录数", lt=100)`

- `skip: int`：跳过**条数，整型。** 
- `Query(0, ...)`：
    - `0`：默认值。若**请求未传 `skip`，则取 0。**
    - `description`：**接口文档说明。**
    - `lt=100`：必须**小于 100。**

4. `limit: int = Query(10, description="返回的记录数")`

- `limit: int`：每页返回条数。
- `10`：默认返回 10 条。
- 未设置大小约束，理论上可传任意整数（建议真实项目补充 `gt`、`le` 等限制）。

5. 返回值 `{"skip": skip, "limit": limit}`

- 用于回显分页参数，便于验证参数解析是否正确。

### 总结

本文件核心是：**通过 `Query` 定义查询参数默认值和校验规则**，非常适合列表分页场景。

---

## 4）`04-请求体参数.py`：请求体模型（Pydantic）

### 代码作用

该文件演示如何**使用 Pydantic 模型接收 POST 请求体**，并对字段**做约束校验。**

### 关键代码与参数说明

1. `from pydantic import BaseModel, Field`

- `BaseModel`：定义数据模型。
- `Field`：定义字段默认值、长度限制、描述等。

2. `class User(BaseModel):`

- 定义**注册数据结构，包含 `username` 和 `password`。**

3. `username: str = Field(default="张三", min_length=2, max_length=10, description="用户名，长度要求2-10个字")`

- `default="张三"`：默认值。如果请求体缺少该字段，将使用默认值。
- `min_length=2`：最短 2 个字符。
- `max_length=10`：最长 10 个字符。
- `description`：用于接口文档描述。

4. `password: str = Field(min_length=3, max_length=20)`

- 没有默认值，因此是必填字段。
- 最短 3 个字符，最长 20 个字符。

5. `@app.post("/register")`

- 注册接口，接收 POST 请求。

6. `async def register(user: User):`

- `user: User`：将**请求体 JSON 自动解析为 `User` 模型对象。**
- FastAPI 会先做字段校验，通过后才进入函数逻辑。

7. `return user`

- **直接返回模型对象，FastAPI 会自动序列化为 JSON。**

### 总结

本文件核心是：**Pydantic 模型定义请求体 + `Field` 细粒度校验**，可显著减少手写参数校验代码。

---

## 5）`05-响应类型-HTML格式.py`：返回 HTML

### 代码作用

该文件演示如何让接口返回 HTML 内容，而不是默认 JSON。

### 关键代码与参数说明

1. `from fastapi.responses import HTMLResponse`

- `HTMLResponse`：用于**构造 `text/html` 类型响应。**

2. `@app.get("/html", response_class=HTMLResponse)`

- `response_class=HTMLResponse`：
    - 指定该路由的响应类型为 HTML。
    - 响应头 `Content-Type` 会是 `text/html; charset=utf-8`（**由框架处理**）。

3. `return "<h1>这是一级标题</h1>"`

- 返回字符串形式的 HTML 片段。
- 浏览器访问时会**按 HTML 渲染，而不是显示纯文本 JSON。**

### 总结

本文件核心是：**通过 `response_class` 明确响应格式**。这在返回页面片段、简单模板结果时很实用。

---

## 6）`06-响应类型-文件格式.py`：返回文件（图片）

### 代码作用

该文件演示接口如何返回本地文件内容（示例中为图片）。

### 关键代码与参数说明

1. `from fastapi.responses import FileResponse`

- `FileResponse`：将文件作为 HTTP 响应返回。

2. `@app.get("/file")`

- 定义文件下载/查看接口。

3. `path = "./files/1.jpeg"`

- 文件相对路径。
- 相对路径是相对于“启动进程的当前工作目录”，实际运行时需保证路径能正确找到文件。

4. `return FileResponse(path)`

- 使用 `FileResponse` 返回该文件。
- 常见用途：下载文档、返回图片、返回静态资源。

### 总结

本文件核心是：**`FileResponse` 用于直接回传文件流**。实际项目中建议增加文件存在性判断与异常处理。

---

## 7）`07-自定义响应数据格式.py`：响应模型（response_model）

### 代码作用

该文件演示如何**给接口定义统一响应结构，保证返回字段和类型符合预期。**

### 关键代码与参数说明

1. `class News(BaseModel):`

- 定义响应数据模型，包含：
    - `id: int`
    - `title: str`
    - `content: str`

2. `@app.get("/news/{id}", response_model=News)`

- `response_model=News`：
    - 声明该接口的**响应结构必须符合 `News`。**
    - FastAPI 会**按模型进行序列化与校验。**
    - 若返回数据含有多余字段，默认会被过滤（提高响应一致性）。

3. `async def get_news(id: int):`

- `id: int`：路径参数，**自动做整型转换与校验。**

4. 返回字典

- 返回的 `id/title/content` **与 `News` 模型字段对应。**
- 因设置了 `response_model`，接口文档中会**清晰展示响应结构。**

### 总结

本文件核心是：**通过 `response_model` 约束响应格式**，让前后端对字段契约更清晰、更稳定。

---

## 总体学习收获

这 7 份代码按学习路径覆盖了 FastAPI 的常用基础能力：

1. 基础路由**注册**（`@app.get`）
2. **路径参数校验**（`Path`）
3. **查询参数校验**（`Query`）
4. **请求体模型与字段约束**（`BaseModel` + `Field`）
5. **自定义响应类型**（`HTMLResponse`）
6. **文件响应**（`FileResponse`）
7. **响应结构约束**（`response_model`）

如果把这 7 个点掌握扎实，已经能搭建一个结构清晰、参数校验规范、文档友好的 FastAPI 入门 API 服务。