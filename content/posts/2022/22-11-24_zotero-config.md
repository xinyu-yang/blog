---
title: "Zotero 配置记录（二：坚果云 WebDAV）"
subtitle: ""
date: 2022-11-24T11:18:16+08:00
lastmod: 2022-11-24T11:18:16+08:00
draft: false
author: "Xinyu Yang"
authorLink: ""

keywords: ["zotero"]
description: "该博客记录了本人所使用的基于坚果云的 Zotero 配置"
images: []

tags: ["config"]
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

之前写过 [一篇](../22-02-08_zotero-config/) 使用 `Zotfile + OneDrive` 软链接的方式实现附件同步的记录，但是最近准备切换到坚果云。

之所以使用坚果云所提供的 WebDAV 做备份，不是因为原来的方案不能用。事实上，原来的方案不管在 Windows 平台还是 MacOS 上都表现得很好。
但是如果想要在移动设备上（主要是平板）使用同步功能，软链接的方式就很麻烦了。因为移动设备不支持软链接。

我之前的做法是在 Zotero 里看一下基本信息，如年份、类别、文件名等。然后去 OneDrive 里找到文件，打开。虽然操作也没那么复杂，但是这种方式总让人觉得不优美，很麻烦。
正好有同学在 iPad 上使用 `Zotero + 坚果云`，我就试了一下，然后发现这种方式太漂亮了！直接沦陷，决定放弃原来的方案，转投坚果云。

## 坚果云配置

坚果云 [免费版](https://www.jianguoyun.com/s/pricing) 提供每月 1GB 上传和 3GB 下载。算了一下，感觉对于纯文档操作来说，绰绰有余。

本来坚果云的配置非常方便，但是网上的教程不是写的冗长，就是分不清软链接硬链接的关系，让人看了一头雾水。简单的一个过程非得写的那么麻烦，真是蛋疼。本文将准确直白的记录下该过程。

首先注册坚果云账号，这步没什么难度。但是值得注意的是网上有人说使用带符号的邮箱注册的话，在 Zotero 中配置会有问题。
我本人的邮箱不存在该问题，但是还是提醒大家注意一下。

登录之后，点击 `账户信息`，然后切换到 `安全选项` 部分，在底部点击`添加应用`。
然后随便起个名字，就写 `Zotero` 吧，注意这个名字仅仅是个名字，跟随后的存储目录无关。
然后你会获得一个密码。到此为止，你所拥有的信息有：

1. 服务器地址：https://dav.jianguoyun.com/dav/ （界面上就有）
2. 账户：xxx@xxx.xx （就是注册邮箱）
3. 应用密码：xxxxx （就是刚才所获得的密码）

## Zotero 配置

使用这三个信息，你就可以在 `Zotero -> Preference -> Sync` 页面的 `File Syncing`部分进行配置。选择 `Using WebDAV`，然后将上述三条信息填上，然后点击 `Verify Server`。
理想情况应该显示同步成功。
随后点一下 Zotero 的同步按钮（就是那个绿色的小圆圈箭头）同步下。

多点几次，耐心等等，确认都同步上去了，这个可以通过网页打开坚果云查看，点击 `我的文件`，就有一个叫 `zotero` 的文件夹。

待同步之后，还要进行的一个操作就是删除在 Zotero 云盘中占用的空间。
首先打开 https://www.zotero.org/settings/storage , 登录。
然后在该页面底部可以看到一个 `Purge Storage in My Library` 按钮，该按钮将会删除所有的存在 Zotero 云盘中的文件。所以操作之前确定备份好所有的文件！

然后就大功告成了，iPad 和手机端配置类似。
