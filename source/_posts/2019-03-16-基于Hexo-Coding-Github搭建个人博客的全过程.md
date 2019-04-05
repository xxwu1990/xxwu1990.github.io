---
title: 基于Hexo+Coding+Github搭建个人博客的全过程
date: 2019-03-16 23:49:07
categories:
- 博客
tags:
- hexo
- Github
---

![set-up-hexo-with-coding-and-github](https://11.tt/posts/2018/set-up-hexo-with-coding-and-github/set-up-hexo-with-coding-and-github.png)

本文转自[Miaia](https://11.tt/posts/2018/set-up-hexo-with-coding-and-github/)，非常通俗易懂的快速上手指南。

感觉平时工作、瞎倒腾的过程中会遇到好玩的技术、会跳进很深的坑😁，又或是遇到很复杂的情况，以前觉得烂笔头很方便，但是技术越来越复杂加上有各种图片，用笔来做笔记真的是挺落后的了，于是升起了写博客做记录的念头。

一开始我是使用 Nginx+Wordpress 来搭建个人博客的，但是因为要时常检查服务器状况；最近又有一个同学服务器被入侵，删光数据库还装了矿机；阿里的主机面临到期，续费只挂个博客感觉浪费；再加上有了一见钟情的 Hexo+Next 主题并且发现了 Coding和Github 上的 Pages 服务！这不立马就投奔新颖的极简风博客啦！

------

# 概述

我感觉可能是因为最近疯狂改论文，所以开头部分用了“概述”这个标题😅…
言归正传，这次我搭建 Hexo 使用的本地环境如下：

```
* Windows 10 1803
* node-v8.11.2-x64
* git version 2.17.1.windows.2
* hexo-v3.7.1
```

肯定不满足于默认的主题啊，所以使用了 `next-muse-v6.3.0` 这个主题进行美化~
接下来添加网站流量统计以及进行一些小细节的优化。
因为心比较大，感觉会有国外大佬来看我的网站，所以为了提高访问速度，我分别将网站部署到 [Coding](https://coding.net/) 和 [Github](https://github.com/) 以提高国内和国外的访问速度，同时在上面分别启用 https。
然后使用 [腾讯云解析](https://cloud.tencent.com/product/cns) 来进行自定义域名的解析。

------

# 环境安装

这部分主要是将[概述](https://11.tt/posts/2018/set-up-hexo-with-coding-and-github/#%E6%A6%82%E8%BF%B0)中所提到本地环境的安装过程进行记录和介绍~
一般情况下，照着做都能成功的…
其实，一般熟手看到这篇博文的环境安装部分…可能只是…来找几个…官网链接…吧…

## 安装 Node.js

Hexo是基于 Node.js 开发的(应该是)，所以我们首先需要在本地安装 Node.js。
作为一个新世纪的技术行业接班人，我们自然要通过官网来下载安装各种程序了！
打开 [Node.js 官网 (https://nodejs.org)](https://nodejs.org/) 我们就可以看到映入眼帘的就是直接推荐你系统下载安装的页面：

![nodejs-download](https://11.tt/posts/2018/set-up-hexo-with-coding-and-github/nodejs-download.png)

我们点击图中红框内的按钮，下载 `node-v8.11.2-x64.msi` 安装文件。
下载完成后，打开 `node-v8.11.2-x64.msi` 进行安装。
**除非你要修改安装路径或特殊需求，否则一路默认安装即可。**
安装完成后，打开 Powershell 或 cmd，输入 `node -v`，若为以下输出，则安装成功。

```
Windows PowerShell
版权所有 (C) Microsoft Corporation。保留所有权利。

PS C:\Users\Miaia> node -v
v8.11.2
PS C:\Users\Miaia>
```

## 安装 Git

我们需要通过 Git 来下载一些主题文件，当然最重要的功能是将生成的网页文件提交到下文中创建的git仓库中去。
我们仍然通过打开 [Git 官网下载页 (https://git-scm.com/downloads)](https://git-scm.com/downloads) 来下载 Git 的安装程序。
在选择适用系统后，为了方便起见，我选择了安装版的 Git 进行下载(如下图)。

![git-download](https://11.tt/posts/2018/set-up-hexo-with-coding-and-github/git-download.png)

我们点击图中红框内的链接，下载 `Git-2.17.1.2-64-bit.exe` 安装文件。
下载完成后，打开 `Git-2.17.1.2-64-bit.exe` 进行安装。
**除非你了解你在安装时做出的修改，否则一路默认安装即可。**
安装完成后，打开 Powershell 或 cmd，输入 `git --version`，若为以下输出，则安装成功。

```
Windows PowerShell
版权所有 (C) Microsoft Corporation。保留所有权利。

PS C:\Users\Kiana> git --version
git version 2.17.1.windows.2
PS C:\Users\Kiana>
```

**因为我使用的是 Windows 10 系统，所以上面两个程序我都安装了 Windows 版，使用其他系统的同学我相信你们安装这一步能做到的😁。**

## 安装 Hexo

### 打开 Git Bash

在安装完 Windows 版 Git 以后，我们在资源管理器任意文件夹空白处右键即可看到如下界面：

![open-git-bash](https://11.tt/posts/2018/set-up-hexo-with-coding-and-github/open-git-bash.png)

我们单击 `Git Bash Here` 打开 Git Bash，可以看到以下界面：
(Mac、Linux 用户打开终端即可)

![git-bash](https://11.tt/posts/2018/set-up-hexo-with-coding-and-github/git-bash.png)

我们接下来的安装步骤以及以后所有的 hexo 页面生成、部署等工作都在在这个 Git Bash 中完成。

### 使用淘宝 NPM 镜像源(可选)

**由于国内访问官方 NPM 源速度较慢，为了一劳永逸，此处可以将 NPM 源更换为了淘宝 NPM 镜像源。**
**请注意，如果你觉得你的 NPM 源速度够快，更换镜像源这部分可选择性使用**
我们在 Git Bash 中输入如下指令

```
# 将官方 NPM 源更换为 淘宝 NPM 镜像源
$ npm install -g cnpm --registry=https://registry.npm.taobao.org
```

**请注意，如需使用上方安装的淘宝源，需要在使用 npm 命令时将其改为 cnpm**

### 安装 Hexo 命令行工具

即安装 Hexo 主体，需要在命令行界面进行操作，以下命令来自 [Hexo 官方网站](https://hexo.io/)。

```
$ npm install hexo-cli -g
```

请耐心稍等一会儿，如果在安装过程中头部出现 `WARN` ，可能是因为某些内容不支持 Windows，请不要担心，并不影响实际使用。
在安装完成后，输入 `hexo -v`，若出现类似以下内容，则 Hexo 已经安装成功。

```
$ hexo -v
hexo: 3.7.1
hexo-cli: 1.1.0
os: Windows_NT 10.0.17134 win32 x64
http_parser: 2.8.0
node: 8.11.2
...
```

------

# Hexo 博客初始化

## 创建博客主目录

在电脑中任意位置创建一个任意名称的文件夹，例如，我在D盘创建了一个名为 `myblog` 的文件夹。

![create-blog-main-folder](https://11.tt/posts/2018/set-up-hexo-with-coding-and-github/create-blog-main-folder.png)

## 初始化 Hexo 主目录

进入上方创建的文件夹，即双击进入 `myblog` 文件夹。
然后右键打开 `Git Bash` 。
在 Git Bash 中输入下方命令：

```
$ hexo init
```

接下来会自动 clone 需要的文件以及默认的主题到这个文件夹里面，并会获得类似下方的输出：

```
$ hexo init
INFO  Cloning hexo-starter to D:\myblog
Cloning into 'D:\myblog'...
...
Unpacking objects: 100% (65/65), done.
Submodule 'themes/landscape' (https://github.com/hexojs/hexo-theme-landscape.git) registered for path 'themes/landscape'
...
npm WARN optional SKIPPING OPTIONAL DEPENDENCY: fsevents@1.2.4 (node_modules\fsevents):
npm WARN notsup SKIPPING OPTIONAL DEPENDENCY: Unsupported platform for fsevents@1.2.4: wanted {"os":"darwin","arch":"any"} (current: {"os":"win32","arch":"x64"})

added 401 packages in 12.941s
INFO  Start blogging with Hexo!
```

我们看到 `INFO Start blogging with Hexo!` 就可以知道初始化成功啦！我还没遇到初始化失败的…
上面那些 `WARN` 都可以忽略的，不用担心，程序员里只看得到 ERROR，😁😁😁hhh~
接下来要安装一些必要的依赖，同样也非常简单，只要在 Git Bash 中输入：

```
$ npm install
```

然后可以看到输出中有一句：

```
up to date in 1.999s
```

然后 Hexo 博客就真的已经！非常简单滴 **搭！建！完！成！啦！✿✿ヽ(°▽°)ノ✿**

## 生成并运行博客

在我们进行进一步优化之前，我们先来看看我们搭建好的博客~
**以下命令要在 Git Bash 中输入哦！**
首先我们要先生成静态页面，用到的命令是：

```
$ hexo g
```

其中，`g` 的全称是 `generate`，当然也可以用 `hexo generate` 这条命令，但上方命令更简便。
输入完后可以看到类似以下输出：

```
$ hexo g
INFO  Start processing
INFO  Files loaded in 341 ms
INFO  Generated: index.html
...
INFO  Generated: css/images/banner.jpg
INFO  Generated: css/fonts/fontawesome-webfont.svg
INFO  28 files generated in 712 ms
```

一切准备就绪，就差一把东风开启服务器，输入以下命令吹风，偶不，是开启本地服务器：

```
$ hexo s
```

其中， `s` 的全称是 `server`，当然也可以用 `hexo server` 这条命令，但上方命令更简便。
输入之后我们可以看到以下输出：

```
$ hexo s
INFO  Start processing
INFO  Hexo is running at http://localhost:4000/. Press Ctrl+C to stop.
```

显而易见，我们的博客已经运行在本地4000端口上啦！
打开浏览器，在地址输入 `http://localhost:4000/`，我们就可以看到激动人心的界面啦！
**友情提示：平常只使用 Windows 而不熟悉 Linux 的小伙伴可能不知道 Ctrl+C 在 Linux 中表示取消操作，而 Git Bash 正是使用了 Bash 这种 shell。所以这里如果要进行复制，可以右键或者通过 Ctrl+Insert 来实现复制操作。类似地，粘贴操作也不能使用 Ctrl+C，需要改为 Shift+Insert。**

![origin-hexo-page](https://11.tt/posts/2018/set-up-hexo-with-coding-and-github/origin-hexo-page.png)

------

# 博客美化

接下来我们要让博客变得好看起来！
那天我领导问我为什么不用某ssh客户端，在他给我罗列了N条优点之后，我和他说，我这个好看！O(∩_∩)O
好看！好看！好看很重要！谁不希望自己的博客在能实现博客功能的基础上还能更好看呢？
本来我打算这章开始就单独放到一篇文章里面，但是后来想想还是索性都放到这里面算了，毕竟是记录我这个网站的建立过程，分开来优点怪怪的…
好了Y(^o^)Y，话不多说！(已经说了一堆了XD)我们开始美化博客啦！

## 使用 Next-Muse 主题

当时会下定决心重做整个博客的最主要原因就是，看到了使用了 Next-Muse 这个主题的网站！
真的是一见钟情有木有有木有！
Next-Muse 主题是 Next 主题四种设计中的其中一种，其他还有 Mist，Pisces 和 Gemini。
首先附上 Next 的 Github 链接：[hexo-theme-next (https://github.com/theme-next/hexo-theme-next)](https://github.com/theme-next/hexo-theme-next)
在这个仓库里面可以看到不同主题设计的 demo 网址~
o(*^＠^*)o吼吼！接下来开始操作啦！

### 下载 Next 主题

首先，我们要将 Next 主题下载到本地。
在网站根目录右键打开 Git Bash，输入以下命令将主题下载到本地：

```
$ git clone https://github.com/theme-next/hexo-theme-next themes/next
```

这个只是非常简单的克隆操作，除非克隆到一半的时候网突然断了…否则一般来说是不会失败哒！

### 启用 Next 主题

我们在网站根目录下可以看到有一个名为 `_config.yml` 的配置文件，我们用编辑器打开它，搜索 `theme`，我们可以找到以下内容：

```
# Extensions
## Plugins: https://hexo.io/plugins/
## Themes: https://hexo.io/themes/
theme: landscape
```

我们要将其中的 `landscape` 修改为我们要是用的 `next`，修改完如下所示：

```
# Extensions
## Plugins: https://hexo.io/plugins/
## Themes: https://hexo.io/themes/
theme: next
```

然而这个网站默认是英文的呢，所以我们要修改一下显示语言，完美支持中文的哦！仍然在这个配置文件中，搜索 `language`，我们可以找到以下内容：

```
...
author: John Doe
language:
timezone:
...
```

我们要在其中的 `language` 的冒号后面添加 `zh-CN`。
**请注意：根据语法要求，冒号需要为英文冒号，冒号后需要有一个空格。**
修改后，如下所示：

```
...
author: John Doe
language: zh-CN
timezone:
...
```

### 查看已启用的主题

我们在 Git Bash 依次输入下方三条命令：

```
$ hexo clean
$ hexo g
$ hexo s
```

其中，`hexo clean` 的作用是删除本地已经生成的所有静态文件，哈复习一下！有没有忘记下面两条是什么命令呀？忘记了的话，请看[这里](https://11.tt/posts/2018/set-up-hexo-with-coding-and-github/#%E7%94%9F%E6%88%90%E5%B9%B6%E8%BF%90%E8%A1%8C%E5%8D%9A%E5%AE%A2)哦！
每一条命令输完都会有对应的输出，因为比较简单或者上方出现过，所以在这里我就省略啦~
输入完 `hexo s` 启动服务器后，我们再次打开浏览器，在地址栏输入 `http://localhost:4000/`，我们可以看到我们的主题已经启用成功啦！

![next-theme-enabled](https://11.tt/posts/2018/set-up-hexo-with-coding-and-github/next-theme-enabled.png)

**到这里，我们的主题修改已经完成啦！**

## 细节美化

### 页面底部跳动的爱心

**拉到本页面底部看一下~那个跳动的爱心是不是超好看！！！来来来我教你怎么弄！**
首先我来为难一下选择困难症们！！！先去这里选择一个图标！！→→→ [The Icons](https://fontawesome.com/v4.7.0/icons/)
然后复制这个图标的代码，如我现在选择下图中的爱心，那么我要复制的内容就是下图红框中的 `fa-heartbeat`。

![copy-icon-code](https://11.tt/posts/2018/set-up-hexo-with-coding-and-github/copy-icon-code.png)

然后我们需要找到主题的配置文件，定位到目录中 `themes/next/_config.yml`，**请注意！这个配置文件与根目录中的配置文件不一样！不一样！我们不一样！**
同样是用编辑器打开这个配置文件，搜索 `footer`，我们可以看到以下内容：

```
footer:
  # Specify the date when the site was setup.
  # If not defined, current year will be used.
  #since: 2015

  # Icon between year and copyright info.
  icon:
    # Icon name in fontawesome, see: https://fontawesome.com/v4.7.0/icons
    # `heart` is recommended with animation in red (#ff0000).
    name: user
    # If you want to animate the icon, set it to true.
    animated: false
    # Change the color of icon, using Hex Code.
    color: "#808080"
```

然后我们将前面复制到的代码替换 `name` 字段中的 `user`，并且需要在前面加上 `fas`，需要爱心跳动，那么我们需要修改 `animated` 字段为 `true`，修改图标 `color` 为红色 `#ff0000`，这部分修改完如下所示：

```
footer:
  # Specify the date when the site was setup.
  # If not defined, current year will be used.
  #since: 2015

  # Icon between year and copyright info.
  icon:
    # Icon name in fontawesome, see: https://fontawesome.com/v4.7.0/icons
    # `heart` is recommended with animation in red (#ff0000).
    name: fas fa-heartbeat
    # If you want to animate the icon, set it to true.
    animated: true
    # Change the color of icon, using Hex Code.
    color: "#ff0000"
```

嘻嘻！刷新一下页面，看看是不是下面有一颗躁动的心脏啦~~

### 页面访问统计

我们仅仅需要非常简单地修改一个字段，就可以给页面添加页面的访问统计功能啦！！！
仍然是打开主题的配置文件 `themes/next/_config.yml`，搜索 `busuanzi_count`，可以看到以下内容：

```
busuanzi_count:
  enable: false
  total_visitors: true
  total_visitors_icon: user
  total_views: true
  total_views_icon: eye
  post_views: true
  post_views_icon: eye
```

我们只需要非常简单地将 `enable` 字段修改为 `true`，就可以为页面添加访问统计功能啦！修改好后刷新页面！就可以看到：

![add-page-count](https://11.tt/posts/2018/set-up-hexo-with-coding-and-github/add-page-count.png)

是不是真的很简单呀！！
**好啦~简单的美化部分就到这里啦！接下来我们要部署到 coding 和 Github 了哦！**

# 部署

我们要开始部署啦！
coding 是国内的，Github 是国外的，因为一些众所周知的原因，Github 时常会访问比较慢呀~
**这里不得不吐槽一下！coding 普通会员的访问速度真的很感人，非常慢…所以我还是开了个黄金会员…相比之下，Github 就比较良心了，挺快的…国内…好像也挺快的…**

## 部署到 coding

### 创建 coding 项目

首先，一如既往，先发一下 coding 的官网链接：[Coding (https://coding.net/)](https://coding.net/)
然后注册一个账号这我就不说了吧…
然后新建一个项目，项目名称为你注册时填写的用户名后面加上 `.coding.me`，比如我的就是 `miaia.coding.me`，其余部分默认就行啦！！！你信我！！！
**请非常非常注意！.coding.me 前面填写的用户名必须是你的用户名！！！不然是不能用的哦！！！真的注意！！！**

![create-coding-project](https://11.tt/posts/2018/set-up-hexo-with-coding-and-github/create-coding-project.png)

### 创建公钥

然后我们需要在本地创建一个公钥，打开 Git Bash，输入如下命令，然后回车回车回车直到创建完毕：

```
$ ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```

我们可以看到类似的输出如下：

```
$ ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
Generating public/private rsa key pair.
Enter file in which to save the key (/c/Users/Kiana/.ssh/id_rsa):
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /c/Users/Kiana/.ssh/id_rsa.
Your public key has been saved in /c/Users/Kiana/.ssh/id_rsa.pub.
The key fingerprint is:
SHA256:w5YVly1GUy+HlmBtBUd2wSigTK5LYrvH54LRPNzBGhs Kiana@Kiana
The key's randomart image is:
+---[RSA 4096]----+
|       . .o.*===B|
|      + .  =+++B.|
|      .+  ...o= o|
|     E.+ o   . o |
|   o+o* S        |
|  ..+B.o .       |
|   .+..          |
|   ..+ .         |
|   .. +.         |
+----[SHA256]-----+
```

我们可以看到生成的密钥在路径 `/c/Users/Kiana/.ssh/id_rsa.pub`，**我们直接打开这个文件，复制其中的所有内容**
然后复制到下图中并保存，这下你就可以随时进行 commit 啦！

![add-coding-ssh](https://11.tt/posts/2018/set-up-hexo-with-coding-and-github/add-coding-ssh.png)

### 配置库名并提交

我们打开根目录下的 `_config.yml`，搜索 `deploy`，可以看到以下内容：

```
deploy:
  type:
```

我们需要将其修改为类似这样的形式：

```
deploy:
- type: git
  repo: 
    coding: git@git.coding.net:miaia/miaia.coding.me.git
```

然后我们需要安装一个提交的插件，在 Git Bash中输入：

```
$ npm install hexo-deployer-git --save
```

在安装完后，输入命令：

```
$ hexo clean && hexo g && hexo d
```

正常情况下，我们就可以非常顺利地提交到库中啦！
这时候访问 `用户名.coding.me`，就可以看到你的页面咯！

### coding 配置自定义域名

进入到项目页面，选择侧边栏中的 “Pages 服务”，如图：

![choose-pages-service](https://11.tt/posts/2018/set-up-hexo-with-coding-and-github/choose-pages-service.png)

然后我相信你看到页面上的提示，能够非常容易地添加自定义域名并开启 https 哒！

![add-coding-domain](https://11.tt/posts/2018/set-up-hexo-with-coding-and-github/add-coding-domain.png)

## 部署到 Github

### 创建 Github 仓库

首先，一如既往，先发一下 Github 的官网链接：[Github (https://github.com/)](https://github.com/)
然后注册一个账号这我就不说了吧…
然后新建一个仓库，项目名称为你注册时填写的用户名后面加上 `.github.io`，比如我的就是 `miaia.github.io`，其余部分默认就行啦！！！你信我！！！
**请非常非常注意！.github.io 前面填写的用户名必须是你的用户名！！！不然是不能用的哦！！！真的注意！！！**

![create-github-repository](https://11.tt/posts/2018/set-up-hexo-with-coding-and-github/create-github-repository.png)

### 添加公钥

我们前面已经在本地创建了一个密钥，生成的密钥在路径 `/c/Users/Kiana/.ssh/id_rsa.pub`。
**我们再次打开这个文件，复制其中的所有内容**
然后复制到下图中并保存，这下你就可以随时进行 commit 啦！

![add-github-ssh](https://11.tt/posts/2018/set-up-hexo-with-coding-and-github/add-github-ssh.png)

### 配置库名并提交

我们再次打开根目录下的 `_config.yml`，搜索 `deploy`，可以看到以下内容：

```
deploy:
- type: git
  repo: 
    coding: git@git.coding.net:miaia/miaia.coding.me.git
```

我们需要在其中添加 Github 库的地址，所以需要将其修改为类似这样的形式：

```
deploy:
- type: git
  repo: 
    coding: git@git.coding.net:miaia/miaia.coding.me.git
    github: git@github.com:miaia/miaia.github.io.git
```

在安装完后，输入命令：

```
$ hexo clean && hexo g && hexo d
```

正常情况下，我们就可以非常顺利地提交到两个库中啦！
这时候访问 `用户名.github.io`，就可以看到你的页面咯！

### Github 配置自定义域名

进入到项目页面，选择 “Settings”，在该页面中往下拉找到如图位置：

![add-github-domain](https://11.tt/posts/2018/set-up-hexo-with-coding-and-github/add-github-domain.png)

然后我相信你看到页面上的提示，能够非常容易地添加自定义域名并开启 https 哒！

## 配置解析

**这里我们要单独说一下 CNAME 解析的问题，因为我们要实现国内外访问不同的服务，所以需要分别设置解析，无论是腾讯云还是阿里云的解析服务，都能够很好滴区分国内外节点，我们需要将国内的 CNAME 设置到 pages.coding.me，将国外的 CNAME 设置到 pages.github.io，然后你就会发现，非常神奇啦！**

------

# 总结

到这里，我们整个的建立和部署都已经基本完成了，我会把其他的诸如添加评论、SEO优化、谷歌分析等其他细节内容在下一篇日志当中给出，希望大家看了我的这个文章能够顺利滴搭建起自己的博客！当然也可以给我留言哦！结文撒花✿✿ヽ(°▽°)ノ✿

坚持原创！欢迎各位客官给我打赏买小饼干吖！✿✿ヽ(°▽°)ノ✿

- **本文作者：** Miaia
- **本文链接：** <https://11.tt/posts/2018/set-up-hexo-with-coding-and-github/>
- **版权声明：** 本博客所有文章除特别声明外，均采用[ BY-NC-SA](https://creativecommons.org/licenses/by-nc-sa/4.0/) 许可协议。转载请注明出处！