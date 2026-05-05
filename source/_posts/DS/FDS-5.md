---
title: Chapter 4 - Heap
date: 2026-04-19 10:02:14
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

# 1 Heap (Priority Queue)
优先队列：出队不是先进的出，而是有优先级
这里我们讨论的优先级为最大/最小，即出队时最大/最小的元素先出队，那么有用数组/链表实现时，出队就要先查找最小/最大元素
![](img/DS/5-1.png)
如果使用BST，我们将面临一个问题：每次删除最小/最大元素时都只会动最左边/最右边的结点，或插入新元素，慢慢的这棵树就偏了。
而堆的核心操作就是「频繁删除堆顶、插入新元素」，每次删根节点都需要找右子树的最小值来补位，长期操作下来，BST几乎必然会出现单侧倾斜，性能暴跌。

# 2 Binary Heap
The implementation we will use is known as a ***binary heap***. Its use is so common for priority queue implementations that when the word heap is used without a qualifier, it is generally assumed to be referring to this implementation of the data structure.

It is easy to show that a complete binary tree of height $h$ has between $2^h$ and $2^{h+1} - 1$ nodes. This implies that the height of a complete binary tree is $\left \lfloor log N \right \rfloor$, which is clearly $O(log n)$ 

我们可以用数组来表示完全二叉树，根的下标为1，按层序编号即可。对于下标为$i$的结点
它的parent是$\left \lfloor i/2 \right \rfloor$(在c语言中，直接i/2即可)
它的左孩子是$2i$，若$2i > n$则没有左孩子
它的右孩子是$2i+1$，若$2i+1 > n$则没有右孩子

A min tree is a tree in which the key value in each node is no larger than the key values in its children (if any).  A min heap is a complete binary tree that is also a min tree.
## 2.1 Initialize
```c
//创建堆
typedef struct Heap* MinHeap;
struct Heap{
    ElementType* elements;//数组
    int size;//当前个数
    int capacity;//最大容量
}；

MinHeap Createheap(int maxsize)
{
    MaxHeap heap = malloc(sizeof(struct Heap));
    heap->elements = malloc((maxsize+1)*sizeof(ElementType));
    //因为从下标1开始
    heap->size = 0;
    heap->capacity = maxsize;
    heap->elements[0] = mindata;
    //哨兵，小于最小堆中最小元素
    return heap;
}
```

## 2.2 Insert
插入的算法：
插到（数组）末尾，然后跟父结点比较，比父结点小，就跟父结点交换然后继续比，直到比父结点大
时间复杂度为完全二叉树的高度$h=logn$
```c
void Insert(MinHeap heap,ElementType item)
{
    int i = 0;
    if(IsFull(heap))
    {
        puts("最小堆已满");
        return;
    }
    i = ++heap->size;//插入到数组末尾
    for(;heap->elements[i/2] > item;i/=2)
    {
        heap->elemets[i] = heap->elements[i/2];
        //比item大的就放到item位置
        /*注意这里没有交换，但可以当作交换了
        因为i/=2了，自己画下就知道了*/
    }
    heap->elements[i] = item;
    //有最小哨兵，所以i最后变成0一定循环停止
    //没哨兵就加个i>1的条件
}

int IsFull(MinHeap heap)
{
    return heap->size >= heap->capacity;
}

```

## 2.3 Delete
最大/最小堆中删除就是删除根结点，但要保证删完依然是最大/最小堆
算法：
1. 完全二叉树的最后一个元素跟根交换并删掉最后一个结点（size--），这时根被删除了，但问题是我们要调整这个堆使它依然是最小堆
2. 然后最后一个元素（tmp）开始跟左右儿子比，如果tmp比左右儿子中最小的还小，那就待在那里；或者tmp没有左儿子（自然就没有右儿子），也停止
3. 如果两个儿子中最小的比tmp小，那么它跟tmp交换，接着重复上一步
```c
/*
完全二叉树中，父结点i，它的左儿子是2i
右儿子是2i+1
*/
ElementType Deleteinx(MinHeap heap)
//取出最大元素并删除一个结点
{
    if(IsEmpty(heap))
    {
        puts("最小堆已空");
        return ERROR;
    }
    ElementType min = heap->elements[1];
    ElementType tmp = heap->elements[heap->size--];
    //最后一个元素
    int parent = 0;
    int child = 0;
    for(parent = 1;parent*2 <= heap->size;parent = child)
    {
        //从1开始比，parent表示tmp目前在的位置
        //parent*2是它的左儿子
        //如果大于size，说明没有左儿子，
        //由于是完全二叉树，更没有右儿子，不用再比了
        child = parent*2;//左child
        if(child!=heap->size && heap->elements[child]>heap->elements[child+1])
        {
            //第一个条件表示有右儿子
            child++;
        }
        if(tmp <= heap->elements[child])
        {
            break;
        }
        else
        {
            heap->elements[parent] = heap->elements[child];
            //这时想象tmp被换下来了
            /*
            其实也可以加上
            heap->elements[child]=tmp;
            最终交换就可以不要，更好理解
            */
        }
    }
    heap->elements[parent] = tmp;
    //最终交换
    return min;
}

int IsEmpty(MinHeap heap)
{
    return heap->size == 0;
}

```
![](img/DS/5-2.jpg)

## 2.4 Other Operations
对于最小堆，如果我们知道某个结点的位置，
- 要增加它的key，那么增加后要进行下滤(percolate down)

- 如果要减少它的key，那么减少后要上滤(percolate up)

- 删除这个结点，那么等于把这个结点的值减小到负无穷，然后上滤，最后这个结点会出现在根的位置，然后对堆进行删除即可。

## 2.5 Build Heap
给n个元素，如何建立最大堆呢？
1. 对n个元素，每个都执行一次插入，时间复杂度为$Nlog_2N$
2. 先把n个元素输入进去，形成完全二叉树，再调整位置

我们考虑第2种，借鉴删除的操作
删除是把最后的元素跟根“交换”，然后一层层往下下滤直到找到自己的位置
但这是有前提的，前提是最后的元素成为根后，它的左右子树都是堆（这样，只需跟左右儿子比才是合理的，因为左右子树都是堆说明只有左右儿子可能是当前位置为根的树中最大的了）

所以我们先找到最后一个结点的父结点，这个父结点的左右子树都是堆，因为就一个元素
然后做类似删除的操作（选左右儿子较大的那个，跟父结点比，如果父结点小就交换），再把同层的父结点都调整好，调成堆，再往上层走，这样左右子树又都是堆

```c
//先把n个结点依次输入heap->elements[i]

void Build_MaxHeap(MaxHeap heap)
{
    int last_father = heap->size / 2;
    for(int i = last_father;i>=1;i--)
    {
        PercDown(heap,i);
    }
}

void PercDown(MaxHeap heap,int i)
{
    /*
    将heap中以heap->elements[i]为根的堆调成最大堆
    */
    ElementType tmp = heap->elements[i];
    //这里tmp就是父结点，暂时的根节点
    //删除中取名tmp是因为没有实际换
    //但现在tmp确实就在这个局部的根上
    int parent = 0;
    int child = 0;
    for(parent = i;parent*2 <= heap->size;parent = child)
    {
        //从1开始比，parent表示tmp目前在的位置
        //parent*2是它的左儿子
        //如果大于size，说明没有左儿子，
        //由于是完全二叉树，更没有右儿子，不用再比了
        child = parent*2;//左child
        if(child!=heap->size && heap->elements[child]<heap->elements[child+1])
        {
            //第一个条件表示有右儿子
            child++;
        }
        if(tmp >= heap->elements[child])
        {
            break;
        }
        else
        {
            heap->elements[parent] = heap->elements[child];
            //这时想象tmp被换下来了
            /*
            其实也可以加上
            heap->elements[child]=tmp;
            最终交换就可以不要，更好理解
            */
        }
    }
    heap->elements[parent] = tmp;   
}
//所以上面的删除最后部分也可以封装成PercDown(heap,1)
//不过要先
// heap->elements[1]=heap->elements[size--];
```
最坏情况就是每次都要移元素，即操作了各结点高度之和次
设高度为H，那么一个有H层，根在第0层，第k层$2^k$个元素
$n=2^{H+1}-1$
$S=\sum_{k=0}^H 2^k(H-k)$
错位相减得$S=2^{H+1}-2-H$
故$S=n-1-H<n<2n$
故时间复杂度为$O(N)$

## 2.6 Percolate Down/Percolate Up
我们可以看到，上滤/下滤操作在堆中是很常见的，下面一道练习题。