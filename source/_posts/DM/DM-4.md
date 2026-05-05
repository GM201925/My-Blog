---
title: Chapter 8 - Advanced Counting Techniques
date: 2026-04-26 23:11:31
categories: Discrete Mathematics
mathjax: true
---
{% callout success %} 
定义
{% endcallout %}
{% callout warning %} 
注意
{% endcallout %}
{% callout info %} 
补充、解释
{% endcallout %}
{% callout primary %} 
其他
{% endcallout %}

>本章全程高能（

# 1 Applications of Recurrence Relations

>“这种题找递推关系是最难的一步”

We distinguish them by the initial conditions, the values of a0 , a1 , a2 , ... to uniquely identify a sequence.
例如$C(n+1,k)=C(n,k)+C(n,k-1)$
我们要定义C(n,0)和C(n,1) （和非法组合数为0）

【Example】A  young pair of rabbits is placed on an island. A pair of rabbits does not breed until they are 2 months old. After they are 2 months old, each pair of rabbits produces another pair each month. Find a recurrence relation for the number of pairs of rabbits on the island after n months, assuming that no rabbits ever die. 

$f_0 = 1$
$f_1 = 1$
n个月后，原先有$f_{n-1}$对，新出生的$f_{n-2}$对，因为$f_{n-2}$代表两个月前的兔子对数，这些对兔子现在都能生育了
$$f_{n} = f_{n-1} + f_{n-2}$$

【Example】Let $H_n$ denote the number of moves needed to solve The Tower of Hanoi problem with n disks H1 , H2 , … , Hn , … 

汉诺塔的任务是三个杆，把n个盘从1号运到2号
先把n-1个盘运到3号，再把底下的盘运到2号，再把n-1个盘运回2号
$$H_n = 2H_{n-1} + 1$$
$H_1=1$，两边同时+1即可解得$H_n = 2^n-1$

这个算法看上去很简单易懂，其实没那么显然……

【Example】Find a recurrence relation for the number of bit strings of length n that don’t have two consecutive 0s. 

如果这个长度为n的字符串最后一位是1，那么只有$a_{n-1}$个
如果结尾是10，那么只有$a_{n-2}$个

这已经涵盖了所有情况，因为末两位只能是11，10，01
$$a_n = a_{n-1} + a_{n-2}, n\le 3$$
$a_1 = 2,a_2=3$

# 2 Solving Linear Recurrence Relations 

>这让我想起我高中时的一位数学老师
## 2.1 Linear homogeneous recurrence relation of degree k with constant coefficients 

![](/img/DM/4-1.png)

把$a_i$换成$r^i$
得到characteristic equation
$$r^k-c_1r^{k-1}-...-c_k=0$$
得到k个characteristic root
![](/img/DM/4-2.png)
证明：
首先将这个形式代入递推式，再利用$r_1^2=c_1r_1+c_2$，是符合的
利用初始条件解出α，根据递推的唯一性（归纳法证明），若an是解，可以写成这种形式

![](/img/DM/4-3.png)
![](/img/DM/4-4.png)
![](/img/DM/4-5.png)

## 2.2 Linear Nonhomogeneous Recurrence Relation With Constant Coefficients 
> 怎么想到了线性代数？

![](/img/DM/4-6.png)

特解的形式，注意s=1的情况
![](/img/DM/4-7.png)
![](/img/DM/4-8.png)

# 3 Generating Functions
> 期中考之前，我还挺喜欢级数的，好吧，现在也挺喜欢的

{% callout success::Definition %} 
The **generating function** for the sequence $a_0,a_1,...,a_k,...$ of real numbers is the infinite series
$$G(x) = a_0 + a_1x + .. + a_kx^k + ... = \sum_{k=0}^\infin a_kx^k$$
{% endcallout %}
例如，数列1,1,1,1,1,...的生成函数是
$$G(x)=1+x+x^2+...=\sum_{k=0}^\infin x^k = \frac{1}{1-x} $$
其中$\mid x\mid < 1$

![](img/DM/4-9.png)

{% callout primary::Example %} 
【1】What is the generating function for the sequence 0,1,2,3,4,...

$$G(x) = \sum_{k=0}^\infin kx^k = x\sum_{k=1}^\infin kx^{k-1} = x(\frac{1}{1-x})' = \frac{x}{(1-x)^2}$$

【2】Suppose that the generating function of the sequence: $a_0,a_1,,...,a_n,...$ is  G(x). What is the generating function for the sequence $b_k = \sum_{i=0}^k a_i$

$$c_k = 1$$
$$b_k = \sum_{i=0}^k a_i = \sum_{i=0}^k a_ic_{k-i}$$
$$F(x) = G(x) \frac{1}{1-x}$$

所以上一题也可以这么做：
$a_k = 1$
$b_k = \sum_{i=0}^k a_i$
$$G(x) = xF(x) = x(\frac{1}{1-x})^2$$

【3】What is the generating function for the sequence $a_k = k^2$              
$x+2x^2+3x^3+4x^4+... = \frac{x}{(1-x)^2}$
$$x+4x^2+9x^3+16x^4+... = x(1+2x+3x^2+4x^3+...)$$
$$G(x) = x(\frac{x}{(1-x)^2})' = \frac{x(1+x)}{(1-x)^3}$$

【4】What is the generating function for the sequence $a_k = \sum_{i=0}^k i^2$?

$b_k = 1$
$c_k = k^2$
$$a_k = \sum_{i=0}^k c_ib_{k-i}$$
$$G(x) = \frac{x(1+x)}{(1-x)^4}$$

【5】$f(x) = \frac{1}{1-4x^2}$. Find the coefficient $a_0,a_1,...$ in the expansion $$f(x)=\sum^{\infin}_{k=0} a_kx^k$$

$$f(x) = \frac{1}{2}(\frac{1}{1-2x}+\frac{1}{1+2x})$$
$$a_k = \frac{1}{2}(2^k+(-2)^k)$$
{% endcallout %}

## 3.1 The extended binomial coefficient