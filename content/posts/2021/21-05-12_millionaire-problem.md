---
title: "百万富翁问题"
subtitle: ""
date: 2021-05-12T23:50:52+08:00
lastmod: 2021-05-12T23:50:52+08:00
draft: false
author: ""
authorLink: ""
description: ""

tags: ["ZK"]
categories: ["math"]

hiddenFromHomePage: false
hiddenFromSearch: false

featuredImage: ""
featuredImagePreview: ""

toc:
  enable: true
math:
  enable: true
comment:
  enable: true

lightgallery: false
license: ""
---

<!--more-->

## 问题概述
该问题由第一位也是唯一一位华人图灵奖得主姚期智先生于1982年提出，问题很简单：
> 在缺少第三方公信平台的情况下，有两个百万富翁希望比较财富的高低，但是又不想让对方知道自己的具体的财富有多少。

该问题是另外一个通用问题的特殊形式，即：
> $a_1, a_2 ... a_n$分别拥有值$x_1, x_2 ... x_n$，如何在互相不泄露这些值的情况下计算$F(x_1, x_2 ... x_n)$

姚期智的[这篇论文](https://research.cs.wisc.edu/areas/sec/yao1982-ocr.pdf)就是讨论了以上问题，对于百万富翁问题，记得有人提出可以采用称重的方法，即将富翁所有的财富转换成金子，并放置在不透明的盒子里，然后用一个天平来测量哪一个盒子更重。这是一个不错的方案，但是还是隐含借助了第三方的可信机构，抛开天平的可信性不谈，给富翁兑换金子的人（即使是多个）总是知道富翁财产的。

## 解决方案
那能不能借助科学的力量解决该问题呢？姚期智在他的论文里给出了肯定答案，论文中提到其一共提出了三种解决方案，但是限于篇幅只介绍了其中一种，本文也主要介绍该方法，该方法非常巧妙，让人拍案称绝，也让我感受到了图灵奖实力的冰山一角。

该方法主要借助公钥加密和单向函数来实现双方对对方消息的不可知。
具体内容如下：

1. 假设双方为A和B，财产分别为a和b，财产范围为[0, N]。首先由A生成一对公私钥对$(PK,P)$（不失一般性），然后将公$PK$发送给B。B选择一个随机数r，然后计算$n=Enc(PK,r)-b$，并将$n$发送给A。

2. A收到之后便使用$n+i,\ i\in [0,N]$构造一个数组，并使用私钥进行加密，最终的结果为：$(Enc(P,n),Enc(P,n+1)...Enc(P,n+N))$，其中有一个$i$的值为A的财产a，即有一个$n+i$的值为$Enc(PK,r)$，但是和明显A并不知道是哪一个。

3. 接下来A进行进一步操作，其将数组中序号大于其财产的值加一，即最终值为$Enc(P,n+i)+1,\text{if i > a}$。然后选取一个位于财产范围内的素数$p$，将数组中的所有数值对$p$取余。最终得到：
$$
(Enc(P,n)\mod p, Enc(P,n+1)\mod p, ...
$$
$$
(Enc(P,n+a)+1)\mod p, ... (Enc(P,n+N)+1)\mod p)
$$
A将该数组和$p$发送给B。

4. B收到该数组之后，使用PK对公式中的每一项进行解密，如果第b项的值为$r\mod p$，那么说明A的财富值大于B的财富值，否则，A的财富值小于等于B的财富值。因为，如果A的财富值大于B的财富值，则数组中的第b项为$Enc(P,n+b)\mod p = Enc(P, Enc(PK, r))\mod p$，否则该值将为$Enc(P,n+b)+1\mod p$。

通过以上方式可以实现在不互相泄露信息的情况下比较财富的数值，其中加一和取余是核心操作，尤其是取余，因为其相当于一个单向函数，所以可以向B隐藏A的信息，如果没有该步骤，那么A的信息将很容易被泄露。



## 参考文献
[1] [A. C. Yao, “Protocols for Secure Computations,” ](https://research.cs.wisc.edu/areas/sec/yao1982-ocr.pdf)

[2] https://zhiqiang.org/cs/yao-millionaires-problem.html
