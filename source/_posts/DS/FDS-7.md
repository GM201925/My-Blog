---
title: Chapter 6 - The Segment Tree
date: 2026-04-20 16:16:30
categories: Data Structure
mathjax: true
thumbnail: img/test.png

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

# 1 Motivation
Given an array A[1000000], and we need to frequently calculate the sum of the numbers in an arbitrary range [L, R].
如果每次都从L算到R，区间有$N$个数，时间复杂度都是$O(N)$，如果计算频繁，就比较慢

利用线段树，我们可以做到查询某个区间的和以及更新区间都是$O(log N)$的复杂度
线段树可以支持任何区间聚合操作，比如区间里找最大/最小值，我们以区间求和为例来说明。

把整个数组区间不断二分，每个节点代表一个连续区间，得到一棵树，我们可以补上一定的空间使其成为完全二叉树，按理说，区间共有N个数，即有$N$个叶结点，最后得到的是满二叉树，对于满二叉树，假设有$i$个内部节点，那么总结点数等于$i+N=2i+1$(树中除了root每个结点都是内部结点的孩子)，则理论上共$2N-1$个结点，一般需要$4N$的空间，图中的例子如果6结点下面还会多出两个区间（比如多一个数，3放在数组中6号位置），那这时5结点下面还要多出两个空的空间来构成完全二叉树，$2N-1$未必够用
```
          1
       /     \
      2       3
            /   \
           6     7
                / \
               14 15
```
这个例子$2N-1$也是不够的，$N=4$，但下标到了15(或 16)
![](img/DS/7-1.png)

# 2 Build
```c
int A[MAX];
int tree[4*MAX];
void Build(int node, int start, int end)
// node: 当前节点编号
// start, end: 当前节点代表的区间 [start, end]
{
    //Base Case: Leaf Node
    if(start == end)
    {
        tree[node] = A[start]; // 例如区间4-4的和就是4号元素
        return;
    }

    int mid = (start + end) / 2;
    Build(2*node,start,mid);
    Build(2*node+1,mid+1,end);

    tree[node] = tree[2*node] + tree[2*node + 1];
}
```
例如
```
          1
       /     \
      2       3
    /   \   /   \
   4     5 6     7
  / \
 8   9
```
Build 过程会依次访问：1 → 2 → 4 → 8 → 9 → 5 → 3 → 6 → 7一共 9 次访问，每次都是 O (1) 操作。
总时间 = 9 × O (1) = O (9) = O (5) = O (N)
如果我们树开$4N$空间，每个结点只会被访问一次，一共$2N-1$个结点，$T(N) = O(N)$
Build只用做一次

# 3 Query
![](img/DS/7-2.png)
我们查找[2,4]的区间和，先看根结点，它是[0,4]区间和，有重叠，它的左边是[0,2]，右边是[3,4]，都不完全覆盖查找的区间，所以都要进入

先看左边，左边我们只需要找和[2,4]重合的[2,2]，进入[0,2]后检查左右，右边刚好重合，所以返回5

再看右边，右边我们只需要找和[2,4]重合的[3,4]，刚好进入[3,4]后直接刚好重合，返回11

最后相加，返回16

```c
int Query(int node, int start, int end, int left, int right)
// node: 当前节点编号
// start, end: 当前节点代表的区间 [start, end]
// left, right: 查找区间的范围[left,right]
{
    // Case 1:No Overlap, node is completely outside query range
    if(right < start || end < left)
    {
        return 0;
    }

    // Case 2: Total Overlap, node is completely inside query range
    // 如上例进入11，11的范围[3,4]是在[2,4]中的，直接返回就行，因为它直接作为[2,4]区间和的一部分
    if(left <= start && end <= right)
    {
        return tree[node];
    }

    // Case 3: Partial Overlap, go deeper into both children
    int mid = (start + end) / 2;
    int left_sum = Query(2*node, start, mid, left, right);
    int right_sum = Query(2*node+1, mid+1, end, left, right);

    return left_sum + right_sum;

}
```
也就是说，通过case 3，我们总能把情况拆成case 1/2
{% callout info %} 
注意程序实现中和算法有些许不同，程序实现中我们的查询区间left，right在传递中从没变过。
但在算法中，如*先看左边，左边我们只需要找和[2,4]重合的[2,2]，进入[0,2]后检查左右，右边刚好重合，所以返回5*
好像我们进入[0,2]判断时改成了判断它和[2,2]的关系，但这个[2,2]是怎么来的呢？其实是我们发现[0,2]和[2,4]只有[2,2]是重合的，但实际程序中，我们并没有改变left和right，所以case 2是``left <= start && end <= right``而不是``left == start && end == right``，如[0,2]属于case 3，进入右边后其实是``left=2,right=4,start=end=2``，它仍属于``left(2) <= start(2) && end(2) <= right(4)``，所以我们应该这么理解：
我们查找[2,4]的区间和，先看根结点，它是[0,4]区间和，与[2,4]有重叠(即不是完全不重合或完全被包住)，它的左边是[0,2]，右边是[3,4]，都要进入

先看左边，左边[0,2]与[2,4]也只是部分重叠，左边没交集，返回0，右边[2,2]被[2,4]完全包住，所以返回5，最后返回0+5=5

再看右边，[2,4]完全包住[3,4]返回11

最后返回5+11=16

总结：
每个节点只需要回答一个问题：
在我负责的这个小区间 [start, end] 里，属于全局查询 [L, R] 的部分，总和是多少？
它**不需要改查询条件**，它只需要拿自己的区间 [start, end] 去和固定的 [L, R] 比：
如果我管的人一个都不在查询里 → 我贡献 0
如果我管的人全部都在查询里 → 我直接贡献我管的所有人的总和
如果一部分在一部分不在 → 我去问我的两个下属同样的问题，然后加起来
{% endcallout %}

由于每次查询最多访问 2*logN 个结点，即从根走到底，而完全二叉树的树高是$O(log N)$，$T(N) = O(log N)$

# 4 Update
单点更新：找到对应的叶子节点，更新它的值，然后回溯向上更新所有父节点的值。
![](img/DS/7-3.png)
```c
void Update(int node, int start, int end, int idx, int val)
// node: 当前节点编号
// start, end: 当前节点代表的区间 [start, end]
// idx: 更新的点的编号
// val: 更新的值
{
    // Base Case: Leaf Node Found
    if(start == end)
    {
        tree[node] = val;
        return;
    }

    int mid = (start + end) / 2;
    if(start <= idx && mid >= idx)
    {
        // Index is in the left child
        Update(2*node, start, mid, idx, val);
    }
    else
    {
        // Index is in the right child
        Update(2*node+1, mid+1, end, idx, val);
    }

    // Backtracking Step: Update current node based on new children values
    tree[node] = tree[2*node] + tree[2*node + 1];
}
```
$T(N) = O(log N)$，最多走树的高度

# 5 Lazy Propagation
现在我们要对区间[L,R]里的所有数都加上一个常数，该怎么办？
如果是单点更新，复杂度就是$O(Nlog N)$

懒标记的核心思想
延迟更新：
我先不着急把更新传到每一个叶子节点，而是先在对应的大节点上记个账（打个标记）。只有当我必须去查这个大节点的子节点时，我才把之前记的账推下去（Propagate）给子节点。

```
          根1: [0,4] 25
        /         \
      2:[0,2]14  3:[3,4]11
     /   \          /   \
   4:[0,1]9 5:[2]5 6:[3]3 7:[4]8
```
区间更新:[0, 2] 加 3
我们要给 0-2 号元素都加 3。
根节点 [0,4]：部分重叠，往左右儿子走
1. 节点 2 [0,2]：✅ 完全包含！
先更新节点 2 的值：14 + 3*3 = 23（区间长度 3，每个加 3，总和加 9）
给节点 2 打个懒标记：lazy[2] = 3
直接返回！不往下走了！
2. 节点 3 [3,4]：无重叠，不管
3. 回溯更新，把1更新成34

现在查询 [0, 1] 的和
这时候我们必须去访问节点 2 的子节点了，所以必须把懒标记推下去！
PushDown（推标记）操作：
检查节点 2 有没有懒标记：有，lazy[2]=3
把标记推给左孩子 4：
更新节点 4 的值：9 + 2\*3 = 15（区间长度 2）
给节点 4 打标记：lazy[4] += 3
把标记推给右孩子 5：
更新节点 5 的值：5 + 1\*3 = 8（区间长度 1）
给节点 5 打标记：lazy[5] += 3
清除节点 2 的标记：lazy[2] = 0

ai的实现
```c
#include <stdio.h>

#define MAXN 100005
int A[MAXN];
int tree[4 * MAXN];
int lazy[4 * MAXN]; // 懒标记数组

// ===================== 核心：PushDown 推标记 =====================
// node: 当前节点
// l, r: 当前节点区间
void PushDown(int node, int l, int r) {
    if (lazy[node] == 0) return; // 没有标记，直接返回

    int mid = (l + r) / 2;
    int left = 2 * node;
    int right_node = 2 * node + 1;

    // 1. 推给左孩子
    tree[left] += lazy[node] * (mid - l + 1); // 更新左孩子的值
    lazy[left] += lazy[node];                  // 左孩子打标记

    // 2. 推给右孩子
    tree[right_node] += lazy[node] * (r - (mid + 1) + 1); // 更新右孩子的值
    lazy[right_node] += lazy[node];                          // 右孩子打标记

    // 3. 清除当前节点的标记
    lazy[node] = 0;
}

// ===================== 建树 =====================
void Build(int node, int l, int r) {
    lazy[node] = 0; // 初始化懒标记为0
    if (l == r) {
        tree[node] = A[l];
        return;
    }
    int mid = (l + r) / 2;
    Build(2 * node, l, mid);
    Build(2 * node + 1, mid + 1, r);
    tree[node] = tree[2 * node] + tree[2 * node + 1];
}

// ===================== 区间更新：给 [ul, ur] 加 val =====================
void UpdateRange(int node, int l, int r, int ul, int ur, int val) {
    if (ur < l || r < ul) {
        return; // 无重叠
    }
    if (ul <= l && r <= ur) {
        // 完全包含：直接更新当前节点，打标记，不往下走
        tree[node] += val * (r - l + 1);
        lazy[node] += val;
        return;
    }
    // 部分重叠：必须先推标记，再递归
    PushDown(node, l, r);
    int mid = (l + r) / 2;
    UpdateRange(2 * node, l, mid, ul, ur, val);
    UpdateRange(2 * node + 1, mid + 1, r, ul, ur, val);
    tree[node] = tree[2 * node] + tree[2 * node + 1];
}

// ===================== 区间查询：查 [ql, qr] 的和 =====================
int QueryRange(int node, int l, int r, int ql, int qr) {
    if (qr < l || r < ql) {
        return 0; // 无重叠
    }
    if (ql <= l && r <= qr) {
        return tree[node]; // 完全包含
    }
    // 部分重叠：必须先推标记，再递归
    PushDown(node, l, r);
    int mid = (l + r) / 2;
    return QueryRange(2 * node, l, mid, ql, qr) + QueryRange(2 * node + 1, mid + 1, r, ql, qr);
}

// ===================== 测试主函数 =====================
int main() {
    int n = 5;
    A[0] = 7; A[1] = 2; A[2] = 5; A[3] = 3; A[4] = 8;
    
    Build(1, 0, n-1);
    printf("初始总和：%d\n", QueryRange(1,0,4,0,4)); // 25
    
    // 区间更新：[0,2] 加 3
    printf("\n--- 给 [0,2] 加 3 ---\n");
    UpdateRange(1,0,4,0,2,3);
    printf("查询 [0,1]：%d\n", QueryRange(1,0,4,0,1)); // 7+3 + 2+3 = 15
    printf("查询 [0,2]：%d\n", QueryRange(1,0,4,0,2)); // 15 + 5+3 = 23
    printf("查询总和：%d\n", QueryRange(1,0,4,0,4)); // 23 + 3+8 = 34
    
    return 0;
}
```
这样，一次区间更新就做到了$O(log N)$的复杂度