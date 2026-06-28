---
title: Chapter 9 - Relations
date: 2026-05-19 16:25:58
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

# 1 Relations and Their Properties
{% callout success::Definition %} 
A ***binary relation*** $R$ from a set $A$ to a set $B$ is a subset of $A\times B$. 
{% endcallout %}

例如，从A到B的函数f的图像就是一个从A到B的关系
A={1,0,-1}, B={0,1}
R={(1,1), (-1,1), (0,0)}

{% callout success::Definition %} 
A ***relation on the set $A$*** is a relation from $A$ to $A$.
{% endcallout %}

如果A有n个元素，有多少个作用于A的二元关系呢？
我们知道，R是$A\times A$的子集，而$A\times A$有$n^2$个元素，所以R一共有$2^{n^2}$个

# 2 Representing Relations
## 2.1 2D Table
![](img/DM/6-1.png)

## 2.2 Connection Matrices
![](img/DM/6-2.png)
使用关系矩阵表示关系，我们也可以知道，如果A有n个元素，那么作用于A的关系就可以用一个n x n的矩阵表示，其中每个元素可以为0或1，所以一共有$2^{n^2}$种关系

## 2.3 Directed Graph / Digraph
![](img/DM/6-3.png)

## 2.4 Special Properties of Binary Relations
### 2.4.1 Reflexive
如果$\forall x (x\in A\rightarrow (x,x)\in R)$
那么作用于A的关系R是自反的

根据定义，表示一个自反的关系的矩阵，它的主对角线应该都是1，如果用有向图表示，那么每个顶点都有一个箭头指向自己

### 2.4.2 Irreflexive
如果$\forall x (x\in A\rightarrow (x,x)\notin R)$
那么作用于A的关系R是反自反的

矩阵要求主对角线全是0

反自反并不是自反的反面，读者可以自行推导自反的反面是什么，一个关系可以既不是reflexive也不是irreflexive
比如一个对角线上既有0又有1的矩阵

### 2.4.3 Symmetric
如果$\forall x\forall y ((x,y)\in R\rightarrow (y,x)\in R)$
那么作用于集合A的关系R是对称的

矩阵关于主对角线对称

### 2.4.4 Antisymmetric
如果$\forall x\forall y ((x,y)\in R\land (y,x)\in R \rightarrow x=y)$
那么作用于A的关系R是反对称的
也就是说，如果R是反对称的，它的主对角线无所谓，不是主对角线的地方，如果有一个地方是1，那么它关于主对角线对称的地方必须是0（否则不符合antisymmetric），但是如果一个不是主对角线的地方它是0，那么跟它对称的地方可以是0也可以是1，因为前提已经为假了

也就是说，只要有关于主对角线对称的地方都为1，就不是反对称


同样，对称和反对称并不是对立的
我们可以找到两者都符合的：比如对角线全1其他地方全0
也可以找到两者都不符合的：比如有一处关于对角线不对称，有一处关于主对角线对称

### 2.4.5 Transitive
如果$\forall x\forall y\forall z ((x,y)\in R\land (y,z)\in R\rightarrow (x,z)\in R)$
那么作用于A的R具有传递性

我们来看一个例子，如果R对称而且传递，能否推出自反呢？
看上去(a,b)在就有(b,a)在，从而推出(a,a)在
但问题是，要证明自反，从定义上，任意的在A中的a，$(a,a)\in R$
问题在于任意的a，都存在一个b使得$(a,b)\in R$吗，这是不一定的
以此就可以构造反例

{% callout primary::Example %} 
【1】一个作用于有n个元素的集合A的关系R，R有多少种？
(1) R是自反的
(2) R是对称的
(3) R是反对称的
(4) R既对称又自反

自反要求对角线全为1，其他随便，有$2^{n^2-n}$种

对称要求关于主对角线的地方要么都为1，要么都为0，主对角线上无所谓所以有$2^n \times 2^{(n-1)+(n-2)+...+1}$

反对称要求不能出现对称的地方，主对角线上无所谓，不是主对角线的两个对称位置有这些选择：都为0，一个为0一个为1（这是2种）
对称位置的个数等于除去主对角线的上三角区域的元素个数，所以答案为$2^n \times 3^{(n-1)+(n-2)+...+1}$

和(2)类似，但主对角线全1，所以答案为$2^{(n-1)+(n-2)+...+1}$
{% endcallout %}

## 2.5 Combining Relations
关系R是两个集合的笛卡尔积的子集，所以关系之间也可以有集合的运算、复合等
![](img/DM/6-4.png)

### 2.5.1 Composition
![](img/DM/6-5.png)
![](img/DM/6-6.png)

{% callout success::Definition %} 
R是一个作用于A的关系，$R^n$定义为
$R^1=R, R^{n+1}=R^n\circ R$
{% endcallout %}
![](img/DM/6-7.png)
有向图中，其实就是能找到从一个顶点A到另一个顶点B，中间只走两步，这两个顶点(A,B)就在$R^2$里

{% callout info::Theorem %} 
作用在A上的关系R具有传递性当且仅当$R^n$包含于$R$
n=1, 2, 3, ...
{% endcallout %}
证明：
如果$R^n$包含于$R$
那么$R^2$包含于$R$

对于$R$里的(a,b),(b,c)（如果有的话）
$R^2$里就会有(a,c)，由于$R^2$包含于$R$，它也在$R$里
所以R具有传递性

反过来，如果R具有传递性，n=1，成立
假设n=k成立，k大于等于1
对于$R^{k+1}$里的元素(a,b)，由于$R^{k+1}$由$R^{k}$和$R$复合而来，一定有$(a,x)\in R$且$(x,b)\in R^k$
又$(x,b)\in R$且R有传递性
所以$(a,b)\in R$


{% callout primary::Example %} 
已知$R^1=R, R^{n+1}=R^n\circ R$，证明$R^{n+1}=R\circ R^n$

我们先证明复合具有结合律
对于$(a,b)\in (P\circ U)\circ T$
$(a,x)\in T, (x,b)\in (P\circ U)$
于是$(x,c)\in U, (c,b)\in P$

于是有$(a,c)\in (U\circ T)$
又$(c,b)\in P$
于是$(a,b)\in P\circ (U\circ T)$

根据递归定义，实际上$R^n$就是n个R复合（$R^2$是两个R复合，$R^3$计算又可以把$R^2$拆开成两个R复合……），所以前n-1个R结合和后n-1个R结合是一样的，证明完毕
{% endcallout %}

### 2.5.2 Inverse Relation
![](img/DM/6-8.png)
![](img/DM/6-9.png)
我们证明第一个
对于$(x,y)\in (R\cup S)^{-1}, (y,x)\in R\cup S$
那么$(y,x)\in R或(y,x)\in S$
那么$(x,y)\in R^{-1}或(x,y)\in S^{-1}$
证明完毕

剩下的以（liu）后（gei）有（du）时（zhe）间（zi）补（zheng）

# 3 Closures of Relations
{% callout success %} 
The closure of a relation $R$ with respect to property $P$ is the relation $S$ with property $P$ containing $R$ such that $S$ is a subset of every relation with property $P$ containing $R$. 
{% endcallout %}
就是包含$R$的具有性质$P$的最小的关系

## 3.1 Reflexive Closure
{% callout info::Theorem %} 
R是作用于集合A的关系，R的自反闭包记作$r(R)$，是$R\cup I_A$
其中$I_A$={$(x,x)\mid x\in A$}
{% endcallout %}

从这里我们就能看出什么叫“最小”
例如，A={1,2,3}
R={(3,1),(2,3)}
给出R的自反闭包？
{(3,1),(2,3),(1,1),(2,2),(3,3)}

又例如，R={(a,b)}其中a，b为整数且满足a < b
那么R的自反闭包就是{(a,b)}
其中a，b为整数且满足a小于等于b

下面我们证明这个定理
首先这个关系包含R，其次具有自反性质
假设R'是一个包含R且自反的关系，那么R肯定包含于R'，$I_A$也包含于R'，所以r(R)包含于R'

【推论】如果关系R作用于集合A，则$R=R\cup I_A$ 等价于 R是一个具有自反性的关系
如果R有自反性，那么一定包含$I_A$
如果$R=R\cup I_A$，那么R包含$I_A$，于是R是一个自反的关系

## 3.2 Symmetric Closure
Let R be a relation on A. The symmetric closure 
of R, denoted by s(R), is  $R\cup R^{-1}$

我们来证明
首先这个关系包含R，其次对于$(a,b)\in R\cup R^{-1}$
如果$(a,b)\in R$，那么$(b,a)\in R^{-1}$
如果$(a,b)\in R^{-1}$，那么$(b,a)\in R$
所以$(b,a)\in R\cup R^{-1}$
故这个关系是对称的

假设R'也是对称且包含R的关系
如果$(a,b)\in R$，且R包含于R'，那么$(a,b)\in R'$
如果$(a,b)\in R^{-1}$，则$(b,a)\in R$，所以$(b,a)\in R'$，考虑R'的对称性，可以得到$(a,b)\in R'$，所以$R\cup R^{-1}$包含于R'

其实这个不难理解，就是把现有的全部对称一遍加进来
也有类似自反闭包的推论

## 3.3 Transitive Closure

There is a path of length n from a to b in R:
$\exists a, x_1, x_2,. . . , x_{n-1}, b$  such that $(a, x_1)\in R, (x_1, x_2)\in R, …,( x_{n-1}, b)\in R$
用有向图表示关系，路径就不难理解

Let R be a relation on A. There is a path of length n from a to b if and only if $(a,b)\in R^n$
这个定理就说明了复合就是找路径
用归纳法证明，n=1成立
Inductive step     
      There is a path of length n+1 from a to b if and only if there is an x in A such that there is a path of length 1 from a to x and a path of length n from x to b.  
       From the Induction Hypothesis, 
$(a,x)\in R,(x,b)\in R^n$
于是有$(a,b)\in R^{n+1}$

{% callout success::Definition %} 
The connectivity relation denoted by R\*, is the set of ordered pairs (a, b) such that there is a path (in R) from a to b.
R\*等于所有$R^n$的并（n从1到无穷）
{% endcallout %}

{% callout info::Theorem %} 
$t(R)=R^{*}$
{% endcallout %}
孩子们，来不及复习了，我先略去证明

同样地，传递闭包具有传递性
{% callout info::Theorem %} 
If |A| = n, then any path of length > n must 
contain a cycle. 

所以这时只要找R的1到n次方的并即可
{% endcallout %}

![](img/DM/6-10.png)
![](img/DM/6-11.png)
如图，k=1时，我们要看有没有$w_{i1}\land w_{1j}$为1的$w_{ij}$，也就是找最开始两道横线上的点有没有各出一个1的情况
如k=2时，两道线中，第一行第二列和第二行第四列都是1，所以$w_{14}$如果不是1就更新为1，同样地，$w_{44}$也更新为1

# 4 Equivalence Relations
## 4.1 Equivalence relations and equivalence classes 
作用于A的关系R是一个等价关系，如果它具有自反性、对称性、传递性
如果$(a,b)\in R$那么a和b是等价(equivalent)的，记作a~b
A中元素x，所有和x等价的元素的集合称为x的等价类(equivalent class)

例如R={(a,b)}其中a和b是整数，除以3的余数一样
即$a\equiv b (mod 3)$
可以证明R是等价关系，比如0的等价类就是3k，k为整数
![](img/DM/6-12.png)
证明这三项等价，只需证明1能推出2，2能推出3，3能推出1即可
对于$x\in [a]$，$(a,x)\in R$
又$aRb$，所以$(a,b)\in R$
利用对称、传递性得到$(b,x)\in R$
所以$x\in [b]$
同理可得[b]包含于[a]
R有自反性，[a]不会是空集，于是由2，3成立
由3，存在一个x，$(a,x),(b,x)\in R$
所以$(a,b)\in R$

## 4.2 Equivalence Relations and Partitions
{% callout success::Definition %}
A  partition of set A is a collection of disjoint nonempty subsets of A that have A as their union.
{% endcallout %}

{% callout info::Theorem %} 
集合A的一种划分对应一个等价关系R
也就是说，一个作用于A的等价关系R，它的等价类构成一个A的划分
反过来，对于A的一个划分$A_i$，存在一个等价关系，它以$A_i$为等价类
{% endcallout %}
对于一个等价关系R，它的等价类为$A_1, ... ,A_n$
每个等价类不是空的
每个等价类没有共有的部分，即任意两个等价类的交集为空集
对A中任意一个元素，它一定属于一个等价类，如果没有元素跟它等价，它至少跟自己等价，故而所有等价类并起来为A

反过来，对于A的一个划分
关系R为{(a,b)，$a,b\in A_i$}
任取一个元素a，它一定属于$A_i$，所以$(a,a)\in R$
若$(a,b)\in R$，意味着a和b在同一个划分的集合里，所以b和a在同一个划分里，故$(b,a)\in R$
若a和b在$A_i$里，b和c在$A_j$里，由于划分交集为空，所以这一定是同一个集合，即a，b，b在同一个划分的集合里，所以$(a,c)\in R$

![](img/DM/6-13.png)

## 4.3 The Operations of Equivalence Relations
{% callout info::Theorem %} 
若$R_1,R_2$是作用于A的等价关系，那么$R_1\cap R_2$也是等价关系

若$R_1,R_2$是作用于A的等价关系，那么$R_1\cup R_2$是自反、对称的关系

若$R_1,R_2$是作用于A的等价关系，那么$(R_1\cup R_2)^*$是等价关系
{% endcallout %}
前两个定理读者可以自己证明。
对于第三个定理的传递性，若 $(x,y) \in R^\*$ 且 $(y,z) \in R^\*$，则存在 $m,n$ 使得 $(x,y) \in R^m$，$(y,z) \in R^n$。由复合定义得 $(x,z) \in R^m \circ R^n = R^{m+n} \subseteq R^\*$。所以 $R^\*$ 是传递的，当然，由于本身是个传递闭包，所以当然具有传递性。 
对于第三个定理的自反性，任意$a\in A,(a,a)\in R_1$，证明完毕
对于第三个定理的对称性，任意$(a,b)\in (R_1\cup R_2)^*$，有$(a,b)\in (R_1\cup R_2)^m$，那么一定有从a到b的长度为m的路径，即$(a,x_1)\in (R_1\cup R_2), (x_1,x_2)\in (R_1\cup R_2), ... ,(x_{m-1},b)\in (R_1\cup R_2)$
而$(R_1\cup R_2)$有对称性，证明完毕

# 5 Partial Orderings
## 5.1 Partial Orderings
{% callout success::Definition %} 
R是作用于S的关系，如果R具有自反性，反对称性(antisymmetric)和传递性，那么称R是一个偏序关系
$(S,R)$称为一个***poset***
例如$(Z,\le)$

若$(S,R)$是一个poset且$(a,b)\in R$
记作$a\preceq b$，这里的像小于等于号的东西只是个记号，指一种关系，如果$a\preceq b$且$a\ne b$，我们记作$a\prec b$
如果poset$(S,\preceq)$中的a，b元素，要么$a\preceq b$，要么$b\preceq a$，则a和b是comparable的
例如，$(Z,\mid)$，比如2和7就是incomparable的

如果poset$(S,\preceq)$每两个S中的元素都是可比的，那么S称为totally ordered set / chain
$\preceq$称作total order
{% endcallout %}

## 5.2 Lexicographic Order
![](img/DM/6-14.png)
字典序本质上还是个关系，事实上，这个关系作用于集合$A_1\times A_2$，而且是个偏序关系

{% folding title="Proof" class="green" open=false %}
设 $$(A_1, \preceq_1)$$ 和 $$(A_2, \preceq_2)$$ 是两个偏序集。
我们在它们的笛卡尔积 $$A_1 \times A_2$$ 上定义字典序（Lexicographic Ordering） $$\preccurlyeq$$：
对任意 $$(a_1,b_1), (a_2,b_2) \in A_1 \times A_2$$，
$$(a_1,b_1) \preccurlyeq (a_2,b_2) \iff$$
要么 $$a_1 \preceq_1 a_2$$，要么 $$a_1 =_1 a_2$$ 且 $$b_1 \preceq_2 b_2$$
- 符号说明：$$\preceq_1$$ 是 $$A_1$$ 上的偏序，$$\preceq_2$$ 是 $$A_2$$ 上的偏序；$$=_1$$ 是 $$A_1$$ 上的相等，$$=_2$$ 是 $$A_2$$ 上的相等。
- 直观理解：像查字典一样，先比较第一个分量，第一个分量小的整体就小；只有第一个分量相等时，才比较第二个分量。

---
三、定理证明：$$(A_1 \times A_2, \preccurlyeq)$$ 是偏序集
我们需要证明字典序 $$\preccurlyeq$$ 同时满足自反性、反对称性、传递性。

---
1. 证明自反性（Reflexive）
要证：$$\forall (a,b) \in A_1 \times A_2, \quad (a,b) \preccurlyeq (a,b)$$
证明过程：
因为 $$(A_1, \preceq_1)$$ 是偏序集，所以 $$\preceq_1$$ 满足自反性，即 $$a \preceq_1 a$$。
根据字典序的定义，只要第一个分量满足 $$a \preceq_1 a$$，就有 $$(a,b) \preccurlyeq (a,b)$$。
自反性得证。

---
2. 证明反对称性（Antisymmetric）
要证：若 $$(a_1,b_1) \preccurlyeq (a_2,b_2)$$ 且 $$(a_2,b_2) \preccurlyeq (a_1,b_1)$$，则 $$a_1 =_1 a_2$$ 且 $$b_1 =_2 b_2$$（即 $$(a_1,b_1)=(a_2,b_2)$$）
证明过程：
从两个前提出发，先分析第一个分量的关系：
- 由 $$(a_1,b_1) \preccurlyeq (a_2,b_2)$$，根据字典序定义，必然有 $$a_1 \preceq_1 a_2$$（无论第二个分量如何，第一个条件总是成立）。
- 同理，由 $$(a_2,b_2) \preccurlyeq (a_1,b_1)$$，必然有 $$a_2 \preceq_1 a_1$$。
因为 $$(A_1, \preceq_1)$$ 是偏序集，$$\preceq_1$$ 满足反对称性，所以：
$$a_1 \preceq_1 a_2$$ 且 $$a_2 \preceq_1 a_1 \implies a_1 =_1 a_2$$
现在已知 $$a_1 =_1 a_2$$，再回到字典序定义分析第二个分量：
- 因为 $$a_1 =_1 a_2$$，所以 $$(a_1,b_1) \preccurlyeq (a_2,b_2)$$ 必须满足第二个条件：$$b_1 \preceq_2 b_2$$。
- 同理，$$(a_2,b_2) \preccurlyeq (a_1,b_1)$$ 必须满足：$$b_2 \preceq_2 b_1$$。
又因为 $$(A_2, \preceq_2)$$ 是偏序集，$$\preceq_2$$ 满足反对称性，所以：
$$b_1 \preceq_2 b_2$$ 且 $$b_2 \preceq_2 b_1 \implies b_1 =_2 b_2$$
综上，$$a_1 =_1 a_2$$ 且 $$b_1 =_2 b_2$$，反对称性得证。

---
3. 证明传递性（Transitive）
要证：若 $$(a_1,b_1) \preccurlyeq (a_2,b_2)$$ 且 $$(a_2,b_2) \preccurlyeq (a_3,b_3)$$，则 $$(a_1,b_1) \preccurlyeq (a_3,b_3)$$
证明过程：
我们分两种大情况讨论（基于第一个分量的关系）：
情况 1：$$a_1 \prec_1 a_2$$（即 $$a_1 \preceq_1 a_2$$ 且 $$a_1 \neq_1 a_2$$）
由第二个前提 $$(a_2,b_2) \preccurlyeq (a_3,b_3)$$，必然有 $$a_2 \preceq_1 a_3$$。
因为 $$(A_1, \preceq_1)$$ 是偏序集，$$\preceq_1$$ 满足传递性，所以：
$$a_1 \preceq_1 a_2 \preceq_1 a_3 \implies a_1 \preceq_1 a_3$$
根据字典序定义，只要第一个分量满足 $$a_1 \preceq_1 a_3$$，就有 $$(a_1,b_1) \preccurlyeq (a_3,b_3)$$。
情况 2：$$a_1 =_1 a_2$$
此时由第一个前提 $$(a_1,b_1) \preccurlyeq (a_2,b_2)$$，必须满足 $$b_1 \preceq_2 b_2$$。
现在再看第二个前提中 $$a_2$$ 和 $$a_3$$ 的关系：
- 子情况 2.1：$$a_2 \prec_1 a_3$$
因为 $$a_1 =_1 a_2$$，所以 $$a_1 \prec_1 a_3$$，满足字典序第一个条件，故 $$(a_1,b_1) \preccurlyeq (a_3,b_3)$$。
- 子情况 2.2：$$a_2 =_1 a_3$$
此时由第二个前提 $$(a_2,b_2) \preccurlyeq (a_3,b_3)$$，必须满足 $$b_2 \preceq_2 b_3$$。
因为 $$(A_2, \preceq_2)$$ 是偏序集，$$\preceq_2$$ 满足传递性，所以：
$$b_1 \preceq_2 b_2 \preceq_2 b_3 \implies b_1 \preceq_2 b_3$$
又因为 $$a_1 =_1 a_2 =_1 a_3$$，根据字典序定义，$$a_1 =_1 a_3$$ 且 $$b_1 \preceq_2 b_3$$，故 $$(a_1,b_1) \preccurlyeq (a_3,b_3)$$。
所有情况均成立，传递性得证。

$=_1$表示在$A_1$里相等
{% endfolding %}
![](img/DM/6-15.png)
字符串的字典序可能出现$(a_1,a_2,...,a_m),(b_1,b_2,...,b_n)$但$m<n$的情况，这时如果前m个字符都相等，那么仍认为$(a_1,a_2,...,a_m)\prec (b_1,b_2,...,b_n)$，这里的$\prec$指字典序

## 5.3 Hasse Diagrams
表示偏序关系的一种方式，它是一个有向图，所有除了自己指向自己的边都向上指
然后去掉所有自己指向自己的边，去掉因为传递性产生的边，去掉箭头，因为认为是向上指的
![](img/DM/6-16.png)

## 5.4 Chain and Antichain
$(A,\preceq )$是一个poset，B包含于A
如果$(B,\preceq )$是一个全序集合，那么B称为$(A,\preceq )$的一个chain
如果B中任意两个不同的元素a，b，(a,b)和(b,a)都不属于偏序关系R，那么B称作$(A,\preceq )$的一个antichain

## 5.5 Maximal and Minimal Elements
***Maximal Element***: $(A,\preceq )$是一个poset，对于元素a，如果不存在元素b使得$a \prec b$，那么a被称作maximal element

![](img/DM/6-17.png)

## 5.6 Greatest and Least Element
***Greatest Element***: $(A,\preceq )$是一个poset，对于元素a，所有在A中的元素b，都有$b\preceq a$
那么a被称作A的greatest element

可以证明$(A,\preceq )$如果有greatest或least元素，它们是唯一的

## 5.7 Upper and Lower Bound
Let A be a subset of S in the poset $(S,\preceq )$. If 
there exists an element a in S such that $b\preceq a$ for all b in A, then $a$ is called an upper bound of A. 

![](img/DM/6-18.png)

## 5.8 Well-ordered Sets
A poset (A, ≼) is ***well-ordered set*** if ≼ is a total order and every nonempty subset of A has a least element. 

A well-ordered set is a totally ordered set.

## 5.9 Lattices
A poset is called a lattice if every pair of elements has a lub and a glb. 
![](img/DM/6-19.png)

## 5.10 Topological Sorting
给定一个偏序集 (A,R)，我们想要在其上定义一个全序 ≼
≼（即任意两个元素都可比较的线性顺序），使得这个全序与原来的偏序 R 相容。相容的条件是：只要原偏序中 aRb 成立（即 a 在偏序下先于或等于b，那么在全序中就有 a≼b
换句话说，全序必须保持偏序中所有已有的顺序关系，不能破坏它们。
【定理】每个有限非空poset$(S,\preceq )$有一个minimal元素
每次选minimal元素，把它从S中删掉，就构成poset(S,R)的拓扑排序

比如先干a才能干b，先干b才能干c，先干b才能干d.
R是一个偏序关系，我们知道aRb,bRc,bRd
但R并不是一个全序关系，因为c和d不可比
我们构建一个全序关系，如a≼b≼c≼d
这时任意两个元素都可比，且保持了原来已有的偏序关系，比如由于bRd，所以b(≼c)≼d