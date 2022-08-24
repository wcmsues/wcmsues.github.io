# 从零搭建基于VuePress的文档博客


# 从零搭建基于VuePress的文档博客

本文会帮助你从头搭建一个简单的 VuePress 文档，并且做一些简单的配置。

## 一、前提条件

VuePress 需要 [Node.js](https://nodejs.org/en/) >= 8.6

<img src="https://pic.jitudisk.com/public/2022/08/20/2b6b456a42da3.png" alt="nodejs.png" style="zoom: 33%;" /> 

直接点击下载，或者在DOWNLOADS里按需下载即可

<img src="https://pic.jitudisk.com/public/2022/08/20/dce2937529926.png" alt="WeChatb406d3e5ba4e311b520a3109d95ab6d6.png" style="zoom:33%;" /> 

安装好后，查看nodejs版本

```sh
node
```

<img src="https://pic.jitudisk.com/public/2022/08/20/dd58be07deffa.png" alt="nodejsbanben.png" style="zoom:50%;" /> 

## 二、快速上手

### 1.创建并进入一个新目录

```sh
mkdir vuepress-milo && cd vuepress-milo
```

### 2.使用你喜欢的包管理器进行初始化

```sh
yarn init # npm init
```

初始化过程如无特殊需要，一路回车即可

<img src="https://pic.jitudisk.com/public/2022/08/20/f9c5a1ab6951a.png" alt="chushihua.png" style="zoom: 50%;" /> 

### 3.将 VuePress 安装为本地依赖

不再推荐全局安装 VuePress

```sh
yarn add -D vuepress # npm install -D vuepress
```

### 4.创建docs文件夹，和一篇md文档

```sh
mkdir docs && echo '# Hello VuePress' > docs/README.md
```

### 5.在 `package.json` 中添加一些 `scripts`

这里推荐用vscode打开后编辑

```sh
code .
```

<img src="https://pic.jitudisk.com/public/2022/08/20/21da3354667a8.png" alt="tianjiasp.png" style="zoom:50%;" /> 

这一步骤是可选的，但我们推荐你完成它。

```json
{
  "scripts": {
    "docs:dev": "vuepress dev docs",
    "docs:build": "vuepress build docs"
  }
}
```

### 6.在本地启动服务器

```sh
yarn docs:dev # npm run docs:dev
```

<img src="https://pic.jitudisk.com/public/2022/08/20/67f9d52dd2e62.png" alt="qidongbendi.png" style="zoom:33%;" /> 

## 三、基本设置

此时的文档博客很丑，下一步我们需要做一些基本配置

### 1.目录结构设置

VuePress 遵循 **“约定优于配置”** 的原则，推荐的目录结构如下：

```sh
.
├── docs
│   ├── .vuepress
│   │   ├── public
│   │   ├── styles
│   │   │   └── palette.styl
│   │   └── config.js
│   ├── pages
│   └── README.md
│ 
└── package.json
```

⚠️建议：先把这些都创建好，然后在再写入内容

<img src="https://pic.jitudisk.com/public/2022/08/20/a1c120e3279e8.png" alt="mulujiegoutu.png" style="zoom: 50%;" /> 

### 2.public静态资源目录

`docs/.vuepress/public`: 静态资源目录，这里我放了图片作为页面主图和偏爱图标

![aniya.png](https://pic.jitudisk.com/public/2022/08/20/821c2bdf31883.png) 

### 3.重写默认颜色常量

`docs/.vuepress/styles/palette.styl`: 用于重写默认颜色常量，或者设置新的 stylus 颜色常量

默认是绿色，我这里更改为蓝色

默认页面有点窄，这里更改为900px宽度

```stylus
// 颜色
$accentColor = #235DC8
// 页面设置
.page .theme-default-content:not(.custom){
    max-width: 900px!important;
} 
```

### 4.config基本设置

`docs/.vuepress/config.js`: 配置文件的入口文件，也可以是 `YML` 或 `toml`，这里简单设置了网站标题、网站描述、偏爱图标。

```js
module.exports = {
    // 网站标题
    title: 'Milo的学习笔记',
    // 网站描述
    description: '拒绝重复造轮子',
    // 偏爱图标
    head: [
        ['link', { rel: 'icon', href: '/aniya.png' }]
    ],
}
```

### 5.首页设置

默认的主题提供了一个首页（Homepage）的布局，需要在你的根级 `README.md` 指定 `home: true`。以下是一个如何使用的例子：

```markdown
---
home: true
heroImage: /aniya.webp
heroText: Milo的学习笔记
tagline: 无比热爱，来日方长！
actionText: Milo的周记 →
actionLink: /
features:
- title: 科研之路
  details: 主攻推荐系统、知识图谱，拒绝重复造轮
- title: 目前现状
  details: 目前在读研二，本科和研究生都就读于上海工程技术大学，论文撰写中。
- title: 短期规划
  details: 完成毕业论文，发表两篇中文核心。
# footer: MIT Licensed | Copyright © 2018-present Evan You
---
```

然后重新启动本地服务

```sh
yarn docs:dev
```

<img src="https://pic.jitudisk.com/public/2022/08/20/3f08802c9184f.png" alt="jibenshezhi.png"  />

## 四、pages存放markdown文档

`docs/pages` 用于存放markdown文件，例如pages中有以下几个md文件

```sh
.
└── pages
    ├── codes
    ├── papers
    │   ├── kg
    │   ├── rs
    │   └── rskg
    │       ├── AKGE.md
    │       ├── DKN.md
    │       ├── KGAT.md
    │       ├── KGCN.md
    │       ├── KGNN-LS.md
    │       ├── MKR.md
    │       ├── NGCF.md
    │       ├── RippleNet.md
    │       └── start.md
    └── weekly
        ├── 20220822.md
        └── start.md
```

## 五、导航栏设置

在 `docs/.vuepress/config.js` 基本设置的基础上，添加导航栏设置

```js
module.exports = {
    // 网站标题
    // 网站描述
    // 偏爱图标
    // 导航栏与侧边栏设置
    themeConfig: {
        // logo设置
        logo: '/aniya.png',
        // 导航栏设置
        nav: [
            { text: '首页', link: '/' },
            {
                text: '推荐系统',
                ariaLabel: 'recommender systems',
                items: [
                    { text: '基础知识', link: '/' },
                ]
            },
            {
                text: '知识图谱',
                ariaLabel: 'knowledge graph',
                items: [
                    { text: '基础知识', link: '/' },
                ]
            },
            {
                text: '相关论文',
                ariaLabel: 'papers',
                items: [
                    { text: '论文学习', link: '/pages/papers/rskg/start.md' },
                    { text: '源码学习', link: '/' },
                ]
            },
            {
                text: '我的论文',
                ariaLabel: 'mypaper',
                items: [
                    { text: '论文1', link: '/' },
                    { text: '论文2', link: '/' },
                    { text: '论文3', link: '/' },
                ]
            },
        ],
        //侧边栏设置（后面讲）
    }
}
```

然后重新启动本地服务

```sh
yarn docs:dev
```

![daohanglan.png](https://pic.jitudisk.com/public/2022/08/20/d1898c76fbe77.png) 

## 六、侧边栏设置

在 `docs/.vuepress/config.js` 基本设置与导航栏设置的基础上，添加侧边栏设置

```js
module.exports = {
    // 网站标题
    // 网站描述
    // 偏爱图标
    // 导航栏与侧边栏设置
    themeConfig: {
        // logo设置
        logo: '/aniya.png',
        // 导航栏设置
        // 侧边栏设置
        sidebar: {
            // 周报
            '/pages/weekly/':
                [
                    {
                        title: 'Milo的学习周报',   // 必要的
                        collapsable: true, // 是否可折叠, 默认值是 true,
                        sidebarDepth: 2,    // 侧边栏深度, 默认值是1
                        children: [
                            'start',
                            '20220822',
                        ]
                    },
                ],
            // 推荐系统&&知识图谱
            '/pages/papers/':
                [
                    {
                        title: '推荐系统&&知识图谱',   // 必要的
                        collapsable: true, // 是否可折叠, 默认值是 true,
                        sidebarDepth: 2,    // 侧边栏深度, 默认值是 1
                        children: [
                            'rskg/start',
                            'rskg/DKN',
                            'rskg/RippleNet',
                            'rskg/MKR',
                            'rskg/KGCN',
                            'rskg/KGNN-LS',
                            'rskg/NGCF',
                            'rskg/AKGE',
                            'rskg/KGAT',
                        ]
                    },
                    {
                        title: '知识图谱',   // 必要的
                        collapsable: true, // 是否可折叠, 默认值是 true,
                        sidebarDepth: 2,    // 侧边栏深度, 默认值是 1
                        children: [
                            
                        ]
                    },
                    {
                        title: '推荐系统',   // 必要的
                        collapsable: true, // 是否可折叠, 默认值是 true,
                        sidebarDepth: 2,    // 侧边栏深度, 默认值是 1
                        children: [
                
                        ]
                    },
                ],
        },
    }
}
```

然后重新启动本地服务

```sh
yarn docs:dev
```

![cebianlan.png](https://pic.jitudisk.com/public/2022/08/20/7de435608514d.png)

## 七、总结

1. 安装nodejs
2. 创建并进入一个新目录
3. 使用你喜欢的包管理器进行初始化
4. 将 VuePress 安装为本地依赖
5. 创建docs文件夹，和一篇md文档
6. 在 `package.json` 中添加一些 `scripts`
7. 目录结构设置
8. public静态资源目录
9. 重写默认颜色常量
10. config基本设置
11. 首页设置
12. pages存放markdown文档
13. 导航栏设置
14. 侧边栏设置

自此所有基本操作都已完成，大家可根据此文章搭建属于自己的文档博客



## 其他案例

[Milo的前端日记](https://miloreact.github.io/)

