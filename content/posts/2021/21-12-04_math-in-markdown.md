---
title: "Markdown 中的数学公式"
subtitle: ""
date: 2021-12-04T00:11:45+08:00
lastmod: 2021-12-04T00:11:45+08:00
draft: false
author: "Xinyu Yang"
authorLink: ""

keywords: ["markdown", "mathjax", "katex"]
description: "本文主要记录在 markdown 中编写公式所遇到的几个问题"
images: []

tags: ["markdown", "blog", "math"]
categories: ["record"]

hiddenFromHomePage: false
hiddenFromSearch: false
featuredImage: ""
featuredImagePreview: "https://pic.imgdb.cn/item/62853a460947543129f3afc6.jpg"

toc:
  enable: false
math:
  enable: true
comment:
  enable: true
lightgallery: false

license: ""
---

<!--more-->

本文主要记录在使用不同的主题时，所遇到的有关 markdown 中公式渲染的问题。

首先是 even 主题，该主题使用的是 mathjax 进行公式的渲染，经过网上了解， mathjax 是一个很成熟的解决方案，所以其对数学公式的支持还是很完善的，但是在使用较复杂的数学公式环境过程中还是遇到了一个问题，如:
```markdown
\begin{align}
x &= y + 1 \\
y &= z + 2
\end{align}
```
在 $\LaTeX$ 中是没有问题的，但是在 hugo 就无法被正确渲染，只有将换行符`\\`更改为`\\\\`才能正常工作，好像看到网上有相关的讨论（没有记录），说导致该问题的原因是 markdown 编译器在进行解析的时候会将第一斜杠解释为*转义*，那么最后的结果是只剩一个斜杠，所以如果想让其最后仍然有两个斜杠，则不得不用四个斜杠。虽说敲四个斜杠是麻烦了点，但是好歹问题算是大概解决了。

后来试用 CodeIT 主题时则出现了更严重的问题，主要是因为该主题使用的是据说是性能更好的 $KaTeX$ 渲染器。该渲染器作为托管在 github 的开源项目还是挺受欢迎的，但是 katex 对于公式的支持就不那么完善了，截止到目前的最新版本 0.13.0，刚能够支持诸如`align,align*`等环境。而 CodeIT 使用的仍然是 0.11.1 的老版本，但这还不是主要问题，主要问题是原本在 mathjax 中可行的方案在 katex 中不可行了，比如根据[文档](https://katex.org/docs/0.11.1/supported.html) katex v0.11.1 中是支持`aligned`环境的，但是以下代码并不能够被正确渲染：
```markdown
\begin{aligned}
x &= y + 1 \\\\
y &= z + 2
\end{aligned}
```
该代码可以在 even 主题下正常工作，但是无法在 CodeIT 在工作，经网上搜索，发现了该[解决方案](https://github.com/halogenica/beautifulhugo/issues/183)，经该方案介绍，使用如下代码可正常工作：
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

主要改动为将斜杠更换为`\cr`，经测试可以正常工作，然后将该方式在 even 主题中测试也可以工作，所以就计划采用这种方式来代替双斜杠的功能，虽然感觉像是最后对问题的妥协，但是好歹是解决了该问题:joy:

后来有对问题做进一步的探索，做法是在浏览器中查看网页的源代码，我发现使用斜杠做为换行符的方式都会在公式行的最后引入一个`<br>`标签，如下所示：
```html
\begin{aligned}
x &amp;= y + 1 \\ <br>
y &amp;= z + 2
\end{aligned}
```
具有标签的代码在使用 mathjax 渲染器的时候是可以被正常渲染的，但是在 katex 中却不行。而使用`\cr`进行换行则不会引入`<br>`标签，因此在 katex 中可用。
最后附上将换行符更改为`\cr`的命令:
```shell
sed -i 's/\\\\\\\\/\\cr/' [blogname].md
```

------------21/04/08更新-----------

除上述所示问题外，今天又遇到了另外一个问题，那就是在公式中使用特殊字符（如%）时需要进行转义，一般写为`\%`，但是这又会遇到一个问题，就是在\$符号之间的字符不能够像在 \` 符号中那样被免于转义。也就是说`\%`中的斜杠会在markdown解析的过程中解析为转义字符，因此到公式解析器的时候剩下的就只剩一个%符号了，所以需要使用两个符号才行。关于这个问题，感觉主要问题还是在markdown解析器那里没有执行正确的解析。当然，以上只是我的猜想，有时间我会去验证的。

知道问题之后的解决方式呢？如果实在不得不用特殊字符的转义，那就使用上述方法，可以解决。如果不是必要，比如我这里就是将%作为模运算符号，其实math里有专门的符号`\mod`表示该含义，所以出于兼容性期间，决定使用`\mod`符号，因为万一哪一天markdown解析器可以正常解析了，岂不是还要把两个斜线换成一个。

同时这里有一个总结：
{{< admonition type=tip title="总结" >}}
**在markdown的公式环境中尽量减少使用转义符:point_right:`\`:point_left:**
{{< /admonition >}}

