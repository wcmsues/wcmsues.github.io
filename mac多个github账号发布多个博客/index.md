# 解决一台电脑实现多个Github账号发布博客


## 解决一台电脑实现多个Github账号发布博客

Milo之前在自己的GitHub Pages 搭建 Hugo 静态博客网站，Hugo写博客挺好，但是放一些比较多的学习笔记、文档什么的很麻烦，接触到vuepress，非常适合做我的在线文档和笔记记录的地方。

于是Milo想再申请一个账号搭建一个新的vuepress博客，我想在同一台mac上完成两个博客的更新，但因为每次都要重新删github在mac上存留的密码，十分麻烦，所以今天一次性解决它，顺便记录下来分享给大家。


## Hugo/Vuepress + Github Pages

相信很多同学都用Hugo/Vuepress + Github Pages搭建个人博客，其实用什么都行，Hexo、Halo等

### 关于Github Pages

GitHub Pages 是由 GitHub 官方提供的一种免费的静态站点托管服务，可以在 GitHub 仓库里托管和发布自己的静态网站页面；Hexo 是一个快速、简洁且高效的静态博客框架，它基于 Node.js 运行，可以将我们撰写的 Markdown 文档解析渲染成静态的 HTML 网页

### 关于Hugo

Hugo是由Go语言实现的静态网站生成器。简单、易用、高效、易扩展、快速部署。

我选择hugo写博客，主要原因是因为它快！

### 关于Vuepress

VuePress 需要nodejs，非常适合书写技术文档

### 我的环境

- 操作系统：macOS Catalina 版本10.15.4

- 我的Hugo博客：[hugo博客](https://wcmsues.github.io/)
- 我的Vuepress博客：[vuepress博客](https://miloreact.github.io/)

## 解决思路

思路就是：管理两个SSH key

### 1 密钥/Git配置

#### 1.1 创建新的密钥

**先查看当前已有的密钥 [老密钥]**

```sh
ls ~/.ssh/
```

显示`id_rsa` 与` id_rsa.pub`说明已经有一对密钥

```
milo@MilodeMacBook-Pro ~ %ls ~/.ssh/
config       id_rsa       id_rsa.pub
```

⚠️注意：如果没有 **[老密钥]**，就生成两个**[新密钥]**即可，一个叫`id_rsa`，另一个叫`id_rsa_c`

**生成新的密钥 [新密钥]**

```sh
cd ~/.ssh/
ssh-keygen -t rsa -C "你的邮箱xxx@xx.com"
```

⚠️注意：当出现下面这一句时，需要给新的密钥起名字，比如：`id_rsa_c`

```sh
Generating public/private rsa key pair. Enter file in which to save the key (/c/Users/you/.ssh/id_rsa): id_rsa_c
```

然后不用填写，一路回车就行

**验证密钥是否生成**

```sh
ls ~/.ssh/
```

显示`id_rsa`、`id_rsa.pub`与`id_rsa_c`、`id_rsa_c.pub`说明已经有两对密钥

```
milo@MilodeMacBook-Pro ~ %ls ~/.ssh/
config       id_rsa       id_rsa.pub      id_rsa_c       id_rsa_c.pub
```

#### 1.2 配置两个密钥

查看`.ssh/`根路径下, 有没有`config`文件,没有则**创建**一个`config`文件(`config`本身无后缀名)

```sh
touch config
```

**用Text打开config**

```sh
open -a TextEdit config
```

**写入如下配置**

```
#第一个账号，默认使用的账号，不用做任何更改
Host github.com
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_rsa
#第二个新账号，#"xxxxx"为前缀名，可以任意设置，要记住，后面需要用到
Host xxxxx.github.com
    HostName github.com
    User git
    IdentityFile ~/.ssh/这里是你创建的新密钥的名称
```

例如

```
Host github.com
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_rsa
Host vue.github.com
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_rsa_c
```

#### 1.3 添加到SSH agent

**先清空本地的 SSH 缓存**

```sh
ssh-add -D
```

**添加新的 SSH 密钥 到 SSH agent中**

```sh
ssh-add xxxxxx #旧密钥名称，一般是id_rsa
ssh-add xxxxxx #新创建的密钥名称
```

例如

```sh
ssh-add id_rsa
ssh-add id_rsa_c
```

⚠️注意：如果出现错`Could not open a connection to your authentication agent.`，先执行`ssh-agent bash`，再执行以上命令，虽然我没遇到这个错误😄

#### 1.4 新的SSH-GitHub

之前我们生成新密钥的时候执行了`cd ~/.ssh/`，所以我们当前应该在`.ssh`目录下

```
milo@MilodeMacBook-Pro ~/.ssh %
```

在`.ssh`目录下找到创建的新的公钥`id_rsa_c.pub`，**用Text打开它**，并把里面的**内容复制**

```sh
open -a TextEdit id_rsa_c.pub
```

打开新GitHub账号主页 Settings —> SSH and GPG keys —> 点击New SSH key

title可以随便填，将刚复制的内容**粘贴到Key**那里，点击Add Key保存即可

然后回到命令行**验证**一下是不是设置好了

```sh
ssh -T git@github.com #老密钥的ssh_key验证
ssh -T git@xxxxxxx.github.com #新密钥的ssh_key验证
```

例如

```sh
ssh -T git@github.com #老密钥的ssh_key验证
Hi Milo980412! You've successfully authenticated, but GitHub does not provide shell access.
```

```sh
ssh -T git@vue.github.com #新密钥的ssh_key验证
Hi MiloReact! You've successfully authenticated, but GitHub does not provide shell access.
```

⚠️注意：如果**[老密钥]**没有验证成功，把**[老密钥]**也执行一遍上述过程

#### 1.5 Git配置更改

如果已经设置了全局，**取消全局用户名和邮箱配置**

```sh
git config --global --unset user.name
git config --global --unset user.email
```

### 2 Hugo和Vuepress设置

这边讲一下怎么在博客（本地仓库）使用我们配置好的两个SSH Key

#### 2.1 重新设置Git配置

**设置全局用户名和邮箱配置**

```sh
git config --global user.name XXXX
git config --global user.email XXXX
```

例如

```sh
git config --global user.name "Milo980412"
git config --global user.email XXX@XX.com
```

⚠️注意：1.5 Git配置更改 中如果全局设置没啥问题的 1.5中就不用更改了，这里也不用重新设置了

#### 2.2 Git命令push博客项目

**一般我们push更新Hugo博客内容的流程是**

```sh
# 生成静态文件
hugo --theme=LoveIt --baseUrl="https://wcmsues.github.io"
# 进入生成的文件夹
cd public
# 创建本地代码仓库
git init
# 将修改的文件保存到暂存区
git add -A
# 需要将文件提交到本地仓库
git commit -m 'deploy'
# 推送代码，如果发布到 https://<USERNAME>.github.io
git push -f git@github.com:wcmsues/wcmsues.github.io.git master
```

**一般我们push更新Vuepress博客内容的流程是**

```sh
# 生成静态文件
npm run docs:build
# 进入生成的文件夹
cd docs/.vuepress/dist
# 创建本地代码仓库
git init
# 将修改的文件保存到暂存区
git add -A
# 需要将文件提交到本地仓库
git commit -m 'deploy'
# 如果发布到 https://<USERNAME>.github.io
git push -f git@github.com:MiloReact/MiloReact.github.io.git master
```

⚠️注意：**[老密钥]**的push没有区别，但是**[新密钥]**的push有一点不同

这里我的vuepress是我的**[新密钥]**，在git push时，git地址要写我们在**1.2 配置两个密钥**时带有前缀的vue.github.com，如下所示

```sh
git push -f git@vue.github.com:MiloReact/MiloReact.github.io.git master
```

谁是新密钥就改谁的push命令就行，到此为止就解决了一台电脑实现多个Github账号发布博客

#### 2.3 小建议

我不太喜欢用自动化部署，我喜欢用脚本，比如我在博客项目根目录创建一个deploy.sh文件，命令如下

Hugo

```sh
# 生成静态文件
hugo --theme=LoveIt --baseUrl="https://wcmsues.github.io"
# 进入生成的文件夹
cd public
# 创建本地代码仓库
git init
# 将修改的文件保存到暂存区
git add -A
# 需要将文件提交到本地仓库
git commit -m 'deploy'
# 推送代码，如果发布到 https://<USERNAME>.github.io
git push -f git@github.com:wcmsues/wcmsues.github.io.git master
```

Vuepress

```sh
# 生成静态文件
npm run docs:build
# 进入生成的文件夹
cd docs/.vuepress/dist
# 创建本地代码仓库
git init
# 将修改的文件保存到暂存区
git add -A
# 需要将文件提交到本地仓库
git commit -m 'deploy'
# 如果发布到 https://<USERNAME>.github.io
git push -f git@vue.github.com:MiloReact/MiloReact.github.io.git master
```

执行脚本完成push更新

```sh
./deploy.sh
```


