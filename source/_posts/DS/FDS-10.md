---
title: Chapter 9 - Network Flow & MST
date: 2026-05-11 20:54:15
categories: Data Structure
mathjax: true
cover: img/test.png

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

# 1 Network Flow Problem
![](img/DS/10-1.png)
从s到t，有若干管道，每个管道的数字代表每秒流过的最大容量，如果每秒有5立方米的水从s流出，每秒从s到t流过的最大量是多少（不考虑水流经管道的时间）？
左边流出3，这个3在a处流一份到d，这样总共就能流出5，最大

一个简单的算法如图所示，每次找到一条从s到t的路径(不能有环)(***增广路径 augmenting path***)，然后用最小容量确定这条路能流多少水，然后在residual path的管道的“余量”减去对应的值，然后重复进行直到找不到从s到t的路径

用最左边的图减去最右边的图就得到Flow的图，即中间的图，就是结果

然而，这种算法有时会失败，如果我们先找了`s->a->d->t`这条路，算法直接终止，而这并不是最大流，而是阻塞流，这个算法的问题就在于无法“反悔”，如果优先找到了不是最大流的阻塞流，算法就会失效

## 1.1 Ford-Fulkerson Algorithm
![](img/DS/10-2.png)
注意我们要多出来一个操作，找到一条路径后，例如`s->a->d->t`，我们首先在residual graph里减去这条路的最小容量3，然后添加反向路径`t->d,d->a,a->s`，反向路径大小为3

算法结束后，忽略residual graph里的反向路径，就得到了residual graph，原图减去剩余就是流的图。

当然也可以使用Flow的图，走一条路就画在Flow的图上，但最后要减去反向的路径(如a到d是3-2=1)，从上面的Flow图也可看出到底“反悔(Undo)”是怎么实现的，我们从d到a那条其实就是反悔之前直接从a到d流了3份的操作，改为流一份。
![](img/DS/10-3.png)
如图所示，每次找路径都会让流量至少增加1，所以至少找f次路径，每次找路径复杂度为$O(E)$ (一般使用DFS)，所以时间复杂度为$O(f\cdot E)$

## 1.2 Edmonds-Karp Algorithm
跟上面相比只是规定了找路径的方式，从而得到找路径次数的上界。找路径时把图看作无权图，使用BFS找最短路径，其他和上面的算法一样，找路径复杂度为$O(V+E)=O(E),(V-1\le E)$，可以证明最多循环$O(VE)$次
复杂度为$O(E^2V)$

```c
/*以下代码由ai生成*/
#include <stdio.h>
#include <string.h>
#include <stdbool.h>

#define MAX_V 100
#define INF 1e9

int capacity[MAX_V][MAX_V]; // 残余网络：存储每条边的剩余容量
int parent[MAX_V];          // 用于记录 BFS 路径
int n;                      // 顶点数

// 使用 BFS 寻找增广路径
bool bfs(int s, int t) {
    bool visited[MAX_V];
    memset(visited, 0, sizeof(visited));
    
    int queue[MAX_V];
    int head = 0, tail = 0;
    
    queue[tail++] = s;
    visited[s] = true;
    parent[s] = -1;
    
    while (head < tail) {
        int u = queue[head++];
        
        for (int v = 0; v < n; v++) {
            // 如果 v 没访问过，且边 (u, v) 还有剩余容量
            if (!visited[v] && capacity[u][v] > 0) {
                if (v == t) {
                    parent[v] = u;
                    return true; // 找到一条通往汇点的增广路
                }
                queue[tail++] = v;
                parent[v] = u;
                visited[v] = true;
            }
        }
    }
    return false;
}

// Edmonds-Karp 算法主函数
int edmondsKarp(int s, int t) {
    int max_flow = 0;
    
    // 只要残余网络中还存在增广路
    while (bfs(s, t)) {
        // 1. 找到这条路径上的“瓶颈”容量（最小容量）
        int path_flow = INF;
        for (int v = t; v != s; v = parent[v]) {
            int u = parent[v];
            if (capacity[u][v] < path_flow) {
                path_flow = capacity[u][v];
            }
        }
        
        // 2. 更新残余网络
        for (int v = t; v != s; v = parent[v]) {
            int u = parent[v];
            capacity[u][v] -= path_flow; // 正向边减去流量
            capacity[v][u] += path_flow; // 反向边加上流量（允许“反悔”的机制）
        }
        
        max_flow += path_flow;
    }
    
    return max_flow;
}

int main() {
    // 示例图：n个点，设置容量
    
    n = 6;
    memset(capacity, 0, sizeof(capacity));
    
    // 假设添加边：source s=0, sink t=5
    capacity[0][1] = 16; capacity[0][2] = 13;
    capacity[1][2] = 10; capacity[1][3] = 12;
    capacity[2][1] = 4;  capacity[2][4] = 14;
    capacity[3][2] = 9;  capacity[3][5] = 20;
    capacity[4][3] = 7;  capacity[4][5] = 4;

    printf("该网络的最大流量为: %d\n", edmondsKarp(0, 5));
    
    return 0;
}
```

# 2 Minimum Spanning Tree
![](img/DS/10-4.png)
生成树有$V$个顶点且无环，有$V-1$条边，最小生成树就是边权之和最小的生成树

## 2.1 Prim's Algorithm
跟Dijsktra算法很像，先选一个源点，加入集合（这个集合存储着在生成树里的点），然后更新邻接的边权，接着再选出到集合里的点边权最小的点，重复操作
```c
/*以下代码由ai生成*/
#include <stdio.h>
#include <stdlib.h>
#include <limits.h>

#define Infinity INT_MAX
#define NotAVertex -1
#define MaxVertexNum 100

// 完全复用 Dijkstra 的 Table 结构体
typedef struct {
    int Dist;    // 关键！Prim中：顶点到已选集合的最小边权
    int Known;   // 是否已加入生成树
    int Path;    // 生成树中该顶点的父节点（用于输出边）
} TableEntry;

typedef TableEntry Table[MaxVertexNum];
typedef int Vertex;
typedef int Weight;

// 邻接表表示图（和 Dijkstra 完全一样）
typedef struct AdjNode {
    Vertex adjvex;
    Weight weight;
    struct AdjNode *next;
} AdjNode;

typedef struct {
    AdjNode *head[MaxVertexNum];
    int vertexNum;
    int edgeNum;
} Graph;

// 初始化Table数组（和 Dijkstra 几乎一样）
void InitTable(Vertex start, Graph *G, Table T) {
    for (int i = 0; i < G->vertexNum; i++) {
        T[i].Dist = Infinity;
        T[i].Known = 0;
        T[i].Path = NotAVertex;
    }
    T[start].Dist = 0;  // 从任意顶点开始（通常选0号顶点）
}

// 找Dist最小的未处理顶点（和 Dijkstra 完全一样！）
Vertex FindMinDist(Table T, Graph *G) {
    int minDist = Infinity;
    Vertex minV = NotAVertex;
    for (int i = 0; i < G->vertexNum; i++) {
        if (!T[i].Known && T[i].Dist < minDist) {
            minDist = T[i].Dist;
            minV = i;
        }
    }
    return minV;
}

// 数组版 Prim 算法（教材标准实现）
int Prim_Basic(Graph *G, Vertex start, Table T) {
    InitTable(start, G, T);
    Vertex V, W;
    AdjNode *p;
    int totalWeight = 0;  // 生成树总权值

    for (int i = 0; i < G->vertexNum; i++) {  // 共选V个顶点
        // 步骤1：找距离已选集合最近的顶点（和 Dijkstra 完全一样）
        V = FindMinDist(T, G);
        if (V == NotAVertex) {
            printf("图不连通，无最小生成树\n");
            return -1;
        }

        // 步骤2：将V加入生成树
        T[V].Known = 1;
        totalWeight += T[V].Dist;  // 累加边权（注意：源点的Dist是0，不影响）

        // 步骤3：更新所有邻接点到已选集合的最小边权
        p = G->head[V];
        while (p != NULL) {
            W = p->adjvex;
            if (!T[W].Known) {
                // ✅ 唯一和 Dijkstra 不同的一行！
                // Dijkstra: if (T[V].Dist + p->weight < T[W].Dist)
                if (p->weight < T[W].Dist) {
                    T[W].Dist = p->weight;
                    T[W].Path = V;  // 记录父节点，用于输出边
                }
            }
            p = p->next;
        }
    }

    return totalWeight;
}
```
![](img/DS/10-5.png)
如图，我们从A开始，A的dist一开始是0，其他边的dist都是无穷大
首先A加入集合，即known=1，然后更新AD,AB距离
接着继续找，找到D最近，D加入树，更新CD距离
接着再找到A或D最近且不在树里的点，是B（注意这里dist的意义，是到当前生成树里的各点最近的距离，所以更新时只要边权小于Dist就直接更新）
最后找距离当前生成树各点最近的点，是C，C距离B为3最近，这时所有点都选完了，算法停止，得到最小生成树

外层循环执行 | V | 次（每个顶点处理 1 次）
每次FindMinDist是 O (|V|)
所有边总共被处理 1 次（O (|E|)）
总时间：
$O(∣V∣^2+∣E∣)=O(∣V∣^2)$（稠密图中 | E|≈|V|²，这个复杂度是最优的）

## 2.2 Kruskal's Algorithm
Kruskal 是基于边的贪心 —— 把所有边按权值从小到大排序，依次加入生成树，只要不形成环就保留
```C
void Kruskal ( Graph G )
{   T = { } ;
    while  ( T contains less than |V| - 1 edges 
                   && E is not empty ) {
        choose a least cost edge (v, w) from E ;
        delete (v, w) from E ;
        if  ( (v, w) does not create a cycle in T )     
	add (v, w) to T ;
        else     
	discard (v, w) ;
    }
    if  ( T contains fewer than |V| - 1 edges )
        Error ( “No spanning tree” ) ;
}
```
我们以上面的图片为例，首先选AD边，然后选AB边，然后选BC边，这时已经有三条边了，结束
时间复杂度取决于对所有边的排序，后面我们会学习排序，为$O(ElogE)$，该算法更适合稀疏图

```c
/*以下代码由ai生成*/

#include <stdio.h>
#include <stdlib.h>
#include <limits.h>

#define MaxVertexNum 100
#define MaxEdgeNum 10000

typedef int Vertex;
typedef int Weight;

// 边的结构体（Kruskal需要把所有边单独存起来排序）
typedef struct {
    Vertex u;
    Vertex v;
    Weight weight;
} Edge;

// 图的结构体（Kruskal只需要存所有边）
typedef struct {
    Edge edges[MaxEdgeNum];
    int vertexNum;
    int edgeNum;
} Graph;

// 并查集数组
int parent[MaxVertexNum];
int rank[MaxVertexNum];

// 并查集初始化
void InitUnionFind(int n) {
    for (int i = 0; i < n; i++) {
        parent[i] = i;
        rank[i] = 0;
    }
}

// 查找根节点（路径压缩）
int Find(int v) {
    if (parent[v] != v) {
        parent[v] = Find(parent[v]);
    }
    return parent[v];
}

// 合并集合（按秩合并）
void Union(int v, int w) {
    int rootV = Find(v);
    int rootW = Find(w);
    if (rootV == rootW) return;
    if (rank[rootV] < rank[rootW]) {
        parent[rootV] = rootW;
    } else {
        parent[rootW] = rootV;
        if (rank[rootV] == rank[rootW]) {
            rank[rootV]++;
        }
    }
}

// 边的比较函数（用于qsort排序）
int CompareEdge(const void *a, const void *b) {
    Edge *edgeA = (Edge *)a;
    Edge *edgeB = (Edge *)b;
    return edgeA->weight - edgeB->weight;
}

// Kruskal算法核心实现
int Kruskal(Graph *G, Edge *mstEdges) {
    int totalWeight = 0;
    int mstEdgeCount = 0;

    // 步骤1：初始化并查集
    InitUnionFind(G->vertexNum);

    // 步骤2：将所有边按权值从小到大排序
    qsort(G->edges, G->edgeNum, sizeof(Edge), CompareEdge);

    // 步骤3：依次处理每条边
    for (int i = 0; i < G->edgeNum && mstEdgeCount < G->vertexNum - 1; i++) {
        // 前提：边数够用而且边数还没到达V-1
        Edge e = G->edges[i];
        int rootU = Find(e.u);
        int rootV = Find(e.v);

        // 如果不在同一个集合，加入生成树，否则在一个集合里说明已经连通，再加这条边就会构成环
        if (rootU != rootV) {
            mstEdges[mstEdgeCount++] = e;
            totalWeight += e.weight;
            Union(rootU, rootV);
        }
    }

    // 检查是否生成了完整的生成树
    if (mstEdgeCount < G->vertexNum - 1) {
        printf("图不连通，无最小生成树\n");
        return -1;
    }

    return totalWeight;
}

// 输出生成树的边
void PrintMST(Edge *mstEdges, int vertexNum, int totalWeight) {
    printf("最小生成树总权值：%d\n", totalWeight);
    printf("生成树边：\n");
    for (int i = 0; i < vertexNum - 1; i++) {
        printf("v%d -- v%d (权值：%d)\n",
               mstEdges[i].u + 1, mstEdges[i].v + 1, mstEdges[i].weight);
    }
}
```