---
pubDatetime: 2024-01-05T10:00:00.0Z
title: Astro搭建博客
featured: true
draft: false
tags:
  - blog
description: 使用Astro搭建博客 vercel部署到全球
---

![Astro Logo](https://pic.lookcos.cn/i/2023/02/28/hey1iq.png)

## 什么是 Astro ?

Astro JS 是一个用于构建高性能、内容为中心的网站的 web 框架。它可以让你使用你喜欢的 JavaScript web 框架（React，Svelte，Vue 等）来编写 UI 组件，然后在构建时将整个网站渲染为静态 HTML。这样，你就可以得到一个完全静态的网站，不需要加载任何 JavaScript。Astro 还支持多种前端工具，如 Tailwind CSS2，并且可以部署到任何地方，甚至是边缘服务器。Astro 最新版本是 2.0，它引入了一种新的前端架构（称为 Astro Islands），可以优化你的网站，让它加载速度提高 33%，JavaScript 减少 90%。

## Astro Air Blog 是什么？

它是我基于 Astro 2.0 开发的一个博客模板，你可以直接使用它来搭建你的博客。我没有使用任何 CSS 框架以及 React 等前端框架，而是使用了 Astro 的原生能力，这样可以让你的博客加载速度更快，同时也可以让你的博客更加轻量。本站就是使用这个模板搭建的。

下面我准备一步步教你如何使用 Astro Air Blog 搭建你的博客。
可能需要一些下面列举的知识：

- [Markdown](https://www.markdownguide.org/) 一种轻量级的标记语言，用于编写文档。
- [Git](https://git-scm.com/) 一个开源的分布式版本控制系统，用于敏捷高效地处理任何或小或大的项目。
- [GitHub](https://github.com) 一个面向开源及私有软件项目的托管平台，因为只支持 Git 作为唯一的版本库格式进行托管，故名 GitHub。
- [HTML](https://developer.mozilla.org/zh-CN/docs/Web/HTML) 一种用于创建网页的标准标记语言。
- [CSS](https://developer.mozilla.org/zh-CN/docs/Web/CSS) 一种用于描述网页样式的语言。
- [JavaScript](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript) 一种具有函数优先的轻量级，解释型或即时编译型的编程语言。

其中 Makrdown 是必须的，其他的都是可选的。

## 第一步：使用模板创建你的博客仓库

首先你需要一个 GitHub 账号，如果你没有的话，可以点击 [这里](https://github.com) 注册一个。
打开 [Astro Air Blog](https://github.com/austin2035/astro-air-blog) 仓库，点击右上角的 `Use this template` 按钮。

![使用模板创建你的博客仓库|inline](https://pic.lookcos.cn/i/2023/02/27/qhv71p.png)

点击 `Create repository from template` 按钮，然后输入你的仓库名称， 就可以创建你的博客仓库了。

![设置仓库信息|inline](https://pic.lookcos.cn/i/2023/02/27/qhv0k0.png)

## 第二步：克隆你的博客仓库到本地

![3](https://pic.lookcos.cn/i/2023/02/27/qhv4gl.png)

点击 `Code` 按钮，复制仓库地址。有两种方式，一种是使用 SSH，一种是使用 HTTPS (推荐新手使用)，这里我使用的是 SSH，如果你不知道怎么使用 SSH，可以点击 [这里](https://docs.github.com/cn/github/authenticating-to-github/connecting-to-github-with-ssh) 查看。

接着在你的电脑上打开终端，输入下面的命令，将你的博客仓库克隆到本地。

```bash
git clone 复制的仓库地址
```

克隆完毕后，我们进入仓库所在的根目录。

## 第三步：安装依赖

在终端中输入下面的命令，安装依赖。

```bash
npm install
```

## 第四步：启动本地调试服务

在终端中输入下面的命令，启动本地调试服务。

```bash
npm run dev
```

![本地调试服务的启动|inline](https://pic.lookcos.cn/i/2023/02/27/qhv6fk.png)

如图所示，本地调试服务启动成功，打开浏览器，输入 `http://localhost:3000`，就可以看到你的博客了。并且修改你的博客内容，浏览器会自动刷新。

## 第五步：查看文章所在路径

在你的博客中，你可以看到有很多篇文章，这些篇文章的内容都是我写好的，你可以直接修改它们的内容，也可以删除它们，然后添加你自己的文章。

![已经存在的文章|inline](https://pic.lookcos.cn/i/2023/02/27/qhv62a.png)

## 第六步：了解项目结构

在你的博客仓库中，你可以看到有很多文件，这些文件都是用来干什么的呢？请查看这张图片。

![项目结构|inline](https://pic.lookcos.cn/i/2023/02/27/qhv3dz.png)

## 第七步：了解文章的结构

打开任意一篇文章，你可以看到有很多元数据，这些元数据都是用来设置什么的呢？请查看这张图片。

![文章的结构](https://pic.lookcos.cn/i/2023/02/27/qhv4dn.png)

元数据的前后有三个连续的短横线，中间是元数据的内容。元数据的内容是由键值对组成的，键值对之间用冒号分隔，键值对之间用换行分隔。

元数据下面是文章的内容，文章的内容是使用 Markdown 语法编写的。

添加文章的时候，你需要添加元数据，然后在元数据下面添加文章的内容即可。然后再主页中就可以看到你添加的文章了。

## 第八步：博客网站的设置

在你的博客仓库中，你可以看到有一个 `consts.js` 文件，这个文件是用来设置博客网站的一些固定信息的。请查看图片对应的说明。

![博客网站的固定信息](https://pic.lookcos.cn/i/2023/02/27/qhv31s.png)

## 第九步：提交你的修改

到目前为止，我们都在本地修改博客的内容，但是这些修改并没有提交到 GitHub 上，所以别人是看不到你的修改的。接下来我们将把我们的修改提交到 GitHub 上。

![所有的增加、删除、修改记录都可以在这里看到|inline](https://pic.lookcos.cn/i/2023/02/27/qhv6v0.png)

我这里使用 VS Code 编辑器，你也可以使用其他编辑器，只要你熟悉就行。在 VS Code 中，我们可以看到有很多文件被修改了，这些文件就是我们刚刚修改的文件。接着我们把这些文件提交到 GitHub 上。
点击加号，将所有文件添加到暂存区。然后在输入框中输入提交信息，最后点击 `提交` 按钮，将修改提交到本地仓库。

![同步更改](https://pic.lookcos.cn/i/2023/02/27/qhvbmi.png)

然后点击 `同步更改` 按钮，将修改推送到 GitHub 上。

## 第十步：查看你的 Github 仓库

![可以看到，成功提交](https://pic.lookcos.cn/i/2023/02/27/qhveeh.png)

## 第十一步：将你的博客部署到 Vercel

通过 vercle 可以将你的博客部署到云端，这样你就可以通过网址访问你的博客了。vercel 还是免费的，并且绑定之后，你的博客每次修改之后（本地 push 到Github 仓库之后），vercel 都会自动帮你部署。

![通过 vercel 新建一个项目](https://pic.lookcos.cn/i/2023/02/27/qhvf3m.png)

登录 vercel 后，点击 `New Project` 按钮，将你的博客仓库导入到 vercel 中。

![选择自己的博客仓库](https://pic.lookcos.cn/i/2023/02/27/qhvavy.png)

选择你的博客仓库，然后点击 `Import` 按钮。

![配置项目的信息|inline](https://pic.lookcos.cn/i/2023/02/27/qhvcdn.png)

配置项目的信息，包括项目的名称、项目的描述、项目的环境变量等。

在 vercel 中，我们只需要配置一下项目名称即可，其他的都不需要配置。这里的 Framework Preset 应该会自动选择 `Astro`，如果没有自动选择，请选择 `Astro`。

**注意：**
如果你使用的是 Cloudflare 的 Pages 来部署此博客，那么需要指定一下项目的环境变量，否则你的博客无法构建。这个变量如下表格所示：

| 变量名       | 变量值  |
| ------------ | ------- |
| NODE_VERSION | 16.12.0 |

如果遇到了问题，可以参考这篇文章：[Astro 部署到 Cloudflare Pages](https://developers.cloudflare.com/pages/framework-guides/astro/)

最后点击 `Deploy` 按钮，将你的博客部署到 vercel 中。

![部署成功](https://pic.lookcos.cn/i/2023/02/27/qhvesf.png)

等待部署成功，然后点击 `Visit` 按钮，就可以看到你的博客了。

## 第十二步：绑定域名

这时候，你的博客已经可以通过 vercel 提供的网址访问了。

![可以看到有一个默认的网址](https://pic.lookcos.cn/i/2023/02/27/qhvagk.png)

但是这个网址不是很好记，所以我们需要将它绑定到自己的域名上。这样，我们就可以通过自己的域名来访问自己的博客了。
点击 `Visit Domains` 按钮，进入域名设置页面。

![添加自己的域名](https://pic.lookcos.cn/i/2023/02/27/qhvcy4.png)

输入你想要绑定的域名，然后点击 `Add` 按钮，将域名添加到 vercel 中。

![21](https://pic.lookcos.cn/i/2023/02/27/qhv96q.png)

按照提示，将域名的 DNS 解析指向 vercel 的 DNS 服务器，然后点击 `Continue` 按钮。
等到域名的 DNS 解析生效之后，vercle 会自动帮你将域名绑定到你的博客上，并且生成一个 SSL 证书。这时候，你就可以通过你的域名来访问你的博客了。

以上内容来自于[austin2035](https://github.com/austin2035/blog) 感谢他！

## 遇到的坑

Git clone到本地后修改完不能push 报错husky not found err 127

解决方案：找到本地项目目录下/.husky.pre-commit文件

终端输入👇拿到路径

```zsh
where npx
```

添加到

```zsh
#!/usr/bin/env sh
. "$(dirname -- "$0")/_/husky.sh"

///去掉/npx 比如路径是/usr/local/bin/npx
PATH="/usr/local/bin:$PATH"

npx lint-staged
```

问题解决 感谢观看
