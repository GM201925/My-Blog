---
title: Chapter 7 - Graph
date: 2026-04-25 21:00:59
categories: Data Structure
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

# 1 Definition
{% callout success::Definition %} 
图(Graph)由顶点集合V和边集合E构成，其中顶点集不能为空，每条边一定要有两个端点
想象一个三角形ABC，这个图的顶点集合就是{A,B,C}
边集合就是{AB,AC,BC}

**Undirected Graph**: 边AB代表着A可以到B，B也可以到A
**Directed Graph**: 边A->B和边B->A是两条边
无向图的边用圆括号表示，如(A,B)
有向图的边用尖括号表示，如<A,B>，代表从A指向B，它不等于<B,A>

图的边不会重复，比如对有向图，从A到B就只有一条边，也不会出现自己指向自己的边

**Complete graph(完全图)**:  a graph that has the maximum number of edges
对于$n$个顶点的有向完全图，有$n(n-1)$条边，或说$n$个顶点的有向图最多有$n(n-1)$条边
对于$n$个顶点的无向完全图，有$C_n^2$条边

{% endcallout %}
无图中，两个顶点间有条边，称$v_i$ and $v_j$ are adjacent 
有向图中，从$v_i$指向$v_j$，称$v_i$ is adjacent to $v_j$ ; $v_j$ is adjacent from $v_i$ 

**Degree(度)**: number of edges incident to v.For a directed G, we have in-degree and out-degree.

对于无向图，一条边贡献两个度，所以各顶点度之和等于边数乘2
对于有向图，一条边贡献一个出度一个入度，各顶点出度之和等于各顶点入度之和等于图的总边数，同时各顶点度之和等于边数乘2

**Path from $v_p$ to $v_q$** ::= { $v_p$, $v_{i1}$, $v_{i2}$, ..., $v_{in}$, $v_q$ } such that ( $v_p$, $v_{i1}$ ), ( $v_{i1}$, $v_{i2}$ ), ..., ( $v_{in}$, $v_q$ ) or < $v_p$, $v_{i1}$ >, ..., < $v_{in}$, $v_q$> belong to E( G ) 

**Length of a path** :  number of edges on the path

**Simple path** : 走过的顶点不重复
**Cycle** ::= simple path with $v_p$ = $v_q$ 
**Subgraph**: 图的一部分，它仍是一个图

$v_i$ and $v_j$ in an undirected G are **connected** if there is a path from $v_i$ to $v_j$ (and hence there is also a path from $v_j$ to $v_i$)

An undirected graph G is connected if every pair of distinct vi and vj are connected

**(Connected) Component of an undirected G** ::= the maximal connected subgraph
![](img/DS/8-1.png)
图来自b站up主蓝不过海牙的视频，up主简介里写禁止搬运/二改，不知是否可以截图用于非商用笔记，若有侵权我会立刻删除！
[视频链接]("https://www.bilibili.com/video/BV12QASzrErF/?spm_id_from=333.337.search-card.all.click&vd_source=308caf317dcb3e0468fc18c3550b8b6a")
图中单选BE也是连通图，但不是最大，如果选CBEA，就不连通了，连通图就只有自己这一个连通分量

**Strongly connected directed graph G** ::= for every pair of vi and vj in V( G ), there exist directed paths from vi to vj and from vj to vi.  If the graph is connected without direction to the edges, then it is said to be **weakly connected**

Strongly connected component ::=  the maximal subgraph that is strongly connected

对无向图，n个顶点的连通图至少要n-1条边（一排）
有向图，n个顶点的强连通图至少要n条边
![](img/DS/8-2.png)
图来自b站up主蓝不过海牙的视频，up主简介里写禁止搬运/二改，不知是否可以截图用于非商用笔记，若有侵权我会立刻删除！
[视频链接]("https://www.bilibili.com/video/BV12QASzrErF/?spm_id_from=333.337.search-card.all.click&vd_source=308caf317dcb3e0468fc18c3550b8b6a")

# 2 Representation of Graphs

## 2.1 Adjacency Matrix
用一个二维数组来存储图，
``a[i][j]=1``表示i邻接到j
``a[j][i]=1``表示i邻接自j
可见对无向图，这个矩阵是对称的，我们可以把下三角矩阵一行一行遍历，存成一个一维数组

优点：
- 容易判断两个顶点是否邻接
- 容易计算顶点的度，对行求和得到出度，对列求和得到入度，对于无向图，对行/列任一求和得到度，$O(N)$

缺点：
- 统计边数量很难，要遍历整个矩阵，$O(N^2)$
- 空间复杂度高

```c
/*以下代码由ai生成*/

#define MAXV 100    // 最大顶点数，根据题目要求调整
#define INF 0x3f3f3f3f  // 表示无穷大（带权图无边时用）
// 顶点数据类型，根据题目修改（这里用int）
typedef int VertexType;
// 边权值类型，根据题目修改（这里用int）
typedef int WeightType;

// 邻接矩阵表示的图
typedef struct {
    VertexType vex[MAXV];       // 顶点表
    WeightType edge[MAXV][MAXV];// 邻接矩阵（边表）
    int numVex;                 // 顶点数
    int numEdge;                // 边数
} MGraph;

// 初始化一个有n个顶点的空图
void InitMGraph(MGraph* G, int n) {
    G->numVex = n;
    G->numEdge = 0;
    
    // 初始化顶点表
    for (int i = 0; i < n; i++) {
        G->vex[i] = i;  // 默认顶点编号为0,1,2,...,n-1
    }
    
    // 初始化邻接矩阵
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            if (i == j)
                G->edge[i][j] = 0;  // 自己到自己的距离为0
            else
                G->edge[i][j] = INF; // 无边时为无穷大
            //不带权值，赋成0就行了
        }
    }
}

// 添加一条从v到u的边，权值为w
// 无向图需要调用两次：AddMEdge(G, v, u, w) 和 AddMEdge(G, u, v, w)
void AddMEdge(MGraph *G, int v, int u, WeightType w) {
    // 边界检查
    if (v < 0 || v >= G->numVex || u < 0 || u >= G->numVex) {
        printf("顶点编号错误！\n");
        return;
    }
    
    G->edge[v][u] = w;
    G->numEdge++;
}

// 简化版：添加无权图的边（权值默认为1）
void AddMEdgeUnweighted(MGraph *G, int v, int u) {
    AddMEdge(G, v, u, 1);
}
```
## 2.2 Adjacency Lists
![](img/DS/8-3.png)
对于无向图，空间$S=n$ heads + $2e$ nodes
对于无向图，一个顶点的度就是它这个链表的结点数
有向图遍历所有边的时间复杂度是$O(n+e)$，无向图是$O(n+2e)$

有向图我们需要知道入度，所以需要**Inverse Adjacency List**，每个顶点存指向它的边，整体和上面的表一样

```c
/*以下代码由ai生成*/

// 边节点
typedef struct EdgeNode {
    int adjvex;               // 邻接点的下标
    WeightType weight;        // 边的权值（无权图可省略，但建议保留）
    struct EdgeNode *next;    // 指向下一个边节点
} EdgeNode;

// 顶点节点
typedef struct VertexNode {
    VertexType data;          // 顶点的数据
    EdgeNode *firstedge;      // 指向第一个边节点
} VertexNode, AdjList[MAXV];
//

// 邻接表表示的图
typedef struct {
    AdjList adjlist;          // adjlist是长度为 MAXV 的 VertexNode 数组类型
    int numVex;               // 顶点数
    int numEdge;              // 边数
} ALGraph;

// 初始化一个有n个顶点的空图
void InitALGraph(ALGraph *G, int n) {
    G->numVex = n;
    G->numEdge = 0;
    
    // 初始化顶点表
    for (int i = 0; i < n; i++) {
        G->adjlist[i].data = i;  // 默认顶点编号为0,1,2,...,n-1
        G->adjlist[i].firstedge = NULL;  // 关键：初始化为空指针
    }
}

// 添加一条从v到u的边，权值为w
// 无向图需要调用两次：AddALEdge(G, v, u, w) 和 AddALEdge(G, u, v, w)
void AddALEdge(ALGraph *G, int v, int u, WeightType w) {
    // 边界检查
    if (v < 0 || v >= G->numVex || u < 0 || u >= G->numVex) {
        printf("顶点编号错误！\n");
        return;
    }
    
    // 创建新的边节点
    EdgeNode *newEdge = (EdgeNode *)malloc(sizeof(EdgeNode));
    newEdge->adjvex = u;
    newEdge->weight = w;
    // 头插法：插入到链表头部
    newEdge->next = G->adjlist[v].firstedge;
    G->adjlist[v].firstedge = newEdge;
    
    G->numEdge++;
}

// 简化版：添加无权图的边（权值默认为1）
void AddALEdgeUnweighted(ALGraph *G, int v, int u) {
    AddALEdge(G, v, u, 1);
}

void DestroyALGraph(ALGraph *G) {
    for (int i = 0; i < G->numVex; i++) {
        EdgeNode *p = G->adjlist[i].firstedge;
        while (p != NULL) {
            EdgeNode *temp = p;
            p = p->next;
            free(temp);
        }
        G->adjlist[i].firstedge = NULL;
    }
    G->numVex = 0;
    G->numEdge = 0;
}
```

对于有向图，同时表示出度和入度，还可以使用十字链表
(画图耗时间，但莫名地喜欢)
![](img/DS/8-4.png)
对于B，“竖着”的部分就代表入度，“横着”的部分代表出度，我们采用头插法，所以不用担心不知道什么时候判断出/入的边统计完了

代码实现就挖坑罢（

## 2.3 Adjacency Multilists
![](img/DS/8-5.png)
适用于无向图，如果要标记某条边会方便些，好吧其实我现在也不清楚它的应用
右边的块是边，每个块有这个边的两个结点和两个指针，如第一个结构，0的指针指向下一个有0的边，1的指针同理

# 3 AOV and Topological Order
引入：
顶点：表示一门课程（活动）
有向边 <C1, C3>：表示 C1 是 C3 的先修课（C1 必须在 C3 之前完成）
比如：
你必须先学《高等数学》，才能学《线性代数》
你必须先学《C 语言》，才能学《数据结构》

**AOV 网（Activity On Vertex Network）**：
顶点表示活动（Activity），有向边表示活动之间的前驱关系的有向图。
边 <i, j> 表示：活动 i 是活动 j 的直接前驱(immediate predecessor)，活动 j 必须在活动 i 完成后才能开始
活动 i 是活动 j 的前驱(predecessor)：存在一条从 i 到 j 的路径（不一定是直接边）
活动 j 是活动 i 的后继(successor)：对应前驱的反向关系

**偏序关系(Partial Order)** = 传递性 + 反自反性
可行的 AOV 网一定是 DAG（**有向无环图 directed acyclic graph**）

{% callout success::Definition %} 
A **topological order (拓扑排序)** is a linear ordering of the vertices of a graph such that, for any two vertices, i, j, if i is a predecessor of j in the network then i precedes j in the linear ordering.

The topological orders may not be unique for a  network.  
{% endcallout %}

算法就是每次找到入度为0的点（当前优先级最高），删除这个顶点和它的所有（出去的）边，然后重复步骤直到AOV网中没有入度为0的顶点或AOV网中没有顶点
```
void Topsort(Graph G) {
    int Counter;
    Vertex V, W;
    for (Counter = 0; Counter < NumVertex; Counter++) {
        // 每次遍历所有顶点，找一个入度为0的顶点
        V = FindNewVertexOfDegreeZero();
        if (V == NotAVertex) {
            Error("Graph has a cycle"); // 找不到入度为0的顶点，说明有环
            break;
        }
        TopNum[V] = Counter; // 记录拓扑序号
        // 删除V的所有出边：所有邻接点的入度减1
        for (each W adjacent from V)
            Indegree[W]--;
    }
}
```
每次找入度为 0 的顶点都要遍历所有 n 个顶点，总时间复杂度 O (n²)，效率低。

改进：
1. 计算所有顶点的入度，将入度为 0 的顶点入队(或用stack也可以，那么就是push)
2. 当队列不为空时：
出队一个顶点 V，记录它的拓扑序号
遍历 V 的所有邻接点 W，将 W 的入度减 1
如果 W 的入度变为 0，将 W 入队
3. 如果最终记录的顶点数小于总顶点数，说明图中有环

```
void Topsort(Graph G) {
    Queue Q;
    int Counter = 0;
    Vertex V, W;
    
    // 1. 创建并初始化队列
    Q = CreateQueue(NumVertex);
    MakeEmpty(Q);
    
    // 2. 遍历所有顶点，将入度为0的顶点入队
    for (each vertex V)
        if (Indegree[V] == 0)
            Enqueue(V, Q);
    
    // 3. 处理队列中的顶点
    while (!IsEmpty(Q)) {
        V = Dequeue(Q);
        TopNum[V] = ++Counter; // 分配拓扑序号（从1开始）
        
        // 遍历V的所有邻接点，入度减1
        for (each W adjacent from V)
            if (--Indegree[W] == 0)
                Enqueue(W, Q);
    }
    
    // 4. 判断是否有环
    if (Counter != NumVertex)
        Error("Graph has a cycle");
    
    DisposeQueue(Q); // 释放队列内存
}
```
初始计算每个点的入度，要遍历整个图，$O(n+e)$
删除当前入度为0的点的边，只要AOV网能进行拓扑排序，所有顶点都会经历，$O(e)$，每个点还会入队/出队，$O(n)$
时间复杂度为$O(n+e)$

下面以邻接表为例实现
```c
// 边节点
typedef struct EdgeNode {
    int adjvex;               // 邻接点的下标
    WeightType weight;        // 边的权值（无权图可省略，但建议保留）
    struct EdgeNode *next;    // 指向下一个边节点
} EdgeNode;

// 顶点节点
typedef struct VertexNode {
    VertexType data;          // 顶点的数据
    EdgeNode *firstedge;      // 指向第一个边节点
} VertexNode, AdjList[MAXV];
//

// 邻接表表示的图
typedef struct {
    AdjList adjlist;          // adjlist是长度为 MAXV 的 VertexNode 数组类型
    int numVex;               // 顶点数
    int numEdge;              // 边数
} ALGraph;

typedef struct {
    int data[MAXV];
    int front, rear;
} Queue;

// 初始化队列
void InitQueue(Queue *Q) {
    Q->front = Q->rear = 0;
}

// 判断队列是否为空
int IsEmpty(Queue *Q) {
    return Q->front == Q->rear;
}

// 入队
void Enqueue(Queue *Q, int v) {
    Q->data[Q->rear++] = v;
}

// 出队
int Dequeue(Queue *Q) {
    return Q->data[Q->front++];
}


void CalcInDegree(ALGraph *G, int Indegree[]) {
    for(int i = 0;i<G->numVex;++i)
    {
        Indegree[i] = 0;
    } //初始化

    for(int i = 0;i<G->numVex;++i)
    {
        EdgeNode* p = G->adjlist[i].firstedge;
        while(p!=NULL)
        {
            Indgree[p->adjvex]++;
            p = p->next;
        }
    }

}

int Topsort(ALGraph *G, int TopNum[]) {
    Queue q;
    int Indegree[MAXV];
    int count = 0; // 多少个顶点已排序
    InitQueue(&q);
    CalcInDegree(G, Indegree);
    int v = 0;
    int tmp = 0;
    for(v = 0;v<G->numVex;++v)
    {
        if(Indegree[v] == 0)
        {
            Enqueue(&q,v);
        }
    }

    while(!IsEmpty(&q))
    {
        v = Dequeue(&q);
        TopNum[count++] = v;
        EdgeNode* p = G->adjlist[v].firstedge;
        while(p!=NULL)
        {
            // 删除该顶点所有边
            tmp = p->adjvex;
            if(--Indegree[tmp] == 0)
            {
                Enqueue(&q,tmp);
            }
            p = p->next;
        }
    }

    if(count != G->numVex)
    {
        puts("Not DAG, there exists a circle !");
        return 0;
    }
    return 1;
}
```