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
$$G(x) = a_0 + a_1x + .. + a_kx^k + ... = \sum_{k=0}^\infty a_kx^k$$
{% endcallout %}
例如，数列1,1,1,1,1,...的生成函数是
$$G(x)=1+x+x^2+...=\sum_{k=0}^\infty x^k = \frac{1}{1-x} $$
其中$\mid x\mid < 1$

![](img/DM/4-9.png)

{% callout primary::Example %} 
【1】What is the generating function for the sequence 0,1,2,3,4,...

$$G(x) = \sum_{k=0}^\infty kx^k = x\sum_{k=1}^\infty kx^{k-1} = x(\frac{1}{1-x})' = \frac{x}{(1-x)^2}$$

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
$$x+4x^2+9x^3+16x^4+... = x(1+4x+9x^2+16x^3+...)$$
$$G(x) = x(\frac{x}{(1-x)^2})' = \frac{x(1+x)}{(1-x)^3}$$

【4】What is the generating function for the sequence $a_k = \sum_{i=0}^k i^2$?

$b_k = 1$
$c_k = k^2$
$$a_k = \sum_{i=0}^k c_ib_{k-i}$$
$$G(x) = \frac{x(1+x)}{(1-x)^4}$$

【5】$f(x) = \frac{1}{1-4x^2}$. Find the coefficient $a_0,a_1,...$ in the expansion $$f(x)=\sum^{\infty}_{k=0} a_kx^k$$

$$f(x) = \frac{1}{2}(\frac{1}{1-2x}+\frac{1}{1+2x})$$
$$a_k = \frac{1}{2}(2^k+(-2)^k)$$
{% endcallout %}

## 3.1 The extended binomial coefficient
![](img/DM/4-10.png)
![](img/DM/4-11.png)
请利用上面的定理证明下图中的7,8
![](img/DM/4-12.png)
![](img/DM/4-13.png)

# 4 Counting with Generating Functions
{% callout primary::Example %} 
【1】从n个元素里选r个，允许重复，有多少种可能
根据之前学的知识，这属于可重复的组合问题，比如A元素可以被选1次，或2次...
看成n个盒子，一共放r个球，如果A盒子放了3个球，意味着被选3次
n-1个bar，r个star
故答案为$C(n-1+r, r)$

我们使用生成函数来做这道题，一个元素可以被选0次，1次……
对应着x的0次方，1次方……
$$G(x) = (1+x+x^2+...)^n = (\frac{1}{1-x})^n$$
n个元素，每个元素有它被选的次数（每个括号贡献的指数），一共选r个，每一种选择乘起来就是$x^r$，所以最后结果中$x^r$的系数就是答案
根据之前的常见数列的生成函数，答案是$C(n-1+r, r)$

不过，我对这个方法的可靠性有一定的怀疑，但说不出来，目前先接受，如果你看到这句话的话，说明我还没说服自己（

【2】请找出$e_1+e_2+e_3=17$的所有非负整数解的个数，其中$2\le e_1\le 5, 3\le e_2\le 6, 4\le e_3\le 7$
这是带限制的相同球放不同盒子问题，一般方法还是比较难做的，但用生成函数可以很好地解决

$$G(x) = (x^2+x^3+x^4+x^5)(x^3+x^4+x^5+x^6)(x^4+x^5+x^6+x^7)$$

这三个括号分别代表$e_1,e_2,e_3$
找出这个式子中$x^{17}$的系数就是答案
这个方法还是没有疑问的，与原问题直接对应
找系数就只能慢慢找了，所以其实这道题直接枚举应该也可以（

【3】Suppose that there are 2r red balls, 2r blue balls, and 2r white balls. How many ways to select 3r balls from these balls? 

![](img/DM/4-14.png)

注意这里找$x^{3r}$系数的方法，其实如果是无穷项的话大概也是这个方法，只不过减$x^{2r+1}$会变成减去x的无穷大的次方，即之前【1】中应为$(\frac{1-x^\infty}{1-x})^n$，所以分子只用1就行，x的无穷大次方哪怕一次方也不用考虑了，这也许可以作为一个解释，当然还是要依赖于$(1-x)^{-n}$可以展开成无穷级数

【4】Determine the number of ways to insert tokens worth \\$1,\\$2 and \\$5 into a vending machine to pay for an item that costs r dollars in both the cases when the order in which the tokens are inserted does not matter and when the order does matter. 
先考虑次序不重要的情况，同样地，这道题也是1元、2元、5元可以被选多次，只不过要注意它们的数值
生成函数为$$G(x) = (1+x+x^2+...)(1+x^2+x^4+x^6+...)(1+x^5+x^{10}+x^{15}+...)$$找到$x^r$的系数即可

如果不使用生成函数，那么我们就按5元的个数分类讨论即可，例如r=10的情况，5元可以选0、1、2个，比如5元为1的情况，剩下5元又2元1元组成，2元可以选0、1、2个，就有三种。

如果考虑次序，那么每次我们可以选择1、2、5元，如果一共选了$n$个硬币，那么就是$(x+x^2+x^5)^n$
其中$x^r$的系数就是符合的情况，但是，我们又不知道一共会选多少个硬币，所以应该是下面这个式子中$x^r$的系数
$$G(x)=1+(x+x^2+x^5)+(x+x^2+x^5)^2+(x+x^2+x^5)^3+...=\frac{1}{1-x-x^2-x^5}$$

如果不用生成函数，这也是个不太简单的问题
假设有$f_r$种方法
如果最后选的是1元，那么前面有$f_{r-1}$种
2元，前面有$f_{r-2}$种
5元，前面有$f_{r-5}$种，得到递推式$$f_r=f_{r-1}+f_{r-2}+f_{r-5}$$
如果我们把$G(x)$看作$\sum f_kx^k$，$f_k$恰好代表r=k时的答案，右边分母乘过来
比较$x^n$系数，也可以得到相同的递推式
{% endcallout %}

{% folding title="Extra" class="green" open=false %}
[GPT对于生成函数计数的解释，也许可以参考](https://chatgpt.com/s/t_6a1bea6ed6748191bc0598208e03a629)
{% endfolding %}

# 5 Using Generating Functions to Solve Recurrence Relations
![](img/DM/4-15.png)

最后是靠$x^n$的系数对应$a_n$

# 6 Proving Identities via Generating Functions
例如证明$C(n,r) = C(n-1,r)+C(n-1,r-1)$
![](img/DM/4-16.png)

# 7 Inclusion-Exclusion
![](img/DM/4-17.png)

{% callout primary::Example %} 
【1】不超过1000的正整数中，不被5、6、8整除的数有多少个
总数减去被5或6或8整除的数即可
被5或6或8整除的数等于
200+166+125-33-25-41+8=400
故答案为600

【2】26个字母的排列，不包含字符串fish、rat、bird的有多少
一共有26!种
包含fish的：把fish看作整体，然后全排列，有23!个
既包含fish又包含rat的，分别看作整体，21!个
而fish和bird不能同时出现，rat和bird不会同时出现（都有r），这三个也不会同时出现
所以答案为26!-(23!+24!+23!-21!-0-0+0)
{% endcallout %}