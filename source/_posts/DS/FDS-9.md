---
title: Chapter 8 - Graph-Shortest Paths
date: 2026-05-04 15:12:07
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

# 1 Shortest Paths
![](img/DS/9-1.png)
如果图中存在负权环（环上所有边的代价之和为负数），则最短路径不存在 —— 因为可以绕环无限次，让路径长度无限减小。
若图中无负权环，定义源点到自身的最短路径长度为 0。

![](img/DS/9-2.png)
```
void Unweighted(Table T) {
    int CurrDist;
    Vertex V, W;
    // 按距离从小到大处理所有顶点
    for (CurrDist = 0; CurrDist < NumVertex; CurrDist++) {
        // 找到所有距离为CurrDist且未处理的顶点
        for (each vertex V) {
            if (!T[V].Known && T[V].Dist == CurrDist) {
                T[V].Known = true;
                // 更新邻接点的距离
                for (each W adjacent to V) {
                    if (T[W].Dist == Infinity) {
                        T[W].Dist = CurrDist + 1;
                        T[W].Path = V;
                    }
                }
            }
        }
    }
}
```

初始状态：T[v3].Dist = 0，其他顶点的 Dist = Infinity，所有 Known = false。
CurrDist = 0：
中间循环扫所有顶点，找到 Dist=0 且 Known=false 的 v3。
把 T[v3].Known 设为 true。
看 v3 的邻接点（比如 v1、v6），它们的 Dist 是 Infinity，所以：
T[v1].Dist = 0+1 = 1，T[v1].Path = v3；
T[v6].Dist = 0+1 = 1，T[v6].Path = v3。
CurrDist = 1：
中间循环扫所有顶点，找到 Dist=1 且 Known=false 的 v1 和 v6。
先处理 v1：标记为 Known=true，看它的邻接点 v2、v4，把它们的 Dist 设为 1+1=2，Path 设为 v1。
再处理 v6：同理标记并更新它的邻接点。
CurrDist = 2：
找到 v2、v4，处理它们的邻接点 v5、v7，把距离设为 3。

**CurrDist**其实就代表了是**第几层**，本质上就是广度优先搜索

但是对于一条链，比如
$v_1\rightarrow v_2\rightarrow v_3\rightarrow ... \rightarrow v_n$
找dist为0的点，遍历一遍，找到$v_1$
找dist为1的点，遍历一遍，找到$v_2$
找dist为2的点，遍历一遍，找到$v_3$
...
所以复杂度是$O(n^2)$，太大了

优化：使用队列，对于每一层的每个顶点，把它的下一层，即它邻接的点入队
```
void Unweighted(Table T) {
    Queue Q;
    Vertex V, W;
    Q = CreateQueue(NumVertex);
    MakeEmpty(Q);
    Enqueue(S, Q); // 源点入队，源点的Dist是0

    while (!IsEmpty(Q)) {
        V = Dequeue(Q);
        T[V].Known = true; // 此标记非必需，因为每个顶点仅入队一次
        // 更新所有未访问的邻接点
        for (each W adjacent to V) {
            if (T[W].Dist == Infinity) {
                T[W].Dist = T[V].Dist + 1;
                T[W].Path = V;
                Enqueue(W, Q);
            }
        }
    }
    DisposeQueue(Q);
}
```
时间复杂度为$O(∣V∣+∣E∣)$（每个顶点入队 / 出队 1 次，每条边被处理 1 次）

# 2 Dijkstra's Algorithm
这是最经典的带权图最短路径算法，基于**贪心策略**，要求**所有边的权值非负**。

- 定义集合 $S$：包含源点 $s$ 和所有已经找到最短路径的顶点。
- 对于不在 $S$ 中的顶点 $u$，定义 `distance[u]`：从 $s$ 出发，**只经过 $S$ 中的顶点**到达 $u$ 的最短路径长度。
- 算法步骤：
  1. 初始化 $S=\{s\}$，`distance[s]=0`，其他顶点为无穷大。
  2. 重复直到 $S$ 包含所有顶点：
     - 选择不在 $S$ 中且 `distance` 最小的顶点 $u$，加入 $S$（贪心选择）。
     - 对 $u$ 的每个邻接点 $v$，更新 `distance[v] = min(distance[v], distance[u]+c(u,v))`。

**证明**
假设选择 $u$ 加入 $S$ 时，`distance[u]` 其实不是 $s$ 到 $u$ 的最短路径长度。那么存在一条更短的路径 $s \to \dots \to w \to \dots \to u$，其中 $w$ 不在 $S$ 中，否则这条路径长度肯定小于等于 `distance[u]`。此时 `distance[w] < distance[u]`，与"$u$ 是不在 $S$ 中距离最小的顶点"矛盾。因此贪心选择是正确的。

**伪代码实现**
```c
void Dijkstra(Table T) {
    Vertex V, W;
    for (;;) {
        // 找到距离最小的未处理顶点
        V = smallest unknown distance vertex;
        if (V == NotAVertex) break; // 所有顶点处理完毕

        T[V].Known = true;
        // 更新所有邻接点的距离
        for (each W adjacent to V) {
            if (!T[W].Known) {
                if (T[V].Dist + Cvw < T[W].Dist) {
                    T[W].Dist = T[V].Dist + Cvw;
                    T[W].Path = V;
                }
            }
        }
    }
}
```

例如
带权图结构：
![](img/DS/9-3.png)

算法执行过程：
1. 初始：v1.Dist=0，其他为∞。选v1加入S，更新v2.Dist=2，v4.Dist=1。
2. 选v4（最小未处理）加入S，更新v3.Dist=3，v5.Dist=3，v6.Dist=9，v7.Dist=5。
3. 选v2（最小未处理）加入S，v5当前Dist=3 < 2+10=12，不更新。
4. 选v3（最小未处理）加入S，更新v6.Dist=min(9,3+5)=8。
5. 选v5（最小未处理）加入S，v7当前Dist=5 < 3+6=9，不更新。
6. 选v7（最小未处理）加入S，更新v6.Dist=min(8,5+1)=6。
7. 选v6加入S，算法结束。

## 2.1 Implementation
```c
#include <stdio.h>
#include <stdlib.h>
#include <limits.h>

#define Infinity INT_MAX  // 用int最大值表示无穷大
#define NotAVertex -1     // 表示没有找到顶点
#define MaxVertexNum 100  // 最大顶点数

// PPT里的Table结构体，一字不差对应三个字段
typedef struct {
    int Dist;    // 源点到该顶点的最短距离
    int Known;   // 是否已加入S集合（0=未加入，1=已加入）
    int Path;    // 前驱顶点编号（用于打印路径）
} TableEntry;

typedef TableEntry Table[MaxVertexNum];  // Table是结构体数组
typedef int Vertex;                      // 顶点用整数编号（0~n-1）
typedef int Weight;                      // 边权用整数表示

// 邻接表表示图（PPT里的图用邻接表存储最方便）
typedef struct AdjNode {
    Vertex adjvex;          // 邻接点编号
    Weight weight;          // 边的权值
    struct AdjNode *next;   // 下一个邻接点
} AdjNode;

typedef struct {
    AdjNode *head[MaxVertexNum];  // 每个顶点的邻接表头
    int vertexNum;                // 顶点总数
    int edgeNum;                  // 边总数
} Graph;

// 初始化Table数组（PPT要求的初始化方式）
void InitTable(Vertex start, Graph *G, Table T) {
    for (int i = 0; i < G->vertexNum; i++) {
        T[i].Dist = Infinity;
        T[i].Known = 0;
        T[i].Path = NotAVertex;
    }
    T[start].Dist = 0;  // 源点距离为0
}

// 核心：找到距离最小的未处理顶点（PPT里的V = smallest unknown distance vertex）
Vertex FindMinDist(Table T, Graph *G) {
    int minDist = Infinity;
    Vertex minV = NotAVertex;
    // 暴力扫描所有顶点（这就是基础版的核心）
    for (int i = 0; i < G->vertexNum; i++) {
        if (!T[i].Known && T[i].Dist < minDist) {
            minDist = T[i].Dist;
            minV = i;
        }
    }
    return minV;
}

// PPT第6页的Dijkstra伪代码的C语言实现
void Dijkstra_Basic(Graph *G, Vertex start, Table T) {
    InitTable(start, G, T);
    Vertex V, W;
    AdjNode *p;

    for (;;) {  // 对应PPT里的for(;;)循环
        // 步骤1：找最小距离的未处理顶点
        V = FindMinDist(T, G);
        if (V == NotAVertex) break;  // 所有顶点处理完毕，退出

        // 步骤2：将V加入S集合
        T[V].Known = 1;

        // 步骤3：遍历V的所有邻接点，更新距离
        p = G->head[V];
        while (p != NULL) {
            W = p->adjvex;
            if (!T[W].Known) {  // 只更新未加入S的顶点
                // 松弛操作：如果经过V到W更短，就更新
                if (T[V].Dist + p->weight < T[W].Dist) {
                    T[W].Dist = T[V].Dist + p->weight;
                    T[W].Path = V;  // 记录前驱
                }
            }
            p = p->next;
        }
    }
}
```
外层循环执行 | V | 次（每个顶点处理 1 次）
每次FindMinDist是 O (|V|)
所有边总共被处理 1 次（O (|E|)）
总时间：
$O(∣V∣^2+∣E∣)=O(∣V∣^2)$（稠密图中 | E|≈|V|²，这个复杂度是最优的）

可以用堆来优化
```c
// 优先队列元素：(距离, 顶点编号)
typedef struct {
    int dist;
    Vertex v;
} PQNode;

// 最小堆实现（简化版，只实现必要操作）
typedef struct {
    PQNode data[MaxVertexNum * 10];  // 足够大的空间存重复条目
    int size;
} MinHeap;

// 堆的插入操作（O(logn)）
void InsertHeap(MinHeap *heap, int dist, Vertex v) {
    int i = ++heap->size;
    while (i > 1 && dist < heap->data[i/2].dist) {
        heap->data[i] = heap->data[i/2];
        i /= 2;
    }
    heap->data[i].dist = dist;
    heap->data[i].v = v;
}

// 堆的删除最小值操作（O(logn)）
PQNode DeleteMin(MinHeap *heap) {
    PQNode minNode = heap->data[1];
    PQNode lastNode = heap->data[heap->size--];
    int i = 1, child;
    while (i * 2 <= heap->size) {
        child = i * 2;
        if (child != heap->size && heap->data[child+1].dist < heap->data[child].dist)
            child++;
        if (lastNode.dist <= heap->data[child].dist)
            break;
        heap->data[i] = heap->data[child];
        i = child;
    }
    heap->data[i] = lastNode;
    return minNode;
}

// 优先队列版Dijkstra
void Dijkstra_PriorityQueue(Graph *G, Vertex start, Table T) {
    InitTable(start, G, T);
    MinHeap heap;
    heap.size = 0;
    InsertHeap(&heap, 0, start);  // 源点入堆

    Vertex V, W;
    AdjNode *p;
    PQNode node;

    while (heap.size > 0) {
        // 步骤1：取出距离最小的顶点
        node = DeleteMin(&heap);
        V = node.v;
        if (T[V].Known) continue;  // 关键：如果已经处理过，直接跳过

        // 步骤2：标记为已处理
        T[V].Known = 1;

        // 步骤3：更新邻接点
        p = G->head[V];
        while (p != NULL) {
            W = p->adjvex;
            if (!T[W].Known && T[V].Dist + p->weight < T[W].Dist) {
                T[W].Dist = T[V].Dist + p->weight;
                T[W].Path = V;
                InsertHeap(&heap, T[W].Dist, W);  // 插入新条目到堆中
            }
            p = p->next;
        }
    }
}
```
更新时，我们把更新后的元素插入到堆中，最多可能要更新$\mid E\mid$次，每次插入需要$O(log k),k$为堆中元素个数，由于Dijkstra算法对于每条边只会插入堆一次，所以堆中元素最多$\mid E\mid$个，而$E\le V^2, log|E| ≤ log|V|² = 2log|V| = O (log|V|)$，故总时间复杂度为$O(\mid E\mid log V)$

# 3 Graph With Negative Edge Costs
```c
void WeightedNegative(Table T) {
    Queue Q;
    Vertex V, W;
    Q = CreateQueue(NumVertex);
    MakeEmpty(Q);
    Enqueue(S, Q);

    while (!IsEmpty(Q)) {
        V = Dequeue(Q);
        for (each W adjacent to V) {
            if (T[V].Dist + Cvw < T[W].Dist) {
                T[W].Dist = T[V].Dist + Cvw;
                T[W].Path = V;
                if (W不在队列中) {
                    Enqueue(W, Q);
                }
            }
        }
    }
    DisposeQueue(Q);
}
```
该算法(SPFA)的思路是，从源点开始，每次检查所有边需不需要更新，核心就在于负权边并不是像Dijsktra算法一样当前最短就能直接视为最短路径了，每次有顶点的最短距离更新了，都要再检查，即把它入队使得之后可以再检查它的邻居的距离有没有变小。SPFA 的队列允许一个人反复进出。只要你拿到了更短的距离，不管你之前进过几次队列，你都可以重新排队

![](img/DS/9-4.png)
如上图，左边的图中，A先入队，然后出队，更新A到C是3，A到B是2，按Dijkstra算法，B就会被认为已经找到了最短路径。
接下来B,C入队，B出队，没有能到达的，C出队，D入队，A到D更新成-3，D出队，更新A到B是-5，B入队；B出队，没有更新的，队空，结束。这时A到B,C,D的最短路是-5，3，-3，正确！

而上图右边的图，A先入队，然后出队，C入队；C出队，D入队；D出队，B入队；B出队，A入队……循环不会停止

怎么避免这种循环不停的情况呢？一条非环的最短路径最多V-1条边，对于一个顶点B，从A到B最多V-1条边，每次更新相当于在原有的路上又多加了一条边，所以一个顶点最多入队V-1次，若入队V次说明有环，如果是0或正的环，不会入队的，所以一定是负权环，所以只需要记录每个顶点入队次数就可以检验

每个顶点最多入队V次（为了检验负权环情况，最多V次），时间复杂度为$O(VE)$

# 4  Acyclic Graphs & AOE
If the graph is directed and acyclic, vertices may be selected in topological order since when a vertex is selected, its distance can no longer be lowered without any incoming edges from unknown nodes.

对于有向无环图，按拓扑排序去遍历，到某个顶点时，指向这个顶点的路径已经“没”了，所以此时的dist就是最小的了
1. 计算所有顶点的入度。   
2. 将入度为 0 的顶点放入队列。   
3. 取出顶点 $V$，遍历其邻居 $W$：更新距离：if (dist[V] + Cvw < dist[W]) dist[W] = dist[V] + Cvw;   
4. 将 $W$ 的入度减 1，若减为 0 则将 $W$ 入队。

每条边被遍历一次，每个顶点计算入度、入队、出队，复杂度为$O(V+E)$
## 4.1 AOE
![](img/DS/9-5.png)
假设有这么一个项目，边称作活动，顶点称作事件，每个事件必须在前一个指向它的事件做完后才可以开始做，求完成这个项目的最短时间

活动可以同时进行，所以时间取决于最耗时的那条路，称作**关键路径**

**关键活动**：不能拖延的活动，即该活动的最早开始时间和最晚开始时间一样
我们先看0到4，上面那条路要7，下面是5，所以上面那条路是关键路径，而活动1的最早开始时间当然是第0天，最晚开始时间是第2天，因为这样2+4+1=7刚好跟上面的路同步完成。或者说，事件2的最早完成时间是第4天，最晚完成时间是第6天

求关键路径：
每个点的最早开始时间为$ve$，最晚开始时间为$vl$，最开始，$ve$都初始化为0

求每个点的最早开始时间，这取决于前面最慢的任务，也采用拓扑排序的方法，每次取入度为0的点$i$，检查与它邻接的顶点$j$，若边权（活动时间）+ $i$的$ve$大于$j$的$ve$，那么就把$j$的$ve$更新为边权（活动时间）+ $i$的$ve$

操作完成后得到最后一个任务的最早开始时间，它的最早开始时间和最晚开始时间一样，然后把所有顶点的$vl$初始化为它的$vl$，开始逆拓扑排序
唯一的区别是，若$i$的$vl$-边权（活动时间）小于$j$的$vl$，那么就把$j$的$vl$更新为$i$的$vl$-边权（活动时间），因为如果$i$的$vl$-边权（活动时间）小于$j$的$vl$，那么以当前$j$的$vl$开始的话，开始$i$的时间就会更晚，超过了$i$最晚开始时间

最后求每个活动的最早开始时间和最晚开始时间，最早开始时间就是发出它的顶点的最早开始时间，最晚开始时间就是这条边指向的顶点的最晚开始时间减去这条边的边权

对于最早开始时间等于最晚开始时间的活动，就是关键活动，它们连起来构成关键路径。

事实上，AOE找关键路径，关键路径的长度就是网络中从起点到终点的最长路径长度，我们之所以要有上面的反推行为，是为了找到具体的关键路径，而不仅仅是长度，如果只是长度，参考有向无环图的最短路径找法就能找到，其实就是正推那一步在干的