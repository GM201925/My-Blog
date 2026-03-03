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
