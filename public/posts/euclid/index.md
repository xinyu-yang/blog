# Euclid Algorithm and Extended Euclid Algorithm.

<!--# 欧几里得（Euclid）与拓展的欧几里得算法-->

## 欧几里得算法

### 原理 

欧几里得算法是一种快速计算最大公约数的算法，对于任意的两个数$(a,b)$，其最大公约数表示为$gcd(a,b)$，根据欧几里得算法，$gcd(a,b)=gcd(b,a\mod b)$ 。证明如下：

>如果$b>a$，显然成立；因此只需考虑$b < a$的情况。根据初等数学知识，可知$a,b$的关系可表示为$a=qb+r$，其中$q$为商，$r$为余数。

>对于$(a,b)$的最大公约数$g1=gcd(a,b)$，当然$g1|a,g1|b$（$g1|a$表示$g1$整除$a$），所以易知对于$r=a-qb$，同样满足$g1|r$；

>又因为$a\mod b=r$，所以对于$a,b$的最大公约数$g1$，同样满足$g1|(a\mod b),g1|b$，即$(b,a\mod b)$的最大公约数至少为$g1$，即$gcd(b,a\mod b)>g1=gcd(a,b)$。

>反过来，对于$(b,a\mod b)$的最大公约数$g2=gcd(b,a\mod b)$，同样满足$g2|a, g2|b$，即$gcd(a,b)>g2=gcd(b,a\mod b)$。

>因此$gcd(a,b)=gcd(b,a\mod b)$证明成立。下面对该算法进行实现。

### 实现
```c++
#include <iostream>                                     
using namespace std;          
                              
int euclid(int a, int b)      
{                             
    if (b!=0)                 
    {                         
        return euclid(b, a%b);
    }                         
    else                      
    {                         
        return a;             
    }                         
}                                                         
int main()                    
{                             
    int a(0),b(0);            
    cin >> a >> b;            
    cout << euclid(a,b);      
    return 0;                 
}                             
```
## 拓展的欧几里得算法
### 原理
拓展的欧几里得算法在密码学中有着重要的应用，现给出定理：
> 对正整数a，b；总是存在一组整数X,Y，使得$Xa+Yb=gcd(a,b)$成立，且$gcd(a,b)$为满足这种条件的最小整数。

这里不对该定理进行证明，欧几里得算法给出了在已知$a，b$的情况下求$gcd(a,b)$的方法，但是如果想要求得X，Y的值，就要求助于**拓展的欧几里得算法**。怎么才能从欧几里得算法的计算过程当中得到我们想要求解的值呢？我们再次详细回顾欧几里得算法的求解过程。

对于已知整数$a,b$，我们的算法求解过程如下：

| $a$       | $b$   | 余数$r$                | 商$q$                  |
| --------- | ----- | ---------------------- | ---------------------- |
| a         | b     | $r_1=a\mod b$          | $q_1=a/b$              |
| b         | $r_1$ | $r_2=b\mod r_1$        | $q_2=b/r_1$            |
| $r_1$     | $r_2$ | $r_3=r_1\mod r_2$      | $q_3=r_1/r_2$          |
| ...       | ...   | ...                    | ...                    |
| $r_{n-1}$ | $r_n$ | $r_{n+1}=r_{n-1}\mod r_n$ | $q_{n+1}=r_{n-1}/r_n$ |

逐步计算，直到某一步出现$r_{n-1}\mod r_{n}=0$的情况，这时候就找到了最大公约数，最大公约数即为$r_n$，以上就是欧几里得算法的全过程。通过这个过程当中多产生的一些中间结果我们能不能求得$X,Y$的值呢？下面进行两种求解方法的推导。

### 递归求解

根据上面的表格我知道，$Xa+Yb=gcd(a,b)$，并且对于中间所求解的每一步我们所得到的$r_i$都满足$X_i\cdot r_i+Y_i\cdot r_{i+1}=gcd(a,b)$，因为很明显每一对$r_i,r_{i+1}$都满足最大公约数为$gcd(a,b)$，这也是欧几里得算法的原理。

我们试着寻找$(X_i,Y_i),(X_{i+1},Y_{i+1})$之间的递推关系，由以上阐述可知:

$$
\begin{cases}
X_i\cdot r_i+Y_i\cdot r_{i+1}=gcd(a,b), &\text{(1)} \cr
X_{i+1}\cdot r_{i+1}+Y_{i+1}\cdot r_{i+2}=gcd(a,b),&\text{(2)}
\end{cases}
$$

为了将上式转换成$r_i$的方程组，我们使用$r_{i+1},r_{i+2}来表示r_i$，通过以上可知$r_i=r_i/r_{i+1}\cdot r_{i+1}+r_{i+2}$，将该式带入上式（1），并将两式合并可得：
$$    
X_i\cdot (r_i/r_{i+1}\cdot r_{i+1}+r_{i+2})+Y_i\cdot r_{i+1}=X_{i+1}\cdot r_{i+1}+Y_{i+1}\cdot r_{i+2}
$$
进一步化简可得：
$$    
(X_i\cdot r_i/r_{i+1}+Y_i)\cdot r_{i+1}+X_i\cdot r_{i+2}=X_{i+1}\cdot r_{i+1}+Y_{i+1}\cdot r_{i+2}
$$
根据系数相等的原则可得：
$$
\begin{cases}
X_i\cdot r_i/r_{i+1}+Y_i=X_{i+1}, &\text{(1)}\cr
X_i=Y_{i+1},&\text{(2)}
\end{cases}
$$
以上就得到了$(X_i,Y_i),(X_{i+1},Y_{i+1})$之间的递推关系，那么我们接下来的工作就是找到一对可以求出其值的$(X_i,Y_i)$，通过以上可知当出现某一次计算使得$r_{i}\mod r_{i+1}=0$时，我们可知对于$X_i\cdot r_i+Y_i\cdot r_{i+1}=gcd(a,b)$，满足$gcd(a,b)=r_{i+1}$，那么很显然$X_i=0,Y_i=1$。于是我们就得到了一对$(X_i,Y_i)$的值，我们已经知道了最后一对$r_{i},r_{i+1}$所对应的$(X_i,Y_i)$才能够推知前面的值，所以我们的推导是从后往前推的，因此我们将上面的递推关系稍微变换一下形式：
$$
\begin{cases}
X_i=Y_{i+1},&\text{(1)}\cr
Y_i=X_{i+1}-Y_{i+1}\cdot r_i/r_{i+1}, &\text{(2)}
\end{cases}
$$
此时我们就得到了推导关系和初值，通过计算我们就可以求得满足$Xa+Yb=gcd(a,b)$的$X,Y$值。下面通过代码对其进行实现：
```cpp
#include <iostream>                    
using namespace std;                   
                                       
void extEuc(int a, int b, int&x, int&y)
{                                      
    int rx,ry;                         
    int r(0);                          
    if (a%b==0)                        
    {                                  
        x=0;y=1;                       
        return;                        
    }                                  
    else                               
    {                                  
        r = a%b;                       
        extEuc(b, r, rx, ry);          
        x = ry;                        
        y = rx - ry*a/b;               
        return;                        
    }                                  
}                                      
                                       
int main()                             
{                                      
    int a,b;                           
    int x,y;                           
    cin >> a >> b;                     
    extEuc(a, b, x, y);                
    cout << x <<' '<< y << endl;       
    return 0;                          
}                                      
```
> 输入42 2017可求得输出为-48，1。

### 迭代求解

较多的递归调用可能会影响计算速度，所以我们接下来推一下迭代的计算方式，已知上面表格中所列欧几里得算法的计算步骤。已知$gcd(a,b)$是满足该集合的最小值$\lbrace Xa+Yb | X,Y\in Z \rbrace$，已知对于每一步所产生的余数均能被$gcd(a,b)$整除，现在考虑每一步迭代所产生的余数满足的等式：
$$
\begin{cases}
r_i=X_i\cdot a+Y_i\cdot b \cr
r_{i+1}=X_{i+1}\cdot a + Y_{i+1}\cdot b
\end{cases}
$$
且已知$r_i, r_{i+1}$满足$r_{i-1}=r_{i-1}/r_i\cdot r_i+r_{i+1}$，将上面两式代入到该式，可得：
$$
r_{i-1}=(r_{i-1}/r_i\cdot X_i+X_{i+1})\cdot a+(r_{i-1}/r_i\cdot Y_i +Y_{i+1})\cdot b.
$$
值得注意的是此处的$X_i,Y_i$与递归方法中的值含义不同。根据上式可推知以下递推关系：
$$\begin{cases}
X_{i+1}=X_{i-1}-r_{i-1}/r_i\cdot X_i \cr
Y_{i+1}=Y_{i-1}-r_{i-1}/r_i\cdot Y_i
\end{cases}$$
已知中间的递推关系，关键是考虑如何判断循环的起始值和结束条件，对于$a,b$也可看做是余数$r_i$，那么对于$a,b$来说，其满足的值为：
$$\begin{cases}
a = a\cdot 1 + b\cdot 0\cr
b = a\cdot 0 + b\cdot 1
\end{cases}$$
所以就得到了两对$(X,Y)$的值，分别为$(1,0),(0,1)$，并且已知$r_i$之间的递推关系为$r_{i+1}=r_{i-1}\mod r_i$。我们也知道循环结束的条件为$r_i=gcd(a,b)$，其最后的形式为$Xa+Yb=gcd(a,b)$，其直接判断方式为$r_{i-1}\mod r_i=0$，然后我们就得到了最终的$X,Y$值，根据以上递推形式，我们有以下实现：
```cpp
#include <iostream>             
using namespace std;            
                                
int main()                      
{                               
    int a,b;                    
    int x1(1),y1(0),x2(0),y2(1);
    int temp;                   
    cin >> a >> b;              
    while (a%b!=0)              
    {                           
        temp = x2;              
        x2 = x1 - a/b*x2;       
        x1 = temp;              
        temp = y2;              
        y2 = y1 - a/b*y2;       
        y1 = temp;              
        temp = a%b;             
        a = b;                  
        b = temp;               
    }                           
    cout << x2 <<' '<<y2<<endl; 
    return 0;                   
}                               
```
> 输入42 2017可求得输出为-48，1。

[这里](http://rosettacode.org/wiki/Modular_inverse#Python)有一个用尽可能多的程序语言实现求逆元的网站，大家也可以参考这里的不同实现。

*参考文献*
> [1] Katz J，Lindel Y．Introduction to Modern Cryptography—Principle and Protocol现代密码学——原理与协议【M】任伟．北京：国防工业出版社．2010：10-15．

