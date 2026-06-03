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
