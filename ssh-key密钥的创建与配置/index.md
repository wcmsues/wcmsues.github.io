# SSH key密钥的创建与配置

# SSH key密钥的创建与配置

因为要使用SSH的方式来和GitHub仓库进行交互，我们需要生成一对密钥对，然后将公钥配置到GitHub账号上。

## 一、创建密钥

### 1 先查看当前已有的密钥 [老密钥]

```sh
ls ~/.ssh/
```

显示 `id_rsa` 与 `id_rsa.pub` 说明已经有一对密钥，如果需要可以删除重新创建，或者创建新的与之名字不同的密钥。

```sh
milo@MilodeMacBook-Pro ~ %ls ~/.ssh/
config       id_rsa       id_rsa.pub
```

### 2 生成新的密钥 [新密钥]

```sh
cd ~/.ssh/
ssh-keygen -t rsa -C "你的邮箱xxx@xx.com"
```

⚠️注意：当出现下面这一句时，需要给新的密钥起名字，比如：`id_rsa`

```sh
Generating public/private rsa key pair. Enter file in which to save the key (～/.ssh/id_rsa): id_rsa
```

然后不用填写，一路回车就行

### 3 验证密钥是否生成

```sh
ls ~/.ssh/
```

显示`id_rsa`、`id_rsa.pub` 说明创建密钥成功

```sh
milo@MilodeMacBook-Pro ~ %ls ~/.ssh/
config       id_rsa       id_rsa.pub
```

## 二、配置密钥

查看`.ssh/`根路径下, 有没有`config`文件,没有则**创建**一个`config`文件(`config`本身无后缀名)

```sh
touch config
```

### 1 用Text打开config

```sh
open -a TextEdit config
```

### 2 写入如下配置

```sh
Host github.com
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_rsa
```

### 3 添加到SSH agent

**先清空本地的 SSH 缓存**

```sh
ssh-add -D
```

**添加新的 SSH 密钥 到 SSH agent中**

```sh
ssh-add id_rsa
```

⚠️注意：如果出现错`Could not open a connection to your authentication agent.`，先执行`ssh-agent bash`，再执行以上命令，虽然我没遇到这个错误。

### 4 新的SSH-GitHub

之前我们生成新密钥的时候执行了`cd ~/.ssh/`，所以我们当前应该在`.ssh`目录下

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

⚠️注意：如果没有验证成功，就再执行一遍上述过程


