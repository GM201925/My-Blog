---
title: Chapter 4 - Number Theory and Cryptography
date: 2026-06-27 17:54:39
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

如果a和b是整数，存在整数c，b=ac，那么a divides b，记作$a\mid b$

如果a是整数，d是正整数，存在唯一的整数q，r，r大于等于0小于d，使得$a=dq+r$
d is the divisor
a is the dividend
q is the quotient
r is the remainder (has to be positive)

例如-11除以3，-11=3x(-4)+1, quotient是-4，r是1

$$q= a \ div \ d, r = a \ mod \ d$$

If a and b are integers and m is a positive integer, then a is congruent to b modulo m if m divides a-b.
记作$a\equiv b \ (mod \ m)$

这等价于a和b除以m的余数相同
m is the modulus of the congruence $a\equiv b \ (mod \ m)$

![](img/DM/9-1.png)
a+b 除以m的余数等于(a除以m的余数+b除以m的余数)除以m的余数
ab除以m的余数等于(a除以m的余数 乘 b除以m的余数)除以m的余数

![](img/DM/9-2.png)
有结合、交换、分配律，而且运算封闭，始终在$Z_m$

composite 合数
算术基本定理，任何不为1的正整数可以写成若干素数的乘积，且不考虑顺序下这一形式唯一
例如2=2
$100=2^2 5^2$

如果n是合数，那么n一定有个小于等于根号n的因数
又这个因数大于1，所以可以拆成素数乘积，于是
如果n是合数，那么n一定有个小于等于根号n的素因数

可以用逆否命题证明一个数是素数，例如113，小于等于根号113的素数为2，3，5，7，它们都不是113的因数

分解质因数
例如分解7007，先从2开始一个素数一个素数试，是否整除7007
例如找到7，现在开始对1001重复这一过程
又是7，对143重复
找到11，对13重复
虽然13是素数很显然，但一般过程中我们应该试了2，5都不行，而5已经大于根号13了，说明13是素数
于是$7007=7^2 x 11 x 13$

素数有无穷多个

Mersenne素数：$2^p-1$形式的素数，其中p也是素数

**The Prime Number Theorem**: 不超过 x 的素数的个数”与 “x / ln x” 的比值，当 x 无限增长时，这个比值趋近于 1

互素：relatively prime 如果a和b的gcd是1
The integers a1,a2,…an are pairwise relatively prime if gcd(ai, aj)=1 whenever 1≦i<j ≦n.

lcm least common multiple

【Theorem】a和b为正整数，那么$ab=gcd(a,b)\times lcm(a,b)$
用算术基本定理分解a和b，对于所有分解出来的素数，gcd就是分别在a，b中取较小的次方，lcm是取a，b中较大的次方
例如$12=2^2\times 3$
$10=2^1 \times 5$
那么gcd就是$2^1\times 3^0\times 5^0$
lcm就是$2^2\times 3^1\times 5^1$

Euclidean 算法
【Lemma】a=bq+r，都是整数，那么gcd(a,b)=gcd(b,r)
证明：假设m是a和b的公因数，那么r也是m的倍数，故m也是b和r的公因数

假设w是b和r的公因数，根据等式，w也是a的因数
因此a和b的公因数和b和r的公因数完全一样
所以求a和b的最大公约数，先a除以b，然后把除数变成被除数，余数变成除数，继续下去直到r=0，这时除数b和0的最大公因数就是除数

【Bézout’s Theorem】Let a and b be positive integers, then
 there exist integers $s$ and $t$ such that  $gcd(a,b) = sa + tb$, also
known as Bézout’s identity

s and t : Bézout coefficients of a and b.

For example,  gcd(6,14) = (−2)∙6 + 1∙14
![](img/DM/9-4.png)

Let m be a positive integer and let a, b, and c be integers.
 If ac ≡ bc (mod m) and gcd(c,m) = 1, then a ≡ b (mod m).
也就是当c和m互质时，可以同时去掉c

A congruence of the form  ax ≡ b( mod m),
where m is a positive integer, a and b are integers, and x is a variable, is called a linear congruence.

满足linear congruence的所有x就是linear congruence的解

An integer ā such that āa ≡ 1( mod m) is said to be an inverse of a modulo m.
例如5 is an inverse of 3 modulo 7 since 5*3=15 $\equiv$ 1 (mod 7)

If a and m are relatively prime integers and m > 1, 
then an inverse of a modulo m exists. Furthermore, this inverse is unique 
modulo m. (This means that there is a unique positive integer ā less than 
m that is an inverse of a modulo m and every other inverse of a modulo m
 is congruent to ā modulo m.) 

![](img/DM/9-5.png)

求解$3x\equiv 4(mod\ 7)$
先得到3模7的逆元，先求最大公约数，7=2*3+1
3=3*1+0
最大公约数为1
那么1=7-2*3
两边同时模7
-2*3和1在模7时同余
所以逆元是-2
由于-2和-2同余
根据定理5，-6x和-8同余
又-6和1同余
所以-6x和x同余
所以x和-8同余
所以x和6同余

这只是必要条件，验证一下，如果x和6模7同余，那么3x和3*6=18同余，又18和4同余，验证完毕
![](img/DM/9-6.png)
![](img/DM/9-7.png)
![](img/DM/9-8.png)
![](img/DM/9-9.png)
![](img/DM/9-10.png)
![](img/DM/9-11.png)
![](img/DM/9-12.png)


