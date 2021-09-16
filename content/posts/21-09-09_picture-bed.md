---
title: "可同步图床配置——PicGo+Gitee/Github/Gitlab+Onedrive"
subtitle: ""
date: 2021-09-09T11:05:02+08:00
lastmod: 2021-09-09T11:05:02+08:00
draft: true
author: ""
authorLink: ""
description: ""

tags: ["picture bed"]
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
本文主要用来记录图床的搭建过程，在写博客的过程中不可避免的可能会用到一些图片，因此图片的保存位置就成了一个需要思考的问题，这个问题在使用博客网站CSDN、博客园等是不存在的，因为在使用这些网站时，只需要把图片传上去就行，图片会自行保存在这些平台所提供的图床上。曾经尝试着在本博客里直接引用博客园和CSDN中的图片，不出意外，果然加载不出来，可能是因为这些网站在处理请求的时候会检查`Referer`字段吧，如果仅是这种检查的话似乎可以避开，但是终究不是长久之计，因此就在网上搜索解决之道，经琢磨了多篇解决方案之后，决定选择可能目前最合适的方案：使用PicGo作为图床管理软件，Gitee作为存储平台，并且使用Onedrive来同步PicGo的配置文件和数据库（新功能:clap:），以实现备份和同步。接下来将就该方案进行介绍：
## 申请Gitee账号，创建仓库

## PicGo端配置

## Onedrive同步配置

