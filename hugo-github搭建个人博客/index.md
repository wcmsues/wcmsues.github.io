# 基于Hugo+Github搭建个人博客



# 基于Hugo+Github搭建个人博客

## 一、安装

对于 Windows 平台，一般是一个zip文件，解压后里面有个 `hugo.exe` 文件。将该文件所在目录添加到环境变量 `path` 里，即可在cmd里通过 `hugo version` 检测是否能正常运行hugo命令。

我的是 Mac 环境，相对简单，使用 `homebrew `安装即可，没有安装 `homebrew`的可以参考[MacOS下HomBrew安装教程](https://wcmsues.github.io/macos%E4%B8%8Bhombrew%E5%AE%89%E8%A3%85%E6%95%99%E7%A8%8B/)

```sh
brew install hugo
```

使用hugo version检查是否能正常运行hugo命令

```sh
hugo version #hugo v0.100.0+extended darwin/amd64 BuildDate=unknown
```

## 二、创建站点

首先需要在合适的位置创建一个新的个人站点：

```sh
hugo new site myblog
```

`myblog` 就是你的博客站点所在的目录，也是这个站点的根目录，创建站点后目录结构如下：

```sh
.
├── archetypes
├── content
├── data
├── layouts
├── static
├── themes
└── config.toml
```

下面简单介绍下Hugo根目录下的各个文件目录的作用：

- `archetypes` 存放创建文件时使用的模板，可以自定义`front matter`属性。
- `content `存放的各种md文件用于部署站点，该目录下可以自行创建若干个子目录来便于对文章进行分类。
- `data` 目录存放的是用于定义变量的模板文件，一般用不到该功能。
- `layouts` 目录存放的模板文件用于渲染html页面，模板里可以定义不同页面的html代码。
- `static` 目录存放的是静态内容：图片、CSS、JavaScript等。
- `config`是配置文件

## 三、添加主题

为新站点添加一个主题，以我使用的 `LoveIt` 主题为例，先将主题代码放置到 `themes` 目录下：

```sh
cd myblog
git clone https://github.com/dillonzq/LoveIt.git themes/LoveIt
```

## 四、基本配置

以下是 LoveIt 主题的基本配置:

```toml
baseURL = "http://example.org/"

# 更改使用 Hugo 构建网站时使用的默认主题
theme = "LoveIt"

# 网站标题
title = "我的全新 Hugo 网站"

# 网站语言, 仅在这里 CN 大写 ["en", "zh-CN", "fr", "pl", ...]
languageCode = "zh-CN"
# 语言名称 ["English", "简体中文", "Français", "Polski", ...]
languageName = "简体中文"
# 是否包括中日韩文字
hasCJKLanguage = true

# 作者配置
[author]
  name = "xxxx"
  email = ""
  link = ""

# 菜单配置
[menu]
  [[menu.main]]
    weight = 1
    identifier = "posts"
    # 你可以在名称 (允许 HTML 格式) 之前添加其他信息, 例如图标
    pre = ""
    # 你可以在名称 (允许 HTML 格式) 之后添加其他信息, 例如图标
    post = ""
    name = "文章"
    url = "/posts/"
    # 当你将鼠标悬停在此菜单链接上时, 将显示的标题
    title = ""
  [[menu.main]]
    weight = 2
    identifier = "tags"
    pre = ""
    post = ""
    name = "标签"
    url = "/tags/"
    title = ""
  [[menu.main]]
    weight = 3
    identifier = "categories"
    pre = ""
    post = ""
    name = "分类"
    url = "/categories/"
    title = ""

# Hugo 解析文档的配置
[markup]
  # 语法高亮设置 (https://gohugo.io/content-management/syntax-highlighting)
  [markup.highlight]
    # false 是必要的设置 (https://github.com/dillonzq/LoveIt/issues/158)
    noClasses = false
```

## 五、新建文章

新建一篇文章：

```sh
hugo new posts/first.md
```

该命令会在 `content/posts` 目录下生成 `first.md` 文件，打开进行编辑：

```
---
title: "XXX"
date: 2022-08-10T13:45:43+08:00
draft: false
description: "XXX"
tags: ["XXX","XXX","XXX"]
categories: ["XXX"]
lightgallery: true
toc:
  auto: false
---

## 我的第一篇文章
我的第一篇文章
```

两行 `---` 里的属性是 `front matter`，用来设置当前文章的属性配置，如上是我经常使用的配置。

平时写博客，把写好的markdown文档放到这个 `content/posts` 目录就可以了，记得在开头加上 `front matter`

## 六、启动Hugo服务

```sh
hugo server -D
```

在本地启动服务后可以在 http://localhost:1313/ 访问个人站点。该命令仅用于本地调试，支持热修改，也就是说在启动服务时修改文章会实时生效，但是该命令不会真正生成静态文件。

![](https://hugoloveit.com/zh-cn/theme-documentation-basics/basic-configuration-preview.zh-cn.png) 

## 七、生成静态页面

输入命令：

```sh
hugo
```

会生成一个 `public` 目录, 其中包含你网站的所有静态内容和资源，现在可以将其部署在任何 Web 服务器上。

但我习惯上会加上指定主题（如果有多个主题）等内容：

```sh
hugo --theme=LoveIt --baseUrl="https://XXX.github.io"
```

## 八、远程部署到Github Pages

### 1 在GitHub上创建一个仓库

首先在GitHub上创建一个仓库，仓库的名字格式为`<username>.github.io`。比如我的GitHub用户名是 `wcmsues`，那么这个仓库就命名为 `wcmsues.github.io`。

> 之所以这样规定命名，是因为GitHub默认会把 `<username>.github.io` 的master分支的内容部署到 GitHub Pages 站点上。

### 2 SSH key的创建与配置

因为要使用SSH的方式来和GitHub仓库进行交互，我们需要生成一对密钥对，然后将公钥配置到GitHub账号上。详细内容可看我之前发的[SSH key的创建与配置](https://wcmsues.github.io/ssh-key%E5%AF%86%E9%92%A5%E7%9A%84%E5%88%9B%E5%BB%BA%E4%B8%8E%E9%85%8D%E7%BD%AE/)，这里我只简单讲必要的步骤

**生成密钥**

```sh
cd ~/.ssh/
ssh-keygen -t rsa -C "你的邮箱xxx@xx.com"
```

⚠️注意：当出现下面这一句时，需要给新的密钥起名字，比如：`id_rsa`

```sh
Generating public/private rsa key pair. Enter file in which to save the key (～/.ssh/id_rsa): id_rsa
```

然后不用填写，一路回车就行

**验证密钥是否生成**

```sh
ls ~/.ssh/
```

显示`id_rsa`、`id_rsa.pub` 说明创建密钥成功

```sh
milo@MilodeMacBook-Pro ~ %ls ~/.ssh/
config       id_rsa       id_rsa.pub
```

**配置密钥**

查看`.ssh/`根路径下, 有没有`config`文件,没有则**创建**一个`config`文件(`config`本身无后缀名)

```sh
touch config
```

用Text打开config

```sh
open -a TextEdit config
```

写入如下配置

```sh
Host github.com
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_rsa
```

**添加到SSH agent**

先清空本地的 SSH 缓存

```sh
ssh-add -D
```

添加新的 SSH 密钥 到 SSH agent中

```sh
ssh-add id_rsa
```

**新的SSH-GitHub**

在`.ssh`目录下找到创建的新的公钥`id_rsa_c.pub`，**用Text打开它**，并把里面的**内容复制**

```sh
open -a TextEdit id_rsa_c.pub
```

打开新GitHub账号主页 Settings —> SSH and GPG keys —> 点击New SSH key

title可以随便填，将刚复制的内容**粘贴到Key**那里，点击Add Key保存即可

然后回到命令行**验证**一下是不是设置好了

```sh
ssh -T git@github.com #密钥的ssh_key验证
```

如果显示下方文字，说明成功

```sh
Hi wcmsues! You've successfully authenticated, but GitHub does not provide shell access.
```

### 3 部署到Github Pages

执行下方脚本

```sh
# 确保脚本抛出遇到的错误
set -e

# 生成静态文件
hugo --theme=LoveIt --baseUrl="https://wcmsues.github.io"
# 进入生成的文件夹
cd public

# 基本操作
git init
git add -A
git commit -m 'deploy'

# 如果发布到 https://<USERNAME>.github.io
git push git@github.com:wcmsues/wcmsues.github.io.git master
```

## 九、美化配置

可以参考
- [Lovelt主题](https://hugoloveit.com/zh-cn/)
- [Lewis的Hugo-Lovelt主题美化](https://lewky.cn/categories/hugo%E7%B3%BB%E5%88%97/)

---

最终我的基于Hugo+Github搭建的个人博客：[Milo's Blog](https://wcmsues.github.io/)

慢慢美化配置好之后，就不用动了，我平时也就写写md文件，放到 `content/posts` 目录，开头加上 `front matter`，然后用脚本上传即可。

