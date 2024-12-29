---
title: "Count-Min Sketch"
date: 2021-04-06T20:14:43+08:00
lastmod: 2021-04-06T20:14:43+08:00
draft: false
description: ""
tags: ["hash"]
categories: ["algorithm"]
author: ""
---

<!--more-->
## 摘要
本文主要介绍 Count-Min Sketch，这是一种用来统计元素出现频率的数据结构，其基于 hash 函数进行构建，常用来解决 "Heavy-Hitter" 问题，即统计一个集合中经常出现的元素。本文除对算法原理进行介绍，更重要的是要对其进行数学分析，从而更好的理解该算法。

## 算法介绍
首先介绍算法的过程，初始化 $t$ 个独立的hash函数$h_1, h_2, ...,h_t$，每一个 hash 函数都是从集合$[n] \to [k]$的映射。然后初始化一个二维矩阵，如下图所示：

<center>
<a><img src="https://raw.githubusercontent.com/xinyu-yang/imgs/refs/heads/master/2024/3zKatdnRrbDgxeO.png" alt="image" align="middle" border="0"></a>
</center>

我们假设$t=log(1/\delta), k=2/\epsilon$，至于$\delta,\epsilon$是什么，为什么那么赋值后面会解释，先说一下结论：**当$t,k$选择上述值时，假阳性的值大于$m\cdot \epsilon$的概率小于$\delta$。**

对于具有m个元素的集合A，对其进行如下操作:

```
Init all C(i,j)=0
Init h[1...t]
for i=0 to m do
    for j=0 to t do
        C(j, h[j](Ai)) += 1
```

当查找一个元素出现的次数时，只需要计算
$$\check{f_{q}}=min C_{j,h_{j}(q)},j\in [t]$$

也就是说找到元素$q$映射到的位置当中的最小值，很明显通过此方法得到的值$\check{f_q}$值肯定是大于等于实际值$f_q$的，因为$q$对应的值有可能与其他的值碰撞，从而产生**假阳性**，但是肯定不会产生假阴性。
那么到底会产生多少假阳性呢？

## 分析
我们希望最后的结果是能够以一定的概率保证$\check{f_q} < f_q + w$，那么下面需要做的就是怎么确定这个概率和$w$之间的关系呢？其实我们在上面已经假设$w=m\cdot \epsilon$，那么我们就将去求其概率。

首先我们假设一个符号 $r_{i,j}$，表示由于 $i$ 在 hash 函数 $h_j$ 上导致的 $q$ 的数量的增加，那么显然：
$$
r_{i,j}=
\begin{cases}
    1, & \text{when collision happen.} \cr
    0, & \text{otherwise.}
\end{cases}
$$
也就是当碰撞发生时，其值为 1，其他为 0。如果我们想求出 $r_{i,j}$ 的期望，就应该先求其概率。回顾该值的定义，该值定义为另外一个值 $i$ 与 $q$ 的碰撞所导致的值的增加，那么碰撞的概率是多少呢？如果所使用的 hash 函数理想的话，任意一个值与$q$的 hash 值相同的概率都为$1/k$。也就是说：
$$
E(r_{i,j})=1/k
$$
那么容易求得：
$$
E(r_{.,j})=\sum_{i\neq q}^m E(r_{i,j})=\sum_{i\neq q}^m 1/k \approx m/k
$$

也就是说由在 $h_j$ 处所产生的碰撞所导致的 $C_{j,h_j(q)}$ 的值的增加 $r_{.,j}$ 的数学期望为$m/k$。那么根据[马尔可夫不等式](../21-03-21_markov-and-chebyshev)可以得出：
$$
P(r_{.,j} > w) < E(r_{.,j})/w = m/kw
$$

回顾我们在一开始时设的$k=2/\epsilon$，并将$w=m\cdot \epsilon$代入，可得
$$
P(r_{.,j} > w) < 1/2
$$
我们以上只是分析了由 $h_j$ 碰撞所导致的假阳性，根据算法可知，最后取得 $f_q$ 值为所计算的 hash 函数的最小值，也就是说 $f_q$ 假阳性的值取决于由所有 hash 函数导致的假阳性值的最小值。如果 $f_q$ 的假阳性大于 $w$，那么：
$$
P(\bar{f_q} - f_q > w) = P(\underset{j\in t}{min}\ r_{.,j} > w) < (1/2)^t
$$
又由上文可知，我们已经假设$t=log(1/\delta)$，所以最后的结果为：
$$
P(\bar{f_q} - f_q > w) < \delta
$$
这也就解释了我们一开始对于$t,k$的选值原因。
再次回顾 $w,\delta$ 的取值：
$$
w = m\cdot \epsilon = m \cdot 2/k
$$
$$
\delta = 1/2^{t}
$$
通过此式可以看出，假阳性值的大小跟数据量 m 和哈希表中每行元素的个数 k 有关，准确的说是跟它们的比值成正比。
而可保证的概率大小 $\delta$ 跟哈希函数的个数 t 有关。

如果想要减少假阳性，可以采取增加 k 和 t 的值来实现（主要是 k）。
当然，随之计算量和空间消耗也会增加。

最后的空间复杂度为：
$$
t\cdot \log n + tk\cdot \log n = (\frac{2}{\epsilon} \log m + \log n)\log \frac{1}{\delta}
$$

## 代码实现
TODO:smile:

## 参考文献
https://www.cs.utah.edu/~jeffp/teaching/cs5140-S16/cs5140/L12-Count-Min+Apriori.pdf
https://courses.cs.duke.edu//fall18/compsci531/LabLectures/lab5.pdf
