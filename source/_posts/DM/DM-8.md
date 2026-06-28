---
title: Chapter 11 - Trees
date: 2026-06-22 19:43:50
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

# 1 Introduction to Trees
{% callout success::Definition %} 
A tree is a **connected undirected** graph with **no simple circuits**.(当然，circuit也不能有，事实上，如果有更一般的，比如允许边重复的circuit，一定就会有simple circuit，所以这样定义直接处理了最小化的条件)

Forest is an undirected graph with no simple circuits.

- Any tree must be a simple graph.
- Each connected components of forest is a tree.
{% endcallout %}

{% callout info::Theorem %} 
An undirected graph is a tree if and only if there is a unique simple path between any two of its vertices.
{% endcallout %}
证明：
如果图是树，那么它连通故任意两个顶点间有路径，且没有simple circuit，故路径是唯一的，假设不唯一，那么从$v_i$到$v_j$可以再从$v_j$到$v_i$，形成一个circuit

如果任意两个顶点都有唯一的简单路径，那么这个图连通，而且由于任意两个顶点间路径唯一，假设有simple circuit，那么就会有两条不一样的简单路径，矛盾

***rooted tree***:Once we specify a root, we direct each edge away from the root. A tree together with its root produces a directed graph called a rooted tree.

一些术语，对于节点v
- parent：v头上指向v的节点
- child：v指向的节点
- siblings：和v有相同parent的节点
- ancestor：从v到root的路径上的所有节点（包含root，不包含v）
- descendants：所有祖先中有v的节点
- internal vertices：有孩子的节点
- leaf：没有孩子的节点

{% callout success::Definition %} 
A rooted tree is called a m-ary tree if every internal vertex has no more than m children. 

It is a binary tree if m = 2. 

The tree is called a full  m-ary tree if every internal vertex has exactly m children.
{% endcallout %}

{% callout success::Definition %} 
An ordered rooted tree is a rooted tree where the children of each internal vertex are ordered. 

In an ordered binary tree, the two possible children of a vertex are called the left child and the right child, if they exist.

The tree rooted at the left child is called the left subtree, and that rooted at the right child is called the right subtree
{% endcallout %}

{% callout primary::Example %} 
How many nonisomorphic unrooted trees are there with n vertices if n=5 ?

按树的最长路径长度分类
1. 最长路径长度为4，一条直线，1种
2. 最长路径为3，只能在中间两个顶点分支出来，1种
3. 最长路径为2，在中间顶点分出两个孩子，1种
   
How many nonisomorphic rooted trees are there with n vertices if n=5 ?

![](img/DM/8-1.png)
{% endcallout %}

## 1.1 Tree Properties
{% callout warning::Theorem %} 
A tree with $n$ vertices has $n-1$ edges
{% endcallout %}
除了根，每个节点上面一条边。也可以用Euler公式证明

{% callout warning::Theorem %} 
A full m-ary tree with $i$ internal vertices contains $n=mi+1$ vertices.
{% endcallout %}
除了root，每个节点都是internal节点的孩子，所以除了root，一共有$mi$个节点

{% callout warning::Theorem %} 
A full m-ary tree with
- $n$ vertices has $i=(n-1)/m$ internal vertices and $l=[(m-1)n+1]/m$ leaves
- $i$ internal vertices has $n=mi+1$ vertices and $l=(m-1)i+1$ leaves
- $l$ leaves has $n=(ml-1)/(m-1)$ vertices and $i=(l-1)/(m-1)$ internal vertices
{% endcallout %}
利用上一个定理和$n=i+l$即可证明

The level of vertex v in a rooted tree is the length of the unique path from the root to v.    

The height of a rooted tree is the maximum of the levels of its vertices.    

A rooted m-ary tree of height h is called balanced if all its leaves are at levels h or h-1.  

{% callout warning::Theorem %} 
There are at most $m^h$ leaves in an m-ary tree of height h. 
{% endcallout %}
h=1，成立，假设对于所有h小于等于k的m-ary树都成立
这时，对于h=k+1的这棵树，它最多有m个高度小于等于k的子树，每个子树最多$m^k$个叶(即此时每个子树的高度都为k)
所以这棵树最多$m^{k+1}$个叶节点

{% callout warning::Corallary %} 
If an m-ary tree of height $h$ has $l$ leaves, then $h\ge \left \lceil log_m l \right \rceil $
If the m-ary tree is full and balanced, then
If an m-ary tree of height $h$ has $l$ leaves, and is full and balanced, then $h = \left \lceil log_m l \right \rceil $

{% endcallout %}
第一条由上个定理得证，第二条，由于树是平衡的，叶节点的要么在第h层要么在第h-1层，且不能全在h-1层，所以第h层有叶节点，又这棵树是full的，有第h层说明第h-1层是满的，而且叶节点数量一定多于第k-1层的节点数
所以$m^{h-1} < l$且$l\le m^h$

树都是二分图：每隔一层涂上不同颜色即可，一共两种颜色

# 2 Application of Trees
## 2.1 Binary Search Trees
- An ordered rooted binary tree
- Each vertex contains a distinct key value
- The key values in the tree can be compared using “greater than” and “less than”, and
- The key value of each vertex in the tree is less than every key value in its right subtree, and greater than every key value in its left subtree.

>If a binary search tree is balanced, locating or adding an item requires no more than  $\left \lceil log (n+1) \right \rceil $ comparisons.

Suppose we have a binary search tree T for a list of n items.
We can form a full binary tree U from T by adding unlabeled vertices whenever necessary so that every vertex with a key has two children.
这时，根据上一节的定理，U的internal vertices的数量就是n+1，由上一节推论，$h\ge \left \lceil log (n+1) \right \rceil$

最多需要比较的次数就是从U的根到叶的最长路径长度，U的最长路径的长度就是h，所以比较次数大于等于$\left \lceil log (n+1) \right \rceil$

如果U是平衡的，那么根据上一节推论，它的高度$h= \left \lceil log (n+1) \right \rceil$，所以最多只需要$\left \lceil log (n+1) \right \rceil$次比较，而如果二叉搜索树T是平衡的(应当指出的是，这里的平衡是指每个节点的左右子树高度差不超过1，如果根据之前的balanced的定义，一条链也是平衡的)，U一定平衡，就有对应结论。

## 2.2 Decision Trees
A rooted tree in which each internal vertex corresponds to a decision, with a subtree at these vertices for each possible outcome of the decision, is called a decision tree.
从8个硬币挑出唯一的那个假币，其中假币更轻
![](img/DM/8-2.png)

## 2.3 Prefix Codes
用bit串给字母编码，26个字母，按理说要用5位，为了提高效率，我们可以用不同长度的bit串去编码，比如0代表e，1代表a，01代表t

但这有个问题，0101是eat还是tt呢
如果把e编成0，a编成你0，t编成110，问题就解决了
To ensure that no bit string corresponds to more than one sequence of letters, the bit string for a letter must never occur as the first part of the bit string for another letter. Codes with this property are called prefix codes.

通过二叉树编写前缀码：
- 每个internal vertex的左侧的那条边标记0
- 每个internal vertex的右侧的那条边标记1
- the leaves are labeled by characters which are encoded with the bit string constructed using the labels of the edges in the unique path from the root to the leaves.

### Huffman Coding
每个字符的出现频率不同，对于字符$i$，它的权重，即出现的频率为$w_i$，这个字符的前缀码长度为$l_i$，我们希望这棵树的各叶节点$l_iw_i$之和最小

```c
Algorithm 2 Huffman Coding.
Procedure Huffman (C: symbols ai with frequencies wi, i=1, …, n)
F:=forest of n rooted trees, each consisting of the single vertex ai and assigned weight wi 
While  F is not a tree
 begin
     Replace the rooted trees T and T’ of least weights from F with w(T) ≥w(T’) with a tree having a new root that has T as its left subtree and T’ as its right subtree. Label the new edge to T with 0 and the new edge to T’ with 1.
      Assign w(T) + w(T’) as the weight of the new tree. 
end
 {The Huffman coding for the symbol ai is the concatenation(一系列关联事物) of the labels of the edges in the unique path from the root to the vertex ai }
```
![](img/DM/8-3.png)
例如得到的A的编码就是111，E是10

# 3 Tree Traversal
Preorder: 如果树只有一个根，那么前序遍历就是访问根，否则先访问root，然后对左子树前序遍历，然后对右子树前序遍历

Inorder: 如果树只有一个根，那么中序遍历就是访问根，先对左子树中序遍历，再访问根，再对右子树中序遍历

Postorder: 如果树只有一个根，那么后序遍历就是访问根，先对左子树后序遍历，再对右子树后序遍历，再访问根

## 3.1 Infix, prefix, and postfix notation
A Binary Expression Tree is a kind of binary tree in which:
1. Each leaf node contains a single operand,
2.  Each nonleaf node contains a single operator, and
3.  The left and right subtrees of an operator node represent subexpressions that must be evaluated before applying the operator at the root of the subtree.

**Infix Form**
The fully parenthesized expression obtained by an inorder traversal of the binary tree is said to be in infix form.
![](img/DM/8-4.png)

{% callout primary::Example %} 
计算Prefix Form : * - 8 5 / + 4 2 3

可以画出树，也可以从右到左计算，先看到3，然后2，然后4，然后+，这时计算4+2=6，然后看到/，计算(4+2)/3=6/3=2，然后5，8，得到-，计算8-5=3，然后*，计算3*[(4+2)/3]=3*2=6

计算Postfix Form : 8 5 - 4 2 + 3 / *
从左往右计算，先8，5，-，计算8-5=3，然后4，2，+，计算4+2=6，然后3，/，计算6/3=2，然后*，计算3*2=6

注意这里的顺序，prefix里是后遇到6，用计算6/3，postfix是后遇到3，但也是计算6/3，又如后缀表达式是8-5，实在不行可以画树检验一下

{% endcallout %}

# 4 Spanning Trees
{% callout success::Definition %} 
Let G be a simple graph. A spanning tree of G is a subgraph of G that is a tree containing every vertex of G.
{% endcallout %}
找生成树：每次移除掉simple circuites里的一条边

{% callout info::Theorem %} 
A simple graph is connected if and only if it has a spanning tree.
{% endcallout %}
如果有生成树，树本身连通，G肯定也连通
如果G连通，移除simple circuits里的边来得到生成树(怎么跟没证一样)

其他构造生成树的算法
1. 深度优先搜索。随便选一个顶点作为根，从这个顶点开始加一条边，然后从连接到的那个顶点继续加一条边，一直这样做下去，如果走不下去就回溯
2. 广度优先搜索。随便选图中的一个顶点，把它邻接的顶点和边全加进来，作为生成树的level 1，随意排列它们，然后按这个顺序遍历第一层的顶点，对每个顶点进行同样的操作（除非加进来一个邻接顶点后构成了simple circuit），一直做下去直到所有顶点都被添加

![](img/DM/8-5.png)

**Backtracking scheme**
用决策树找出符合题意的情况

# 5 Minimum Spanning Trees
见FDS
Prim: 从最短的边和它的两个顶点开始，这构成图T，每次选择与T中顶点邻接的边权最小的边，如果加入后不构成simple circuit，加入T

Kruskal: 边的权从小到大排好，每次选边权最小的边加入，只要不构成simple circuit

