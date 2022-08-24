# MacOS如何生成目录树？

# MacOS如何生成目录树？

在一个项目的 README 中，我们往往会想把项目的目录结构进行一下介绍或者注释，这样方便其他人接手项目时快速熟悉，同时也方便自己回忆起项目结构。那么效果比较好的呈现方式自然就是目录树，如何快速生成项目的目录树呢？

我们第一反应是 tree 命令，可 MacOS 并不自带 tree 命令，所以需要手动安装！

## HomeBrew 安装 tree

用 homebrew 安装最方便，没有 brew 的 mac 用户，或者下载失败的，可以看我之前那篇文章

[MacOS下HomBrew安装教程](https://milo980412.github.io/macos%E4%B8%8Bhombrew%E5%AE%89%E8%A3%85%E6%95%99%E7%A8%8B/)

安装好 homebrew 后，直接安装 tree

```shell
brew install tree
```

## 使用 tree

我们先 cd 到要生存目录树的文件夹，然后使用

```shell
tree
```

![jIxRkq.png](https://s1.ax1x.com/2022/07/18/jIxRkq.png)

## 输出指定层级的目录树

直接使用 `tree` 命令，它会把所有层级都输出，但如果我们只要输出 2 个层级就可以了，那么可以这样使用

```shell
// -L level 表示只会遍历到指定层级
tree -L 2
```

## 忽略某些文件夹输出目录树

前端项目基本都会有这个问题，就是会把 `node_modules` 也输出成目录树，一下子目录树会无比巨大，而且一般我们也并不需要输出这个文件夹下的内容，那么可以这样使用

```shell
// -I pattern 表示不会将匹配到的文件/文件夹输出，即类似 ignore 的功能
tree -I node_modules
```

![jIzkNt.png](https://s1.ax1x.com/2022/07/18/jIzkNt.png)

其他命令，可以通过 `tree --help` 来查看需要的参数

