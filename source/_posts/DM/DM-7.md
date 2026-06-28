---
title: Chapter 10 - Graphs
date: 2026-06-03 11:03:28
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

# 1 Graphs and Graph Models
A graph G=(V,E) consists of V, a nonempty set of vertices (or nodes) and E, a set of edges. Each  edge has either one or two vertices associated with it, called its endpoints. An edge is said to connect its endpoints.

无向图有几种类型
Simple Graph: A  graph in which each edge connects two different vertices and where no two edges connect the same pair of vertices.

Multigraph: Graphs that may have multiple edges connecting the same vertices.

Pseudograph: Graphs that may include loops, and possibly multiple edges connecting the same pair of vertices

有向图有几种类型
Simple Directed Graph: 没有loop，没有multiple directed edges

Directed Multigraph: a directed graphs that may have multiple directed edges  from a vertex to a second (possibly the same) vertex. 
也就是说如果有loop没有multiple edge，也算directed multigraph（虽然这玩意我也不知道有啥用

# 2 Graph Terminology
![](img/DM/7-1.png)
对于无向图，所有顶点的度之和为边数的二倍
有loop或multiple edges也成立

因此一个无向图，它的度为奇数的点只能有偶数个
对于有向图，所有顶点的入度之和等于所有顶点的出度之和，等于边数
也就是说，所有顶点的入度+出度之和为边数的二倍

## 2.1 Some Special Simple Graphs
**Complete Graphs $K_n$**: 
完全图，是一个无向图，每对顶点都有一条边

![](img/DM/7-2.png)
![](img/DM/7-3.png)
对于$Q_n$,每个顶点有n条边，代表有n个位可以不同，有点像格雷码

## 2.2 Bipartite Graphs
A simple graph G is bipartite if V can be partitioned into two disjoint subsets V1 and V2 such that every edge connects a vertex in V1 and a vertex in V2.
![](img/DM/7-4.png)
![](img/DM/7-5.png)

{% callout info::Theorem %} 
A simple graph is bipartite if and only if it is possible to assign one of two different colors to each vertex of the graph so that no two adjacent vertices are assigned the same color. 

读者可以自行证明
{% endcallout %}

## 2.3 Regular Graph
A simply graph is called regular if every vertex of this graph has the same degree.
A regular graph is called n-regular if every vertex in this graph has degree n.

例如，当m=n时，$K_{m,n}$是regular的

{% callout primary::Example %} 
How many subgraphs with at least one vertex does $W_3$ have?

$W_3$有四个顶点（事实上它和$K_4$是一个图）
如果子图只有一个顶点，四选一，有四个
如果子图有两个顶点，选两个顶点，可以有边，也可以没边，故C(4,2)×2
如果子图有三个顶点，选三个，这三个顶点有三条边，每条边都可以连/不连，故C(4,3)×$2^3$

如果子图有四个顶点，它们间有n(n-1)/2=6条边，故为C(4,4)×$2^6$
{% endcallout %}

两个简单图的并就是把顶点集合并起来，边集合并起来

# 3 Representing Graphs and Graph Isomorphism
## 3.1 Representing Graphs
邻接矩阵，邻接表

对于multigraph（无向图），有几条边矩阵位置就写几

对于有向图，邻接矩阵（只有0和1）每行的数值和代表该顶点的出度
对于无向图，邻接矩阵（可能有looop、multiedge）每列的数值和代表该顶点接了多少条边，也就是它的度减去loop的数量，要注意一个loop会贡献2个度

对于有向图，邻接矩阵每列的数值和代表该顶点入度

**Incidence Matrices**
每行代表每个顶点连接了哪些边
![](img/DM/7-6.png)

## 3.2 Isomorphism of Graphs
Graphs with the same structure are said to be isomorphic. 

Two simple graphs G1= (V1, E1) and G2= (V2, E2) are isomorphic if there is a 1-1 and onto function f from V1 to V2 such that for all a and b in V1, a and b are adjacent in G1 iff f(a) and f(b) are adjacent in G2.

Such a function f is called an isomorphism. 
简单来说，就是有一个两个图顶点的双射，A图的两个点如果相邻，映射到B图的两个点也要相邻

![](img/DM/7-7.png)
要检验两个图是否同构，直接找双射是很难的，毕竟有n!种双射，如果两个图同构，那么它们有一些性质必须一样，这可以作为检验两个图是否同构的依据

![](img/DM/7-8.png)
【1】中，圈出来的点度为4，但左图没有度为4的点
【2】中，左图圈出来的点度为3，必须跟右图两个度为3的点对应，考虑到右图对称，考虑圈出来的那个点，左图中圈出来的点的邻接点里有两个度为2，而右图不满足，所以不同构

【3】中，左图左上角的点出度为2，对应右图左下角的点，左图左上角的点两个邻接点的入度=出度=1，但右图左下角的点的一个邻接点入度=2，故不同构

# 4 Connectivity
>下午小测，上午十二点，终于补到这里了

## 4.1 Paths
A path of length n in a simple graph is a sequence of vertices v0, v1, . . . , vn such that {v0, v1} , {v1, v2} , ..., {v n-1, vn} are n edges in the graph.

The path is a circuit if it begins and ends at the same vertex (length greater than 0).

A path is simple if it does not contain the same edge more than once.

长度为0的路径只有一个顶点，有向图的路径也是同样的定义
{% callout info::Theorem %} 
从$v_i$到$v_j$的长度为r的不同路径，一共有x条，x=$A^r$中的$a_{ij}$
{% endcallout %}

## 4.2 Connectedness in undirected graphs
An undirected graph is called connected if there is a path between every pair of distinct vertices of the graph.   

【Theorem】There is a simple path between every pair of 
distinct vertices of a connected undirected graph. 

The maximally connected subgraphs of G are called the connected components or just the components. 

A vertex is a **cut vertex (or articulation point)**, if removing it and all edges incident with it results in more connected components than in the original graph.
Similarly if removal of an edge creates more components the edge is called **a cut edge or bridge**. 

## 4.3 Paths and Isomorphism
Some other  invariants 
- The number and size of connected components
- Path
  Two graphs are isomorphic only if they have simple circuits of the same length. 
  Two graphs are isomorphic only if they contain paths that go through vertices so that the corresponding vertices in the two graphs have the same degree. 

![](img/DM/7-9.png)


# 5 Euler and Hamilton Paths
Euler Path
     An Euler path is a **simple path(边不能重复)** containing every edge of G.
Euler Circuit
      An Euler circuit is a **simple circuit(边不能重复)** containing every edge of G.
Euler Graph
      A graph contains an Euler circuit.

A Hamilton path in a graph G  is a path which visits every vertex in G exactly once.  

A Hamilton circuit (or Hamilton cycle) is a cycle which visits every vertex exactly once, except for the first vertex, which is also visited at the end of the cycle.

If a connected graph G has a Hamilton circuit, then G is called a Hamilton graph.
{% callout warning %} 
A path of length greater than 0 that begins and ends at the same vertex is called a circuit or cycle.

A path or circuit is called simple if it doesn't contain the same edge more than once.
{% endcallout %}
## 5.1 Necessary and sufficient conditions for Euler circuit and paths
{% callout info::Theorem %} 
A **connected** multigraph has an Euler circuit if and only if each of its vertices has even degree.
{% endcallout %}
证明：如果一个图有欧拉回路，那么回路中间的每个顶点每被进去一次，就要出来一次，假设回路从a开始，回到a，a也是出去一次回来一次，当然，中途也可能经过a，所以所有顶点的度都是偶数

由于所有顶点度数均为偶数，从任意顶点a出发，沿着边行走，不重复边，因为每进入一个非起点顶点时总有未用边可离开（度数为偶保证不会卡住），最终必然回到a，得到一条simple circuit
$x_0=a, x_1, x_2, ... , x_n=a$
如果所有边都被使用了，这就是欧拉回路
如果还有边，那么把被使用过的边删掉，之后再把不和剩下的边连着的顶点都删掉，得到G的子图H
由于G是连通的，既然还有边剩下，肯定连在这个回路中的某个顶点w上了，从w开始构造一个H里结束于w（H中所有顶点度数仍为偶数，可以做到）的最长的simple path，这样$x_0, ... , w, ...(w在H的回路), w, ..., x_n$又是一个新回路
一直做下去直到所有边都被用完

{% callout info::Theorem %} 
A connected multigraph has an Euler path 
but not an Euler circuit if and only if it has exactly two vertices of odd degree.

{% endcallout %}
证明：
如果是欧拉路径，那么起点出去一次，终点进来一次，其余点出去一次进来一次

如果刚好两个顶点是奇数度，在它们之间加一条边，一定有欧拉回路，去掉回路中的这条边，就得到一个遍历所有边的路径，但由于这两个顶点不一样，所以并不是回路

## 5.2 Euler circuit and paths in directed graphs 
A directed multigraph having no isolated vertices has an Euler circuit if and only if 
- the graph is weakly connected 
- the in-degree and out-degree of each vertex are equal.


A directed multigraph having no isolated vertices has an Euler path but not an Euler circuit if and only if 
- the graph is weakly connected
- the in-degree and out-degree of each vertex are equal for all but two vertices, one that has in-degree 1 larger than its out-degree and the other that has out-degree 1 larger than its in-degree. 

## 5.3 The sufficient condition for the existence of Hamilton path and Hamilton circuit
{% callout info::Dirac's Theorem %} 
If G is a simple graph with n vertices with n>=3 such that the degree of every vertex in G is at least n/2, then G has a Hamilton circuit. 
{% endcallout %}

{% callout info::Ore's Theorem %} 
If G is a simple graph with n vertices with n>=3 such that
deg(u)+deg(v) >= n for every pair of nonadjacent vertices u and v in G , then G has a Hamilton circuit.

{% endcallout %}

## 5.4 The necessary condition for the existence of Hamilton path and Hamilton circuit

For undirected graph:
The necessary condition for the existence of Hamilton path:
- G is connected;
- There are at most two vertices which degree are less than 2.

The necessary condition for the existence of Hamilton circuit: 
- The degree of each vertex is larger than 1.
Some properties:
- If a vertex in the graph has degree two, then both edges that are incident with this vertex must be part of any Hamilton circuit.
- When a Hamilton circuit is being constructed and this circuit has passed through a vertex, then all remaining edges incident with this vertex, other than the two used in the circuit , can be removed from consideration.  

![](img/DM/7-10.png)
![](img/DM/7-11.png)

【Example】有七个人，他们每个人会说一些语言，怎么样安排他们坐在一个圆桌，使得相邻的人可以交流
人是顶点，有相同语言的人可以连一条边，然后找Hamilton Circuit即可

【Example】Seven examinations must be arranged in a week. Each day has one examination. The examinations are in charged by the same teacher cannot be arranged in the adjacent two days. One teacher is in charge at most four examinations. Show that such an arrangement is possible.

要证明这种安排是可能的，那么测试看作顶点，如果两个测试的负责老师不一样，那么它们可以连起来。
找一条汉密尔顿路径即可
由于一个老师最多管四个测试，那么对于七个测试，每个测试至少有3个相邻的测试，那么任何两个顶点，它们的度数之和大于等于6，但是，6小于7，不能用Ore定理

下面我们证明一个定理
{% callout info::Theorem %} 
If G is a simple graph with n vertices with n>=3 such that
deg(u)+deg(v) >= n-1 for every pair of nonadjacent vertices u and v in G , then G has a Hamilton path.

{% endcallout %}
我们添加一个顶点s，并把它跟G里所有顶点相连，这样，任何两个不相邻顶点u，v，它们的度数和大于等于n+1
由Ore定理，存在汉密尔顿回路，把其中的s和它的两条边去掉，就得到一个汉密尔顿路径，证明完毕

利用这个定理，一定存在汉密尔顿路径，所以这样的安排是可能的

# 6 Shortest Path Problems
Dijkstra’s Algorithm (undirected graph with positive weights)

```
Algorithm 1  Dijkstra’s Algorithm.
Procedure Dijkstra(G: weighted connected simple graph, with 
all weights positive)
{G has vertices a = v0, v1 , · · · ,vn =z and weights w(vi ,vj )
     where w(vi ,vj )=∞ if {vi ,vj }is not an edge in G}
For i:= 1 to n
    L(vi ):= ∞
L(a):=0
S := ø 
{the labels are now initialized so that the  label of a is zero and 
all other labels  are ∞,and S is the empty set } 

While z not in S
Begin 
    u :=a vertex not in S with L(u) minimal 
    S:=S∪ {u}
   for all vertices v not in S
      if L(u) +w(u,v)<L(v)  L(v) :=L(u)+w(u,v)
{this adds a vertex  to S with minimal label and updates the 
labels of vertices not in S}
End {L(z)=length of shortest path from a to z }

```
![](img/DM/7-12.png)

{% callout info::Theorem %} 
Dijkstra’s algorithm finds the length of a 
shortest path between two vertices in a connected simple undirected weighted graph. 

{% endcallout %}
证明：
假设第k轮循环时，
1. 在S中的每个顶点v的label（a到v的距离）都是a到该顶点的最短路径长度
2. 不在S中的顶点v的label是：只经过包含在S中的点的，a到v的最短路径的长度

k=0，即第0轮，即遍历所有点，只有L(a)=0，S此时是空集，a到a距离为0，这时这两条性质都成立

设顶点v是在第k+1轮加入S的顶点，由于我们假设第k轮上述两个性质成立，所以v在第k轮结束时还不在S里，而且它是第k+1轮时label最小且不在S里的顶点
我们先证明第k+1轮结束后性质1成立，只需证明新加入进来的v的label是a到v的最短路径的长度

假设不是，那么存在一条a到v的更短的路径，设它的长度为l，**这个路径中一定有不在S的点**，因为根据性质2，v的label就是只经过包含在S中的点的，a到v的最短路径的长度。

我们设这个点为u，那么$L(u)\le l\le L(v)$
所有u是label比v还小的点，这与v的label是不在S的点中最小的矛盾！

再证明性质2仍保持：
把v加入S后，对于仍然不在S中的顶点u
第k+1轮结束后，对于从a到u的只经过S内的点的最短路径，如果这条路径不包含v，那么u仍然保持上一轮的label，性质2成立

如果这条路径包含v，它一定是a到v最短+v到u距离
为什么？首先既然包含v，那么路径一定长成
$a\rightarrow ...\rightarrow v\rightarrow ...\rightarrow u$
a无论如何要先到v，所以肯定选a到v的最短路径M，对于后半段，假设v到u之间还有别的点x，即$a\rightarrow ...\rightarrow v\rightarrow ...\rightarrow x\rightarrow ...\rightarrow u$这个x属于S，那么从a到v再到x不如从a到x的最短的路径（不包含v），再走M中x到u的路径，这条路径就比M还短，矛盾

既然这两条性质一直保持，那么该算法就是找的最短路径
{% callout info::Theorem %} 
Dijkstra’s algorithm uses $O(n^2)$operations 
(additions and comparisons) to find the length of the shortest path between two vertices in a connected simple undirected weighted graph. 

{% endcallout %}
n个顶点，最多n-1次循环，每次循环最多比n-1次找到label最小的不在S的点，对至多n-1个顶点，比较并更新，2(n-1)次操作
所以复杂度为$O(N^2)$

# 7 Planar Graphs
{% callout success::Definition %} 
A graph is called ***planar*** if it can be drawn in the plane without any edges crossing .

Such a drawing is called a planar representation of the graph. 
{% endcallout %}

![](img/DM/7-13.png)

## 7.1 Euler's Formula
> 到底有几个欧拉公式

{% callout success::Definition %} 
A region is a part of the plane completely disconnected off from other parts of the plane by the edges of the graph.
{% endcallout %}

![](img/DM/7-14.png)
{% callout info::Theorem %} 
***Euler’s formula*** 
Let $G$ be a connected planar simple graph with $e$ edges and $v$ vertices. Let $r$ be the number of regions in a planar representation of $G$. Then $r=e-v+2$.

{% endcallout %}
证明：对于图$G$，我们随意选择一条边（和它带的两个顶点）作为$G_1$，从$G_{n-1}$到$G_n$，我们选一条和$G_{n-1}$中某个顶点连着an edge that is incident with a vertex already in $G_{n-1}$的边，加入这条边和这条边连接的另一个顶点（如果这个顶点不在$G_{n-1}$中）

$r_1=e_1-v_1+2$是对的，其中$r_n$就是$G_n$的区域数
假设$r_n=e_n-v_n+2$
添加一条边，设这条边是{a,b}，其中a在$G_n$中
如果b在$G_n$中，那么a，b一定在一个区域R的边界上，（这个R内部没有区域，当然如果有的话R也称不上区域）
那么r+1，v不变，e+1，在$G_{n+1}$依旧成立

如果如果b不在$G_n$中，那么e+1，v+1，r不变，依然成立
![](img/DM/7-15.png)

{% callout success::Definition %} 
Suppose R is a region of a connected planar simple graph, the number of the edges on the boundary of R is called the Degree of R .
记作$Deg(R)$
{% endcallout %}
![](img/DM/7-16.png)
注意，准确地说，区域的度的定义应该是走完区域的边界（从一个顶点开始走回这个顶点），每走一条边，度+1，所以R1的度应该是12而不是8

{% callout warning::Corollary 1 %} 
If $G$ is a connected planar simple graph with $e$ edges and $v$ vertices where $v\ge 3$, then $e\le 3v-6$ 
{% endcallout %}

假设这个图被分为r个区域，由于图是简单图、连通，每个区域的度至少是3，度是1说明自环，度是2要么重边，要么就是从a到b再从b到a，但至少三个顶点且连通，这种情况是不可能的

事实上，三个顶点的情况，要么三角形，要么两条边三个顶点（这时只有一个区域，度为4）
$2e=\sum deg(R_i) \ge 3r$
利用Euler公式即可得到$e\le 3v-6$ 
对不连通的图也适用，因为每一个连通分量有这条性质

{% callout warning::Corollary 2 %} 
If $G$ is a connected planar simple graph, then $G$ has a vertex of degree not exceeding 5
{% endcallout %}
证明：如果G只有一个或两个顶点，成立
如果G有超过三个顶点，如果每个顶点的度都大于5，$2e\ge 6v$
但由推论1，$2e\le 6v-12$，矛盾

{% callout warning::Corollary 3 %} 
If a connected planar simple graph has $e$ edges 
and $v$ vertices with $v\ge 3$ and no circuits of length 3,then $e \le 2v-4$.

{% endcallout %}
如果一个region的度数为3，只能是一个闭合的三角形，所以每个region的度数大于等于4，根据推论1的做法即可得证

更一般地，如果每个区域都至少有k个边，他们的度数一定大于等于k，可以得到$e\le \frac{(v-2)k}{k-2}$

有了Euler公式，我们可以判断一个图是不是planar的，因为Euler公式是一个图是planar的必要条件
例如，对于$K_5$，它有5个顶点，10条边，不满足$e\le 3v-6$
对于$K_{3,3}$，它有6个顶点，9条边，由于是二分图，它没有回路，所以可以用推论3验证，不满足$e\le 2v-4$

## 7.2 Kuratowski's Theorem
{% callout success::Definition %} 
The graph $G_1=(V_1,E_1)$ and $G_2=(V_2,E_2)$ are called homeomorphic(同胚) if they can be obtained from the same graph by a sequence of elementary subdivision.

一次elementary subdivision指在一条边中间插入一个度为2的顶点
{% endcallout %}

{% callout info::Kuratowski's Theorem %} 
A graph is nonplanar if and only if it contains a subgraph homeomorphic to $K_{3,3}$ or $K_5$.
{% endcallout %}
![](img/DM/7-17.png)
![](img/DM/7-18.png)

有点考验注意力了

# 8 Graph Coloring
> 我有种自己很强的感觉，因为我在学习染色的问题

一个平面上的地图可以用图表示，这个图称为the dual graph of the map

地图的每个区域用一个顶点表示，如果两个区域相邻（有共同边界），就在这两个顶点连上一条边

![](img/DM/7-19.png)

coloring: 对图的顶点染色，使得相邻的顶点颜色不同

The chromatic number of a graph
is the least number of colors needed for a coloring of this graph,  denoted by $\chi (G)$
{% callout info::The Four Color Theorem %} 
The chromatic number of a planar graph is no greater than four.
{% endcallout %}

我们看一些例子，比如对于$C_n$，如果n为偶数，那么两个颜色就够了，如果n是奇数，比如三角形，那么需要三种颜色
对于$K_n$，由于一个顶点跟n-1个顶点都连着，所以需要n个颜色

由2.2节的定理，
A simple graph with a chromatic number of 2 is bipartite.

A connected bipartite graph has a chromatic number of 2. 

{% callout primary::Example %} 
证明六色定理

证明：
对图的顶点数n进行归纳
若只有一个顶点，n=1，成立
假设n=k成立，即k个顶点的planar graph的chromatic number小于等于6
现在加入一个顶点，由之前的推论，n=k+1的图中有一个顶点$i$的度小于等于5，把$i$先移除，这时这个图依然只有k个顶点，chromatic number小于等于6
这时再把顶点$i$放回来，最坏的情况，$i$会连上5个颜色不同的顶点，但我们有6种颜色，所以依然可以染色，证明完毕
{% endcallout %}

{% folding title="Challenge" class="purple" open=false %}
证明五色定理

证明：
归纳法，大部分过程和六色定理的证明一样，但是，对于$i$，如果它加回去之后连了5个颜色不同的顶点，这种情况最坏
我们将与它相邻的五个顶点v1,v2,v3,v4,v5的颜色，按顺时针命名为1,2,3,4,5
我们考虑所有染成颜色1或3的连通分量，如果v1和v3不在同一个连通分量中，把v1所在的连通分量里的颜色1和3互换，这样$i$就可以染上颜色1

如果1和3在同一个连通分量里，那么对v2，v4进行同样操作，它们不可能在同一个分量里（因为planar），所以$i$可以被成功染色

证明完毕
![](img/DM/7-20.png)
{% endfolding %}