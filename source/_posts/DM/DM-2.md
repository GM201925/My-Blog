---
title: Chapter 2 - Basic Structures
date: 2026-04-02 21:54:55
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
# 1 Sets
## 1.1 Definition
{% callout success::Definition %}
A ***set*** is an unordered collection of objects.

The objects in a set are called the elements of the set.

在这门课中，集合的元素可以重复

Subset
$A\subseteq B$ (A is a subset of B.)
$$A\subseteq B\leftrightarrow \forall x(x\in A\rightarrow x\in B)$$ 
is a tautology.

Equal
$$A=B \Leftrightarrow A\subseteq B \land B\subseteq A$$
这很重要，再证明集合相等时，请考虑它

Proper subset
$A\subset B$ 真子集
$$\forall x(x\in A\rightarrow x\in B)\land \exists x(x\in B\land x\notin A)$$

Cardinality
Let S be a set. If there are exactly n distinct elements in S where n in a nonnegative integer, we say that S is a ***finite*** set and that n is the ***cardinality*** of S.
Notation: |S|

A set is said to be infinite if it isn't finite.
{% endcallout %}

## 1.2 The Power Set
对任何一个有$n$个元素的集合A，它有$2^n$个子集

{% callout success::Definition %}
Given a set S, the power set of S is the set of all subsets of the set S.
Notation: P(S)
$$P(S) = \\{x|x\subseteq S\\}$$
{% endcallout %}

{% callout primary::Example %} 
What is the power set of {$\emptyset$} and $\\{\emptyset ,\\{\emptyset \\} \\}$

1. $S=\\{ \emptyset \\}$
   $P(S) = \\{\emptyset ,\\{\emptyset \\} \\}$
2. $S = \\{\emptyset ,\\{\emptyset \\} \\}$
   我们设S两个元素为A,B.有四个：空集，只有A元素的集合，只有B元素的集合，S自己

证明：如果$P(A)\in P(B)$, 那么$A\in B$
$$P(A)\subseteq B$$
那么P(A)里的元素也都属于B
$$A\in P(A)$$
$$A\in B$$

{% endcallout %}

## 1.3 Cartesian Products
**ordered pair**:(a,b)=(c,d)当且仅当a=c,b=d

**The Cartesian product of A and B**
$$A\times B = \\{ (a,b)|a\in A\land a \in B \\}$$

## 1.4 Truth Sets
给定一个谓词P，一个domain D
P的truth set就是在D中的满足P(x)的x的集合
例如 P(x): |x| = 1,domain是整数
P的truth set是$\\{ x\in Z\mid |x|=1 \\} = \\{1,-1 \\}$

# 2 Set Operations
1. Union
   $A\cup B = \\{ x\mid x\in A\lor x \in B \\}$
2. Intersection
   交集，A和B交集为空时称它们是disjoint的
3. Difference of A and B
   $A - B = \\{ x \mid x\in A\land x \notin B \\} = A\cap \overline{B}$
4. The complement of a set
   $\overline{A} = \\{ x\mid x\notin A\land x\in U \\}$
5. Symmetric difference
   $A\oplus B = \\{ (A\cup B) - (A\cap B) \\}$

{% callout primary::Example %} 
证明$\overline{(A\cup B)} = \overline{A}\cap \overline{B}$

Key 1:
Suppose that $x\in \overline{(A\cup B)}$
It follows that $x\notin A\cup B$
so $x\notin A$ and $x\notin B$
That is, $\neg (x\in A\lor x\in B)$ is true
Hence, $x\in \overline{A}$ and $x\in \overline{B}$ (De Morgan's law)
Thus $x\in \overline{A}\cap \overline{B}$

Suppose that $x\in \overline{A}\cap \overline{B}$
Hence, $x\in \overline{A}$ and $x\in \overline{B}$
so $x\notin A$ and $x\notin B$
By De Morgan's law,
 $\neg (x\in A\lor x\in B)$ is true
so $x\notin A\cup B$
so $x\in \overline{(A\cup B)}$

Key 2:
$\overline{(A\cup B)} = \\{ x\mid x\notin A\cup B \\}$
后面一系列逻辑等价最后变成$\\{ x\mid x\in \overline{A}\cup \overline{B} \\}$
{% endcallout %}

## 2.1 Set Identities
集合的运算都能转成逻辑运算，所以有相似之处
Distributive laws:
$A\cup (B\cap C) = (A\cup B)\cap (A\cup C)$
$A\cap (B\cup C) = (A\cap B)\cup (A\cap C)$

De Mogran's laws:
$\overline{(A\cap B)} = \overline{A}\cup \overline{B}$
另一条在上面的例子中

Absorption laws:
$A\cup (A\cap B) = A$
$A\cap (A\cup B) = A$

{% callout primary::Example %} 
例1：化简
$$((A\cup B\cup C)\cap (A\cup B)) - ((A\cup (B-C))\cap A)$$
利用吸收律
$=(A\cup B) - A$
$=(A\cup B)\cap \overline{A}$
$=B\cap \overline{A}$
$=B-A$

例2：证明：如果$(A-B)\cup (B-A)=A\cup B$,那么$A\cap B = \emptyset$
Key 1:
化简得到$(A\cup B)\cap (\overline{A}\cup \overline{B}) = (A\cup B)$
那么$(A\cup B) - (A\cap B) = (A\cup B)$
于是$(A\cup B) \cap (A\cap B) = \emptyset$
而$(A\cup B) \cap (A\cap B) = (A\cap B)$
故$(A\cap B) = \emptyset$

Key 2:
反证法
Suppose that $A\cap B\ne \emptyset$ and $x\in A\cap B$
Then x is not in $(A-B)\cup (B-A)$
But x is in $A\cup B$
so $(A-B)\cup (B-A) \ne A\cup B$
{% endcallout %}

## 2.2 Computer Representation of Sets
对于全集U的所有元素，按顺序排好，如果在集合A里就是1，不在就是0
例如A={0,1,2},U={0,1,2,3,4,5},B={3,5}
A就可以表示成111000,B可表示成000101
并就是bitwise OR，交就是bitwise AND
如A∩B是{0,1,2,3,5}即111101
就是111000 或 000101

# 3 Functions
{% callout success::Definition %} 
Let A and B be nonempty sets.
A ***function*** $f$ from A to B:
$$f: A\rightarrow B$$
$$\forall a(a\in A\rightarrow \exists !b(b\in B\land f(a)=b))$$
$b$ is called the image of $a$ under f
$a$ is called the preimage of $b$ under f
A: domain of f
B: codomain of f
f(A): the range of f is the set of all images of elements in A under f.

Let f be a function from A to B and let S be a subset of A. The image of S is the subset of B that consists of the images of the elements of S.
$$f(S)=\\{ f(s)\mid s\in S \\}$$

例如
$f(A\cup B) = f(A)\cup f(B)$
$f(A\cap B)\subseteq f(A)\cap f(B)$
请读者自己证明

第二个的反例
a->1
b->2
c->1
A={a,b}
B={b,c}
{% endcallout %}
Graph of the function f:A->B
$$\\{(a,b)\mid a\in A and f(a)=b \\}$$

## 3.1 One-to-one, Onto, One-to-one correspondence Functions
{% callout success::Definition %} 
***One-to-one functions***:
A function f is ***one-to-one***, or ***injective***, if and only if $$\forall a\forall b(f(a)=f(b)\rightarrow a=b)$$
单射

***Onto functions***:
A function f from A to B is called ***onto***, or ***surjective***, if and only if$$\forall b\in B\exists a\in A(f(a)=b)$$

That is, every b in B has at least one preimage.
满射

***One-to-one correspondence functions***:
The function f is a ***one-to-oone correspondence***, or a ***bijection***, if it's both one-to-one and onto.

双射，一对一

***Inverse Function***:
必须是bijection，即双射才有反函数，如果只是满射可能出现f的反函数一对多的情况
{% endcallout %}

## 3.2 Some Important Functions
***The floor function***:
The floor function f(x) is the largest integer less than or equal to the **real number** x.
Notation: $\left \lfloor x \right \rfloor$ or $[x]$

***The celling function***:
The floor function f(x) is the samllest integer greater than or equal to the **real number** x.
Notation: $\left \lceil  x \right \rceil $

例 证明：$[2x]=[x]+[x+\frac{1}{2}]$
证明：
设$n\le x < n+1$
$[x]=n,[2x]=2n或2n+1$
若$[2x]=2n$,则$x < n+\frac{1}{2}$
则$[x+\frac{1}{2}]=n$
若$[2x]=2n+1$,则$x \ge n+\frac{1}{2}$
则$[x+\frac{1}{2}]=n+1$
QED

当然从上面也可看出可以按$x=n+k$
$k<\frac{1}{2}$和$k\ge \frac{1}{2}$来分类

## 3.4 Sequence and Summations
$$\sum_{k=0}^\infty x^k = \frac{1}{1-x}  (|x|<1)$$
$$\sum_{k=1}^\infty kx^{k-1} = \frac{1}{(1-x)^2}  (|x|<1)$$
666还和级数联动

## 3.5 Cardinality of Sets
本章最重要的一节
对于无穷的集合，我们怎么数元素个数呢？
正整数集和奇数集哪个集合元素多？ 
这似乎是我们目前都没有解决的问题
{% callout success::Definition %} 
The cardinality of a set $A$ is equal to the cardinality of a set $B$, denoted $\mid A\mid = \mid B \mid$, iff there exists a ***bijection*** from $A$ to $B$.

---

If there is an ***injection*** from $A$ to $B$, the cardinality of $A$ is less than or the same as the cardinality of $B$

---

A set that is either finite or has the same cardinality as the set of positive integers called ***countable***. 

A set that is not countable is called uncountable. 

When an infinite set $S$ is countable, we denote the cardinality of $S$ by $\aleph_0$ ( aleph null ).$$\mid S\mid = \aleph_0$$

If $\mid A\mid$ = $\mid Z^+\mid$, the set $A$ is countably infinite. 
{% endcallout %}

{% callout primary::Example %} 
例1 | 证明整数集可数
Proof:
An infinite set is countable iff it's possible to list the elements of the set in a sequence (indexed by positive integers)

对于整数集，我们可以这样列
0,1,-1,2,-2,3,-3,...
$f(n) = -\frac{n-1}{2}$ if n is odd
$f(n) = \frac{n}{2}$ if n is even
这是一个从$Z^+$到$Z$的双射
QED

{% endcallout %}


{% callout info::Schrőder-Bernstein Theorem %} 

If A and B are sets with $\mid A\mid \le \mid B\mid$ and $\mid B\mid \le \mid A\mid$ then $\mid A\mid = \mid B\mid$.  
In other words, if there are one-to-one functions f from A to B and g from B to A, then there is a one to –one correspondence between a A and B.

证明略。感兴趣的读者可以自行了解
{% endcallout %}

{% callout primary::Example %}
例2 | 证明正有理数集可数
$$S=\{(p,q)\mid p,q为正整数\} = Z^+ × Z^+$$
$r=\frac{p}{q}$
$\frac{p}{q}\rightarrow (p,q)$是单射
故$\mid Q^+\mid \le \mid S\mid$
![](img/DM/2-1.png)
下面写出了正整数和(p,q)的对应关系，即(p,q)对应着正整数n
这是怎么来的呢？就是从右上往左下这样数
第一个对角线一个，和为2，第二个对角线有两个，p+q=3，第三个对角线有三个，p+q=4……
所以(p,q)所在对角线和应该为p+q，这条对角线有p+q-1个，上一条对角线就有p+q-2个，求和，它是这条对角线第q个，加上q
这里我们也证明了正整数集的笛卡尔积也是可数的

正整数集包含于正有理数集（只需要让正整数映射到正有理数集里的正整数，就构成单射）
所以$\mid Z^+\mid \le \mid Q^+\mid$
综上，正有理数集可数

事实上，有理数集是可数的
{% endcallout %}

{% callout info::The properties of the countable sets: 
 %} 
- No infinite set has a smaller cardinality than a countable set.
- The union of two countable sets is countable.
- The union of finite number of countable sets is countable.
- The union of a countable number of countable sets is countable.

{% endcallout %}

## 3.6 Cantor Diagonalization  Argument
{% callout primary::Example %}
The set of real numbers between 0 and 1 is uncountable. 

$$B=\{ \frac{1}{n+1}\mid n\in Z^+ \}$$
B的cardinality和正整数集一样（单射），B又包含于A，所以正整数集的cardinality小于等于A的cardinality

![](img/DM/2-2.png)
{% endcallout %}


{% callout info::Theorem %} 
The cardinality of the power set of an arbitrary 
set has a greater cardinality than the original arbitrary set.

证明：
对于有限集，原集合的cardinality为n，power set的就是$2^n$
对于无限集呢？下面这个证法对于有限/无限集合是通用的

把A中的每个元素映射到只包含它自己的单元素子集
$f(a) = \{a\}$
$\{a\}$是P(A)的元素
所以$\mid A\mid$小于等于$\mid P(A)\mid$


**假设**：存在一个满射 $\( g: A \to \mathcal{P}(A) \)$。
这意味着对于 $\( A \)$ 的**每一个子集** $\( S \subseteq A \)$，都存在至少一个元素 $\( a \in A \) 使得 \( g(a) = S \)$。

我们定义：
$\[
B = \{ x \in A \mid x \notin g(x) \}
\]$
即 $\( B \)$ 是 $\( A \)$ 中所有**不属于自身在映射 $ g $ 下的像**的元素构成的集合。

显然 $\( B \subseteq A \)$，因此 $\( B \in \mathcal{P}(A) \)$。


因为 \( g \) 是满射，所以必然存在某个元素 $\( b \in A \)$，使得：
$\[
g(b) = B
\]$

现在我们问一个关键问题：**\( b \) 是否属于 \( B \)？** 无论答案是“是”还是“否”，都会导致矛盾：

- **情况1：若 $\( b \in B \)$**
  根据 \( B \) 的定义，\( B \) 中的元素满足 \( x \notin g(x) \)，因此：
  \[
  b \notin g(b)
  \]
  但我们已知 \( g(b) = B \)，代入得：
  \[
  b \notin B
  \]
  这与假设 \( b \in B \) 矛盾。

- **情况2：若 \( b \notin B \)**
  根据 \( B \) 的定义，不在 \( B \) 中的元素不满足 \( x \notin g(x) \)，即：
  \[
  b \in g(b)
  \]
  同样代入 \( g(b) = B \)，得：
  \[
  b \in B
  \]
  这又与假设 \( b \notin B \) 矛盾。

两种情况都导出矛盾，说明我们最初的假设 “存在满射g:A→P(A)” 是错误的。
因此不存在从A到P(A)的满射，更不存在双射
所以$\mid A\mid$小于$\mid P(A)\mid$

这跟对角线证明是很像的，我们想象一个表格，列是A的各个元素，行是各个元素的像
每一行都代表A的一个子集，也就是$g(i)$
比如每列是A的元素a,b,c,...
每行就是g(a),g(b),...
如果$j\in g(i)$，那么就在对应的格子$(i,j)$写1

我们构造的集合，就是如果(1,1)是1，那么它就写0，代表A的第一个元素不在这个集合里，比如(2,2)是1，即$b\in g(b)$，那么b就不在这个集合里，如果是0，比如$c\notin g(c)$，那么就把c放进这个集合

这样，我们也得到了“一行”，即一个集合，B是一个新的子集，它和表格中的每一行都至少有一个位置不同 —— 就是对角线的位置

而如果真的存在这个满射g，那么应该像涵盖了P(A)的每个元素，即A的所有子集，这就矛盾了
{% endcallout %}

{% callout primary::Example %} 
【1】证明[0,1]和(0,1)的cardinality一样
B=(0,1)包含于A=[0,1]
B的cardinality小于等于A

$g(x) = \frac{x}{2} + \frac{1}{4}, x\in [0,1]$
这是一个从[0,1]到[1/4,3/4]的bijection
所以B的cardinality大于等于A
证明完毕

【2】证明(a,b)和(0,1)的cardinality一样
比如我们可以让0.5映射到a+0.5(b-a)，也就是在(0,1)走了多少的比例，在(a,b)也走相同比例
x映射到a+x(b-a)，0 < x < 1
$f(x) = a+(b-a)x, x\in (0,1)$
这是个双射，证明完毕

【3】证明实数集的cardinality和(0,1)一样
$g(x) = tan(x)$
这是一个从$(-\pi/2,\pi/2)$到实数集的双射
由【2】证明完毕

【4】Show that the set of finite strings S over a finite alphabet A is countably infinite.

对于A里的字符，我们先确定一个顺序
将长度为1的字符串按这个顺序排好
然后长度为2的字符串（如果第一位一样，就比第二位的顺序，以此类推）
……
这样对于任意一个**有限长度**字符串，只要知道了它的长度，比如k，就能对应到一个整数，接下来只要看它在长度为k那组里的顺序，由原先确定好的顺序就可得到，这是一个从正整数到S的双射

或利用可数个可数集合的并仍是可数集合

**注意**
这里我们区分一下“有限”和“可数”
有限集一定是可数集，可数集包含有限集以及无限可数集
这题中，"finite strings" 是指字符串长度有限，也就是不考虑无限长度的字符串，但是！字符串的长度的**取值**有无限个，也就是正整数集
我们把S分成按长度划分的若干集合，这些集合有无限多个，如果我们把这些集合放到一个集合G里，那么G应该是无限可数的
所以我们其实是利用了可数个有限集合的并仍是可数集合，但这个结论是“可数个可数集合的并仍是可数集合”的特殊情况，因为有限集是可数集
{% endcallout %}