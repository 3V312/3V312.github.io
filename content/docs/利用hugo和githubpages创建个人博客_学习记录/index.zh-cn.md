---
date: 2026-07-10T00:00:00+08:00
title: 利用hugo和githubpages创建个人博客_学习记录
weight: 13
draft: false
description: "结构还是有一些混乱，复盘还是很有用的"
slug: "hugo-github-pages"
tags: ["hugo", "githubpages", "个人博客创建","个人博客","复盘"]

---
# hugo学习记录

# Hugo 学习记录（2026年1月22日-27日）

22号开始搞这个个人博客，现在是27号。一直断断续续的搞，每天解决一步，这样其实挺好的。

每当我尝试从互联网上新学习什么东西，总是先去看B站的教程，其实这样不好。因为那些教程并不会教给你为什么这样做，为什么不这样做，因为大家都是分享，也没有教学的宏愿。

**主要学习渠道：**

- B站教程
- Blowfish官方文档（[入门指南 · Blowfish](https://blowfish.page/zh-cn/docs/getting-started/)）
- DeepSeek（DS）

**核心问题：** 一切的一切要从我选择和UP主不一样的主题开始。之前说了他只是分享，没有讲原理。而不同主题之间的差异比我所能接受的还要大，所以我只能不断出错，粘贴错误给DS，乱修改或者乱重启，直到表面看起来勉强可以接受。

---

## 第一部分：环境配置

### 1. 下载 Hugo

**❌ 我的错误操作：**

- 把Hugo下到了G盘的download文件夹里然后直接解压
- 按理说download都是放不重要的文件的

**🤔 我的困惑：**

在之后某一次我尝试重来的时候，我先后把Hugo挪到C盘和G盘。挪到G盘时，说需要权限（此时我尝试修改也失败了），挪到C盘之后，说【Hugo】文件夹的外面应该还有一个文件夹，这时似乎仍然提示无权限。

**💡 DS的解答：**

#### 问题1：到底放在哪？

重要的不是放在哪，而是全局路径（PATH）里面有没有它。放在**任何一个你能稳定访问、且愿意手动添加到系统PATH的文件夹**里都可以。

#### 问题2：有没有权限有什么区别？怎么获得权限？

**获取权限的方法：**

| 场景 | 操作方法 | 说明与警告 |
| --- | --- | --- |
| 临时获得一次最高权限 | 以管理员身份运行当前程序 | 1. 找到程序（如文件资源管理器、命令行）
2. 右键点击其图标 → 选择"以管理员身份运行"
3. 在弹出的UAC窗口中点击"是"
此后，这个窗口内的操作暂时拥有最高权限 |
| 永久为自己添加对某个文件夹的权限 | 手动修改文件夹的安全属性 | 1. 右键点击目标文件夹 → "属性"
2. 切换到"安全"选项卡
3. 点击"编辑" → "添加" → 输入你的用户名 → 勾选"完全控制" → 确定
⚠️ 警告：对系统目录这样做会降低安全性，强烈不推荐 |
| 一劳永逸避免权限问题 | 将软件安装/放置在你已有完全控制权的个人文件夹中 | 例如，在C:或D:盘根目录下新建一个Tools或Dev文件夹。你在此类自己创建的文件夹中天生拥有"完全控制权"，永远不需要额外获取权限 |

#### 问题3：所以有权限就等于有路径吗？

**答案：不等于**

| 条件 | 结果 | 原因分析 |
| --- | --- | --- |
| 有权限，无路径 | 在任何地方输入hugo version，会显示"命令未找到" | 系统不知道hugo这个命令在哪里。你有权操作文件，但系统找不到它 |
| 有路径，无权限 | 可以运行hugo version，但创建文件等操作时可能失败 | 系统知道命令在哪，但当你试图在受保护的目录里创建、修改文件时，会被权限墙挡住 |

**✅ 结论：你需要同时满足两个条件**

1. **位置上有权限**：把hugo.exe放在一个你天生拥有"完全控制权"的文件夹里（如C:Tools、D:Hugo）
2. **系统中有路径**：把上述文件夹的路径添加到系统的PATH环境变量

---

### 2. 下载 Git

**🤔 我的困惑：**

其实我感觉我用到的都是GitHub而没有用到Git，但是其实Git是啥我也不知道。所以这个Git到底起到怎样的作用呢？

**💡 DS的解答：**

#### 问题1：Git和GitHub都是起了什么作用？

Git用于本地版本控制（管理博客源码），GitHub Pages用于托管编译后的静态网页。

**本地版本控制的意思：** 就是把你要传的东西传到GitHub上。

#### 追问：那我没有Git就不能用GitHub吗？

但是我之前一直用GitHub存代码很正常啊，也是在终端里用Git指令的。

**DS的回答：** 那个是IDE里集成的Git，要在本地命令行用Git指令的话就是需要安装Git的（也许这就是我一直没有给Git设置上代理的原因）。

#### 追问：在命令行安装Git，这是什么原理？

我似乎在命令行安装过Git，也在命令行安过Blowfish的助手，这是什么原理？

| 对比项 | npx blowfish-tools（临时调用工具） | winget install Git.Git（安装系统软件） |
| --- | --- | --- |
| 核心动作 | 临时调用并执行 | 永久安装到系统 |
| 存储位置 | 临时缓存（通常为系统npm缓存目录） | 系统程序目录（如C:Program FilesGit） |
| 留存状态 | 执行后主要代码不保留，可清理缓存 | 完整保留，成为系统的一部分 |
| 使用方式 | 每次需在项目目录重新执行完整命令npx... | 安装后，可在任意目录直接使用命令git |
| 更新机制 | 每次执行自动尝试获取最新版本 | 需手动执行更新命令或重新运行安装程序 |
| 依赖环境 | 需提前安装Node.js（包含npm/npx） | 只需操作系统本身 |
| 类比 | 像是在网上租用一个工具，用完即还 | 像是在电脑城购买一个工具，放在自家工具箱里随时取用 |

**一句话总结：**

- npx是"临时借用并执行"，用完即走
- winget、官网安装包是"永久安装到系统"，一劳永逸

#### 问题2：为什么安装了梯子之后，Git还需要经过配置才能使用呢？

**配置本质：**

```
git config --global http.proxy http://127.0.0.1:6666
git config --global https.proxy http://127.0.0.1:6666
```

[或使用SSH（git@github.com](mailto:或使用SSH（git@github.com):...）绕过HTTP代理问题。

**配置的作用：**

这两条命令告诉Git：

- 通过HTTP协议访问远程仓库时，使用指定的代理服务器
- 通过HTTPS协议访问远程仓库时，同样使用指定的代理服务器

**具体参数含义：**

```
git config --global http.proxy http://127.0.0.1:6666
git config --global https.proxy http://127.0.0.1:6666
```

- `git config`：用来设置、查看、修改Git配置的核心命令。Git的所有行为都通过这些配置来控制
- `--global`：全局配置，对当前用户的所有仓库生效
- [`http://127.0.0.1:6666`](http://127.0.0.1:6666)：代理服务器地址和端口
- `127.0.0.1`：本地回环地址（[localhost](http://localhost)）
- `:6666`：代理软件监听的端口（如1080、7890等），这是从你的设置→代理里面获得的，并不是机场给的那个端口

**简单来说：** 需要GitHub到电脑，电脑的梯子（也就是那个软件）来处理，转发给机场的代理服务器。机场给的端口就是这个远程的代理服务器端口。GitHub并不知道怎么上远程服务器（没有加密和协议转换），当然是连接不到的。

#### 问题3：SSH是什么？

我已经知道我需要从GitHub的settings去生成一个，SSH同时似乎代表着一个在C盘的文件和一串代码，这个为什么可以绕过HTTPS呢，有什么不同？什么好处吗？

**SSH基本概念：**

SSH（Secure Shell）是一个加密的网络协议，用于安全地远程访问和管理计算机。在Git中，SSH提供了另一种访问GitHub的方式。

**SSH的"三个部分"：**

1. **SSH密钥对（C盘的文件）**

```
# 通常位置
~/.ssh/id_rsa       # 私钥（私有的，绝不给别人）
~/.ssh/id_rsa.pub   # 公钥（公开的，给GitHub）
```

- 私钥：在你电脑上，证明"你是你"
- 公钥：给GitHub，用来验证你的私钥
1. **GitHub配置（settings中的配置）**
- 把公钥内容复制到GitHub → Settings → SSH and GPG keys
- GitHub会说："好的，这个公钥对应xx用户"
1. **SSH地址（一串代码）**

格式：[`git@github.com](mailto:git@github.com):用户名/仓库.git`

**SSH vs HTTPS的本质区别：**

| 方式 | 协议 | 认证方式 | 端口 | 特点 |
| --- | --- | --- | --- | --- |
| HTTPS | HTTP协议 + SSL/TLS加密 | 每次操作都需要用户名+密码（或token） | 443端口 | 容易被网络代理/防火墙拦截 |
| SSH | SSH协议 | 使用密钥对认证（一次配置，永久使用） | 22端口 | 更难被拦截 |

**SSH设置全流程：**

**第1步：生成SSH密钥**

```
# 检查是否已有
ls -al ~/.ssh

# 生成新密钥（一路回车）
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"

# 或更简单的
ssh-keygen -t ed25519 -C "your_email@example.com"
```

**第2步：复制公钥到GitHub**

```
# Windows
cat ~/.ssh/id_ed25519.pub
# 或直接打开文件 C:\Users\你的用户名\.ssh\id_ed25519.pub

# 复制全部内容，包括开头的ssh-ed25519和结尾的邮箱
```

**第3步：添加到GitHub**

1. 登录GitHub → Settings
2. SSH and GPG keys → New SSH key
3. 粘贴公钥，取个名字（如"My Laptop"）

**第4步：测试连接**

```
ssh -T git@github.com
# 看到这个表示成功：
# Hi 用户名! You've successfully authenticated...
```

**为什么SSH可以绕过HTTP代理配置？**

HTTP/HTTPS方式：

```
# 需要明确告诉Git代理地址
git config --global http.proxy http://127.0.0.1:7890
```

翻译："Git，请把所有HTTP流量发给127.0.0.1:7890这个代理"

SSH方式：

```
# 什么都不用配置！直接使用SSH地址
git clone git@github.com:username/repo.git
```

Git自动知道："哦，这是SSH地址，我该用系统的SSH客户端去连接"

**工作流程对比：**

**HTTP/HTTPS流程（需要配置代理）：**

```
[你的Git命令]
    ↓
[Git程序]
    ↓ 检查配置：发现http.proxy=http://127.0.0.1:7890
[发送请求到127.0.0.1:7890]
    ↓
[代理软件] ← 正在监听7890端口！
    ↓
[代理软件处理请求]
    ↓
[转发到机场服务器]
    ↓
[机场服务器访问GitHub]
```

**SSH流程（什么都不用配）：**

```
你的Git命令
    ↓
[Git] → "我要访问git@github.com..."
    ↓
检查地址格式 → 是SSH格式！
    ↓
直接调用系统SSH客户端
    ↓
[系统SSH] → 连接github.com:22
    ↓
使用~/.ssh/id_rsa私钥认证
    ↓
成功连接，传输数据
```

**SSH也需要配代理（如果SSH被墙）：**

需要在`~/.ssh/config`中添加：

```
Host github.com
    ProxyCommand nc -X 5 -x 127.0.0.1:7890 %h %p
```

操作方法：在SSH的统一文件夹里面新建一个config文本文件，里面写：

```
Host github.com
    ProxyCommand nc -X 5 -x 127.0.0.1:7890 %h %p
```

**总结要点：**

1. HTTPS方式：需要配置Git代理，走443端口，容易被拦截
2. SSH方式：使用密钥认证，走22端口，更难被拦截
3. SSH配置：一次配置永久使用，Git里不需要额外配置
4. 代理原理：HTTPS代理在Git中配置，SSH代理在SSH配置文件中配置
5. 核心区别：HTTPS需要每次验证，SSH一次配置永久使用

---

### 3. 尝试使用 Blowfish-tools

#### 第一次尝试

**❌ 我的报错：**

```
D:\download\hugo_extended_withdeploy_0.154.5_windows-amd64>blowfish-tools new mynewsite
'blowfish-tools' 不是内部或外部命令，也不是可运行的程序或批处理文件
```

**💡 DS的解答：**

因为没有下载blowfish-tools，使用`npm install -g blowfish-tools`来下载。

另一种方法是拉取，然后使用交互式页面`npx blowfish-tools`。注意拉取要重复拉取，不是下载。

#### 第二次尝试：使用交互式页面

拉取后出现交互页面：

```
Welcome to Blowfish tools.
I can help you setup a new project from scratch or configure an existing one (or both).
Please choose one of the options below, start typing to search.
√ What do you need help with? · Setup a new website with Blowfish
```

做出选择是完全重新创建一个新站点

**❌ 报错：**

```
√ Hugo is available
√ Git is available
√ Where do you want to generate your website (. for current folder)? · newSite
√ Folder ok...
√ Hugo site created
√ Git initialized
× Failed to install Blowfish. Please check your network connection and try again.
You can try manually running: git submodule add --depth 1 -b main https://github.com/nunocoracao/blowfish.git themes/blowfish
```

**🤔 我的困惑：**

当时是咋解决的呢，我记得的就是，我出来看了下文件夹发现newsite这个文件夹没有hugo.exe。然后改了一下，把外面的hugo.exe复制了一份进来。因为newsite是它自己创建的所以我之前没复制自然就没有，其实教程里有嘱咐说要复制一个进来，我也不懂为啥要复制，可能还是路径吧。这个好像也不是原因。

这个blowfish其实这里是要链接git下载blowfish吧，还有一个可能是没有git.init，我现在也不明白，git.init到底应该在哪一级文件，它会创建什么文件夹吗？又创建到哪里呢？

**💡 DS的解答：**

**错误原因分析：**

错误提示让你手动运行`git submodule add...`，这说明问题出在Git操作上，而不是网络或Hugo。

结合你的记忆，根本原因很可能是这两点之一或同时发生：

- **原因A：缺少hugo.exe（你的记忆）**：`blowfish-tools`在创建Hugo站点后，需要立即运行一次hugo命令来验证。如果系统PATH里没有hugo，或者项目目录里没有hugo.exe，这个验证失败就会导致整个流程中断，还没执行到添加主题那步就报错了。你复制hugo.exe进来，实际上是为这个"验证"步骤提供了本地可执行文件。
- **原因B：Git未正确初始化（你的猜测）**：`git submodule add`命令必须在Git仓库目录中执行。虽然工具显示`√ Git initialized`，但如果初始化过程被中断或未在正确的子目录（newSite）中完成，也会导致后续步骤失败。

**git init到底在干嘛？它应该在哪一级？**

- **作用**：`git init`会在你执行该命令的当前目录下，创建一个名为`.git`的隐藏文件夹。这个文件夹就是Git仓库的"数据库"，记录所有版本历史。没有它，任何git命令（包括`submodule add`）都无法执行。
- **应该在哪一级**：必须在你的Hugo博客项目根目录下执行。
    - 错误的位置：在D:或包含很多其他文件的目录下运行`git init`
    - 正确的位置：进入你的项目文件夹（例如D:newSite），然后在这里运行`git init`。这样，`.git`文件夹就会创建在D:newSite.git\
- **如何检查**：打开你的newSite文件夹，在"查看"选项中勾选"隐藏的项目"。如果里面有一个`.git`文件夹，说明Git已正确初始化。如果没有，就是原因B。

**关于"复制hugo.exe"：**

教程里嘱咐复制hugo.exe到项目文件夹，是一个古老的、临时的变通方法，并不是标准做法。标准且一劳永逸的做法是：将hugo.exe所在目录（如C:Hugo）添加到系统PATH环境变量。这样，你在电脑的任何地方（包括newSite文件夹）输入hugo，系统都能找到它，完全不需要复制。

你复制后解决了问题，恰好证明了当时系统PATH没配好，工具无法调用全局的hugo命令。

**✅ 标准流程重做：**

1. **确保Hugo和Git在全局可用**（在任意文件夹打开命令行都能运行）：

```
hugo version
git --version
```

两者都必须成功显示版本号。如果hugo报错，回去将hugo.exe的目录添加到系统PATH。

1. **用blowfish-tools new一键创建**（先安装或者拉取tools）：

```
# 1. 先进入你希望创建项目的父目录（比如D盘根目录）
cd /d D:\

# 2. 运行一键创建命令，myblog是你的项目名
blowfish-tools new myblog
```

这个命令会自动化所有步骤，并处理内部的依赖关系，比交互菜单更健壮。

如果不想全局安装blowfish-tools，也可以用npx执行这个命令：

```
npx blowfish-tools new myblog
```

#### 第三次尝试：手动下载主题

但是在当时，我手动下载了blowfish主题，就是从网站上解压下来。然后放在文件夹里。它解压的时候会出现一个blowfish/blowfish/巴拉巴拉的结构，我每次还要重新整理成blowfish/巴拉巴拉。

**隐患：** 直接解压下来的blowfish是有版本号后缀的，它的文件名不是blowfish，这导致它找不到blowfish主题。不过这是之后的问题。

**🤔 我的困惑：**

我当时觉得它链接git就是为了下载blowfish吗，我猜到git链接有问题（即使我有代理），就想规避，直接下载blowfish。但是链接git下载好像是不一样的，可以自动更新还是什么？我也不知道咋实现的自动更新，也不知道最后连接成功没有。

**💡 DS的解答：**

| 方式 | 本质 | 优点 | 缺点 |
| --- | --- | --- | --- |
| git submodule add ... | 在你的仓库里建立一个指向Blowfish官方Git仓库的固定链接（指针） | 1. 自动更新：在主目录执行git submodule update --remote，可一键将主题更新到最新版本
2. 版本追踪：记录使用的是哪个具体提交，确保项目稳定 | 依赖网络和Git配置，对新手可能因代理等问题失败 |
| 手动下载解压 | 把某个时刻的源代码快照复制到你的themes文件夹里 | 简单直接，无需网络，绕过所有Git问题 | 1. 无法自动更新：官方修复Bug或发布新功能后，你需要手动重新下载、解压、覆盖
2. 容易出错：解压后的文件夹名（如blowfish-3.0.0）需要手动改为blowfish |

**结论：** `git submodule`是"建立动态链接"，手动下载是"复制静态副本"。

**Git子模块的实现原理：**

当你成功运行`git submodule add ...`后：

1. 在你的项目里，`themes/blowfish`文件夹看起来是空的，但其实里面有一个隐藏的`.git`文件，里面只记录了一个提交ID
2. 这个ID指向了官方仓库的某个特定版本
3. 当你需要更新时，Git会根据这个链接，去官方仓库拉取新的代码到这个文件夹

#### 第四次尝试：手动下载后想回到CLI工具

**🤔 我的困惑：**

我现在手动成功下载了，但是怎么回到CLI这个工具呢？此时我还不明白npx不是安装是拉取，必须得重复npx。

在我手动下载后，又尝试使用tools，我觉得既然它提示我手动下载，那么我下载之后它应该能检查到然后进行下一步，但是没有，可能是此时我还是没有把版本号去掉，它没找到。

**❌ 报错：**

```
D:\download\hugo_extended_withdeploy_0.154.5_windows-amd64\newSite> git submodule add --depth 1 -b main https://github.com/nunocoracao/blowfish.git themes/blowfish
Cloning into 'D:/download/hugo_extended_withdeploy_0.154.5_windows-amd64/newSite/themes/blowfish'...
fatal: unable to access 'https://github.com/nunocoracao/blowfish.git/': Failed to connect to github.com port 443 after 21387 ms: Could not connect to server
fatal: clone of 'https://github.com/nunocoracao/blowfish.git' into submodule path 'D:/download/hugo_extended_withdeploy_0.154.5_windows-amd64/newSite/themes/blowfish' failed
```

**💡 DS的解答：**

Git说："我正在尝试把仓库克隆（下载）到这个指定的文件夹里。""无法访问这个网址。"

其原因就是没有给git配置代理，详情见第二大条。

#### 问题：git config命令在哪里打？

```
git config --global http.proxy http://127.0.0.1:12690
git config --global https.proxy http://127.0.0.1:12690
```

这个命令在哪里打呢？就是哪一级文件？

**💡 DS的解答：** 任何地方，git代理配置是全局配置。

**❌ 我的错误：** 用的机场提供的端口，应该用自己的端口（详情见前面第2部分）。

#### 第五次尝试：重新来过

但当时我并不知道，于是我决定重新来过，也不是重新来过。我在之前没有使用tools，仅仅是手动下载差不多成功了一个网站，可以在1313打开，但是我发现打开后只有个蓝色背景，啥也没有，B站教程不能适用这个主题，我还是不知道怎么修改主页布局怎么发文章。

当时我想，既然这里已经差不多成功了，我临时使用tools，不选择重新创建，选择一些修改作者啊啥的不就好了，于是在hugo2运行npx。

**交互界面：**

```
Welcome to Blowfish tools.
I can help you setup a new project from scratch or configure an existing one (or both).
Please choose one of the options below, start typing to search.
? What do you need help with? ...
Enter full configuration mode (all options)
Configure menus
Configure overall site
Configure site author
Configure homepage
...
```

这一次没有选重新创建，我选择了修改模板。

```
Welcome to Blowfish tools.
I can help you setup a new project from scratch or configure an existing one (or both).
Please choose one of the options below, start typing to search.
√ What do you need help with? · Start from a configured template
√ Hugo is available
√ Git is available
? Select your template - more info at https://blowfish.page/examples/ ...
Blowfish Template - A simple template to get you started
Blowfish Artist - A artist portfolio template
Blowfish Lowkey - A low key template
Blowfish Lite - Lite configuration for a clean blog
```

此时它报告git、hugo都可以用，我还以为是这个hugo2的基础，其实是之前把hugo配置对了，git也能找到但还是连不上的。

**🤔 我的困惑：**

```
? Where do you want to generate your website (. for current folder)? » newSite
```

在我选择了一个主题后它这么提示，这说明它根本没有识别到我们正在一个用blowfish的站点里？DS让我输入一个点，在现有的newsite里配置，而不是hugo2吗？

```
√ Where do you want to generate your website (. for current folder)? · .
× Directory already exists and is not empty.
```

而newsite是一个混乱的残渣，所以失败了。

**💡 DS的解答：**

**重要发现：Start from a configured template仍然是新建，不是从已有的基础开始，我完全误解了！**

你当时在hugo2文件夹，里面有一个你手动搭建、能用但很简陋的站点。你希望工具来"优化"或"完善"它。

1. **你的期望**："我选'从模板开始'，然后工具会扫描我当前文件夹，发现这里有个Hugo站点，然后给我一些选项，让我套用一个好看的模板来'装修'它。"
2. **工具的逻辑**："用户选了'从模板开始'，这意味着我要无视当前文件夹里的一切，按照我内置的某个模板（如'Blowfish Lowkey'），重新创建一个完整的、全新的网站文件结构，并放到用户指定的文件夹里。"
3. **矛盾的爆发**：当你输入`.`（当前目录）时，工具开始执行它的剧本：准备往里写入一整套新文件。但它首先检查了一下目标文件夹（`.`也就是你的hugo2文件夹），发现里面不是空的（有你之前手动创建的所有文件）。于是它立刻报错并停止，因为它被设计为不能在非空文件夹执行"创建"操作，以防覆盖用户的未知文件。

**为什么工具"视而不见"你手动下载的主题？**

因为工具在"创建新项目"的流程里，blowfish主题不是以"文件夹"的形式被识别，而是以"Git子模块"的形式被添加的。它的流程是：

1. `git init` → 2. `git submodule add ...` → 3. 拉取主题代码

当你手动把一个blowfish文件夹放在themes里时，在工具的"创建逻辑"里，这等于什么都没有。它不会去检查themes里有没有现成的东西，它只认"通过Git子模块命令添加"这一种方式。

**🤔 我的困惑：** 其实当时是从Configure an existing project选项进去的，应该默认是存在project的，不知为何这样。

#### 第六次尝试：重新进入CLI

**交互记录：**

```
Welcome to Blowfish tools.
I can help you setup a new project from scratch or configure an existing one (or both).
Please choose one of the options below, start typing to search.
√ What do you need help with? · Start from a configured template
√ Hugo is available
√ Git is available
√ Select your template - more info at https://blowfish.page/examples/ · Blowfish Lowkey - A low key template
√ Where do you want to generate your website (. for current folder)? · newSite
√ Folder ok...
√ Template cloned

Found existing Git repository at: D:\download\hugo_extended_withdeploy_0.154.5_windows-amd64\newSite
? How would you like to handle Git? ...
> Use existing repository (recommended for subdirectories)
  Create new repository in ./newSite
```

**🤔 我的困惑：** 这是什么意思？

```
√ How would you like to handle Git? · Use existing repository (recommended for subdirectories)
✔ Using existing Git repository
× Failed to install Blowfish. Please check your network connection and try again.
You can try manually running: git submodule add --depth 1 -b main https://github.com/nunocoracao/blowfish.git themes/blowfish

D:\download\hugo_extended_withdeploy_0.154.5_windows-amd64\newSite>git submodule add --depth 1 -b main https://github.com/nunocoracao/blowfish.git themes/blowfish
Cloning into 'D:/download/hugo_extended_withdeploy_0.154.5_windows-amd64/newSite/themes/blowfish'...
fatal: unable to access 'https://github.com/nunocoracao/blowfish.git/': Failed to connect to github.com port 443 via 127.0.0.1 after 2058 ms: Could not connect to server
fatal: clone of 'https://github.com/nunocoracao/blowfish.git' into submodule path 'D:/download/hugo_extended_withdeploy_0.154.5_windows-amd64/newSite/themes/blowfish' failed
```

**💡 DS的解答：**

**为什么"模板下好了"最后又让下载Blowfish？**

这里有一个重要的概念区分：模板（Template）和主题（Theme）不是同一个东西。

- **模板**：是别人已经配置好的一个完整项目示例，包括hugo.toml、content/里的示例文章、assets/里的图片等。它就像一个精装修的样板间。
- **主题**：是Hugo的皮肤，是实现所有页面布局和功能的代码包。它就像毛坯房的建筑结构和水电图纸。

**流程真相：**

1. `Template cloned`的意思是：工具从某个地方（通常是GitHub上的另一个仓库）成功下载了你选择的"样板间"（比如Blowfish Lowkey模板包）
2. 但是，这个"样板间"自己不包含建筑图纸（主题代码）。它只是在配置文件里声明了"我需要使用blowfish这个主题"
3. 因此，工具在布置完"样板间"后，必须执行下一步：去主题的官方仓库拉取主题代码（建筑图纸）。这就是为什么最后又报错`Failed to install Blowfish`

**结论：** 你下载的只是一个"配置壳子"，主题代码仍需单独拉取。

**"How would you like to handle Git?"是什么意思？**

这个提示说明，在你指定的newSite文件夹的上级目录（即D:downloadhugo_extended...），工具发现了一个已经存在的`.git`仓库。

它给你两个选择：

- `Use existing repository`：使用这个已存在的仓库。这意味着newSite将成为这个大仓库里的一个子目录。这通常适用于你希望在一个大的Git项目下管理多个站点（不推荐新手选这个）
- `Create new repository in ./newSite`：在newSite文件夹内创建一个全新的、独立的Git仓库。这才是博客项目的标准做法，让每个项目独立管理版本

**实际上最外面hugo文件夹不应该有大仓库。**

#### 终于成功！

然后我终于发现端口要用自己的电脑的，就成功了。

```
D:\download\hugo_extended_withdeploy_0.154.5_windows-amd64\newSite>git config --global --get http.proxy
socks5://127.0.0.1:10808

D:\download\hugo_extended_withdeploy_0.154.5_windows-amd64\newSite>git submodule add --depth 1 -b main https://github.com/nunocoracao/blowfish.git themes/blowfish
Cloning into 'D:/download/hugo_extended_withdeploy_0.154.5_windows-amd64/newSite/themes/blowfish'...
remote: Enumerating objects: 2180, done.
remote: Counting objects: 100% (2180/2180), done.
remote: Compressing objects: 100% (1204/1204), done.
Receiving objects: 15% (337/2180), 11.93 MiB | 450.00 KiB/s
```

成功了！

**🤔 我的困惑：** 我还是不太明白，我已经手动解压安装包在newsite的themes里下载了啊，现在这又是在干嘛呢？

**💡 DS的解答：** 现在这就是有两个blowfish，我后来把手动下载的那个删掉了，也就删掉了模板。所以最后还是我自己手动装修的。

#### CLI工具配置菜单问题

接下来CLI可以使用了，按理说：

```
√ What do you want to configure?
Open your browser at http://localhost:1313 to see live changes
Start typing to search for options or scroll down to see all options · Configure menus
File ./config/_default/menus.en.toml does not exist.
```

**🤔 我的困惑：** 但是我想配置菜单的时候，说它不存在，一开始以为是后缀没有toml，当然是有的，看不到是windows隐藏了。

最后我发现可能是因为没有复制hugo.exe，复制之后就成功了，但是复制和存不存在有什么关系？

**💡 DS的解答：**

当`blowfish-tools`选择`Configure menus`时，它的工作逻辑是：

1. 目标明确：它要在你的博客项目根目录（即newsite）下创建或修改配置文件
2. 标准路径：它认为标准的配置文件应该放在`newsite/config/_default/`下
3. 执行动作：它调用hugo命令，在`newsite/config/_default/`下生成一个全新的、空的`menus.en.toml`文件，或者如果已存在就修改它

**关键点：** 工具不会、也不应该去主题的`exampleSite/`目录里找文件。它要操作的是你网站的配置，而不是主题的示例。它生成的路径是：

```
你的newsite项目/
└── config/              <-- 工具要操作的是这里
    └── _default/
        └── menus.en.toml  <-- 工具要创建或修改的文件在这里
```

**结论与启示：**

1. 你没错，工具也没错：你之前手动看到的是主题的示例，而工具要创建的是你站点的实际配置。这是两个不同的文件，放在两个不同的地方
2. 最佳实践：标准的Hugo项目结构要求你将所有自定义配置都放在自己项目的config/目录下（即newsite/config/），从而与主题文件（themes/blowfish/）完全分离。这样，更新主题时才不会覆盖你的个人设置
3. 为什么需要hugo.exe：因为生成这个符合Hugo标准结构的`config/_default/menus.en.toml`文件，是hugo这个"引擎"的内部功能。`blowfish-tools`只是触发这个功能的"遥控器"

所以，当工具报错`File ... does not exist`时，它的完整意思是："在你网站的`config/_default/`目录下，没有找到我应该操作的`menus.en.toml`文件，现在我要调用Hugo来创建一个，但连Hugo都找不到，所以彻底失败了。"

后来我根据DS的建议把blowfish里的那个config直接复制到newsite里了。

```
√ What do you want to configure?
Open your browser at http://localhost:1313 to see live changes
Start typing to search for options or scroll down to see all options · Configure menus
√ What do you want to configure? start typing to search for options. · View Menus
Header menus:

Footer menus:

√ What do you want to configure? start typing to search for options. · Create new menu
√ Do you want to create a header or footer menu? · header
? What is the name of the new menu? (you can leave this empty and select an icon after) »
```

然后成功了。

**🤔 我的困惑：** 然后在我的一通设置之下，按理说可以热重载的，但是没有，甚至显示find no page。当时搞不懂直接手动修改去了，发现可以。现在来说，可能因为我的config不是它创建的，是我复制出来的，它没找到？那它让我选什么也没报错？

**💡 DS的解答：**

**问题根源：配置冲突与缓存**

1. 你复制的config：来自`blowfish/exampleSite/config/`。这是一个完整的、为示例网站设计的配置文件，里面已经预先定义好了一整套菜单、参数等
2. 你通过工具创建的新菜单：工具会试图把新菜单的配置追加到现有的菜单配置文件中
3. 冲突发生：这两个动作本身不冲突，但关键在于示例配置文件可能结构老旧，或者包含了一些与新版本工具不兼容的格式或字段。当你保存新菜单后，Hugo在重新构建网站时，可能会因为配置文件中的某些问题（比如格式错误、重复定义、无效参数）而构建失败或构建出空结果，导致浏览器显示`find no page`

**更关键的是：** Hugo的"热重载"严重依赖一个干净的构建过程。如果构建过程因配置错误而中断或产生警告，热重载就会失效。

**当时你"手动修改"为什么成功了？**

因为当你手动编辑配置文件时，你很可能：

1. 没有触动那些导致冲突的字段
2. 或者，你实际上是在清理和简化配置，移除了示例文件中复杂的、可能出错的部分，只保留了最核心、最干净的设置。这让Hugo能够顺利构建

**为什么工具让你选，而且没报错？**

`blowfish-tools`只是一个前端交互界面。它的职责是：

1. 收集你的输入（如"创建header菜单，名字叫XXX"）
2. 将这些输入转换成对应的TOML/YAML配置代码
3. 将这些代码块写入到配置文件里

它不负责检查你整个配置文件的语法是否对Hugo完全友好，也不负责检查写入后配置的全局有效性。只要它自己生成的那段代码格式正确，它就不会报错。

打个比方：工具像是一个"自动填表机"，它确保自己填的那几行字很工整，但它不检查你整张表格（配置文件）的其他部分是不是有涂改、有矛盾，或者表格的模板（Hugo版本）已经升级了。

**实际上从这开始，我就没咋用CLI这个工具了，就开始手动装修了。**

---

## 第二部分：手动装修

### 配置问题

**问题一：** 在config的hugo.toml里，没有把`theme = "blowfish"`取消注释，也就是说根本没有选择主题为blowfish。这一点误打误撞改掉了。

**问题二：** 此时newsite有两个入口，一个是newsite里的hugo.toml，一个是config，而我并不知道config可以作为入口，后来把外面的hugo.toml删了。

### 复制示例文件

然后，我问了DS这些文件夹都是干嘛的，发现blowfish的示例网站里那些文件夹都是满的，所以我抄了一些出来。

**主题文件夹与网站文件夹对应关系：**

| 主题里的文件夹 (themes/blowfish/) | 在你网站里的对应文件夹 (newSite/) | 关系与用途 |
| --- | --- | --- |
| exampleSite/ | config/ 和 content/ | 这是给你抄作业的！exampleSite里是主题作者写的示例网站，包含完整的配置和演示内容。你之前复制的config/_default/文件就来自这里 |
| layouts/ | layouts/ | 主题的HTML模板。如果你想深度自定义页面外观，可以从这里复制文件到你网站的layouts/进行修改 |
| static/ | static/ | 主题自带的CSS、JS、字体、图片等。你网站的static/是放你自己的资源 |
| archetypes/, assets/, data/, i18n/ | (你网站的同名文件夹) | 主题提供的默认模板、资源、数据、翻译。你可以选择性地使用或覆盖 |

**与你无关的文件夹：**

| 文件夹 | 说明 |
| --- | --- |
| .github/ | 主题在GitHub上的工作流配置（如自动测试、发布）。与你无关 |
| .vscode/ | 主题作者为VS Code编辑器推荐的设置。你可忽略 |
| images/ | 主题文档和截图用的图片。与你无关 |
| release-versions/ | 主题的版本发布记录。与你无关 |
| scripts/ | 主题开发用的构建脚本。与你无关 |
| .editorconfig, .prettierrc等 | 代码格式化配置，保证主题代码风格一致。与你无关 |
| blowfish logo | 主题的Logo文件 |

**⚠️ 重要：** 此时，我并没有意识到.github这个文件夹的重要性，所以我的newsite里面仍然没有，这个文件夹是来控制GitHub的自动上传的。

**核心原则：不要修改主题文件**

**记住这个黄金法则：永远不要直接修改`themes/blowfish/`里的任何文件。**

因为：

1. 更新会覆盖：当你运行`git submodule update`升级主题时，所有修改都会丢失
2. 正确做法是"覆盖"：
    - 想改配置？→ 复制`exampleSite/config/`到你网站的`config/`
    - 想改模板？→ 复制`layouts/`里的文件到你网站的`layouts/`
    - 想加资源？→ 放到你自己网站的`static/`或`assets/`

**⚠️ 我的错误：** 还有这一点我没注意，还是略微修改了。就是把原来主题的en语言全改成中文了，把作者名字什么的设置成我自己的。这也埋下了隐患。

### 短代码错误

**❌ 报错：**

```
D:\download\hugo\newSite>hugo server -D
WARN  Skip unknown config key "params.author"
Watching for changes in D:/download/hugo/newSite/archetypes, D:/download/hugo/newSite/assets, D:/download/hugo/newSite/content/{authors,docs,examples,guides,samples,...}, D:/download/hugo/newSite/data, D:/download/hugo/newSite/i18n, D:/download/hugo/newSite/layouts/{_default,partials,shortcodes}, D:/download/hugo/newSite/static, D:/download/hugo/newSite/themes/blowfish/archetypes, D:/download/hugo/newSite/themes/blowfish/assets/{css,icons,img,js,lib}, D:/download/hugo/newSite/themes/blowfish/data, ... and 5 more
Watching for config changes in D:\download\hugo\newSite\config\_default, D:\download\hugo\newSite\themes\blowfish\config.toml, D:\download\hugo\newSite\themes\blowfish\config\_default
Start building sites …
hugo v0.154.5-a6f99cca223a29cad1d4cdaa6a1a90508ac1da71+extended+withdeploy windows/amd64 BuildDate=2026-01-11T20:53:23Z VendorInfo=gohugoio

Built in 370 ms
ERROR error building site: assemble: failed to create page from pageMetaSource /docs/getting-started: "D:\download\hugo\newSite\content\docs\getting-started\index.zh-cn.md:68:1": failed to extract shortcode: template for shortcode "theme-switcher" not found
```

**🤔 我的困惑：** 这个错误是因为我把示例网站的content也抄过来了，但是相应的需要什么东西，按理说我也抄了，但是，没有链接到还是怎样，后来把这个content清空了。

**💡 DS的解答：**

**错误根源：复制了不属于你的"高阶内容"**

你把`exampleSite/content/docs/`整个复制过来，但其中一些文件（如`getting-started/[index.zh-cn.md](http://index.zh-cn.md)`）使用了主题提供的特殊功能，比如短代码`theme-switcher`。

**为什么你抄了还是报错？**

这个短代码`theme-switcher`很可能依赖：

1. 特定的布局文件：它可能位于`themes/blowfish/layouts/shortcodes/`中，这没问题，主题自带
2. 特定的JavaScript或CSS资源：需要被正确加载
3. 特定的网站配置：比如需要开启某个实验性功能，或者在`params.toml`中启用主题切换器

`exampleSite`作为一个功能完整的演示，它的`config/`里已经开启了所有这些高级功能。但你当时复制的`config/`可能不完整，或者你在修改（如改语言、作者名）时，无意中删除了或注释掉了启用这些高级功能的关键配置行。

**结论：** 你复制了依赖（使用了高级短代码的content），但没有完整复制或正确启用其前提条件（完整的配置和资源）。Hugo在处理页面时，发现引用了不存在的短代码模板，于是报错。

删了之后根据blowfish的教程装修了一下，这时候正常热重载。

### 发布文章问题

**🤔 我的困惑：**

此时，我试图发布文章，按照B站的教程，我应该用Hugo的交互系统来创建，就是`content\pages\myfirstlog`大概是这样，然后报错了。

我挨个打开文件夹，发现这个public文件夹好像是跟网站相关的。我从示例的config里面抄了一个docs过来，只留了里面一篇文章，然后网站正常显示。然后我好像又试图改回pages失败了。我发现在public的文件结构里有的是docs没有pages，我就继续用docs，然后把抄的那一篇的文章内容改成了我想要的，这一次成功了。虽然public里的文章标题和我的文章标题不一样。

注意它的文章要md格式，我是直接从Notion导出的，否则我真不知道哪里写md格式的文章。

**💡 DS的解答：**

**为什么用`content/pages`会失败，而`content/docs`会成功？**

这完全由Blowfish主题的配置决定的，和Hugo本身无关。

- **docs是主题预设的"内容类型"**：Blowfish主题在它的模板中，已经定义好了一个名为docs的内容类型（Content Type）。当你创建`content/docs/[xxx.md](http://xxx.md)`时，主题知道该用哪套布局（比如侧边栏导航、特定的页面样式）来渲染它。你从示例里抄来的配置，正好激活了这个类型
- **pages不是预设类型**：B站教程用的主题（通常是最基础的或另一个主题）预设了pages类型。但Blowfish默认没有。所以当你创建`content/pages/[myfirstlog.md](http://myfirstlog.md)`时，Hugo虽然能生成文件，但Blowfish主题找不到对应的布局模板，就会用最基础的布局或者直接报错，导致页面显示异常

**结论：** 内容文件夹的名字（docs、posts、pages）不是随意的，它必须和主题的设计相匹配。对于Blowfish，常用的官方内容类型是docs（文档）和posts（博客文章）。

**public文件夹是什么？为什么里面的结构和你的内容不一致？**

- **public/文件夹是"成品仓库"**：当你运行hugo命令（不带server）时，Hugo会把你的Markdown内容、主题模板、所有资源编译成最终的、纯静态的HTML/CSS/JS文件，并全部输出到这个文件夹。这就是你将来要上传到服务器（如GitHub Pages）的东西
- **结构不一致的原因**：Hugo在构建时，会根据配置文件和文章Front Matter（文件头部的YAML/TOML）对URL路径和文件结构进行重组。比如，一篇位于`content/docs/getting-started/[index.md](http://index.md)`的文章，最终在public/里可能会变成`public/docs/getting-started/index.html`。标题显示什么，也由Front Matter里的title字段决定，而不是文件名。所以，public/里的结构是编译后的结果，你不应该、也不需要直接去修改它

---

## 第三部分：GitHub Pages 托管

此时已经过了两三天吧，我的文章内容可以正常显示了，然后我就想要链接GitHub，好像是需要GitHub来提供一个对外的网站，否则博客只能在本地看。首先我去看了B站教程，是一个非自动的，就是不用GitHub Actions的。

### SSH vs HTTP 连接问题

**问题：** 这个出现的问题，首先是我又链接不上GitHub了，要重新设置代理，还是远程连接？

但是我跟着教程用SSH的方式远程连接了，然后好像不能用SSH，DS让我用HTTP，我就改了。说到这里，之后有一次我发现我的URL是HTTP不是HTTPS，这个可能也造成某种BUG。总之我改用HTTP方式远程链接之后成功了。

### 推送失败问题

**❌ 我的错误：**

首先是推送失败，这是因为没有远程连接上GitHub，但更重要的错误是我跟着教程只推送了public。后来DS说，仓库应该在newsite里。

```
D:\download\hugo\newSite\public>
D:\download\hugo\newSite\public>git push -u origin main
ssh: connect to host github.com port 22: Connection timed out
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.

D:\download\hugo\newSite\public>
```

一开始挣扎于SSH，还额外在你的`.ssh`文件夹中（`C:\Users\你的用户名\.ssh\`），创建一个名为`config`的新文件（注意：没有扩展名，就只是config）。但是似乎没啥用，还是连接失败。

**💡 DS的解答：**

**为什么你的SSH代理配置可能失效了？**

1. 端口错误：你配置的端口是10808（`S 127.0.0.1:10808`）。请确认你的梯子软件（如Clash、V2rayN）开启的本地SOCKS5代理端口确实是10808。常见的端口还有7890、7891、10809等。如果端口不对，代理就形同虚设
2. 工具不存在：`connect`或`nc`(netcat)并不是Windows自带的命令。你需要额外安装这些工具，或者确保你在Git Bash环境中运行（Git Bash自带connect）
3. 代理类型错误：`S`参数指定的是SOCKS5代理。请确认你的梯子开启的是SOCKS5代理，而不是HTTP代理。很多梯子默认只开HTTP代理，SOCKS5需要手动开启

**✅ 解决方案：** 后来用了HTTP（实际应该是HTTPS），那么SSH配置不上其实是因为SSH被墙了，而HTTPS的全局我已经设置了代理，就可以了。

### CSS丢失问题

**问题描述：** 就是网站是白色背景，所有贴图都很大，所有文字都失去样式。但是本地是一切正常。

我反复的删除、上传、删除、上传，没用。现在看原因应该是public的问题，但是那个UP就可以正常打开为什么呢。

**后面又问DS，它说：**

1. 完整性校验失败：你的HTML中使用了integrity属性（用于SRI），但是这些资源的实际哈希值与指定的哈希值不匹配
2. 浏览器安全策略：当SRI校验失败时，浏览器会阻止加载这些CSS和JS文件
3. 结果：CSS文件被阻止加载 → 样式丢失 → 网站变成无样式的原始状态

然后让我从开发者模式的元素里挨个删除integrity，但是页面没有任何变化。

然后让看CSS，你的截图显示[`main.bundle.min.ad](http://main.bundle.min.ad)2bd6a7d9848...`这一行就是CSS文件！

**分析Network截图：**

我看到：

1. ✅ [`main.bundle.min.ad](http://main.bundle.min.ad)2bd6a7d9848...` - 这就是你的CSS文件
    - 状态：200 ✓ (成功)
    - 类型：stylesheet ✓ (样式表)
    - 大小：20.0 kB ✓
2. ✅ 其他JS文件也都加载成功了（状态都是200）

首先，检查了CSS是有效的样式代码。

然后输入：

```
console.log('CSS规则数量:', document.styleSheets[0].cssRules.length);
console.log('Body背景色:', getComputedStyle(document.body).backgroundColor);
console.log('Body宽度:', document.body.clientWidth, 'px');
```

**输出和报错：**

```
console.log('CSS规则数量:', document.styleSheets[0].cssRules.length);
console.log('Body背景色:', getComputedStyle(document.body).backgroundColor);
console.log('Body宽度:', document.body.clientWidth, 'px');
VM1168:1 Uncaught TypeError: Cannot read properties of undefined (reading 'cssRules')
at <anonymous>:1:49
```

这个错误说明CSS没有被正确加载或解析。

然后看了HTML，我注意到你的HTML中使用了大量的Tailwind CSS类（如flex、flex-col、h-screen、bg-neutral等），但页面显示白色，说明Tailwind CSS没有被正确加载或应用。

```
flex类是否应用: true
VM1230:4 bg-neutral类是否应用: true
VM1230:7 Tailwind CSS 是否加载: 否
undefined
```

找到问题了！**Tailwind CSS没有加载**。Blowfish主题依赖Tailwind CSS，但它没有被加载到页面中。

### 配置GitHub Actions

此时我查看了Blowfish的教程发现我没有在newsite里搞.github文件夹，然后配置了一番。

**🤔 我的困惑：**

这时候有个问题，教程描述workflow里有个gh-pages文件，但是我从样例复制的workflow只有pages我就很奇怪但是我没管。教程后来描述：

"将配置文件推动到Github，Github会自动运行工作流。你需要访问Github代码库的Settings > Pages部分，检查YAML配置是否正确。它应该被设置为使用gh-pages分支。"

我问DS这个没有怎么办，他说这个是古老的配置方式我们用actions的话就不用管。

**💡 DS的解答：** 但是其实要管，这里的意思是，你的workflows里是什么，就应该是什么分支。我不懂为什么，如果是pages，就应该出现pages分支，为什么会出现呢？在pages的代码里面应该会创建这个分支的。

### 上传仓库的各种问题

后面我终于在newsite上传了仓库，就是把原来仓库整个删了，然后创建了.gitignore，又去搞怎么上传的问题了。

**问题一：.gitignore文件没有生效**

```
D:\download\hugo\newSite>dir .gitignore
驱动器 D 中的卷没有标签。
卷的序列号是 CC0C-B70D

D:\download\hugo\newSite 的目录

找不到文件

D:\download\hugo\newSite>
```

真说找不到，但我创建了啊。

**💡 DS的解答：** 文件扩展名问题！在Windows上，文件可能被保存为`.gitignore.txt`而不是`.gitignore`。

然后命令行创建成功。

**问题二：** 我看到public/目录下的文件仍然被Git跟踪了。从命令行删除跟踪。

**问题三：** 需要确保`.gitignore`包含更多内容。`resources/_gen/`和`hugo.exe`等文件被添加了，这些不应该提交。

删除，更新gitignore重新上传。

**问题四：** 检查主题子模块有什么未被跟踪的内容时（为什么要跟踪）

```
Changes not staged for commit:
(use "git add <file>..." to update what will be committed)
(use "git restore <file>..." to discard changes in working directory)
(commit or discard the untracked or modified content in submodules)
modified:   .gitignore
modified:   themes/blowfish (untracked content)
```

说明内容被我修改了也就是之前说的。

```
D:\download\hugo\newSite\themes\blowfish>git status
On branch main
Your branch is up to date with 'origin/main'.

Untracked files:
(use "git add <file>..." to include in what will be committed)
config/_default/languages.zh-cn.toml
config/_default/menus.zh-cn.toml

nothing added to commit but untracked files present (use "git add" to track)
```

最后把修改的两个文件直接删了，至今不知是什么原理。

### 工作流运行失败

然后工作流一直运行失败。经过反复重试，我发现是workflow出错，DS给的不对，用了官方的。还有DS莫名其妙觉得我的网站是3v3，但应该是3V3所以URL也不对。

工作流的报错前期一直是：

```
site config value "en" for defaultcontentlanguage does not match any language definition
```

**🤔 我的困惑：** 这到底是为什么呢？

**别人在GitHub上的反映：**

我找到如下信息：

**Error: site config value "en" for defaultContentLanguage does not match any language definition #10917**

一个俄罗斯人想创建俄语和中文版本的站点，并将俄语设为默认内容语言。他不需要英语版本。

他需要以下配置值：

```
defaultContentLanguage = "ru"
defaultContentLanguageInSubdir = true
```

但是，如果添加以下配置：

```
[markup]
[markup.goldmark.renderer]
unsafe = false
```

并尝试启动Hugo服务器，就会出现以下错误消息：

```
$ hugo server -p 1314
Error: site config value "en" for defaultContentLanguage does not match any language definition
```

**解决方法：** 添加一个不必要的英语版本，服务器才能启动。但以下配置值不再生效：

```
defaultContentLanguage = "ru"
defaultContentLanguageInSubdir = true
```

看起来defaultContentLanguage始终是en，defaultContentLanguageInSubdir始终是false。

**官方回复：**

在你的站点配置中，你把这个：

```
defaultContentLanguage = "ru"
defaultContentLanguageInSubdir = true
```

放在了`markup.goldmark.renderer`表中。这些键值对需要在根表中。

**💡 DS的解答：**

他给出了解决方法是，在设置为你自己的语言的同时让hugo.toml出现en的设置但是关闭。

之后检查各类文件，发现我把https写成了http，嗯这是原因吗？我仍然不知道，还有就是false不能加双引号，和DS把我的大小写写错了。

后来这个build错误就没有了，deploy错误了，需要Personal Access Token，这个创建就好了。

然后都正常了，可以打开网站！

---

## 总结：问题与收获对照表

| 阶段 | 你遇到的现象 | 根本原因 | 你学会的核心概念 |
| --- | --- | --- | --- |
| 1. 部署失败(SSH vs HTTPS) | SSH连接超时，HTTPS成功 | 网络代理配置是分协议的。你只为Git配置了HTTPS代理(http.proxy)，但SSH走的是另一套配置(~/.ssh/config)，且其代理工具/端口可能不正确 | Git的HTTPS协议在复杂网络下更可靠；SSH配置更复杂且依赖额外工具 |
| 2. CSS样式丢失 | 线上页面无样式，但本地正常 | 资源路径或完整性校验失败。可能是baseURL设置错误导致资源404，或SRI哈希值不匹配导致浏览器拒绝加载。你最终发现是Tailwind CSS框架未加载 | 部署后需检查baseURL；浏览器的开发者工具(Network/Console)是排查前端问题的利器 |
| 3. 工作流分支困惑 | 教程说用gh-pages，但你的工作流是pages | 分支名称是可配置的。关键是GitHub仓库设置中的"Pages"来源分支必须与工作流文件中deploy步骤推送的目标分支完全一致。叫gh-pages、pages或main都可以 | GitHub Actions工作流是脚本，你可以控制它把编译好的文件推送到哪个分支 |
| 4. .gitignore失效 | 系统提示找不到文件 | Windows隐藏了文件扩展名。你创建的实际是.gitignore.txt，而Git只认无扩展名的.gitignore | 在Windows中创建以点开头的文件，最好在命令行中用echo > .gitignore命令 |
| 5. 主题子模块被修改 | themes/blowfish显示有未跟踪的修改 | 你直接修改了主题内部的文件（如语言文件）。这违反了"覆盖原则"，导致子模块状态"不干净" | 永远不要修改themes/xxx/下的文件。自定义应通过复制文件到项目根目录的对应位置（如i18n/）来实现 |
| 6. 工作流构建错误 | defaultContentLanguage "en" does not match... | Hugo配置文件语法或位置错误。可能是将语言配置错误地嵌套在了其他区块（如[markup]）内，或者格式有误（如给false加了引号） | Hugo的配置文件(hugo.toml)对格式非常敏感。必须确保键值对位于正确的配置层级 |
| 7. 工作流部署错误 | 需要Personal Access Token(PAT) | GitHub安全策略：自2021年起，已禁止使用账户密码进行命令行认证，必须使用个人访问令牌或SSH密钥 | 在GitHub设置中生成PAT，并在仓库的Secrets中设置为ACTIONS_DEPLOY_KEY或类似名称，供工作流使用 |

---

## 从经历中提炼的"生存法则"

1. **环境配置是一切的基础**：PATH、代理、权限，三者缺一不可
2. **工具只是工具**：blowfish-tools很方便，但理解Hugo的原理更重要
3. **永远不要修改主题文件**：用覆盖而不是修改
4. **Git子模块是动态链接**：手动下载是静态副本，各有利弊
5. **内容类型要匹配主题**：docs vs pages，不是随便起名
6. **public是编译结果**：不要直接修改，源文件在content
7. **SSH vs HTTPS**：各有优劣，网络环境决定选择
8. **开发者工具是神器**：Network、Console能解决90%的前端问题
9. **配置文件格式很严格**：嵌套层级、引号、大小写都很重要
10. **GitHub Actions需要三者一致**：工作流文件、分支名、Pages设置
