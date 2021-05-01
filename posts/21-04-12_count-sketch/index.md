# Count Sketch


<!--more-->

## 算法介绍
按照出现时间，该算法比[count-min sketch](../21-04-06_count-min-sketch/)出现的时间更早，但似乎没有count-min sketch应用广泛。该算法的大体思想与count-min sketch相似，只是有少许不同。
首先介绍算法的过程，初始化$t$个独立的hash函数$h_1, h_2, ...,h_t$，每一个hash函数都是从集合$[n] \to [k]$的映射。同时还要初始化$t$个符号函数$s_i : [n] \to [\pm 1]$，然后初始化一个二维矩阵，如下图所示：

<center>
<a href="https://imgbb.com/"><img src="https://i.ibb.co/Sm77Xd9/image.png" alt="image" align="middle" border="0"></a>
</center>

对于具有m个元素的集合A，对其进行如下操作:

----------------
* Set all $C_{i,j}=0$
* for i=0 to m do
    * for j=0 to t do
       * $C_{j,h_{j}(a_i)}$ += $s_j (a_i)$
----------------

该操作与count-min sketch唯一不同的点在于，最后加的值有可能是1或者-1。

当查找一个元素出现的次数时，只需要计算
$$\check{f_{q}}=median\ C_{j,h_{j}(q)},\ j\in [t]$$

这里与count-min sketch不同的是，本方法所产生的值有可能是正也有可能是负，有可能假阳也有可能假阴，而count-min sketch中只有正，所以一定会产生假阳性。仅从直觉上来判断，似乎选中间值是合理的，因为正负相互抵消，比较理想的情况就是恰好抵消完，那就没有误差了。

## 分析
参考了几篇文章，甚至参考了作者的[原文](https://www.cs.rutgers.edu/~farach/pubs/FrequentStream.pdf)，其证明方法都不太一样，尴尬的是我一个都没看懂:joy:，哎~等能看懂再补上吧。。。

> 路漫漫其修远兮，吾将上下而求索啊


## 代码实现
参考文献 https://wangshusen.github.io/code/countsketch.html 中就有一个代码实现，而且有动态图展示。


## 参考文献
1. https://web.stanford.edu/class/cs369g/files/lectures/lec8.pdf
2. https://www.cs.rutgers.edu/~farach/pubs/FrequentStream.pdf
3. https://wangshusen.github.io/code/countsketch.html

