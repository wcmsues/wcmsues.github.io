# MacOS 使用 npm 安装 SASS 与使用教程


# MacOS 使用 npm 安装 SASS 与使用教程

这里 Milo 使用 npm 来安装 sass 并使用，比较方便快捷

## 1 npm 的安装

首先确保你有node.js，才能使用npm

- `npm`是[Node.js (opens new window)](http://nodejs.cn/)默认的软件包管理系统，安装完node后，会默认安装好npm
- 我用的homebrew在我的Mac上安装的node

```sh
brew install node
```

然后用命令查看node版本和npm版本

```sh
node -v  //v18.2.0
npm -v   //8.9.0
```

- 使用命令`npm install -g`全局安装`npm`，会默认更新最新版本

## 2 sass 的安装

使用命令 `npm i npm -g` 全局安装 `npm`，会默认更新最新版本

```sh
npm install -g sass
```

然后用命令查看sass版本

```sh
sass --version  //1.53.0 compiled with dart2js 2.17.3
```

## 3 sass 的使用方法

首先要创建2个文件夹，比如sass、css

![j5Mm8g.png](https://s1.ax1x.com/2022/07/16/j5Mm8g.png) 

在终端启动监听命令，这里的sass:css 就是你两个文件夹的名字

```sh
sass --watch sass:css
```

![j53Wff.png](https://s1.ax1x.com/2022/07/16/j53Wff.png) 

在sass文件夹创建.scss文件，在.scss文件写入css，另一个css文件夹就会自动出现.css文件

![j5356g.png](https://s1.ax1x.com/2022/07/16/j5356g.png) 

## 4 sass 教程

详细内容可看 [Sass中文网](https://www.sass.hk/)
