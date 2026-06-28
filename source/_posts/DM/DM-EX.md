---
title: Exercise
date: 2026-06-22 20:52:09
categories: Discrete Mathematics
mathjax: true
---

{% callout warning::Problem 1 %} 
Find a closed form for the generating function of this sequence.
$$a_n = C(n,4),n=0,1,2,...$$
{% endcallout %}

对于扩展的二项式系数，(或者是一个括号，上面写u下面写k)
$$C(u,k)=\frac{u(u-1)...(u-k+1)}{k!}$$
因此对于C(0,4),C(1,4),...,C(3,4)=0
于是生成函数为$$\sum_{n=4}^\infty C(n,4)x^n$$
即$$\sum_{n=4}^\infty C(n,n-4)x^n$$

进一步地，有$$x^4 \sum_{n=0}^\infty C(n+4,n)x^n$$
即$$x^4 \sum_{n=0}^\infty C(n+5-1,n)x^n$$

于是答案为$$x^4(1-x)^{-5}$$