---
title: "Hello World"
date: 2021-03-04T16:25:56+08:00
draft: false
keywords: ["hugo", "even"]
description: ""
tags: ["blog"]
categories: ["record"]

---

<!--# Hello World.-->

**************
The first blog.
**************

## 博客搭建记录

在阅读了众多的博客搭建教程之后，最终选择了使用`github` + [hugo](gohugo.io)进行搭建，主题为[even](https://hugo-theme-even.netlify.app/). 希望借此博客在互联网上留下一点有价值的东西，同时作为自己的学习笔记，温故而知新。

### 软件安装
首先进行相应软件的安装，具体可以参考官方文档的安装步骤。本文以ubuntu举例说明。在ubuntu上可直接使用`apt`进行安装，但是使用该方法安装的hugo软件版本过低，会导致本文所使用的主题even无法使用（我所使用的even要求最低版本为0.60），而且这个问题可能在其他主题上也存在。总而言之，建议直接安装最新版本，可直接到[release](https://github.com/gohugoio/hugo/releases)下载。

### 网站和主题配置
软件安装好了之后，可以创建一个文件夹作为根目录(如`mkdir blog`)并使用`git init`进行初始化，然后在该目录下进行hugo的初始化，初始化操作为

```shell
hugo new site [blogs name]
```

然后在该目录下就会创建一些文件夹，如：

```
README.md  
archetypes 
config.toml
content    
public     
resources  
themes     
```

文件夹的具体作用我正在继续摸索，现在大概知道`config.toml`是全局配置文件；`content`文件夹中放置的是博客的内容，如本博客的路径就是`content/post/hello-world`；`themes`中所存放的就是主题文件，文件夹下可有多个主题，但是在使用的时候只能指定其中一个；`public`中所存放的是一些经过编译的网页内容，即使用markdown编写的博客经过编译以后会放到这个文件夹里，但具体的组织结构还没搞明白，待后续研究；`archetypes`中存放的好像是模板之类的东西，同样待后续研究。

接下来讨论怎么设置主题，经过一段时间纠结的筛选后，决定暂时使用`even`主题，主要原因有：
* 主题看来还算简洁
* 维护还算活跃
* 该主题是一个博客性质的主题，比较符合目前需求
* 该主题的开发者是国人同胞，可能比较符合国人的应用习惯

选定主题后，就将该[主题](https://github.com/olOwOlo/hugo-theme-even)clone到themes文件夹下并命名为even。然后将该仓库添加为本仓库的子模块。
```shell
git submodule add https://github.com/olOwOlo/hugo-theme-even ./themes/even
```

然后可以参考even主题的教程，将其配置文件进行拷贝和自行配置。其配置方面的文档较少，但好处是其配置文档中都使用了中文对其作用和用法进行了大概的注释，同样也可以根据需求进行增添内容，具体细节将后续进行学习。

------------21/04/07更新-----------

感觉even有些功能不理想，并且作者似乎已经停止开发了，所以准备转到LoveIt的继任者CodeIT上，下面准备讲述更新过程。其中有一个问题是数学公式的显示问题，两个主题都有一些问题，接下来将讲述我遇到的问题，以及目前所采用的方式。希望以后能够有机会了解hugo的细节，从而更好的解决该问题。
TODO

### 写博客
上面已经进行了大概的配置，下面将介绍怎么写一篇博客。首先使用
```
hugo new post/hello-world
```
创建一个新的博客，该命令将创建一个文件：`content/post/hello-world.md`。该文件会包含一个文件头
```
title: ***
date: 202***
draft: true
```
draft一栏默认为`true`，如果想要发布该博客，需要将其修改为`false`。

hugo的一个好处就是可以边写博客边进行调试，其具体操作为：在主目录下（即blog/）运行
```
hugo server -D
```
该命令将启动一个本地服务，端口号为1313，在本地使用浏览器访问：[http://localhost:1313](http://localhost:1313)即可在线预览自己的博客内容。每次保存文件时，hugo都会自动进行编译，速度很快，非常方便！

在发布之前应该使用
```
hugo -D
```
编译一下，至此已经可以实现基本的功能

### 使用github发布博客
首先要有一个github账号，然后创建一个新的仓库，其名字为：[your github id].github.io，关于这方面的教程非常多，也很简单，这里就不再赘述。然后其部署可参考[https://gohugo.io/hosting-and-deployment/hosting-on-github/#build-hugo-with-github-action](https://gohugo.io/hosting-and-deployment/hosting-on-github/#build-hugo-with-github-action)。其操作非常方便，只需要创建一个文件，然后将一些github action代码复制进去就行了。具体原理后续研究...(这里不得不感叹一句，github的功能比我想象的要强大多了!)

然后就是将本地文件(即blog/文件夹)push到所创建的仓库中。最后需要注意的是在上面所创建的仓库的设置页面要进行一定的设置，其位置为`setting->github pages->source`，需要将branch更改为`gh-pages`，路径为`root`，在这里还可以设置定制化的域名等，这里先暂时不研究。通过以上设置应该就可以成功访问页面了。如本博客主页为：http://xinyu-yang.github.io


还有很多功能待后续学习更新...

