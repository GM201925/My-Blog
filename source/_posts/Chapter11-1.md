---
title: Chapter11-1
date: 2026-03-03 21:25:18
categories: Calculus
mathjax: true
---

## 数项级数的基本概念
### 数项级数的概念
{% callout type="info" title="定义" %}
设$u_1,u_2,...,u_n,...$是一个给定的数列，按照数列{$u_n$}下标的大小排列依次相加，得到形式上的和称为**数项级数**，简称**级数**，记为
$$\sum_{n=1}^{\infty}u_n=u_1+u_2...+u_n+...$$
式中每一个数称为级数的项，$u_n$称为级数的通项
{% endcallout %}

$$S_n=u_1+u_2+...+u_n$$称为级数的第n项部分和.
这样我们得到数列{$S_n$}，且
$$\sum_{n=1}^{\infty}u_n=\lim_{n \to \infty}S_n $$
若$\lim_{n \to \infty}S_n=S(常数)$，则称这个级数**收敛**，称S为级数的和

{% folding title="余项" class="blue" open=true %}
$$r_n=S-S_n$$
$$\lim_{n \to \infty}r_n=S-\lim_{n \to \infty}S_n=0$$
{% endfolding %}    


{% tabs %}

<!-- tab 例1 -->

研究几何级数的敛散性$$a+aq+aq^2+...+aq^{n-1}+...=\sum_{n=1}^{\infty}aq^{n-1}$$

<!-- tab 解答-->

$q\ne 1$时，有$S_n=\frac{a-aq^n}{1-q}$
$|q|<1$时，$$\lim_{n \to \infty}S_n=\frac{a}{1-q}$$
$|q|>1$时，$$\lim_{n \to \infty}q^n=\infty,\lim_{n \to \infty}S_n=\infty$$
可以验证$|q|=1$的情形也不收敛
故$|q|<1$时，几何级数收敛
{% endtabs %}

{% tabs %}

<!-- tab 例2 -->

研究p级数的敛散性$$1+\frac{1}{2^p}+...+\frac{1}{n^p}+...=\sum_{n=1}^{\infty}\frac{1}{n^p}$$

<!-- tab 解答-->

当$p=1$时，称为***调和级数***
$n\le x\le n+1$时，
$\frac{1}{n}\ge\frac{1}{x}$且两者在区间[n,n+1]上不会恒等，故
$$\frac{1}{n}=\int_{n}^{n+1}\frac{1}{n}dx>\int_{n}^{n+1}\frac{1}{x}dx $$
故
$$S_n>\int_{1}^{n+1}\frac{1}{x}dx=ln(n+1)\to \infty(n\to \infty)$$
\
当$p<1$时，$0<n^p<n$，故$\frac{1}{n^p}>\frac{1}{n}$
$$S_n>1+\frac{1}{2}+...+\frac{1}{n}+...$$
\
当$p>1$时，$n\le x\le x+1$时
$$\frac{1}{x^p}\ge \frac{1}{(n+1)^p}$$
$$\int_n^{n+1}\frac{1}{x^p}dx>\int_n^{n+1}\frac{1}{(n+1)^p}dx=\frac{1}{(n+1)^p}$$
故$$S_n<1+\int_1^2\frac{1}{x^p}dx+...+\int_{n-1}^n\frac{1}{x^p}dx$$
而$\int_1^{+\infty}\frac{1}{x^p}dx$收敛，设它等于M，那么
$$S_n<1+M$$
递增，有上界，所以当$n\to \infty$时$S_n$极限存在
{% endtabs %}

{% callout type="warning" icon="fa-solid fa-triangle-exclamation" %}
这样写太累了，还麻烦，先停吧……
{% endcallout %}