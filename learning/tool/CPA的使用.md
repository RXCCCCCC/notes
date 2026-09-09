## [手把手带你用上AI神器 - CLIProxyAPI（配置详细解说）](https://lostip.de/blog/975888197/)

```
# 端口号，CLIProxyAPI运行了个HTTP服务器，需要端口号来进行访问
port: 8317

# 远程管理配置，配合EasyCLI或者WebUI来使用
remote-management:
  # 启用远程管理的开关，如果你部署在服务器上
  # 那么需要设置为true，才能使用EasyCLI或者WebUI连接到CLIProxyAPI进行管理
  # 如果只是本地使用API进行管理的，可以保持false不动
  allow-remote: false

  # 如果想使用EasyCLI或者WebUI通过API对CLIProxyAPI进行管理，必须设置Key
  # 如果不设置，视同关闭了API管理功能，就无法使用EasyCLI或者WebUI进行连接了
  # 如果你不需要使用EasyCLI或者WebUI进行管理，可以留空
  secret-key: ""

  # 是否集成WebUI的开关
  # 设置为false，可以通过http://YOUR_SERVER_IP:8317/management.html打开WebUI
  disable-control-panel: false

# 认证文件存放目录，用于存放Gemini CLI、Gemini Web、Qwen Code、Codex的认证文件
# 默认设置，是在你当前账户目录下的.cli-proxy-api文件夹，适配Windows和Linux环境
# 程序首次启动时会自动创建该文件夹
# Windows下默认为C:\Users\你的用户名\.cli-proxy-api
# Linux下默认为/home/你的用户名/.cli-proxy-api
# 如果在Windows环境下使用非默认位置，需要参照这样的格式修改填写"Z:\\CLIProxyAPI\\auths"
auth-dir: "~/.cli-proxy-api"

# 是否在日志中启用Debug信息，默认不启用，需要作者配合排错的时候打开就行
debug: false

# 是否将日志重定向到日志文件中
# 默认启用，日志会保存在程序目录下的logs文件夹中
# 如果关闭的话，会在控制台显示日志
logging-to-file: true

# 开关使用统计，默认启用
# 需要使用API来查看使用量，可以用EeasyCLI或者WebUI来查看
usage-statistics-enabled: true

# 如果你要使用代理，那么需要进行以下的设置，支持socks5/http/https协议
# 按照这样的格式"socks5://user:pass@192.168.1.1:1080/"填写
proxy-url: ""

# 当请求碰到403, 408, 500, 502, 503, 504这些错误码的时候，程序自动重试请求的次数
request-retry: 3

# 模型受到限制之后的处理行为
quota-exceeded:
  # 多账号轮询的核心配置
  # 设置为true时，例如一个账号触发了429，程序会自动切换到下一个账号重新发起请求
  # 设置为false时，程序会把429的错误信息发给客户端，结束当前请求
  # 也就是说，当设置为true时，只要轮询的账号里至少有一个号是正常的，客户端这里就不会报错
  # 而设置false时，则需要客户端来进行重试或中止操作
  switch-project: true 
  # Gemini CLI独占配置，适用于Gemini 2.5 Pro和Gemini 2.5 Flash模型
  # 当正式版配额用完之后，会自动切换到Preview模型，保持开启即可
  switch-preview-model: true

# 各种AI客户端访问CLIProxyAPI所需要填写的Key，就在这里设置，和后边的各种Key不要弄混淆了
# 通俗点讲，这里的Key是CLIProxyAPI作为服务器所需要设置的
# 后边的各种Key是CLIProxyAPI作为客户端去访问服务器所需要的
api-keys:
  - "your-api-key-1"
  - "your-api-key-2"

# Gemini的官方API Key，如果你已经配了Gemini CLI，那么不建议填
# 因为Gemini CLI是满血的，而官方Key是残血的，填了的话会一起参与轮询
generative-language-api-key:
  - "AIzaSy...01"
  - "AIzaSy...02"
  - "AIzaSy...03"
  - "AIzaSy...04"

# Codex的API Key，各种中转站提供的Codex的key和base-url参数，填在这里就可以接入了
codex-api-key:
  - api-key: "sk-atSM..."
    base-url: "https://www.example.com"

# Claude的API Key，使用官方Key的时候，不要填base-url，使用第三方中转的，填base-url
claude-api-key:
  - api-key: "sk-atSM..."
  - api-key: "sk-atSM..."
    base-url: "https://www.example.com"

# 各种OpenAI兼容的都可以在这里接入，不多解释了
openai-compatibility:
  - name: "openrouter"
    base-url: "https://openrouter.ai/api/v1"
    api-keys:
      - "sk-or-v1-...b780"
      - "sk-or-v1-...b781"
    models:
    	# OpenAI兼容供应商提供的模型名称
      - name: "moonshotai/kimi-k2:free"
      	# 模型别名
        alias: "kimi-k2"

# Gemini Web的相关设置，可以忽略掉，用默认值就行
gemini-web:
    # 此选项用于状态化会话，由于Gemini Web是逆向的
    # 因而如果设置false的话，每条发送的消息程序会携带之前的所有上下文发送给服务器
    # 设置true的话，程序会按客户端发送的报文，根据最长匹配寻找之前的会话
    # 如果已有会话，则只发送当前的消息，而不携带所有上下文
    # 如果使用Nano Banana模型，请务必保持此选项为true，否则无法进行连续会话修图
    # 如果还有不理解的，可以开始切换开关后，在Gemini Web官方网页查看效果
    context: true
    # 最大发送的字符，保持为默认值即可
    max-chars-per-request: 1000000
    # 程序默认超出最大字符，会进行截断，分批发送，截断时，会在报文最后附加一条让模型等待的消息
    # 如果设置true，则不会在报文最后附加这条消息
    # 建议保持false即可，因为只有截断才会附加消息，非截断情况是不会附加的
    disable-continuation-hint: false
    # 编程模式，不用来进行编程，请不要启用，使用Nano Banana模型，请务必关闭
    # 设置ture，会有以下效果
    ## 使用系统自带的编码助手Gem进行对话
    ## 对话时如有思考内容，会把思考内容并入正文
    ## 在报文最后附加一条关于XML的消息
    code-mode: false
```

CLIProxyAPI 是一款使用 Go 语言编写的开源 AI 代理工具。

### **它究竟能做什么？**

| 软件功能特性                                                 | 支持的模型                     |
| :----------------------------------------------------------- | :----------------------------- |
| 为 CLI 模型提供 OpenAI/Gemini/Claude/Codex 兼容的 API 端点   | gemini-2.5-pro                 |
| 新增 OpenAI Codex（GPT 系列）支持（OAuth 登录）              | gemini-2.5-flash               |
| 新增 Claude Code 支持（OAuth 登录）                          | gemini-2.5-flash-lite          |
| 新增 Qwen Code 支持（OAuth 登录）                            | gemini-2.5-flash-image-preview |
| 新增 iFlow 支持（OAuth 登录）                                | gpt-5                          |
| 新增 Gemini Web 支持（通过 Cookie 登录）                     | gpt-5-codex                    |
| 支持流式与非流式响应                                         | claude-opus-4-1-20250805       |
| 函数调用/工具支持                                            | claude-opus-4-20250514         |
| 多模态输入（文本、图片）                                     | claude-sonnet-4-20250514       |
| 多账户支持与轮询负载均衡（Gemini、OpenAI、Claude、Qwen 与 iFlow） | claude-sonnet-4-5-20250929     |
| 简单的 CLI 身份验证流程（Gemini、OpenAI、Claude、Qwen 与 iFlow） | claude-3-7-sonnet-20250219     |
| 支持 Gemini AIStudio API 密钥                                | claude-3-5-haiku-20241022      |
| 支持 Gemini CLI 多账户轮询                                   | qwen3-coder-plus               |
| 支持 Claude Code 多账户轮询                                  | qwen3-coder-flash              |
| 支持 Qwen Code 多账户轮询                                    | qwen3-max                      |
| 支持 iFlow 多账户轮询                                        | qwen3-vl-plus                  |
| 支持 OpenAI Codex 多账户轮询                                 | deepseek-v3.2                  |
| 通过配置接入上游 OpenAI 兼容提供商（例如 OpenRouter）        | deepseek-v3.1                  |
| 可复用的 Go SDK                                              | deepseek-r1                    |
|                                                              | deepseek-v3                    |
|                                                              | kimi-k2                        |
|                                                              | glm-4.5                        |
|                                                              | tstars2.0                      |
|                                                              | 以及其他 iFlow 支持的模型      |

简单来说，CLIProxyAPI 的核心优势包括：

- **无需安装 Gemini CLI**，即可将其授权**转换为通用的 API Key**，从而在任何应用中调用功能完整的 Gemini 2.5 Pro、Gemini 2.5 Flash、Gemini 2.5 Flash Lite 模型。当正式版模型配额用尽后，它会自动切换到 Preview 模型（如 `gemini-2.5-pro-preview-05-06`），基本能用足每天 1000 次的调用配额，轻松实现“Gemini 自由”。
- **无需安装 Qwen Code**，即可将其授权转换为通用的 API Key，在任何地方调用 Qwen3 Coder Plus、Qwen3 Coder Flash 模型，实现“Qwen3 Coder 自由”。
- **无需安装 Codex**，即可将其授权转换为通用的 API Key，在任何地方调用 GPT-5、GPT-5-Codex 模型。尤其在目前可以免费开设 Team 账户的活动下，轻松实现“GPT 自由”。
- **将 Gemini 网页版转换为 API Key**，在任何地方调用 Nano Banana 等网页版模型（需客户端支持。据网友分享，免费版 Gemini 网页账户每天可调用约 100 次，而 Gemini Pro 用户则高达 1000 次）。
- **强大的负载均衡能力**。CLIProxyAPI 支持将不同来源（无论是 API Key 还是 OAuth 授权）的多个账户整合在一起进行负载均衡轮询，这意味着你可以轻松地将调用配额翻倍。
- **极低的资源消耗**。值得一提的是，该程序对系统资源的消耗极低。程序本身仅 10MB 左右，启动时内存占用不到 10MB，长时间峰值内存占用也仅有 100MB 左右，几乎任何电脑都能流畅运行。

程序的使用非常简单。官方**不仅提供了适用于各平台的二进制文件和 Docker 部署方式**，还提供了 EasyCLI 和 WebUI，对新手十分友好。所有设置均通过 `config.yaml` 配置文件管理，且支持热重载——修改配置后即时生效，无需重启程序。完整的配置项解说详见 

## 手把手带你用上AI神器 - CLIProxyAPI（肆：中转转发接入篇）

首先，让我们回顾一下之前使用的配置文件：

```yaml
port: 8317

# 文件夹位置请根据你的实际情况填写
auth-dir: "Z:\\CLIProxyAPI\\auths"

request-retry: 3

quota-exceeded:
  switch-project: true
  switch-preview-model: true

api-keys:
# Key请自行设置，用于客户端访问代理
- "ABC-123456"
```

初次配置后，我们一直没有改动过它。现在，是时候对这个文件进行一些扩展了。

我们先来添加一个 Claude 的中转服务。为此，我们首先需要**获取该服务的 `base-url`，**这个地址通常可以在相应服务商的官方文档或教程中找到。

我们在配置文件中加入 `claude-api-key` 字段：

```yaml
port: 8317
auth-dir: "Z:\\CLIProxyAPI\\auths"
request-retry: 3
quota-exceeded:
  switch-project: true
  switch-preview-model: true
api-keys:
- "ABC-123456"

claude-api-key:
  - api-key: "88_XXXXXXXXXXXXXXXXXXXXXXXXX"
    base-url: "https://www.**code.org/api"
```

同样地，**code 也提供了 Codex 服务。我们依照相同的方法，找到其 `base-url`：

然后，在配置文件中添加 `codex-api-key` 字段：

```yaml
port: 8317
auth-dir: "Z:\\CLIProxyAPI\\auths"
request-retry: 3
quota-exceeded:
  switch-project: true
  switch-preview-model: true
api-keys:
- "ABC-123456"

claude-api-key:
  - api-key: "88_XXXXXXXXXXXXXXXXXXXXXXXXX"
    base-url: "https://www.**code.org/api"
    
codex-api-key:
  - api-key: "88_XXXXXXXXXXXXXXXXXXXXXXXXX"
    base-url: "https://www.**code.org/openai/v1"
```

对于其他服务商，也可以采用类似的方式进行添加。例如，我这里还有几个 PackyCode 的 Codex API Key，我将它们一并加入配置：

```yaml
port: 8317
auth-dir: "Z:\\CLIProxyAPI\\auths"
request-retry: 3
quota-exceeded:
  switch-project: true
  switch-preview-model: true
api-keys:
- "ABC-123456"

claude-api-key:
  - api-key: "88_XXXXXXXXXXXXXXXXXXXXXXXXX"
    base-url: "https://www.**code.org/api"
  - api-key: "sk-4cXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX"
    base-url: "https://api.packycode.com"
  - api-key: "sk-HpYYYYYYYYYYYYYYYYYYYYYYYYYYYYYY"
    base-url: "https://api.packycode.com"

codex-api-key:
  - api-key: "88_XXXXXXXXXXXXXXXXXXXXXXXXX"
    base-url: "https://www.**code.org/openai/v1"
  - api-key: "fk-4cXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX"
    base-url: "https://oai-api.fkclaude.com/v1"
  - api-key: "sk-amXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX"
    base-url: "https://codex-api.packycode.com/v1"
  - api-key: "sk-sTXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX"
    base-url: "https://codex-api.packycode.com/v1"
```

请注意，即使是同一服务商、使用相同 `base-url` 的多个 `api-key`，也需要为每一条 `api-key` 单独声明 `base-url`，不可省略。

此外，CLIProxyAPI 还**支持接入任何兼容 OpenAI 接口的供应商**，这需要**通过 `openai-compatibility` 字段来配置**。在此不再赘述具体步骤，大家可以直接参考下方的配置文件示例进行配置：

```yaml
port: 8317
auth-dir: "Z:\\CLIProxyAPI\\auths"
request-retry: 3
quota-exceeded:
  switch-project: true
  switch-preview-model: true
api-keys:
- "ABC-123456"

claude-api-key:
  - api-key: "88_XXXXXXXXXXXXXXXXXXXXXXXXX"
    base-url: "https://www.**code.org/api"

codex-api-key:
  - api-key: "88_XXXXXXXXXXXXXXXXXXXXXXXXX"
    base-url: "https://www.**code.org/openai/v1"
  - api-key: "fk-4cXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX"
    base-url: "https://oai-api.fkclaude.com/v1"
  - api-key: "sk-amXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX"
    base-url: "https://codex-api.packycode.com/v1"
  - api-key: "sk-sTXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX"
    base-url: "https://codex-api.packycode.com/v1"

openai-compatibility:
  - name: "openrouter"
    base-url: "https://openrouter.ai/api/v1"
    api-keys:
      - "sk-or-v1-aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa"
      - "sk-or-v1-bbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbb"
    models:
      - name: "deepseek/deepseek-chat-v3.1:free"
        alias: "deepseek-v3.1"
      - name: "deepseek/deepseek-r1-0528:free"
        alias: "deepseek-r1-0528"
      - name: "x-ai/grok-4-fast:free"
        alias: "grok-4-fast"
  - name: "groq"
    base-url: "https://api.groq.com/openai/v1"
    api-keys:
      - "gsk_XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX"
    models:
      - name: "deepseek-r1-distill-llama-70b"
        alias: "deepseek-r1-70b"
```

可以看到，`openai-compatibility` 的配置逻辑与之前略有不同：同一供应商（Provider）下的**所有 `api-key` 共享同一个 `base-url`。**

## 手把手带你用上AI神器 - CLIProxyAPI（陆：新人最爱GUI）

楼层: 1

> 由于该系列教程篇幅较长，因此我按主题拆分，大家可以点击目录快速跳转到感兴趣的篇章
>
> - [手把手带你用上AI神器 - CLIProxyAPI（零：配置详细解说）](https://linux.do/t/topic/1011966)
> - [手把手带你用上AI神器 - CLIProxyAPI（壹：项目介绍+Qwen实战）](https://linux.do/t/topic/1011983)
> - [手把手带你用上AI神器 - CLIProxyAPI（贰：Gemini CLI+Codex实战）](https://linux.do/t/topic/1011986)
> - [手把手带你用上AI神器 - CLIProxyAPI（叁：NanoBanana实战）](https://linux.do/t/topic/1011988)
> - [手把手带你用上AI神器 - CLIProxyAPI（肆：中转转发接入篇）](https://linux.do/t/topic/1011999)
> - [手把手带你用上AI神器 - CLIProxyAPI（伍：Docker服务器部署）](https://linux.do/t/topic/1012017)
> - [手把手带你用上AI神器 - CLIProxyAPI（陆：新人最爱GUI）](https://linux.do/t/topic/1033149)
> - [没有VPS？教你零成本在ClawCloud上部署CLIProxyAPI](https://linux.do/t/topic/1033205)
> - [没有VPS？教你零成本在Render上部署CLIProxyAPI](https://linux.do/t/topic/1036951)
> - [没有VPS？教你零成本在Railway上部署CLIProxyAPI](https://linux.do/t/topic/1051621)
> - [没有VPS？教你零成本在HuggingFace上部署CLIProxyAPI](https://linux.do/t/topic/1053666)

在之前的文章中，我们已经介绍了如何通过命令行逐步运行 CLIProxyAPI。其实 CLIProxyAPI 还有配套的两个项目：EasyCLI 和 WebUI。

- **EasyCLI 仓库地址**：`https://github.com/router-for-me/EasyCLI`
- **WebUI 仓库地址**：`https://github.com/router-for-me/Cli-Proxy-API-Management-Center`

这两个项目旨在降低普通用户的使用门槛。EasyCLI 是桌面客户端，而 WebUI 是 Web 管理界面，它们都通过连接 CLIProxyAPI 来工作。

我此前未提供 GUI 教程，是因为旧版本需要用户自行部署或安装，操作相对繁琐。从 `6.0.19` 版本开始，作者已将 WebUI 集成到主程序中。因此，用户现在可以直接通过内置的 Web 界面进行配置。

本文将简要介绍如何启用并访问 WebUI。关于 EasyCLI 的使用方法，将在后续关于容器云部署的文章中详细介绍。

#### 一、启用 WebUI

首先，我们需要在原有的基础配置上进行调整，添加远程管理部分。完整的示例配置如下：

```yaml
port: 8317

auth-dir: "~/.cli-proxy-api"

request-retry: 3

quota-exceeded:

switch-project: true

switch-preview-model: true

api-keys:

- "ABC-123456"

# 本次添加的远程管理部分

remote-management:

allow-remote: true

# 用于远程管理的KEY，和上边的api-keys要区分开

secret-key: "MGT-123456"

disable-control-panel: false
```

**请注意**：修改配置后，需要重启程序才能生效（新版本支持自动热重载）。

#### 二、访问 WebUI

程序成功启动后，在浏览器中访问 `http://YOUR_SERVER_IP:8317/management.html` ，在管理密钥中输入之前设置的密码 `MGT-123456` 即可打开 WebUI 界面。



![img](./CPA%E7%9A%84%E4%BD%BF%E7%94%A8/24d641c30400ce3719b4fd964c6628064d6fb802_2_584x499.png)

#### 三、重要注意事项

WebUI 的界面设计直观，你可以自行探索各项功能。但需要特别注意的是，WebUI 中的 **OAuth 认证功能仅支持本地运行**（例如 `localhost` 或 `127.0.0.1`）的 CLIProxyAPI 实例。对于部署在远程服务器上的实例，由于 OAuth 服务商的安全策略限制，将无法通过 WebUI 直接完成认证。

## OAuth 认证是什么,为我详细解释一下

简单来说，**OAuth (Open Authorization)** 是一个关于**授权**的开放标准。

如果你经常在某个网站上看到“使用微信登录”或“使用 Google 帐号登录”，那么你其实已经在使用 OAuth 了。它的核心意义在于：**让第三方应用在不获取你密码的情况下，获得访问你存储在另一服务上信息的权限。**

------

### 1. 核心比喻：酒店房卡（或代客泊车钥匙）

为了理解 OAuth，最经典的比喻就是**酒店房卡**：

- **你的密码**：相当于酒店大门的**总钥匙**。如果你把这**把钥匙给了第三方应用，他们就能进出你所有的房间，甚至改掉你的锁。**
- **OAuth 令牌 (Token)**：相当于**房卡**。这张卡**只能在限定时间内打开特定的房间**（比如 302 室），且无法访问酒店的财务室或员工通道。

------

### 2. OAuth 中的四个角色

在一次 OAuth 流程中，通常会有四位“演员”登场：

1. **资源所有者 (Resource Owner)**：**就是你**。你拥有数据（比如你的头像、好友列表）。
2. **客户端 (Client)**：**第三方应用**。比如你想用“美图秀秀”访问你“微博”里的照片。
3. **授权服务器 (Authorization Server)**：**认证机构**。比如微博的登录界面，它确认你是本人并询问你是否同意授权。
4. **资源服务器 (Resource Server)**：**存放数据的地方**。比如微博存放照片的服务器。

------

### 3. 标准的 OAuth 2.0 授权流程

目前最常用的模式是**“授权码模式 (Authorization Code)”**，其过程就像一场礼貌的对话：

1. **请求授权**：第三方应用（客户端）把你**重定向到授权服务器（如微信登录页）**。
2. **用户同意**：你输入微信账号密码，点击“同意授权”。
3. **发放授权码**：微信给第三方应用**发一个临时的“授权码”**。
4. **兑换令牌**：第三方应用**拿着这个授权码，后台去找微信换取 Access Token（访问令牌）。**
5. **获取资源**：第三方应用**最后拿着这个令牌，去读取你的头像和好友。**

------

### 4. 为什么不直接给密码？（安全优势）

| **维度**     | **传统方式（给密码）**       | **OAuth 方式（给令牌）**                             |
| ------------ | ---------------------------- | ---------------------------------------------------- |
| **权限控制** | 应用拥有你的全部权限。       | 应用**只能访问你允许的部分权限（如“只读”）**。       |
| **有效期**   | 除非你改密码，否则永久有效。 | 令牌通常**有时效，过期自动作废**。                   |
| **撤销便利** | 必须改密码，会影响所有应用。 | 你**可以单独在后台取消某个应用的授权**，不影响其他。 |
| **密码泄露** | 极度危险。                   | 即使令牌丢了，攻击者也拿不到你的主账号密码。         |

------

### 5. 注意：认证 (Authentication) vs. 授权 (Authorization)

这是一个非常容易混淆的点。虽然 OAuth 经常被用来“登录”，但它**本质上是授权。**

- **认证 (AuthN)**：你是谁？（证明身份）
- **授权 (AuthZ)**：你能做什么？（**允许访问**）

> **小贴士**：现在的“第三方登录”通常是在 OAuth 之上加了一层叫 **OIDC (OpenID Connect)** 的协议，这才真正完成了“身份认证”的功能。

------

### 总结

OAuth 就像是一个**中介通行证**，它解决了“在不告诉陌生人密码的前提下，让他帮我办点事”的问题。它是现代互联网安全协作的基石。

# 自动切换（CPA中转服务）

在经费紧张的情况下，很多时候都**需要切号或者多家中转站稳定性很差，需要经常切换**，每次使用到一般就需要手动切换，非常麻烦，那么可以**搭建一个服务来自动切换，对AI工具发起的请求进行集中中转**，出现**失败报错后自动切换。**

当然这个方式的优点很明确，就是可以**无感使用自动切换以及转协议跨工具调用模型**（比如 claude里面使用gpt模型），而且购买境外服务器部署中转站服务（切勿用作其它用途），对于官方订阅账号不用代理即可使用，也比较方便。但是也有缺点，就是**会话自动切换后，会导致缓存失效，重新建议缓存会有额外的费用**，当然这点也只是对于较贵的第三方API或者官方订阅来说是个问题，但是贵的API或者官方订阅，通常也很稳定，不太会乱切换，所以也算是什么大问题，对于便宜的第三方API，那这额外费用影响不大，毕竟便宜第三方的缓存本身存在问题（一言难尽，缓存相关概念，可搜索教程《AI 编程新阶段：Claude Code功能拆解》）。

首先说明一下，此方式**可本地电脑部署，本机使用自动切换，也可以部署在云服务器上，跨设备使用自动切换。**建议**部署在云服务器上，这样可以在任意一个设备通过网页维护后，多设备自由使用，也可分享给其他朋友**。这点主要是几个人用的场景，**对服务器要求很低**，腾讯云购买1c2g新手活动价通常68元年，其他阿里云等都差不多，比较便宜，2c4g一年费用也在200元以内。

## 服务选择

中转站普遍使用的主要是三个开源服务，分别是 CLIProxyAPI （简称cpa，**扩展版Plus**）、NewAPI、Sub2API，先介绍下三个的使用场景，大家可自行选择。

- CLIProxyAPI ：将订阅账号、第三方API集中管理，并开放统一接口给用户使用，提供后台管理页面，通常个人使用都部署此服务；
- CLIProxyAPIPlus：基于cpa的社区版，在cpa的基础上接受社区开发的功能，所以**比cpa支持的范围更大（比如支持GitHub Copilot）**，但因接受很多社区提交，所以安全稳定性可能稍逊一些。使用方法与cpa几乎相同，如果没有额外需求，个人建议还是上cpa；
- NewAPI：主要是**用于管理API ，通常搭配CPA一起使用**，涉及到后台管理页面、用户前台注册登陆。一般对于提供给其他多人使用（支持注册登陆购买），并且需要各自独立计费的场景会使用到此服务，如果只是几个熟人兄弟间使用，可以不用部署；
- Sub2API：**是CPA + NewAPI的结合体**，支持功能非常多，包括但不限于，用户管理、按量计费、支付对接等等，一般商用会部署此服务，比如说你是要做中转站商人卖token，但是中转站商人不需要此教程，所以本文就不过多阐述了，不过这一整套源码倒是很有学习参考价值；

对于个人使用来说，通常**选择CPA即可满足自动切换的需求**，即使多个人使用，也可以配置多个API-KEY，也有统计页面查看使用情况。如果**需要支持更多的订阅（比如github copilot，那么则需要CPAPlus版本）**，如果是**需要按量计费限制（比如多人拼车套餐就需要按量限制），那么再额外部署NewAPI服务**，由NewAPI服务来对外提供接口。

调用链路：用户->NewAPI->CPA(Plus) 或 用户->CPA(Plus)

## 服务使用

左侧菜单给大家做简单介绍，后续大家可以上手使用一下也就会用了。

- 配置面板：服务相关的一些配置，**对应部署时候的 config.yaml 配置文件**，可以考虑开启 系统配置-使用统计，其余设置内容大家一看就懂（少部分看不懂的就保持默认值即可）；
- **AI 提供商配置：配置第三方API，**与ccs几乎类同（参考 一键切换章节），主要选项API密钥、Base URL、优先级，其中优先级是越大优先级越高，配置多个同类API的时候，会优先使用 优先级高的API。此外**可以设置模型列表以及模型别名，模型列表是允许使用的模型列表，如果未设置则使用全部**，通常不设置（可通过右侧“从 /v1/models 获取”按钮查看当前API支持的列表）。模型别名是的调用方传入a模型，实际调用使用的是b模型；
- **OAuth 登录：配置官方订阅账号，**选择你已有的账号服务商，比如Code Auth则是OpenAI账户，登录后可以使用账户的GPT模型。点击登录，会显示一个url，点击打开链接或这拷贝到任意浏览器访问，访问后正常登录账号，登录成功后会跳转到 “localhost:xxxxxxx” 的url，从浏览器顶部拷贝此链接，输入到“回调 URL”中，然后点击提交回调 URL就完成登录；
- **认证文件管理：用于管理OAuth 登录的认证文件**，包括启用、模型列表、优先级、状态等。**也可以上传导入别人已保存或其他人发给你的认证文件，也可以下载已有的认证文件；**
- 配额管理：查看认证文件对应账户的剩余额度，为避免异常封号（claude封号很严重），通常别频繁刷；
- 使用统计：从各个维度统计的token使用情况，前提是开启 配置面板-系统配置-使用统计 功能；
- 中心信息：查看所有已配置API服务商的支持模型汇总列表，以及当前版本、检查更新；

按照上述配置完成后，**就可以通过 手动配置或ccs配置，将此中转服务的API配置到AI编程工具中**，后续中转服务会**自动切换使用的API服务，无需在AI工具层手动切换**，调用链路：**CLI → CPA(自动切换) → 大模型服务。**

base_url 就是管理控制台页面右上角地址： [http://127.0.0.1:8317/](http://127.0.0.1:8317/management.html#/) （codex还需要补上 /v1）；

api_key 就是 **配置面板 - 认证配置 - API 密钥列表（新增后不会实时生效，需重启），也就是config.yaml 配置文件 api-keys；**

对于使用统计的价格计算，数据来源是最底部的模型价格设置（注意鼠标滚动有点小小的问题，导致没法直接滚动到最底下，拖拽右侧滚动条），可手动输入，也可以使用CPA 一键同步价格脚本：https://github.com/ApliuQ/CPAModelsPrice

### CLIProxyAPIPlus

使用方式与CPA几乎相同，唯一的差异就是社区额外支持的OAuth订阅服务，需要通过命令行来登录。

通过命令 `./cli-proxy-api-plus --help` 查看所有支持的OAuth列表

```powershell
cpaplus@VM-0-17-ubuntu:~$ ./cli-proxy-api-plus --help
CLIProxyAPI Version: 6.9.5-0-plus, Commit: f8d1bc06, BuiltAt: 2026-03-29T04:41:24Z
Usage of ./cli-proxy-api-plus
  -antigravity-login
    Login to Antigravity using OAuth
  -cursor-login
    Login to Cursor using OAuth
  -github-copilot-login
    Login to GitHub Copilot using device flow
-----省略-----
```

然后继续输入命令 `./cli-proxy-api-plus -github-copilot-login` ，就可以按照提示信息进行登录