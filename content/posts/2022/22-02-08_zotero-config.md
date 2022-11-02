---
title: "Zotero 配置记录"
subtitle: ""
date: 2022-02-08T22:21:16+08:00
lastmod: 2022-02-08T22:21:16+08:00
draft: false
author: "Xinyu Yang"
authorLink: ""

keywords: ["zotero"]
description: "该博客记录了本人所使用的 Zotero 配置"
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

## Zotero 配置记录

随着所读论文的增多，渐渐觉得有必要对读过的论文进行整理归类。而之前所用的方法可以称之为“文件夹管理”，即通过文件夹对论文进行分类存放。
但感觉这种方式不够灵活专业，更重要的是在写论文的时候还得一个个手动查找添加 bib 文件。
工具都是为需求而生的，以前没读过几篇论文的时候一直 get 不到文献管理工具的意义。
待我尝试之后，才发现该类工具的强大之处。

我最开始尝试的是 EndNote。原因是以前听说过该工具非常强大，并且我们学校恰好有正版软件。
于是很快我就从学校网站上下载了该软件。鼓捣了一会愣是没搞明白怎么使用，从网上看了一些教程，也还是一头雾水。
按理来说，除了非常专业的工具外，对于经常使用各种软件的我，一般的软件不至于连功能都看不懂。
这对我来说是个很大的打击。

我本人有一个习惯，就是选择某个选项之前，即使是某类软件的选择，也会进行大量的调研，读各种博客经验分享，甚至是实际尝试，最后做出选择。
读了一些网上的帖子之后，我将目光锁定到了 Zotero 和 Mendeley 上。

- Zotero 开源免费（好印象），易用，但同步空间较少（只有 300M）
- Mendeley 闭源免费，易用，空间较多 （2GB）

经过衡量我决定采用 Zotero + Onedrive 的方式，Zotero 进行文献管理，Onedrive 进行文件同步，Nice!
经过实际尝试之后，发现 Zotero 相对于 EndNote 操作简单直观，真是好用太多了。
以下是相关配置：

{{< admonition type=tips title="NOTE" open=true >}}
不同设备的语言设置务必相同
{{< /admonition >}}

### Zotero Sync
链接所注册账户，对文件同步进行简单的设置，具体步骤：

打开 Edit -> Zotero Preferences -> Sync
- Settings -> Data Syncing: 链接账户
- Settings -> File Syncing -> Sync attachment files in My Library using: `Zotero`: True

### ZotFile 插件
该插件可以将所导入的 pdf 等格式的文件自动归类到所设置的目标目录。对我来说，目标目录是 Onedrive 的同步目录。

下载：从[该地址](http://zotfile.com/#how-to-install--set-up-zotfile)下载

安装：Tools -> Add-ons -> ⚙(Install Add-on From File...)

相关配置：
打开 Edit -> Zotero Preferences
打开 Zotero -> Preferences (MacOS)
1. Advanced
    - Files and Folders -> Base directory: `E:\OneDrive\ZotFile`
    - Files and Folders -> Base directory: `/Users/[username]/OneDrive/ZotFile` (MacOS)
    - Files and Folders -> Data Directory Location -> Custom: `E:\Zotero`
    - Files and Folders -> Data Directory Location -> Custom: `/Users/[username]/Zotero` (MacOS 默认)

打开 Tools -> ZotFile Preferences
1. General Settings:
    - Source Folder for Attaching New Files: `E:\Zotero\storage`
    - Source Folder for Attaching New Files: `/Users/[username]/Zotero/storage` (MacOS)
    - Location of Files -> Sustom Location: `E:\OneDrive\ZotFile`
    - Location of Files -> Sustom Location: `/Users/[username]/OneDrive/ZotFile` (MacOS)
    - Location of Files -> Use subfolder defined by `\%y\%T`

2. Renaming Rules:
    - Renaming Format -> Use Zotero to Rename: `False`
    - Renaming Format -> Format for all Item Types: `{%a_}{%j_}{%y_}{%t}`
    - Renaming Format -> Format for Patents: `{%a_}{%y_}{%t}`
    - Additional Setting -> Delimiter: `_`

关于这部分设置可以参考
- https://www.nrel.colostate.edu/set-up-best-reference-manager/
- https://mp.weixin.qq.com/s/0heWcOlwgrF6GHmPTc-poA

### Better bibtex 插件
该插件完善了 Zotero 关于 bib 格式设置上的缺陷，非常好用。

下载：从[该地址](https://retorque.re/zotero-better-bibtex/)下载，安装方式与上述相同

相关配置：打开 Edit -> Zotero Preferences -> Better BibTeX
1. Citation keys
    - Citation key format: `auth.lower+year+shorttitle(1).lower`
    - Keeping citation keys unique -> On conflict ...: `postfixed`
2. Exprot
    - Fields -> When a reference ...: `DOI`
    - Fields -> Fields to omit ...: `abstract,file,note,keywords`

关于这部分可参考上述下载地址。

### delitem 插件
该插件可以删除链接文件：[下载](https://github.com/redleafnew/delitemwithatt)，是 ZotFile 插件的一个功能补充。
该插件不需要额外的设置，直接安装即可使用。
其功能主要体现在：选中某条目右键，显示删除附件等选项。

