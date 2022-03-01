---
title: "可同步图床配置——PicGo+Gitee/Github/Gitlab+Onedrive"
subtitle: ""
date: 2021-09-09T11:05:02+08:00
lastmod: 2021-09-09T11:05:02+08:00
draft: false
author: ""
authorLink: ""
description: "本文用来记录本博客所使用的图床配置"

tags: ["blog"]
categories: ["record"]

hiddenFromHomePage: false
hiddenFromSearch: false

featuredImage: ""
featuredImagePreview: "https://gitee.com/xinyu-yang/pic/raw/master/img/Kobe.jpg"

toc:
  enable: true
math:
  enable: false
comment:
  enable: true

lightgallery: true
license: ""
---

<!--more-->

## 介绍
本文主要用来记录图床的搭建过程，在写博客的过程中不可避免的可能会用到一些图片，因此图片的保存位置就成了一个需要思考的问题，这个问题在使用博客网站 CSDN、博客园等是不存在的，因为在使用这些网站时，只需要把图片传上去就行，图片会自行保存在这些平台所提供的图床上。曾经尝试着在本博客里直接引用博客园和 CSDN 中的图片，不出意外，果然加载不出来，可能是因为这些网站在处理请求的时候会检查`Referer`字段吧，如果仅是这种检查的话似乎可以避开，但是终究不是长久之计。

一开始惊喜的发现有很多的免费图床网站（比如imgbb），开心的以为这个问题解决了，后来发现没那么简单。因为这些免费的图床往往有很多限制，比如上面举例说的这个imgbb，该网站一开始用着还行，可是过几天再在博客里加载图片时只能加载出缩略图了。

因此就又在网上搜索解决之道，经琢磨了多篇解决方案之后，决定选择可能目前最合适的方案：使用 PicGo 作为图床管理软件，Gitee、SM.MS 等作为存储平台，并且使用 Onedrive 来同步 PicGo 的配置文件和数据库（这是`PicGo 2.3`版本以来的新功能:clap:），以实现备份和同步。接下来将就该方案进行介绍：

## 申请 Gitee 账号，创建仓库
因为 Gitee 是国内的网站，加载较快，另外就是还没有使用 Gitee 托管代码的打算，所以不如注册个账号专门用来托管图片，综合以上原因，最终使用了 Gitee。但是 Gitee 也有很多缺点，缺点之一就是空间太小了（这里不得不吐槽一句，本来就是模仿者，又那么小气，没有任何优势，难怪没人用啊~）。

如果没有 Gitee 账号的话，就去申请一个。账号申请就按照步骤来就行，没什么可说的。我申请的时候发现我在 github 上使用的 ID 居然还没被别人占用，就果断申请了该 ID，在 Github、Gitlab 和 Gitee 上做到了统一。

申请完账号后，登录，然后进入个人信息界面，随后点击左侧栏中的仓库空间管理，随后右上角会出现“新建仓库”的按钮。此时就可以新建仓库了，取名随意，但仓库必须得是公有的。

然后下一步是创建私人令牌，以保证 PicGo 可以控制该仓库。同样在个人设置界面的左侧“安全设置”栏下有一个“私人令牌”选项，找到并点击。然后点击右上角的生成新令牌，填写描述信息，然后选择权限，我只选择了前三个权限——`user_info, projects, pull_requests`。
我感觉这些应该就够了，最后点击提交，然后会生成一个 token ，暂时用记事本等保存该 token ，因为它不会再次出现。

## PicGo 端配置
生成并保存完 token 之后，就可以进行 PicGo 端配置了。因为该软件不默认支持 Gitee 图床，所以只能通过安装插件来支持，在插件设置的搜索栏里搜索 gitee-uploader，然后点击安装。安装完之后点击插件图标右下角的设置按钮，找到“配置uploader-gitee”，点击之后会出现一个界面，主要会出现以下几条待填信息：

* repo      (就是刚才所创建的仓库，不过这里的格式是`[your id]/[repo name]`，前面的`gitee.com`就不要填了)
* branch    (一般为master) 
* token     (这就是刚才所保存的token)
* path      (文件的保存路径，可以自行设置，我的设置是“img/$customPath”)
* customPath    (这就是path设置里的那个参数，我选的“年”)
* customUrl (没填)

到这里，差不多就配置好了，可以随便上传一张图片试试能不能用，我这里上传了一张蜗壳的照片。
下图是Gitee示例：

![测试](https://gitee.com/xinyu-yang/pic/raw/master/img/Kobe.jpg "测试图片")


## Onedrive同步配置
PicGo是一个很棒的工具，但是之前有一个很大的问题就是没有办法进行不同主机之间的同步。这就导致在一台主机上上传过的图片只在该电脑保存有记录，其他电脑是找不到的。当然，这可以通过直接去图床网站上找链接，或者是在电脑之间复制PicGo的配置文件来解决。
但是这些解决方案都太麻烦了，而我又是一个比较懒的人。

目前最新的PicGo 2.3.0 仍然不能解决该问题(事实上让一个开源软件作者来搭建服务器并提供同步功能确实有点苛刻)，但是该版本提供了自定义配置文件路径的功能，为解决同步问题提供了可能。
我所采用的方案是将PicGo的配置文件所在的文件夹使用第三方云同步平台进行同步，这样就实现了同步功能。

可行的同步平台有很多，如 Onedrive、Dropbox、Google Drive、seafile (如果有的话)等。甚至 Baidu 云盘好像最近也免费提供了类似的服务“同步空间”，虽然每月只有1G的免费流量，但总比没有强。

> 说到这里岔个题，我一直感觉用户对百度网盘的要求过于苛刻了，对于一样免费的产品，百度云盘是唯一一个能够为我提供2T免费存储空间的平台，这一点迄今还找不到替代者。至于很多用户一直以来报怨的下载速度慢的问题，我只想说反正都是白嫖的，还要啥自行车。从我自己的使用角度来讲，平时用百度网盘保存同步或者是备份一些不经常用的文件，如照片、打包的文档资料等，还是挺好的。

书归正传，首先找到一个适合自己的同步工具，我使用的是 Onedrive，其同步文件夹为`D:\Onedrive\`，如果想改变 PicGo 的默认配置路径，则需要首先打开配置文件，然后加入这么一行配置信息：

```
"configPath": "D:\\Onedrive\\ApplicationData\\picgo\\data.json",
  "picBed": {
```

这是我自己的配置，个人根据情况不同进行修改，另外根据官方建议，可以手动将`data.json, pigco.db`文件复制到自己的自定义文件夹内。


## 总结
到这里就完成了配置和自动同步配置，当然也可以添加一些其他的免费图床`SM.MS, Superbed`等图床作为备份，我也确实是这么做的。


最后不得不说一下，感觉 PicGo 真的是开源代码的典范，自己提供一个框架和一套接口，其他人可以按照接口开发各种各样的代码。我感觉对于实用性的工具应用，这种开发方式非常合适。
功能强大，但不臃肿，用户有足够多的定制选择权。