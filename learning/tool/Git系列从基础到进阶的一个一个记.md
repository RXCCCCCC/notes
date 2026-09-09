---
title: Git系列速查一个一个记
author: RXCCCCCC
date: 2025-07-25 11:15:31
summary:
img:
top:
cover:
coverImg:
tags:
categories:
---

Git 的主要特点：
1.**版本控制**：每次提交都像写了⼀篇新⽇记，**保存**你的开发成果。
2.**分⽀管理**：分⽀就像章节，可以**并⾏开发⽽互不⼲扰。**
3.分布式：每个⼈都拥有完整的“时光机⽇记本”，即便**没有⽹络也可以⼯作**

GitHub：你可以把代码上传到 GitHub，随时随地访问，并与他⼈协作开发，甚⾄分享给全世界。

Gitee：

Gitee 是 GitHub 的“中国版伙伴”。

 优势：速度快、对**国内开发者友好**，能与本地⼯具（如钉钉、企业微信）⽆缝集成。 常⽤于**企业内部**项⽬或对**私有化部署有需求**的团队

# Git 常⽤命令及 SSH 配置

SSH：安全认证和便捷连接 ,允许在本地和远程仓库之间**安全通信**，并省去每次推送或拉取代码时输⼊密码的⿇烦

![cea6e8ca8d4a0c3b8af35814dfcfb48d](./Git%E7%B3%BB%E5%88%97%E4%BB%8E%E5%9F%BA%E7%A1%80%E5%88%B0%E8%BF%9B%E9%98%B6%E7%9A%84%E4%B8%80%E4%B8%AA%E4%B8%80%E4%B8%AA%E8%AE%B0/cea6e8ca8d4a0c3b8af35814dfcfb48d.png)

![6ae805e806aeacb0b8a9274dcd11ab5c](./Git%E7%B3%BB%E5%88%97%E4%BB%8E%E5%9F%BA%E7%A1%80%E5%88%B0%E8%BF%9B%E9%98%B6%E7%9A%84%E4%B8%80%E4%B8%AA%E4%B8%80%E4%B8%AA%E8%AE%B0/6ae805e806aeacb0b8a9274dcd11ab5c.png)

![0128b3779daeee91001684f54a5ac973](./Git%E7%B3%BB%E5%88%97%E4%BB%8E%E5%9F%BA%E7%A1%80%E5%88%B0%E8%BF%9B%E9%98%B6%E7%9A%84%E4%B8%80%E4%B8%AA%E4%B8%80%E4%B8%AA%E8%AE%B0/0128b3779daeee91001684f54a5ac973.png)

## Git Commit message 编写指南

## 介绍

在 Git 中，每次提交代码，都要写 **Commit message（提交说明）**，否则就不允许提交。这个操作将通过 git commit 完成。

```
git commit -m "hello world"
```

> 上面代码的-m参数，就是用来指定 commit mesage 的。

如果一行不够，可以只执行git commit，就会跳出文本编译器，让你写多行。

```
git commit
```

## 格式

Commit message 包括三个部分：Header，Body 和 Footer。可以用下方的格式表示它的结构。

```
<type>(<scope>): <subject>// 空一行<body>// 空一行<footer>
```

> 其中，**Header 是必需的，Body 和 Footer 可以省略(默认忽略)**，一般我们在 `git commit` 提交时指定的 `-m` 参数，就**相当于默认指定 Header**。

> 不管是哪一个部分，任何一行都不得超过72个字符（或100个字符）。这是为了避免自动换行影响美观。

### Header

Header部分只有一行，包括三个字段：**type（必需）、scope（可选）和subject（必需）。**

#### type

- **feat**：新功能（feature）
- **fix**：修补bug
- **docs**：文档（documentation）
- **style**： 格式（不影响代码运行的变动）
- **refactor**：重构（即不是新增功能，也不是修改bug的代码变动）
- **test**：增加测试
- **chore**：构建过程或辅助工具的变动

如果 type 为 feat 和 fix ，则该 commit 将肯定出现**在 Change log 之中**。其他情况（docs、chore、style、refactor、test）由你决定，要不要放入 Change log，建议是不要。

#### scope

scope用于说明 **commit 影响的范围**，比如数据层、控制层、视图层等等，视仓库不同而不同。

#### subject

subject是 commit 目的的**简短描述**，不超过50个字符。

- 以动词开头，使用第一人称现在时，比如change，而不是changed或changes
- **第一个字母小写**
- 结尾**不加句号（.）**

### Body

Body 部分是对本次 commit 的**详细描述**，可以分成多行。下面是一个范例。

> More detailed explanatory text, if necessary. Wrap it to about 72 characters or so. Further paragraphs come after blank lines.- Bullet points are okay, too- Use a hanging indent

有两个注意点。

- 使用第一人称现在时，比如使用change而不是changed或changes。
- 应该说明代码变动的动机，以及与以前行为的对比。

### Footer

Footer 部分只用于两种情况。

#### 1、不兼容变动

如果当前代码与上一个版本不兼容，则 Footer 部分以**BREAKING CHANGE开头**，后面是对变动的描述、以及变动理由和迁移方法。

```
BREAKING CHANGE: isolate scope bindings definition has changed.

    To migrate the code follow the example below:

    Before:

    scope: {
      myAttr: 'attribute',
    }

    After:

    scope: {
      myAttr: '@',
    }

    The removed `inject` wasn't generaly useful for directives so there should be no code using it.
```

#### 2、关闭 Issue

如果当前 commit **针对某个issue**，那么可以在 Footer 部分关闭这个 issue 。

> Closes #234

也可以一次关闭多个 issue 。

> Closes #123, #245, #992

### Revert

还有一种特殊情况，如果当前 commit **用于撤销以前的 commit，则必须以revert:开头**，后面跟着**被撤销 Commit 的 Header**。

> revert: feat(pencil): add 'graphiteWidth' option
>
> This reverts commit 667ecc1654a317a13331b17617d973392f415f02.

Body部分的格式是固定的，必须写成This reverts commit .，其中的hash是被撤销 commit 的 SHA 标识符。

如果当前 commit 与被撤销的 commit，在同一个发布（release）里面，那么它们都不会出现在 Change log 里面。如果两者在不同的发布，那么当前 commit，会出现在 Change log 的Reverts小标题下面。
