# 代码讲解（FastAPI Stage2）

> 说明：本文件按“每个源码文件一份讲解”的方式整理，覆盖各文件中的主要对象、函数、参数与作用。

---

## 09-中间件.py

### 文件作用
该文件演示 FastAPI 中 **HTTP 中间件** 的基本写法，以及中间件与普通路由的执行关系。

### 关键代码与参数说明

#### 1) `app = FastAPI()`
- 作用：创建 FastAPI **应用实例，后续路由和中间件都挂在 `app` 上。**
- 本文件未给构造函数传额外参数，使用默认行为。

#### 2) `@app.middleware("http")`
- 参数 `"http"`：表示**注册的是 HTTP 请求级别中间件。**
- 被装饰函数**签名：`async def middlewareX(request, call_next)`**
  - `request`：当前**请求对象（包含路径、请求头、方法、body 等）**。
  - `call_next`：继续**向下执行请求链（下一个中间件或最终路由处理函数）的回调。**

#### 3) `middleware2` 与 `middleware1`
- 两个中间件都执行了同样流程：
  1. 打印 `start`
  2. `response = await call_next(request)` 继续向后处理
  3. 打印 `end`
  4. 返回 `response`
- 作用：**展示“前置逻辑 + 后置逻辑”模式（**例如计时、日志、鉴权、统一异常包装）。

#### 4) `@app.get("/")`
- 路由参数 `"/"`：根路径。
- `root()` **返回 `{"message": "Hello World"}`，用于验证中间件链路是否生效。**

### 执行逻辑总结
- 请求进入时会**依次经过中间件前置逻辑**；
- 路由处理完成后，**再按相反方向经过中间件后置逻辑**。

---

## 10-依赖注入.py

### 文件作用
该文件演示 FastAPI 的 **依赖注入（Depends）**，将**分页参数逻辑抽离复用给多个接口。**

### 关键代码与参数说明

#### 1) 导入项
- `Query`：声明查询参数**并附带校验规则。**
- `Depends`：**声明依赖项**，把**公共逻辑注入到路由函数。**

#### 2) 依赖函数 `common_parameters`
```python
async def common_parameters(
    skip: int = Query(0, ge=0),
    limit: int = Query(10, le=60)
)
```
- **`skip`：跳过条数（分页偏移量）**
  - **默认值 `0`**
  - **`ge=0`：必须大于等于 0**
- **`limit`：每页条数**
  - **默认值 `10`**
  - **`le=60`：必须小于等于 60**
- **返回值：`{"skip": skip, "limit": limit}`，作为统一分页参数对象。**

#### 3) 路由中的依赖注入
- `commons=Depends(common_parameters)`
  - 含义：**执行 `common_parameters`**，将其**返回值注入给 `commons`。**
- 好处：
  - 参数校验逻辑**只写一次**；
  - 多个路由**共享相同分页规范；**
  - 后续维护时**只需修改一个地方**。

### 执行逻辑总结
访问 `/news/news_list` 或 `/user/user_list` 时，FastAPI 会**先解析并校验 `skip/limit`，再把结果注入路由函数返回。**

## **ORM = Object Relational Mapping，对象关系映射。**

简单说：

> **ORM 是一个把“数据库表”映射成“代码里的对象”的工具。**

比如数据库里有一张表：

| id   | name | age  |
| ---- | ---- | ---- |
| 1    | Tom  | 20   |

不用 ORM 时，你可能要写 SQL：

```
SELECT * FROM user WHERE id = 1;
```

用了 ORM 后，你可以**像操作对象一样操作数据库：**

```
user = User.get(id=1)
print(user.name)
```

这里的 **`User` 类对应数据库里的 `user` 表**；**`user.name` 对应表里的 `name` 字段**。**ORM 负责在对象和数据库表之间转换数据**。AWS 对 ORM 的解释也是：它是一个软件层，用来把对象数据转换到底层数据库，并隐藏部分数据库细节。 Hibernate 官方也把 ORM 描述为让 Java 程序能以更自然、类型安全的方式使用关系型数据。

## 它解决什么问题？

程序员写代码时喜欢用：

```
user.name
user.age
```

数据库保存数据时用的是：

```
user 表
name 字段
age 字段
```

这两套思维不一样。Hibernate 文档把这种差异叫做 **object/relational mismatch**：关系型数据库用表格表示数据，而面向对象语言用对象、继承、关系来表示数据。

ORM 的作用就是**在中间做翻译**。

## ORM 的优点

主要有三个：

1. **少写 SQL**
2. **代码更像正常业务代码**
3. **增删改查更方便**

例如：

```
User.create(name="Tom", age=20)
```

ORM 可能会自动翻译成：

```
INSERT INTO user (name, age) VALUES ('Tom', 20);
```

## ORM 的缺点

ORM 不是万能的。

复杂查询、性能优化、联表、多层嵌套时，ORM **可能会生成低效 SQL**。ORM **抽象层太高**，也可能让你不知道底层到底执行了什么 SQL。维基百科对 ORM 的缺点也提到，高层抽象可能会掩盖实现细节。

## 一句话理解

> **ORM 就是：让你不用直接操作 SQL 表，而是像操作对象一样操作数据库。**

常见 ORM：

| 语言    | ORM                        |
| ------- | -------------------------- |
| Python  | SQLAlchemy、Django ORM     |
| Java    | Hibernate、MyBatis-Plus    |
| Node.js | Prisma、TypeORM、Sequelize |
| PHP     | Laravel Eloquent           |

做项目时可以用 ORM 提高开发效率；但做复杂 SQL、性能优化、CTF 注入分析时，还是要懂原始 SQL。

---

## 11-ORM-建表.py

### 文件作用
该文件演示 **FastAPI + SQLAlchemy 异步 ORM** 的基础搭建：
1. 创建**异步数据库引擎**
2. 定义 ORM 模型
3. 在应用启动时自动建表

### 关键代码与参数说明

#### 1) 数据库连接字符串
```
ASYNC_DATABASE_URL = "mysql+aiomysql://root:123456@localhost:3306/FastAPI_first?charset=utf8"
```

- `mysql+aiomysql`：MySQL 的异步驱动协议。
- `root:123456`：用户名和密码（教学示例；生产环境不建议硬编码）。
- `localhost:3306`：数据库地址和端口。
- `FastAPI_first`：数据库名。
- `charset=utf8`：连接字符集。

#### 2) `create_async_engine(...)`
- `echo=True`：打印 SQL 日志，便于学习与调试。
- `pool_size=10`：连接池保持的活跃连接数。
- `max_overflow=20`：超过 `pool_size` 后允许临时创建的额外连接数。

#### 3) ORM 基类 `Base(DeclarativeBase)`
定义**公共字段：**
- `create_time`
  - **类型 `DateTime`**
  - `insert_default=func.now()`：插入时默认当前时间（数据库端）
  - `default=func.now`：Python 侧**默认值策略**
  - `comment="创建时间"`
- `update_time`
  - 同样**有插入默认值**
  - `onupdate=func.now()`：更新记录时**自动刷新时间**

#### 4) 业务模型 `Book`
- `__tablename__ = "book"`：**映射到 `book` 表。**
- 字段：
  - `id`：**主键**
  - `bookname`：书名，`String(255)`
  - `author`：作者，`String(255)`
  - `price`：价格，`Float`
  - `publisher`：出版社，`String(255)`

#### 5) 建表函数 `create_tables()`
- `async with async_engine.begin() as conn`：开启**异步连接事务上下文。**
- `await conn.run_sync(Base.metadata.create_all)`：用同步元数据 API **在异步连接里执行建表。**

#### 6) 启动事件
```
@app.on_event("startup")
```

- 参数 `"startup"`：应用**启动后触发。**
- 作用：**启动时自动执行建表函数，避免手动建表步骤。**

---

## 12-ORM-路由中使用ORM.py

### 文件作用
在“建表基础”上新增 **数据库会话依赖**，并在路由中执行查询。

### 关键代码与参数说明

#### 1) `AsyncSessionLocal = async_sessionmaker(...)`
- `bind=async_engine`：绑定**前面创建的引擎。**
- `class_=AsyncSession`：指定**异步会话类**。
- `expire_on_commit=False`：提交后**对象不立即过期，避免访问属性时触发额外查询**。

#### 2) 依赖函数 `get_database()`
```python
async def get_database():
    async with AsyncSessionLocal() as session:
        try:
            yield session
            await session.commit()
        except Exception:
            await session.rollback()
            raise
        finally:
            await session.close()
```
- `yield session`：把会**话交给路由。**
- `commit/rollback/finally close`：**统一事务与资源管理。**
- 这是 FastAPI 推荐的**依赖资源管理模式。**

#### 3) 查询接口
```
async def get_book_list(db: AsyncSession = Depends(get_database))
```

- `db: AsyncSession`：类型标注**明确注入对象是异步会话。**
- `Depends(get_database)`：把**依赖返回值注入到 `db`。**
- `await db.execute(select(Book))`：**执行查询语句。**
- `result.scalars().all()`：提取 **ORM 对象列表。**

### 执行逻辑总结
每次请求都会拿到**一个独立会话，成功则提交，异常则回滚，最后关闭连接。**

这行代码：

可以拆成两部分来看：

## 1）`db: AsyncSession`

这是**类型注解**，意思是：

- [db](vscode-file://vscode-app/e:/Microsoft VS Code/10c8e557c8/resources/app/out/vs/code/electron-browser/workbench/workbench.html) 这个**参数，期望是一个 [AsyncSession](vscode-file://vscode-app/e:/Microsoft VS Code/10c8e557c8/resources/app/out/vs/code/electron-browser/workbench/workbench.html)**
- **给编辑器、静态检查工具、阅读代码的人一个提示**

它**不负责创建对象**，只是告诉别人：“这里**拿到的是 SQLAlchemy 的异步数据库会话**”。

------

## 2）`= Depends(get_database)`

这是 FastAPI 的**依赖注入写法**。

意思是：

- [db](vscode-file://vscode-app/e:/Microsoft VS Code/10c8e557c8/resources/app/out/vs/code/electron-browser/workbench/workbench.html) 这个参数**不要用户传**
- 由 FastAPI **自动去执行 [get_database()](vscode-file://vscode-app/e:/Microsoft VS Code/10c8e557c8/resources/app/out/vs/code/electron-browser/workbench/workbench.html)**
- 把 [get_database()](vscode-file://vscode-app/e:/Microsoft VS Code/10c8e557c8/resources/app/out/vs/code/electron-browser/workbench/workbench.html) 产生的**结果，注入到 [db](vscode-file://vscode-app/e:/Microsoft VS Code/10c8e557c8/resources/app/out/vs/code/electron-browser/workbench/workbench.html) 里**

也就是说，**[db](vscode-file://vscode-app/e:/Microsoft VS Code/10c8e557c8/resources/app/out/vs/code/electron-browser/workbench/workbench.html) 不是你手动赋值的，而是 FastAPI 帮你准备好的。**

------

## 这句话的完整含义

合起来就是：

> “这个路由函数**需要一个 [AsyncSession](vscode-file://vscode-app/e:/Microsoft VS Code/10c8e557c8/resources/app/out/vs/code/electron-browser/workbench/workbench.html) 类型的数据库会话**，FastAPI **请你自动调用 [get_database()](vscode-file://vscode-app/e:/Microsoft VS Code/10c8e557c8/resources/app/out/vs/code/electron-browser/workbench/workbench.html) 帮我创建并传进来**。”

------

## 它为什么要这么写

因为数据库连接/会话通常要：

1. 创建
2. 使用
3. 提交或回滚
4. 关闭

如果每个路由都手写这些代码，会很重复，也容易漏掉关闭连接。

所以把它封装成依赖函数：

然后路由里用：

这样就非常干净。

------

## 请求进来时，实际发生了什么

比如你访问：

FastAPI 会按这个流程走：

1. 先发现这个接口依赖 [get_database](vscode-file://vscode-app/e:/Microsoft VS Code/10c8e557c8/resources/app/out/vs/code/electron-browser/workbench/workbench.html)
2. 调用 [get_database()](vscode-file://vscode-app/e:/Microsoft VS Code/10c8e557c8/resources/app/out/vs/code/electron-browser/workbench/workbench.html)
3. 进入 [async with AsyncSessionLocal() as session](vscode-file://vscode-app/e:/Microsoft VS Code/10c8e557c8/resources/app/out/vs/code/electron-browser/workbench/workbench.html)
4. [yield session](vscode-file://vscode-app/e:/Microsoft VS Code/10c8e557c8/resources/app/out/vs/code/electron-browser/workbench/workbench.html) 把 **session 交给路由函数**
5. 路由函数**执行**：
6. 路由执行完后，回到依赖函数后半段
7. 自动 [commit](vscode-file://vscode-app/e:/Microsoft VS Code/10c8e557c8/resources/app/out/vs/code/electron-browser/workbench/workbench.html) / [rollback](vscode-file://vscode-app/e:/Microsoft VS Code/10c8e557c8/resources/app/out/vs/code/electron-browser/workbench/workbench.html) / [close](vscode-file://vscode-app/e:/Microsoft VS Code/10c8e557c8/resources/app/out/vs/code/electron-browser/workbench/workbench.html)

------

## `yield` 为什么能这样用

这里的 [get_database()](vscode-file://vscode-app/e:/Microsoft VS Code/10c8e557c8/resources/app/out/vs/code/electron-browser/workbench/workbench.html) **不是普通函数**，而是**依赖生成器**：

FastAPI 会把 `yield` 前后分成两段理解：

- **`yield` 之前：准备资源**
- **`yield` 之后：清理资源**

这就是为什么它特别适合数据库会话、文件句柄、临时资源这类场景。

------

## 你可以把它理解成伪代码

大概相当于：

只是 FastAPI 帮你自动做了。

------

## 这写法里最容易误解的一点

很多人第一次看到会以为：

是“把 [Depends(get_database)](vscode-file://vscode-app/e:/Microsoft VS Code/10c8e557c8/resources/app/out/vs/code/electron-browser/workbench/workbench.html) 这个**对象赋值给 [db](vscode-file://vscode-app/e:/Microsoft VS Code/10c8e557c8/resources/app/out/vs/code/electron-browser/workbench/workbench.html)”。**

其实**不是**。

[Depends(get_database)](vscode-file://vscode-app/e:/Microsoft VS Code/10c8e557c8/resources/app/out/vs/code/electron-browser/workbench/workbench.html) 只是**一个“标记”，告诉 FastAPI：**

> **“这个参数要由依赖系统来提供。”**

真正**传进去的，是 [get_database()](vscode-file://vscode-app/e:/Microsoft VS Code/10c8e557c8/resources/app/out/vs/code/electron-browser/workbench/workbench.html) 最终 `yield` 出来的那个 [session](vscode-file://vscode-app/e:/Microsoft VS Code/10c8e557c8/resources/app/out/vs/code/electron-browser/workbench/workbench.html)。**

------

## 简单总结

这行代码的意思就是：

- [db: AsyncSession](vscode-file://vscode-app/e:/Microsoft VS Code/10c8e557c8/resources/app/out/vs/code/electron-browser/workbench/workbench.html)：[db](vscode-file://vscode-app/e:/Microsoft VS Code/10c8e557c8/resources/app/out/vs/code/electron-browser/workbench/workbench.html) 是**一个异步数据库会话**
- [Depends(get_database)](vscode-file://vscode-app/e:/Microsoft VS Code/10c8e557c8/resources/app/out/vs/code/electron-browser/workbench/workbench.html)：这个**会话由 FastAPI 自动调用 [get_database()](vscode-file://vscode-app/e:/Microsoft VS Code/10c8e557c8/resources/app/out/vs/code/electron-browser/workbench/workbench.html) 提供**
- [yield session](vscode-file://vscode-app/e:/Microsoft VS Code/10c8e557c8/resources/app/out/vs/code/electron-browser/workbench/workbench.html)：[get_database()](vscode-file://vscode-app/e:/Microsoft VS Code/10c8e557c8/resources/app/out/vs/code/electron-browser/workbench/workbench.html) 负责**创建、交付、清理这个会话**

---

## 13-ORM-数据库操作-查询数据.py

### 文件作用
演示多种“读取数据”方式，尤其是按主键快速查询。

### 关键代码与参数说明

#### 1) 注释中的查询方式
- `result.scalars().all()`：**取全部记录。**
- `result.scalars().first()`：**取第一条记录。**

#### 2) 当前生效代码
```
book = await db.get(Book, 5)
```

- `db.get(模型类, 主键值)`：按**主键查询单条，性能和语义都很直接**。
- 参数 `5`：主键值，示例中固定为 5。
- 若不存在，返回 `None`。

### 使用场景
- `db.get(...)` **适合“已知主键”场景**；
- **复杂条件查询仍建议用 `select(...).where(...)`。**

---

## 14-ORM-数据库操作-查询条件.py

### 文件作用
演示两类**常见条件查询**：
1. 路径参数驱动的**精确查询**
2. **固定条件筛选（如价格区间）**

### 关键代码与参数说明

#### 1) 路径参数查询接口
```
@app.get("/book/get_book/{book_id}")
```

- `book_id: int`：路径参数，**自动转为整数。**
- `select(Book).where(Book.id == book_id)`：按**主键条件查询。**
- `result.scalar_one_or_none()`：
  - **有且仅有一条时返回对象；**
  - 无记录返回 `None`；
  - **多条会报错（有助于保证“唯一性预期”）。**

#### 2) 固定条件查询接口
```
select(Book).where(Book.price >= 200)
```

- 条件参数 `Book.price >= 200`：筛选价格**大于等于 200 的书。**
- `result.scalars().all()`：**返回对象列表。**

---

## 15-ORM-数据库操作-查询条件-模糊&与非&包含.py

### 文件作用
演示 SQLAlchemy 中**常见高级条件：**
- `like` **模糊匹配**
- `& | ~` 逻辑与/或/非
- `in_` 集合**包含**

### 关键代码与参数说明

#### 1) `like("曹_")` / `like("曹%")`
- `_`：匹配**单个字符。**
- `%`：匹配**任意长度字符。**
- 例如作者以“曹”开头可用 `like("曹%")`。

#### 2) 逻辑运算
- `A & B`：同时满足（AND）
- `A | B`：满足其一（OR）
- `~A`：非（NOT）

#### 3) `in_` 包含查询（当前生效）
```python
id_list = [1, 3, 5, 7]
select(Book).where(Book.id.in_(id_list))
```
- `id_list`：**待匹配主键集合**。
- `Book.id.in_(id_list)`：SQL **等价于 `id IN (1,3,5,7)`。**
- 返回**匹配到的全部记录。**

---

## 16-ORM-数据库操作-聚合查询.py

### 文件作用
演示**聚合函数查询（统计类接口）**。

### 关键代码与参数说明

#### 1) 聚合表达式
```
select(func.xxx(Book.price 或 Book.id))
```

- `func.count(Book.id)`：**记录**数量。
- `func.max(Book.price)`：**最大**价格。
- `func.sum(Book.price)`：价格**总和**。
- `func.avg(Book.price)`：价格**平均值（当前使用）**。

#### 2) `result.scalar()`
- 从结果中**取单个标量值（数字）。**
- 聚合查询**通常返回单值，`scalar()` 最简洁。**

### 输出结果
接口返回数值类型（如平均价格），用于报表或概览统计。

---

## 17-ORM-数据库操作-分页查询.py

### 文件作用
演示基于 `page + page_size` 的**标准分页实现。**

### 关键代码与参数说明

#### 1) 路由参数
```python
page: int = 1,
page_size: int = 3
```
- `page`：页码，默认第 1 页。
- `page_size`：**每页条数，默认 3。**

#### 2) 偏移量计算
```
skip = (page - 1) * page_size
```

- 含义：当前页**前面应跳过多少条记录。**

#### 3) SQLAlchemy 分页
- `offset(skip)`：**跳过前 `skip` 条。**
- `limit(page_size)`：最**多取 `page_size` 条。**

### 实际意义
这是最基础、最常见的分页模式，适用于后台列表、管理台数据页等场景。

---

## 18-ORM-数据库操作-新增数据.py

### 文件作用
演示**“请求体 -> Pydantic 模型 -> ORM 对象 -> 入库”**的新增流程。

### 关键代码与参数说明

#### 1) 请求体模型 `BookBase(BaseModel)`
字段：
- `id: int`
- `bookname: str`
- `author: str`
- `price: float`
- `publisher: str`

作用：
- **自动校验请求体数据类型；**
- **生成结构化对象供后续业务使用。**

#### 2) 新增接口
```
async def add_book(book: BookBase, db: AsyncSession = Depends(get_database))
```

- `book`：**请求体解析后的 Pydantic 对象。**
- `db`：**注入的数据库会话。**

#### 3) 数据写入
- `book_obj = Book(**book.__dict__)`：把请求体字段**展开为 ORM 对象。**
- `db.add(book_obj)`：**加入会话。**
- `await db.commit()`：**提交事务，真正写入数据库。**

### 返回值
当前**返回 `book`（请求体内容），可用于回显新增数据。**

---

## 19-ORM-数据库操作-更新数据.py

### 文件作用
演示“**先查后改**”的更新模式，并在**记录不存在时抛出 HTTP 异常。**

### 关键代码与参数说明

#### 1) `BookUpdate(BaseModel)`
包含**可更新字段**：
- `bookname`
- `author`
- `price`
- `publisher`

#### 2) 更新接口参数
```
update_book(book_id: int, data: BookUpdate, db: AsyncSession = Depends(get_database))
```

- `book_id`：路径参数，定位目标记录。
- `data`：请求体新数据。
- `db`：数据库会话。

#### 3) 查找与异常处理
- `db_book = await db.get(Book, book_id)`：**按主键查。**
- 若 `None`：
  - 抛 `HTTPException(status_code=404, detail="查无此书")`
  - **向客户端返回标准 404 错误。**

#### 4) 更新与提交
- 把 `data` 字段**逐一赋给 `db_book`。**
- **`await db.commit()`：提交更新。**
- 返回**更新后的 ORM 对象。**

### 设计价值
- 明确区分“定位参数”和“更新数据”；
- 异常语义清晰，便于前端处理。

---

## 20-ORM-数据库操作-删除数据.py

### 文件作用
演示删除操作的标准流程：**先查、再删、再提交**。

### 关键代码与参数说明

#### 1) 删除接口参数
```
delete_book(book_id: int, db: AsyncSession = Depends(get_database))
```

- `book_id`：待删除记录主键。
- `db`：数据库会话。

#### 2) 删除前检查
- `db_book = await db.get(Book, book_id)`：先确认记录是否存在。
- 若不存在：抛 `HTTPException(404, "查无此书")`。

#### 3) 删除与提交
- `await db.delete(db_book)`：标记删除对象。
- `await db.commit()`：提交事务，执行真实删除。

#### 4) 返回值
- 返回 `{"msg": "删除图书成功"}`，用于前端提示。

---

## 补充说明（跨文件共性）

1. 多个 ORM 教学文件都重复了“引擎、模型、会话依赖”的样板代码，目的是让每个文件都能独立运行、独立讲解。
2. 代码里数据库账号密码是教学写法，真实项目建议放入环境变量并通过配置读取。
3. `get_database()` 中已包含 `commit/rollback/close`，而部分路由里又显式 `commit`，教学上可接受；实际项目建议**统一事务策略，避免重复提交造成语义混淆。**