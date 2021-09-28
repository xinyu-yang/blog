---
title: "Vim学习记录"
subtitle: ""
date: 2021-09-28T10:34:49+08:00
lastmod: 2021-09-28T10:34:49+08:00
draft: false
author: ""
authorLink: ""
description: ""

tags: ["vim"]
categories: ["record"]

hiddenFromHomePage: false
hiddenFromSearch: false

featuredImage: ""
featuredImagePreview: ""

toc:
  enable: true
math:
  enable: false
comment:
  enable: true

lightgallery: false
license: ""
---

<!--more-->

## 介绍
很久以来，一直想写一些关于Vim的使用博客。但是随着对Vim的了解加深，我意识到自己还达不到创作文章介绍Vim的能力。
一方面是因为自己对Vim的了解实在是有限，很多东西都只是鹦鹉学舌，了解简单的用法，而介绍这些基本用法的博客文章等浩如烟海，因此没有重复赘述的必要。
另一方面是因为随着学习的深入，我也接触到了很多优秀的博客文章，值得仔细研读。同时也意识到自己的水平跟这些文章的作者相去甚远，想要达到同等水平况且需要些时日，更不用说写出更有深度的东西了。

鉴于以上原因，我决定首先将我学习过程中所参考的优秀学习资料进行汇总和简介。如果初学者能看到本文，并因此在学习中少走些弯路，那便是本文的价值。
另外，我应该不会写偏向于大而全的Vim介绍博客，至少目前不会。但是我可能会写一些关于某些功能点的深入学习和记录的文章，我认为在当前网络中所能搜索到的文章中，这类更有价值。

## 综合教程

### Vimtutor
Vimtutor作为自带的官方教程，其作为入门教程的权威性自然不言而喻。当然，除了其明面上的地位之外，其作为一个入门教程，给初学者提供了一个很好的实际操作体验，该教程会提供一些例子供阅读者进行实际的操作，比起仅仅列举式的功能介绍，该导论还是挺容易被初学者接受的。
下面是截取的一部分例子：

```vim
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
                     Lesson 1.3: TEXT EDITING - DELETION


           ** Press  x  to delete the character under the cursor. **

  1. Move the cursor to the line below marked --->.

  2. To fix the errors, move the cursor until it is on top of the
     character to be deleted.

  3. Press the  x  key to delete the unwanted character.

  4. Repeat steps 2 through 4 until the sentence is correct.

---> The ccow jumpedd ovverr thhe mooon.

  5. Now that the line is correct, go on to lesson 1.4.

NOTE: As you go through this tutor, do not try to memorize, learn by usage.


~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
```

整篇教程不长，一小时之内就能读完，然后就能够对平时经常用到的绝大部分功能有一个大概的认识。如果想要熟练掌握，还是要勤加练习，并且强迫自己用不同的方式实现某一功能。

该教程是随Vim一块安装的，打开方式很简单，只需在终端敲入`vimtutor`：
```bash
$ vimtutor
```
如果想打开中文版（对，居然自带中文版:smiley:），可以使用`vimtutor zh_cn`命令。
当然，该文档也可以从Vim中打开，在`normal`模式敲入：
```vim
:e $VIMRUNTIME/vim/tutor/tutor.zh_cn.utf-8
```
不过还是推荐使用`vimtutor`进行打开，因为在使用该命令时，`Vim`会自动把原文档复制一份成临时文件然后打开。因此，对通过该方式打开的文档进行操作不会改变原文档。这样使用者就可以放心操作了。

### 其他入门教程
[《简明Vim练级攻略》](https://coolshell.cn/articles/5426.html)

这是一篇11年的教程，虽然时间稍早，但是排版易读，语言诙谐，并且加入了很多图片和动图，对读者比较友好。目前该文已有超多120万的阅读量:thumbsup:。
这篇教程整体内容跟Vimtutor差不多，可以看成是Vimtutor的补充，读完Vimtutor之后可以读下这篇，跟着操作一遍，可以起到复习的效果。


[《Vim实操教程》](https://www.bookstack.cn/read/learn-vim/README.md)

文如其名，这同样是一份注重实际操作的教程，整体内容也跟上两个文档差不多，可以作为补充一块阅读。
该教程的优点是排版较好，有着类似于书籍的章节排版，逻辑清晰。而且相对上两篇而言，比较新一些，同时有一些关于插件的简单介绍。

[《每日一vim》](https://liuzhijun.iteye.com/category/270228)

这是一个教程系列，共计30篇，每篇介绍一个功能，篇幅较短，阅读的时候不会有心里负担，当然对于功能的介绍也是浅尝辄止，读者可以根据需要有选择性的阅读。


### [《Vim从入门到精通（vim-galore-zh_cn）》](https://github.com/wsdjeg/vim-galore-zh_cn)
当搜索Vim的学习教程时，很多人都推荐《Vim galore》，这篇文章就是其中文翻译。虽然中文翻译有点俗套的编程语言教程内味，但是并不影响这是一篇很好的Vim教程。

该仓库下就链接了其他语言的版本，英文好的可以读英文原版，不太好的可以读中文，并且该教程还有日语、西班牙语和俄语选项，都是开源社区贡献的，这也从侧面印证了该教程的牛x。

相对于上面所提到的入门教程，该文章提供了更加深入的介绍，但同时也不那么容易阅读，需要反复咀嚼才能将其学透。
我在第一次看该教程的时候，已经读了上面那些教程，并且有了一段时间的Vim使用经验，但是还是有很多地方读不懂和不理解。后来慢慢的对于里面的内容有了认识（比如那么多的寄存器），但是还是有些不太懂。
所以建议初学者可以先不要读此教程，待有了一定的基础之后再来阅读，当然该教程也需要反复复习。越是经典的教程越是值得我们反复咀嚼，并且每次阅读都会有新的收获。

### [Vim学习笔记](https://yyq123.github.io/learn-vim/)
这是一个颇为低调的学习笔记，如果不是偶然看到，可能很难搜索到该教程，该教程所对应的github博客主页甚至都是空白的。据作者称，这是他从一本书整理而来。



## 插件
### [《所需即所获：像 IDE 一样使用 vim》](https://github.com/yangyangwithgnu/use_vim_as_ide)
这是我阅读的第一篇关于Vim插件配置的文章，主要是我在搜索关于Vim配置的文章的时候，这篇文章高居搜索引擎榜首。虽然已经五年没有更新了，但仍然在github上有9k Star。


