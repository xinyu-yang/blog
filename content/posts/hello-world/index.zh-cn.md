---
title: "Hello World"
date: 2021-03-04T16:25:56+08:00
draft: false
keywords: ["hugo", "blog", "博客搭建"]
description: "本博客第一篇文章，主要记录本博客搭建和配置的过程"
tags: ["blog", "config"]
categories: ["record"]

featuredImage: ""
featuredImagePreview: "../../2021/hello-world/test.png"

math:
  enable: true

lightgallery: true

---

<!--# Hello World.-->

## 博客搭建记录

在阅读了众多的博客搭建教程之后，最终选择了使用`github` + [hugo](gohugo.io) 进行搭建，主题为 [even](https://hugo-theme-even.netlify.app/). 希望借此博客在互联网上留下一点有价值的东西，同时作为自己的学习笔记，温故而知新。

![testImg](test.png "Image test")

### 软件安装
首先进行相应软件的安装，具体可以参考官方文档的安装步骤。本文以 ubuntu 举例说明。在 ubuntu 上可直接使用`apt`进行安装，但是使用该方法安装的 hugo 软件版本过低，会导致本文所使用的主题 even 无法使用（我所使用的 even 要求最低版本为 0.60），而且这个问题可能在其他主题上也存在。总而言之，建议直接安装最新版本，可直接到 [release](https://github.com/gohugoio/hugo/releases) 下载。

### 网站和主题配置
软件安装好了之后，可以创建一个文件夹作为根目录(如`mkdir blog`)并使用`git init`进行初始化，然后在该目录下进行 hugo 的初始化，初始化操作为

```shell
hugo new site [blogs name]
```

然后在该目录下就会创建一些文件夹，如：

```
-- README.md  
-- archetypes 
-- config.toml
-- content    
-- public     
-- resources  
-- themes     
```

文件夹的具体作用我正在继续摸索，现在大概知道`config.toml`是全局配置文件，关于网站的一些常用配置基本都放在这里。当下载和使用某个主题的时候，该主题一般会提供一个配置模板，用户只需要把该文件复制到博客根目录下，并且根据主题或者模板中的注释按需修改即可。具体使用要参考不同主题。

`content`文件夹中放置的是博客的内容，如本博客的路径就是`content/post/hello-world`。经过摸索本人发现，content 文件夹下的子文件夹的名字可以随便起，只要最后在配置文件中使用相同的配置即可。另外，在子文件夹下也可以再建文件夹，这时的文件夹也可以随便取名字，而且无需在配置文件中进行配置。
推测 hugo 的机制是会扫描 content 的子文件夹下 (e.g., post) 所有的 markdown 文件。

`themes`中所存放的就是主题文件，文件夹下可有多个主题，但是在使用的时候只能指定其中一个，具体使用哪个可以在全局配置文件中配置。

`public`中所存放的是一些经过编译的网页内容，即使用 markdown 编写的博客经过编译以后会放到这个文件夹里，该文件夹下的所有东西会被原封不动地发布。对于创作者来说，这部分编译生成的东西不需要过分关心。因为具体编译出什么样的效果都可以在编译之前进行设置。

`archetypes`中存放的好像是模板之类的东西，hugo 官网对此有一些介绍，但我没有深入研究，目前只是用了些简单的配置。简单来说，这部分中有一个名为 `default.md`的模板文件，里面存放着一些默认配置，当使用命令 `hugo new xxx/xxx.md`创建新的博客的时候，会自动按照该模板文件进行创建。该模板包含一些默认配置，用户可以对不同博客的配置进行修改。

接下来讨论怎么设置主题，经过一段时间纠结的筛选后，决定暂时使用`even`主题，主要原因有：
* 主题看来还算简洁
* 维护还算活跃
* 该主题是一个博客性质的主题，比较符合目前需求
* 该主题的开发者是国人同胞，可能比较符合国人的应用习惯

选定主题后，就将该[主题](https://github.com/olOwOlo/hugo-theme-even) clone 到 themes 文件夹下并命名为 even 。然后将该仓库添加为本仓库的子模块。
```shell
git submodule add https://github.com/olOwOlo/hugo-theme-even ./themes/even
```

然后可以参考 even 主题的教程，将其配置文件进行拷贝和自行配置。其配置方面的文档较少，但好处是其配置文档中都使用了中文对其作用和用法进行了大概的注释，同样也可以根据需求进行增添内容，具体细节将后续进行学习。

------------21/04/07更新-----------

感觉 even 有些功能不理想，比如说分类和标签没有任何区分度，并且作者似乎已经停止开发了，所以准备转到 LoveIt 的继任者 [CodeIT](https://github.com/sunt-programator/CodeIT) 上，不过 even 也有优点，就是作者是国人，所以一些特性（如百度统计、不蒜子等）还是挺符合国人习惯和应用需求的。
但是写博客不就是为了折腾吗？:joy:，把仅有的几个博客来回迁移:joy:。选定新的主题后，准备进行迁移，下面准备讲述更新过程。

使用这两个主题的时候都会遇到一个问题，就是数学公式的显示问题，感觉两个主题都有一些不尽如人意的地方，接下来将讲述我遇到的问题，以及目前所采用的方式。希望以后能够有机会了解 hugo 的细节，从而更好的解决该问题。

关于该问题，请参考[相关文章](../21-12-04_math-in-markdown/)
### 主题开发

------------21/10/26更新-----------

鉴于已有主题的不尽如人意，所以本着折腾的精神，本人决定基于 CodeIT 主题进行折腾，主要目标有两个：1.增加自己想要的特性（如不蒜子统计阅读量等）2.删除过于冗余的功能

新的主题名为 [EnjoyIt](https://github.com/xinyu-yang/EnjoyIt)，意为享受折腾写博客的过程。本人所有的对本博客的设置都将体现在该主题上。


------------21/11/4更新------------

感觉原主题的白色背景太过刺眼，所以就换了个米黄色的背景颜色。
另外，感觉原本主题的默认字体不是特别好看，尤其是在同时有中文和英文的情况下，所以本人打算更改下默认字体。但经摸索后发现现有设置是加载最高效的方式（优先使用默认字体），决定暂时还是不改了，以性能为重。

-----------21/11/19更新------------

结合自己对于模板的学习，新增加了该章节。这部分主要记录自己在摸索过程中所学到的一些东西。
首先，对于一个主题，主要有以下目录结构：

```
|-- [theme name]
\
 |-- archetypes
 |-- assets
 |-- exampleSite
 |-- i18n
 |-- images
 |-- layouts
 |-- resources
 |-- src
 |-- static
 |-- theme.toml
```
其中，`theme.toml`即为配置文件模板。archetypes 文件夹中存放的东西与上文介绍相同，即一些模板文件。
assets 文件夹中存放了 css 文件和所有使用到的 javascript 库。
exampleSite 中存放了一个示例站点，感觉是用来调试主题的，切换到该目录下可以直接运行 `hugo server` 进行查看。该目录下的目录结构与上文所述网站目录相似，当然肯定没有 themes ，要不然不就循环依赖了嘛。

i18n 目录下存放的是用来进行多语言支持的一些文件。images 文件夹顾名思义，就是用来存放一些用户的图片。
static 也可以用来存一些图片，但是这部分图片一般是主题的一部分，而不像 images 中存放用户图片。同时该目录下还会存一些如字体、emoji 表情等静态文件。
resources 目录没搞明白，不过看起来像是存了一些自动生成的文件。
至于 src 目录则是存放了该主题的源代码，这个文件夹似乎是自定义的，而不是主题须有的常规文件夹。

之所以把 layouts 文件夹放到最后，就是因为该文件夹下存了关于主题配置的大部分内容。实际上我对于主题的定制化工作，也大部分都是在此目录下完成的。首先看下该文件夹下的重点目录：
```
|-- _default
|-- partials
   \
    |-- assets.html
    |-- comment.html
    |-- footer.html
    |-- function
    |-- head
    |-- header.html
    |-- home
    |-- init.html
    |-- paginator.html
    |-- plugin
    |-- rss
    |-- scratch
    |-- single
|-- posts
|-- robots.txt
|-- shortcodes
|-- taxonomy
```
其中，重点的配置主要都集中在 partials 目录，该部分定义了博客的 header, footer, comment 等，同时针对不同的界面 home, single 等也可以有不同的配置。所有的这些配置采用了 hugo 的语法进行编写，所以如果想要自己定制的话，需要先学习下相应的语法知识。


### 评论系统

不得不说的一个话题是**评论系统**，一个完善的博客平台，需要有与读者直接交流的方式。目前有很多解决方案，成熟的解决方案有 disqus，当时好像需要后端的支持，但像我这种想把博客免费部署到 github 上的人来说，后端支持太奢侈了。

幸运的是开源社区有很多很好的“无后端（白嫖后端）”评论系统，像是 utterances, gitalk, [valine](https://valine.js.org) 等，前两者都使用了 github 的 issue 机制来实现评论的记录和用户的管理。界面漂亮，功能稳定，如果搭载 github page 使用，真的是原汤化原食， github 全家桶。
而且我最近又在 github 上看到了一个类似的项目 giscus, 该项目几乎完全借鉴了 utterances，但改进在于该评论系统使用了 github 最新推出的 discussions 功能进行评论的管理，相对于 issue 机制来说，这种方式明显看起来更专业。因为 discussion 模块本来就是用来讨论的，所以可以对别人的评论进行直接回复，而不是只有一个`@xxx`。对于想使用 github 托管评论的博主，giscus 是更加先进的选择。

{{< admonition type=tip title="总结" >}}
如果使用 github 进行托管的话，就用 giscus
{{< /admonition >}}

以上基于 github 的评论系统做的都非常优秀，但是他们有一个共同的问题，就是用户需要使用 github 账号登录才能进行评论。这对于像我这种程序员来说不是问题，毕竟我本人使用 github 的频率和某信差不多，但是这并不适用所有人。
而且当我看到 [valine](https://valine.js.org) 简洁漂亮的界面，以及无需登录就可以评论的功能时，我决定尝试下该评论系统。配置好之后感觉还是很满意的，具体效果见文章末尾。

关于不同评论系统的对比，有位同学写了篇[文章](https://www.yunyoujun.cn/share/third-party-comment-system/)，我感觉还不错，不了解的同学可以参考下。
另外，从他的文章中我又了解了另外一个评论系统 [waline](https://waline.js.org/)，该博客系统似乎借鉴了 [valine](https://valine.js.org)，属于后来者，似乎也是居上者。

========= 12/3/2021/ 更新=========

在尝试了 valine 评论系统若干天之后，我放弃了，原因包括但不限于：
- 该项目维护不太活跃，而且作者也已经不再开放源码 [参考](https://github.com/xCss/Valine)
- 该项目的配置稍显复杂，而且需要借用其他的项目如 valine-admin 来实现一些功能
- 最重要的一点，该项目所基于的 LeanCloud 似乎在有意遏制免费用户的使用

其中最重要的是第三点，而这一点展开来讲又有以下几点原因：
- LeanCloud 免费版每天有强制至少 6 小时的休眠时间
- 如果 30 分钟无活动，LeanCloud 则自动休眠，有新评论会重新启动，但第一条评论无法触发通知
- 针对上一条问题的解决方案为定时重启，但是后来 LeanCloud 几乎禁止了该操作 [参考](https://www.google.com/search?q=valine+%E5%9B%A0%E6%B5%81%E6%8E%A7%E5%8E%9F%E5%9B%A0%E8%87%AA%E5%8A%A8%E5%94%A4%E9%86%92%E4%BB%BB%E5%8A%A1%E5%A4%B1%E8%B4%A5)
- LeanCloud 经常更换 API 域名 [参考](https://github.com/xCss/Valine/issues/377#issuecomment-983259692)，这一点很难让人理解。

综上所述，我更换为了 [Twikoo](https://twikoo.js.org)，现在刚开始使用，感觉还不错，唯一的问题就是没人评论~

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
该命令将启动一个本地服务，端口号为1313，在本地使用浏览器访问：[http://localhost:1313](http://localhost:1313)即可在线预览自己的博客内容。每次保存文件时，hugo都会自动进行编译，速度很快，几乎实现了markdown的同步预览，非常方便！

在发布之前应该使用
```
hugo -D
```
编译一下，整个网站的源代码会输出到 public 文件夹，至此已经可以实现基本的功能。


### 使用github发布博客
首先要有一个github账号，然后创建一个新的仓库，其名字为：[your github id].github.io，关于这方面的教程非常多，也很简单，这里就不再赘述。然后其部署可参考[https://gohugo.io/hosting-and-deployment/hosting-on-github/#build-hugo-with-github-action](https://gohugo.io/hosting-and-deployment/hosting-on-github/#build-hugo-with-github-action)。其操作非常方便，只需要创建一个文件，然后将一些github action代码复制进去就行了。具体原理后续研究...(这里不得不感叹一句，github的功能比我想象的要强大多了!)

然后就是将本地文件(即blog/文件夹)push到所创建的仓库中。最后需要注意的是在上面所创建的仓库的设置页面要进行一定的设置，其位置为`setting->github pages->source`，需要将branch更改为`gh-pages`，路径为`root`，在这里还可以设置定制化的域名等，这里先暂时不研究。通过以上设置应该就可以成功访问页面了。如本博客主页为：*http://xinyu-yang.github.io*

-----------21/11/19更新------------

后来发现，因为把原文档 push 到 github 仓库之后，github action 会自动进行编译。所以在 push 之前，甚至不用自己编译，直接将原文档 push 上去即可。


### 图床
以前用CSDN和博客园写的时候根本不用考虑这个问题，但是现在自己搭建博客的时候就需要好好考虑一下这个问题。一开始惊喜的发现有很多的免费图床网站（比如imgbb），开心的以为这个问题解决了，后来发现没那么简单。因为这些免费的图床往往有很多限制，比如上面举例说的这个imgbb，该网站一开始用着还行，可是过几天再在博客里加载图片时只能加载出缩略图了。

后来就寻求解决方案，最后的决定是仍然使用开源的代码托管网站托管图片，把白嫖进行到底:joy:。最终我决定使用[PicGo](https://molunerfinn.com/PicGo/)+(Gitee/Gitlab/Github)实现。

图床部分本来只有简单介绍，后来觉得各种东西都放在同一篇博客就显得太混杂了，因此另起了一篇新的博客进行介绍——[文章地址](../21-09-09_picture-bed/)
