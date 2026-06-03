---
title: FDS-Exercise
date: 2026-03-25 10:29:39
categories: Data Structure
---

# 0 写在前面
第一次quiz有点依托，这里总结一下各种练习题

# #1 逆转链表
{% callout primary::Problem %}
```c
typedef struct Node *PtrToNode;
typedef PtrToNode List;
typedef PtrToNode Position;
struct Node {
    ElementType Element;
    Position Next;
};

```
The function ``Reverse`` is supposed to return the reverse linked list of ``L``, with a dummy header.
```c
List Reverse(List L);
```
{% endcallout %}
{% folding title="Solution" class="green" open=false %}
```c
List Reverse(List L)
{
    if(L->Next == NULL)
    {
        //空链表
        return L;
    }
    Position move = L->Next;
    Position pre = L;
    Position tmp;
    while(move != NULL)
    {
        tmp = move->Next; //下一个结点
        move->Next = pre;//指向前一个
        pre = move;
        move = tmp;
    }
    Position first = L->Next; //原链表第一个结点，现在是最后一个
    first->Next = NULL; //原本指向L，现在要指向NULL
    L->Next = pre; //L作为dummy head要指向原来的链表的最后一个结点，即pre
    return L;
}
```
{% endfolding %}

# #2 Is Topological Order
{% callout primary::Problem %}
Write a program to test if a give sequence Seq is a topological order of a given graph Graph.
```c
bool IsTopSeq( LGraph Graph, Vertex Seq[] );
```

```c
typedef struct AdjVNode *PtrToAdjVNode; 
struct AdjVNode{
    Vertex AdjV;
    PtrToAdjVNode Next;
};

typedef struct Vnode{
    PtrToAdjVNode FirstEdge;
} AdjList[MaxVertexNum];

typedef struct GNode *PtrToGNode;
struct GNode{  
    int Nv;
    int Ne;
    AdjList G;
};
typedef PtrToGNode LGraph;
```
The function IsTopSeq must return true if Seq does correspond to a topological order; otherwise return false.

Note: Although the vertices are numbered from 1 to MaxVertexNum, they are indexed from 0 in the LGraph structure.
{% endcallout %}
{% folding title="Solution" class="green" open=false %}
```c
bool IsTopSeq( LGraph Graph, Vertex Seq[] )
{
    int n = Graph->Nv;
    // 先计算各顶点入度
    int* indegree = malloc(sizeof(int)*n);
    for(int i = 0; i<n;++i)
    {
        PtrToAdjVNode p = Graph->AdjList[i].FirstEdge;
        while(!p)
        {
            Vertex v = p->AdjV;
            indegree[v]++;
            p = p->Next;
        }
    }

    // 开始核对
    Vertex current = -1;
    for(int i = 0;i<n;++i)
    {
        current = Seq[i]-1;
        if(indegree[i]!=0)
        {
            return 0;
        }
        PtrToAdjVNode p = Graph->AdjList[current].FirstEdge;
        while(!p)
        {
            Vertex v = p->AdjV;
            indegree[v]--;
            p = p->Next;
        }
    }
    return 1;
}
```
{% endfolding %}


# #3 SCC
{% callout primary::Problem %}
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
    PtrToVNode *Array; 
    // 注意，这里直接存顶点的下一个点了，如果顶点出度为0，数组里放的是NULL
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

{% endcallout %}

{% folding title="Solution" class="green" open=false %}
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
{% endfolding %}





# 理论题
A graph with 90 vertices and 20 edges must have at most __ connected component(s).

要尽可能的让顶点孤立，这样一个顶点就是一个连通分量
所以20条边要用尽可能少的顶点耗完，7个顶点的完全图有21条边，去掉一条边，这7个顶点构成一个连通分量
所以最多83+1=84

---

判断：In a weighted undirected graph, if the length of the shortest path from $b$ to $a$ is 12, and there exists an edge of weight 2 between $c$ and $b$, then the length of the shortest path from $c$ to $a$ must be no less than 10.

b到a是最短路径，所以它的长度小于等于任何从b到a的路径，先从b到c的最短路，再从c到a的最短路也是一条从b到a的路径
所以$min(b,a)\le min(b,c) + min(c,a)$
得到$min(c,a)\ge 10$

---

If a binary search tree of N nodes is complete, which one of the following statements is FALSE?
A.the average search time for all nodes is O(logN)

B.the minimum key must be at a leaf node

C.the maximum key must be at a leaf node

D.the median node must either be the root or in the left subtree


---

Given a sorted file of 1000 records.  To insert a new record by insertion sort with binary search, the maximum number of comparisons is

---

Use simple insertion sort to sort 10 numbers from non-decreasing to non-increasing, the possible numbers of comparisons and movements are:


A.
100, 100


B.
100, 54


C.
54, 63


D.
45, 44