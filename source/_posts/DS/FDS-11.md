---
title: Chapter 10 - DFS & BFS
date: 2026-05-15 16:24:53
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
# 1 Depth-First Search and Breadth-First Search
![](img/DS/11-1.png)

```c
#include <stdio.h>

typedef char VertexType;
typedef int EdgeType;

#define MAXN 100
typedef struct {
    VertexType vertex[MAXN];
    EdgeType arc[MAXN][MAXN];
    int vertex_num;
    int edge_num;
}Graph;

int visited[MAXN];

void dfs(Graph* g, int i)
{
    visited[i] = 1;
    printf("%c ", g->vertex[i]);
    for (int j = 0; j < g->vertex_num; ++j)
    {
        if (g->arc[i][j] == 1 && visited[j] == 0)
        {
            dfs(g, j);
        }
    }
    return;
}

void bfs(Graph* g, int start)
{
    int queue[MAXN] = { 0 };
    int front = 0;
    int rear = 0; //rear指向下一个要放的位置，front指向下一个出队元素的位置
    queue[rear++] = start;
    visited[start] = 1;
    printf("%c ", g->vertex[start]);
    while (front != rear)// 队列不空
    {
        int u = queue[front++];
        for (int j = 0; j < g->vertex_num; ++j)
        {
            if (g->arc[u][j] == 1 && visited[j] == 0)
            {
                visited[j] = 1;
                printf("%c ", g->vertex[j]);
                queue[rear++] = j;
            }
        }
    }
}
int main()
{
    Graph g;
    g.edge_num = 9;
    g.vertex_num = 9;
    g.vertex[0] = 'A';
    g.vertex[1] = 'B';
    g.vertex[2] = 'C';
    g.vertex[3] = 'D';
    g.vertex[4] = 'E';
    g.vertex[5] = 'F';
    g.vertex[6] = 'G';
    g.vertex[7] = 'h';
    g.vertex[8] = 'i';

    g.arc[0][1] = g.arc[1][0] = 1;
    g.arc[0][2] = g.arc[2][0] = 1;
    g.arc[1][3] = g.arc[3][1] = 1;
    g.arc[1][4] = g.arc[4][1] = 1;
    g.arc[4][5] = g.arc[5][4] = 1;
    g.arc[2][5] = g.arc[5][2] = 1;
    g.arc[1][5] = g.arc[5][1] = 1;
    g.arc[2][6] = g.arc[6][2] = 1;
    g.arc[7][8] = g.arc[8][7] = 1;


    for (int i = 0; i < g.vertex_num; ++i)
    {
        if (visited[i] != 1)
        {
            dfs(&g, i);
            printf("\n");
        }
    } // 这将会在每行打印出连通分量
    return 0;
}

```

使用邻接表，深度优先搜索和广度优先搜索的复杂度为$O(V+E)$，因为
- 每个顶点会被访问一次（标记已访问），因此有 V 次顶点访问。

- 每条边会被检查两次（在无向图中，每条边属于两个顶点的邻接表；在有向图中每条边属于一个顶点的邻接表）。对每个顶点的邻接表遍历时，会扫描其所有邻接边，因此所有邻接表的长度之和为 2E（无向图）或 E（有向图）。

# 2 Application
## 2.1 Biconnectivity
{% callout info %} 
$v$ is an ***articulation point*** if G' = DeleteVertex(G, v) has at least 2 connected components.
即删去割点，图会断开

$G$ is a ***biconnected graph*** if $G$ is connected and has no articulation points.
即无论删去哪个点，图依然连通

A ***biconnected component*** is a maximal biconnected subgraph.
{% endcallout %}
![](img/DS/11-2.png)

![](img/DS/11-3.png)
回边：Depth first spanning tree里没有的边，但在原图里有的边，比如1其实在原图是可以到3的，只是这个边没被遍历到而已

另外spanning tree跟从谁开始dfs无关
根如果有至少两个孩子肯定是割点，因为光是在树中，删了根就已经分割成至少两部分了，反过来，如果是割点，一定有至少两个孩子，否则就是一条链，那去掉根仍连通

对于其他点，割点的判断标准是至少有一个孩子，而且至少往下走一步后，不能跳回它的ancestor，也就是Num比它小的点
例如5往下走多少步都回不到3了，但是6往下走一步可以回到5，9没孩子不是割点，4往下两步可以回到3

![](img/DS/11-4.png)
用程序语言来说，如果u不是根，且至少有一个孩子，而且$Low(child)\ge Num(u)$，注意Low(u)的定义实际上就是从u或u的孩子出发向下，最多允许跳一次回边向上，能到达的Num最小的值，所以对于5来说，它的孩子，6的Low为5，意味着5往下走多少步都回不到3了，最多回到5，回到的Num大于等于Num(5)，这跟上面的“不能跳回Num比它小的点”相符

所以为什么至少有一个孩子，不能跳回Num比它小的点就是割点了呢，因为它的孩子比如A，最多只能回到这个点u，如果删去u，A就回不到在u上面的点了（u不是根，上面肯定有点），所以图就断开了

## 2.2 Euler Circuits
***Euler tour***: 一笔画遍历每条边
***Euler circuit***: 一笔画遍历每条边，而且起点和终点要相同
{% callout info %} 
如果存在欧拉回路，那么这个图连通，而且每个顶点的度是偶数

如果恰好有两个顶点的度为奇数，那么欧拉路径(tour)存在，且这两个点中有一个为这条路径的起点
{% endcallout %}
![](img/DS/11-5.png)
怎么找欧拉回路呢，我们看这个图，从5开始dfs，假设
5->10->4->5
回来了，对5来说dfs结束，所以我们记下4-5这条边，或者说，记下5，让它入栈
接着回溯到4，4->1->3->7->10->11->4->7->9->3->4，回到4，4废了，不能dfs了，记下4，入栈
这时dfs会回溯到3，3->2->8->9->6->3，3废了，记下3，3入栈
这时dfs回溯到6，也废了，6入栈
回溯到9，9->12->10->9，9入栈
接着一顿回溯，10入栈，12入栈，9入栈...
最后把栈的元素依次出栈就得到欧拉回路

note中，用链表维护路径，是为了可以头插法入栈，我们知道，第一步，如5->10其实是最后入栈，所以头插法入栈，输出链表就能得到5->10->...(我又想了想，好像也没必要吧，数组也可以？)
要用邻接表实现，而且给每个顶点 u 维护一个指针cur[u]，记录当前应该处理的邻接边的位置。每次处理完cur[u]指向的边后，就把cur[u]向后移动一位,这样：
每条边只会被扫描一次，不会重复处理。
不需要 visited 数组，因为处理过的边再也不会被访问到

最后得到的时间复杂度就是$O(E+V)$

## 2.3 Tarjan's Algorithm
Write a program to find the strongly connected components in a digraph.
```c
#include <stdio.h>
#include <stdlib.h>

#define MaxVertices 10  /* maximum number of vertices */
typedef int Vertex;     /* vertices are numbered from 0 to MaxVertices-1 */
typedef struct VNode *PtrToVNode;
struct VNode {
    Vertex Vert;
    PtrToVNode Next;
};
typedef struct GNode *Graph;
struct GNode {
    int NumOfVertices;
    int NumOfEdges;
    PtrToVNode *Array; // 注意，这里直接存顶点的下一个点了，如果顶点出度为0，数组里放的是NULL
};

Graph ReadG(); /* details omitted */

void PrintV( Vertex V )
{
   printf("%d ", V);
}

void StronglyConnectedComponents( Graph G, void (*visit)(Vertex V) );

int main()
{
    Graph G = ReadG();
    StronglyConnectedComponents( G, PrintV );
    return 0;
}

/* Your function will be put here */


```

```c
void tarjan(void (*visit)(Vertex V), int* belong, int* stack, int* top, int* dfn, int* low,Graph g, Vertex u, int* timer)
{
    dfn[u] = low[u] = ++(*timer);
    stack[++(*top)] = u;
    PtrToVNode p = g->Array[u];
    while(p != NULL)
        {
            Vertex nexe = p->Vert;
            if(dfn[nexe]==-1)
            {
                // 没访问过
                tarjan(visit, belong, stack, top, dfn, low, g, nexe, timer);
                // 更新
                if(low[nexe] < low[u])
                {
                    low[u] = low[nexe];
                }
            }
            else if(belong[nexe]==-1)
            {
                if(dfn[nexe] < low[u])
                {
                    low[u] = dfn[nexe];
                }
            }
            p = p->Next;
        }

    if(dfn[u] == low[u])
    {
        Vertex vertex;
        vertex = stack[(*top)--];
        visit(vertex);
        belong[vertex] = 2;
        while(u != vertex)
            {
                vertex = stack[(*top)--];
                visit(vertex);
                belong[vertex] = 2;
            }
        printf("\n");
    }
}
void StronglyConnectedComponents( Graph G, void (*visit)(Vertex V) )
{
    int n = G->NumOfVertices;
    int* dfn = malloc(sizeof(int)*n);
    int* low = malloc(sizeof(int)*n);
    int* stack = malloc(sizeof(int)*n);
    int* belong = malloc(sizeof(int)*n);
    
    int top = -1;
    int timer = 0;
    for(int i = 0;i<n;++i)
        {
            dfn[i] = -1;
            belong[i] = -1;
            
        }

    for(int i = 0;i<n;++i)
        {
            if(dfn[i] == -1)
            {
                tarjan(visit, belong, stack, &top, dfn, low, G, i, &timer);
                
            }
        }
    free(stack);
    free(dfn);
    free(low);
    free(belong);
    
    
}
```