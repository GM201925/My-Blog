---
title: Chapter 3 - Combinational Logic Design
date: 2026-04-22 17:49:45
mathjax: true
categories: Logic and Computer Design Fundamentals
---
A combinational circuit consists of logic gates whose 
output is a function of only the present input.

Sequential logic is a type of logic circuit whose output 
depends not only on the present value of its input signals but on the sequence of past inputs (state or memory).

# 3 Arithmetic Functions
简单的方法，2N个输入，N+1输出的encoder，比如两位加两位，看作四输入，输出3位
但这意味着输入变量会非常多

我们采用Iterative combinational circuitis，我们的核心思想是设计出一个处理单比特运算的“基本单元 (Cell)”，然后把许多个这样的单元像链条一样串联起来，组成“迭代阵列 (Iterative array)”
- The functional blocks for arithmetic are referred 
to as cells and the overall implementation is an 
array of cells. 
- The iterative array is a method of calculating 
decomposition and takes advantage of the 
regularity to make a large and multiple-level 
arithmetic circuit design feasible. 

## 3.1 Half-Adder
两输入的一位加法器
|X|Y|S|C|
|:-:|:-:|:-:|:-:|
|0|0|0|0|
|0|1|1|0|
|1|0|1|0|
|1|1|0|1|

In essence, a half-adder is a 2-to-2 encoder. 
S(sum bit)是和，C(carry bit)是进位
$$S=X\oplus Y,C=XY$$
当然S和C的表达方式有很多种，上面只是列举了最常见的一种
![](img/LCDF/3-1.png)

## 3.2 Full-Adder
A full adder is similar to a half adder, but includes a carry
in bit from lower stages.  Like the half-adder, it computes a sum bit S, and a carry bit C. 
![](img/LCDF/3-2.png)

