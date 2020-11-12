---
title: 从零开始教你使用hexo搭建个人博客
tags: 
- Hexo 
- 部署
- 教程
---

Hexo 是一个快速、简洁且高效的博客框架。Hexo 使用 Markdown（或其他渲染引擎）解析文章，在几秒内，即可利用靓丽的主题生成静态网页。

###### 萌新一枚，有误轻喷。

## 1.安装node.js

在 Windows 环境下安装 Node.js 非常简单，仅须到[官网下载](https://nodejs.org/en/download/)安装文件并执行即可完成安装。
（无脑下一步就行了，不需要配置环境变量。）

## 2.安装git

去[Git官网](https://git-scm.com/download/win)根据你的电脑参数，下载对应版本。
下载完成，通过在命令行输入 git version 查看是否安装成功，有输出版本号说明安装成功。
``` bash 
git version
```

<!-- more -->

鼠标邮件菜单里就多了Git GUI Here和Git Bash Here两个按钮，一个是图形界面的Git操作，一个是命令行，我们选择Git Bash Here。

## 3.安装Hexo

桌面右键鼠标，点击Git Bash Here，输入npm命令即可安装

``` bash
npm install hexo-cli -g
npm install hexo-deployer-git --save
```
第一句是安装hexo，第二句是安装hexo部署到git page的deployer，两个都需要安装。

## 4.Hexo初始化配置

创建Hexo文件夹
安装完成后，根据自己喜好建立目录（如E:\hexo），直接进入E:\hexo文件夹下右键鼠标，点击Git Bash Here，进入Git命令框，执行以下操作。

``` bash
hexo init
```

本地查看效果
执行下面语句，执行完即可登录localhost:4000查看效果

``` bash
hexo g
hexo s
```

## 5.美化自己博客

一、进入[Hexo的官网主题专栏](https://hexo.io/themes/)

二、挑选我们喜欢的主题
可以看到有很多主题给我们选，我们只要选择喜欢的主题点击进去，然后进入到它的github地址，我们只要把这个地址复制下来(例如我是选择：zzoman2015这个主题)

三、克隆主题
再打开Hexo文件夹下的themes目录（E:\hexo），右键Git Bash，在命令行输入:

``` bash
git clone https://github.com/reumia/hexo-theme-zzoman2015 themes/zzoman2015
```

上面的地址可以换成你喜欢的主题地址，其中themes/zzoman2015代表下载到themes文件下载的next文件夹中，其实themes是所有的主题文件夹，zzoman2015代表当前主题的名称。

四、修改Hexo配置文件
下载完成后，打开Hexo文件夹下的配置文件_config.yml

修改参数为：theme: zzoman2015

五、部署主题，本地查看效果

返回Hexo目录，右键Git Bash，输入

``` bash
hexo g
hexo s
```

六、打开自己的主页，即可看到修改后的效果



#### 一、用hexo发表新文章

hexo n "文章标题" 
其中 java 为文章标题，执行命令 hexo n "java" 后，会在E:\hexo\source_posts中生成 java.md文件，用编辑器打开编写即可。


``` bash
hexo n "java"
```

当然，也可以直接在E:\hexo\source_posts中新建一个md文件，我就是这么做的。 写完后，推送到服务器上，执行以下命令即可在我们的站点看到新的文章。

如果博客中不自己设置博客的简介的话默认会从博客内容中选择，在xx.md中可以使用下面的方式设置博客的简介：


``` bash
简介内容
<!-- more -->
```
推送

``` bash
hexo g #生成
hexo d #部署 # 可与hexo g合并为 hexo d -g
```
