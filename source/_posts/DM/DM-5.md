---
title: Chapter 3 - Algorithms
date: 2026-05-04 09:18:43
categories: Discrete Mathematics
mathjax: true
---

# 1 Algorithms
An algorithm is a finite set of precise instructions for performing a computation or for solving a problem. 

Example: Can we develop a procedure that takes as input a computer program along with its input and determines whether the program will eventually halt with that input.
Does there exist a procedure H(P,I), which takes as input a program P and the input I to P. 
H outputs “halt” if it is the case that P will stop when run with input I. 
Otherwise, H outputs “loops forever.”

即，是否有一个通用的判定程序来判断一个程序是否会停下来？
假设存在这么一个判定程序H
我们构造这样一个程序D
```
def D(P):
    # 调用H，判断"程序P在输入为P自身时是否会停机"
    if H(P, P) == "halt":
        # 如果H说P(P)会停机，那么D就故意进入无限循环
        while True:
            pass  # 永远执行空操作，不会停机
    else:
        # 如果H说P(P)永远循环，那么D就立刻停机
        return  # 程序结束，停机
```
运行D(D)，如果H(D,D)说halt，那么D(D)应该会停下来，但这时D(D)将一直循环下去。
假设 H (D,D) 输出 "loops forever"
这意味着 H 判定：程序 D 在输入为 D 时永远不会停机。但根据 D 的定义，当 H (P,P) 输出 "loops forever" 时，D 会立刻停机。因此：D(D) 会停机
无论 H 给出什么答案，都会和 D (D) 的实际行为矛盾。这说明我们最初的假设 ——“存在一个完美的通用停机判定程序 H”—— 是错误的。

# 2 The Growth of Functions
基础知识点见数据结构的笔记
证明$log(n!) = \Theta(nlogn)$
上界证明是容易的，左边拆成加法就行了
我们将求和式拆分为**前半部分**和**后半部分**，只对后半部分进行下界估计：
$$
\sum_{k=1}^n \log k \geq \sum_{k=\lceil n/2 \rceil}^n \log k
$$
对于后半部分的每一项 $k \geq \lceil n/2 \rceil$，有：
$$
\log k \geq \log\left(\frac{n}{2}\right) = \log n - \log 2
$$
后半部分共有 $\lfloor n/2 \rfloor + 1 \geq \frac{n}{2}$ 项，因此：
$$
\sum_{k=\lceil n/2 \rceil}^n \log k \geq \frac{n}{2} \cdot (\log n - 1) = \frac{1}{2}n\log n - \frac{n}{2}
$$
当 $n \geq 4$ 时，$\frac{n}{2} \leq \frac{1}{4}n\log n$，代入得：
$$
\log(n!) \geq \frac{1}{2}n\log n - \frac{1}{4}n\log n = \frac{1}{4}n\log n
$$
取 $c=\frac{1}{4}$，$n_0=4$，满足 $\Omega(n\log n)$ 的定义。

Put the functions below in order so that each function is big-O of the next function on the list.
$f1(n) = (1.5)^n$ 
$f2(n) = 8n^3+17n^2 +11 $
$f3(n) = (log n )^2$          
$f4(n) = 2^n$ 
$f5(n) = log (log n)$        
$f6(n) = n^2 (log n)^3$
$f7(n) = 2^n (n^2  +1)$       
$f8(n) = n^3+ n(log n)^2$ 
$f9(n) = 10000$             
$f10(n) = n!$
注意顺序，比如我这么列 logn，2n,logn=O(2n)就是对的
**A is big-O of B means A has lower complexity than B**
**f(x) is O(g(x)) is read as "f(x) is big-O of g(x)"**
**9 5 3 6 2=8 1 4 7 10** 

# 3 Complexity of Algorithms
There are two important aspects of the computational complexity of an algorithm:
**Space Complexity**: gives the approximate amount of memory required to solve a problem of size n.
  - Often tied in with the particular data structures used to implement the algorithm

**Time Complexity**: gives the approximate number of operations required to solve a problem of size n.
