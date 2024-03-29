---
title: "Hello World"
date: 2021-03-04T16:25:56+08:00
draft: false
keywords: ["hugo", "even"]
description: ""
tags: ["blog"]
categories: ["record"]

math:
  enable: true

lightgallery: true

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

感觉even有些功能不理想，比如说分类和标签没有任何区分度，并且作者似乎已经停止开发了，所以准备转到LoveIt的继任者[CodeIT](https://github.com/sunt-programator/CodeIT)上，不过even也有优点，就是作者是国人，所以一些特性（如百度统计、不蒜子等）还是挺符合国人习惯和应用需求的。
但是写博客不就是为了折腾吗？:joy:，把仅有的几个博客来回迁移:laugh:。选定新的主题后，准备进行迁移，下面准备讲述更新过程。使用这两个主题的时候都会遇到一个问题，就是数学公式的显示问题，感觉两个主题都有一些不尽如人意的地方，接下来将讲述我遇到的问题，以及目前所采用的方式。希望以后能够有机会了解hugo的细节，从而更好的解决该问题。

首先是even主题，该主题使用的是mathjax进行公式的渲染，经过网上了解，mathjax是一个很成熟的解决方案，所以其对数学公式的支持还是很完善的，但是在使用较复杂的数学公式环境过程中还是遇到了一个问题，如:
```markdown
\begin{align}
x &= y + 1 \\
y &= z + 2
\end{align}
```
在$\LaTeX$中是没有问题的，但是在hugo就无法被正确渲染，只有将换行符`\\`更改为`\\\\`才能正常工作，好像看到网上有相关的讨论（没有记录），说导致该问题的原因是markdown编译器在进行解析的时候会将第一斜杠解释为*转义*，那么最后的结果是只剩一个斜杠，所以如果想让其最后仍然有两个斜杠，则不得不用四个斜杠。虽说敲四个斜杠是麻烦了点，但是好歹问题算是大概解决了。

后来试用CodeIT主题时则出现了更严重的问题，主要是因为该主题使用的是据说是性能更好的$KaTeX$渲染器。该渲染器作为托管在github的开源项目还是挺受欢迎的，但是katex对于公式的支持就不那么完善了，截止到目前的最新版本0.13.0，刚能够支持诸如`align,align*`等环境。而CodeIT使用的仍然是0.11.1的老版本，但这还不是主要问题，主要问题是原本在mathjax中可行的方案在katex中不可行了，比如根据[文档](https://katex.org/docs/0.11.1/supported.html)katex v0.11.1中是支持`aligned`环境的，但是以下代码并不能够被正确渲染：
```markdown
\begin{aligned}
x &= y + 1 \\\\
y &= z + 2
\end{aligned}
```
该代码可以在even主题下正常工作，但是无法在CodeIT在工作，经网上搜索，发现了该[解决方案](https://github.com/halogenica/beautifulhugo/issues/183)，经该方案介绍，使用如下代码可正常工作：
```markdown
\begin{aligned}
x &= y + 1 \cr
y &= z + 2
\end{aligned}
```
其效果为：
$$
\begin{aligned}
x &= y + 1 \cr
y &= z + 2
\end{aligned}
$$

主要改动为将斜杠更换为`\cr`，经测试可以正常工作，然后将该方式在even主题中测试也可以工作，所以就计划采用这种方式来代替双斜杠的功能，虽然感觉像是最后对问题的妥协，但是好歹是解决了该问题:joy:

后来有对问题做进一步的探索，做法是在浏览器中查看网页的源代码，我发现使用斜杠做为换行符的方式都会在公式行的最后引入一个`<br>`标签，如下所示：
```html
\begin{aligned}
x &amp;= y + 1 \\ <br>
y &amp;= z + 2
\end{aligned}
```
具有标签的代码在使用mathjax渲染器的时候是可以被正常渲染的，但是在katex中却不行。而使用`\cr`进行换行则不会引入`<br>`标签，因此在katex中可用。
最后附上将换行符更改为`\cr`的命令:
```shell
sed -i 's/\\\\\\\\/\\cr/' [blogname].md
```

------------21/04/08更新-----------

除上述所示问题外，今天又遇到了另外一个问题，那就是在公式中使用特殊字符（如%）时需要进行转义，一般写为`\%`，但是这又会遇到一个问题，就是在\$符号之间的字符不能够像在 \` 符号中那样被免于转义。也就是说`\%`中的斜杠会在markdown解析的过程中解析为转义字符，因此到公式解析器的时候剩下的就只剩一个%符号了，所以需要使用两个符号才行。关于这个问题，感觉主要问题还是在markdown解析器那里没有执行正确的解析。当然，以上只是我的猜想，有时间我会去验证的。

知道问题之后的解决方式呢？如果实在不得不用特殊字符的转义，那就使用上述方法，可以解决。如果不是必要，比如我这里就是将%作为模运算符号，其实math里有专门的符号`\mod`表示该含义，所以出于兼容性期间，决定使用`\mod`符号，因为万一哪一天markdown解析器可以正常解析了，岂不是还要把两个斜线换成一个。

同时这里有一个总结：**在markdown的公式环境中尽量减少使用转义符:point_right:`\`:point_left:**

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
编译一下，至此已经可以实现基本的功能

### 使用github发布博客
首先要有一个github账号，然后创建一个新的仓库，其名字为：[your github id].github.io，关于这方面的教程非常多，也很简单，这里就不再赘述。然后其部署可参考[https://gohugo.io/hosting-and-deployment/hosting-on-github/#build-hugo-with-github-action](https://gohugo.io/hosting-and-deployment/hosting-on-github/#build-hugo-with-github-action)。其操作非常方便，只需要创建一个文件，然后将一些github action代码复制进去就行了。具体原理后续研究...(这里不得不感叹一句，github的功能比我想象的要强大多了!)

然后就是将本地文件(即blog/文件夹)push到所创建的仓库中。最后需要注意的是在上面所创建的仓库的设置页面要进行一定的设置，其位置为`setting->github pages->source`，需要将branch更改为`gh-pages`，路径为`root`，在这里还可以设置定制化的域名等，这里先暂时不研究。通过以上设置应该就可以成功访问页面了。如本博客主页为：*http://xinyu-yang.github.io*


还有很多功能待后续学习更新...

### 图床
以前用CSDN和博客园写的时候根本不用考虑这个问题，但是现在自己搭建博客的时候就需要好好考虑一下这个问题。一开始惊喜的发现有很多的免费图床网站（比如imgbb），开心的以为这个问题解决了，后来发现没那么简单。因为这些免费的图床往往有很多限制，比如上面举例说的这个imgbb，该网站一开始用着还行，可是过几天再在博客里加载图片时只能加载出缩略图了。
后来就寻求解决方案，最后的决定是仍然使用开源的代码托管网站托管图片，把白嫖进行到底:joy:。最终我决定使用[PicGo](https://picgo.github.io/)+(Gitee/Gitlab/Github)实现。
因为Gitee是国内的网站，加载较快，另外就是还没有使用Gitee托管代码的打算，所以不如注册个账号专门用来托管图片，综合以上原因，最终使用了Gitee。但是Gitee也有很多缺点，缺点之一就是空间太小了（这里不得不吐槽一句，本来就是模仿者，又那么小气，没有任何优势，难怪没人用啊~）。

下图是Gitee示例：

![测试](https://gitee.com/xinyu-yang/pic/raw/master/img/Kobe.jpg "测试图片")

最后不得不说一下，感觉PicGo真的是开源代码的典范，自己提供一个框架和一套接口，其他人可以按照接口开发各种各样的代码。我感觉对于实用性的工具应用，这种开发方式非常合适。
功能强大，但不臃肿，用户有足够多的定制选择权。
