---
title: Chapter 12 - Hashing
date: 2026-06-14 14:52:22
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

二分查找很快，但排序本身也要$O(nlogn)$
| 查找方式 | 前提条件 | 平均时间复杂度 |
|:---:|:---:|:---:|
| 顺序查找 | 无 | O(n) |
| 二分查找 | 数组有序 | O(log n) |
| **哈希查找** | 无 | **O(1)** ← 目标！ |

# 1 Interpolation Search
插值查找是一种在有序数组中比二分更快的查找方式。它不是每次取中点，而是根据目标值在范围内的"比例"来估算位置：
从f[l].key, f[l+1].key, ... , f[u].key里找key
```
i = l + (key - f[l].key) / (f[u].key - f[l].key) * (u - l)
if (f[i].key < key) l=i;
else u=i;
```
这本质上也是"用公式算位置"，是哈希思想的前身。但它依赖数据均匀分布，哈希表则更通用。

# 2 General Ideas
哈希表的抽象模型叫做**符号表(Symbol Table)**（也叫字典），它存储的是 `<名字, 属性>` 键值对。

**现实例子**：
- 英语词典：名字 = 单词，属性 = 释义列表
- 编译器符号表：名字 = 变量名（如 `int`），属性 = 使用行号、类型等信息
## 2.1 Hash Table
哈希表 ht[] 由 b 个"桶"（bucket）组成
每个桶有 s 个"槽"（slot）


```
ht[0]:  [ slot0 | slot1 | ... ]
ht[1]:  [ slot0 | slot1 | ...]
...
ht[b-1]:[ slot0 | slot1 | ... ]
```

对于每个标识符 `x`，哈希函数 `f(x)` 计算出它应该放在哪个桶里，也就是放到的桶的下标
- `T` = 所有可能键值的总数（全集大小）
- `n` = 当前哈希表中实际存储的键的数量
- `s` = 每个桶的槽数
- `b` = 桶的数量
标识符密度  = $n / T$
装载密度 λ  = $n / (s × b)$

- **冲突（Collision）**：两个不同的键 $i_1 ≠ i_2$，但 $f(i_1) = f(i_2)$，映射到了同一个桶
- **溢出（Overflow）**：新键要插入一个**已经满了**的桶

没有溢出时，利用哈希函数$f(x)$，T_search = T_insert = T_delete = O(1)

## 2.2 Hash Function
>能为 key 计算索引之后，我们就可以知道每个键值对应的值 value 应该放在哪里了．假设我们用数组 a 存放数据，哈希函数是 f，那键值对 (key, value) 就应该放在 a[f(key)] 上．不论键值是什么类型，范围有多大，f(key) 都是在可接受范围内的整数，可以作为数组的下标．(摘自[OIWiki]("https://oi-wiki.org/ds/hash/"))

哈希函数的特点
- 计算简单，冲突尽量少
- 尽量均匀，对任意键 x 和任意位置 i，`P(f(x) = i) = 1/b`，满足这个条件的函数称为**均匀哈希函数(uniform hash function)**

一种哈希函数是f(x) = x % TableSize
对于随机的整数keys，TableSize取素数更好 

对于字符串
**方法一**
```c
// 把每个字符的 ASCII 值加起来
f(x) = (sum of x[i]) % TableSize;
```

问题：对于 TableSize = 10,007，字符串长度 ≤ 8，字符 ASCII ∈ [0, 127]，则：
- f(x) 的最大值 = 127 × 8 = 1016
- 哈希值只会落在 [0, 1016]，大量桶永远空着！

**方法二：带权加法**

```c
f(x) = (x[0] + x[1]*27 + x[2]*27*27) % TableSize;
```

把字母看成 27 进制数字，赋予不同位权重。  
例如cat就映射到3+27+20*27*27对TableSize取余
cater也是一样的下标
但实际上单词中前3个字母的组合只有约 3000 种，只看前三位，很大可能算出来一样的数导致冲突，解决方法是多考虑几位，但是乘法也是有开销的

**方法三**
```c
Index Hash3(const char *x, int TableSize) {
    unsigned int HashVal = 0;
    while (*x != '\0')
        HashVal = (HashVal << 5) + *x++;  // 等价于 HashVal * 32 + *x
    return HashVal % TableSize;
}
```
unsigned int占32位的情况下，我们以单词orchid为例
先读o，即x[0]，设它的权值是15，存为01111，此时HashVal为
00000 00000 00000 00000 00000 00011 11
接着读r，即x[1]，值18，存为10010，此时HashVal为
00000 00000 00000 00000 00011 11100 10
unsigned int一般占64位，最多可以考虑十几个字符，算出一样的数可能性很小，虽然可能同余，冲突率也会下降
32是因为移位比乘法快，如果字符还包括数字什么的，32还可以更大

最后x[0]会位于这个“整数”（即64位01串）的最左边，但如果字符串x很长，就会只保留最后几位字符的值（前面的字符被移出去了）如果它们都一样，就可能导致冲突，所以字符的选择也很重要

# 3 Seperate Chaining
冲突总是存在的，怎么解决冲突呢

> 把所有哈希到同一个位置的键，用**链表**串起来放在那个桶里。

```
ht[0] → [acos] → [atoi] → NULL
ht[1] → NULL
ht[2] → [char] → NULL
ht[3] → [define] → NULL
...
```

```c
struct ListNode;
typedef struct ListNode* Position;
// 链表节点
struct ListNode {
    ElementType Element;  // 存储的键（如字符串）
    Position    Next;     // 下一个节点的指针
};

typedef Position List;

// 哈希表
struct HashTbl {
    int   TableSize;    // 桶的数量
    List* TheLists;     // 指针数组，每个元素是一条链表的头节点
    // 如果嫌下面的第四步每个桶都要malloc一个头节点太慢，可以直接ListNode* TheLists，然后
    // H->TheLists = malloc(sizeof(struct ListNode)*H->TableSize);
};
typedef struct HashTbl* HashTable;
```

```c
HashTable InitializeTable(int TableSize) {
    HashTable H;
    int i;

    if (TableSize < MinTableSize) {
        Error("Table size too small");
        return NULL;
    }

    // Step 1: 分配哈希表结构体
    H = malloc(sizeof(struct HashTbl));
    if (H == NULL) FatalError("Out of space!!!");

    // Step 2: 表大小取不小于 TableSize 的最近质数
    H->TableSize = NextPrime(TableSize);

    // Step 3: 分配链表头节点数组
    H->TheLists = malloc(sizeof(List) * H->TableSize);
    if (H->TheLists == NULL) FatalError("Out of space!!!");

    // Step 4: 为每条链表分配头节点，初始化为空链表
    for (i = 0; i < H->TableSize; i++) {
        H->TheLists[i] = malloc(sizeof(struct ListNode));
        if (H->TheLists[i] == NULL) FatalError("Out of space!!!");
        H->TheLists[i]->Next = NULL;  // 链表为空
    }

    return H;
}
```

查找

```c
Position Find(ElementType Key, HashTable H) {
    Position P;
    List L;

    // 1. 通过哈希函数找到对应的链表
    L = H->TheLists[Hash(Key, H->TableSize)];

    // 2. 从头节点的下一个开始遍历（跳过哨兵节点）
    P = L->Next;

    // 3. 线性搜索链表
    while (P != NULL && P->Element != Key)  // 字符串用 strcmp
        P = P->Next;

    return P;  // 找到返回节点指针，没找到返回 NULL
}
```

插入
```c
void Insert(ElementType Key, HashTable H) {
    Position Pos, NewCell;
    List L;

    // 先查找，避免重复插入
    Pos = Find(Key, H);

    if (Pos == NULL) {  // 不存在，才插入
        NewCell = malloc(sizeof(struct ListNode));
        if (NewCell == NULL) FatalError("Out of space!!!");

        // 找到对应的链表头节点
        L = H->TheLists[Hash(Key, H->TableSize)];// 实际实现中可以修改Find，避免算两遍Hash

        // 头插法：插在头节点之后
        NewCell->Next    = L->Next;
        NewCell->Element = Key;   // 字符串用 strcpy
        L->Next          = NewCell;
    }
    // 如果已存在，什么都不做（或更新属性）
}
```
**实践建议**：让 TableSize ≈ 预期键的数量，使 λ ≈ 1，此时每条链表平均长度为 1，接近 O(1)。

# 4 Open Addressing
```c
// 通用算法框架
index = hash(key);
i = 0;  // 探测计数器
while (该位置有冲突) {
    index = (hash(key) + f(i)) % TableSize;
    if (表满了) break;
    else i++;
}
if (表满了)
    ERROR("No space left");
else
    在 index 处插入 key;
```
这里的f(x)是collision resolving function
简单来说，冲突后就在当前下标加一个数，看下一个位置是否冲突
一般我们让λ小于0.5

## 4.1 Linear Probing
$$f(i) = i$$
也就是冲突后，一个位置一个位置往后找
![](img/DS/14-1.png)
p虽然很小，但最坏情况很大，任何哈希值落在 0~10 的新键，都要跳过整个群才能找到空位，使群越来越长，性能越来越差。这就是**primary clustering(一次群集)**问题。


## 4.2 Robin Hood Hashing
> 定义每个键的**探测距离** `d(x)` = (实际存储位置 - 初始哈希位置) % TableSize  
> 插入新键时，若新键的探测距离 > 当前占位键的探测距离，则**抢占**该位置，把原来的键往后移。

![](img/DS/14-2.png)
怎么找atan呢，如果atan在的话，那么插入atan时，先映射到0号桶，发现d(x)是一样的，下一个，这时atan的d(x)变成1，依然和atoi一样，下一个，atan的d(x)变成3，大于char的0，按理说应该占据，但这里是char，说明atan不存在

```c
bool RobinHood_Insert(ElementType Key, HashTable H) {
    Position Pos = Hv = Hash(Key, H->TableSize);  // 起始位置
    int CurrentDisp = 0;      // 当前待插入键的探测距离
    ElementType CurrentKey = Key;

    while (true) {
        if (H->TheCells[Pos].Info != Legitimate) {
            // 当前位置空（Empty 或 Deleted），直接插入
            H->TheCells[Pos].Info    = Legitimate;
            H->TheCells[Pos].Element = CurrentKey;
            H->TheCells[Pos].Disp   = CurrentDisp;
            return true;
        }

        // "劫富济贫"：如果我的探测距离更大，我来抢占这个位置
        if (CurrentDisp > H->TheCells[Pos].Disp) {
            swap(&CurrentKey, &H->TheCells[Pos].Element);
            swap(&CurrentDisp, &H->TheCells[Pos].Disp);
            // 继续为被抢占的键找位置
        }

        Pos = (Pos + 1) % H->TableSize;
        if (Pos == Hv) return false;  // 转了一圈，表满了
        CurrentDisp++;
    }
}
```

## 4.3 Quadratic Probing
$$f(i)=i^2$$

{% callout info::Theorem %} 
If quadratic probing is used, and the table size is prime, then a new element can always be inserted if the table is at least half empty.
{% endcallout %}
我们证明对于前TableSize/2(向下取整)次探测的位置都不一样
也就是对于任意的$0<i\ne j\le TableSize/2$
假设$h(x)+i^2$和$h(x)+j^2$对TableSize同余
那么$i^2,j^2$应该也同余
那么$(i+j)(i-j)$应当是TableSize整数倍
又TableSize是素数，那么$(i+j)$或$(i-j)$应当是TableSize的整数倍，但他们之和最多也不超过TableSize（因为最大取到TableSize/2，但不会有$i=j$），矛盾
证明完毕

另一方面，这些位置也不会和最初映射的位置重合，因为那意味着$i^2$（或$j^2$）模TableSize为0，TableSize为素数，所以$i$至少也要为TableSize

因此，如果哈希表至少一半都是空的，例如哈希表有7个位置，一共4个数据，那我们插入任何一个数据时，找的前3次，位置都不一样.
比如先插入a0，接着a1，如果冲突，到一个不一样的地方，接着a2，如果冲突……可以一直到a3冲突，a3依然到一个与a0,a1,a2不一样的地方

{% callout warning::Note %} 
如果素数可以写成4k+3的形式，那么$f(i)=+-i^2$
即在冲突位置先加$i^2$找下一个，再在冲突位置减$i^2$找下一个……这可以遍历整个哈希表
{% endcallout %}

```c
Position Find(ElementType Key, HashTable H) {
    Position CurrentPos;
    int CollisionNum;

    CollisionNum = 0;
    CurrentPos = Hash(Key, H->TableSize);  // 初始位置

    // 当前位置非空 且 不是目标 key → 继续探测
    while (H->TheCells[CurrentPos].Info != Empty &&
           H->TheCells[CurrentPos].Element != Key) {
        // 二次探测公式的递推形式：
        // f(i) = i²，则 f(i) - f(i-1) = 2i - 1
        CurrentPos += 2 * (++CollisionNum) - 1;
        if (CurrentPos >= H->TableSize)
            CurrentPos -= H->TableSize;  // 比取模更快
    }

    return CurrentPos;  // 返回的是下标
}
```
这里INFO包含DELETED,EMPTY,LEGITIMATE
当数据数量不超过TableSize一半时，CollisionNum的值不超过数据数量，所以CurrentPos不超过2*TableSize-1，小于2*TableSize
因此不用取模，直接减即可

如果当前位置为空，说明没找到，直接返回所找元素要插入的地方

```c
void Insert(ElementType Key, HashTable H) {
    Position Pos;
    Pos = Find(Key, H);

    // Info 有三种状态：Empty（从未用过）、Legitimate（有数据）、Deleted（已删除）
    if (H->TheCells[Pos].Info != Legitimate) {
        H->TheCells[Pos].Info    = Legitimate;
        H->TheCells[Pos].Element = Key;
    }
}
```

查找时，遇到DELETED标记不能停，即不能把删除元素认为是Empty，因为
插入时探测路径：h(x) → h(x)+1 → h(x)+4 → 找到空位
删除 h(x)+1 处的元素，设为 Empty
查找路径：h(x) → h(x)+1 发现 Empty → 认为目标不存在！（但其实在 h(x)+4）

大量删除后，插入会变得很慢，因为查找在遇到Deleted时不能停，Deleted会阻碍探测

虽然二次探测解决了一次集群问题，但初始哈希到同一个位置的键，依然会走一样的探测路径，但这比一次集群会好一些，因为初始哈希不同的元素跳的路径还是不一样的

## 4.4 Double Hashing
$$f(i) = i*hash_2(x)$$
$hash_2(x)$是第二个hash function

```
探测序列：hash(x), hash(x) + hash2(x), hash(x) + 2*hash2(x), ...
```

一个可行的第二个哈希函数
$$hash_2(x) = R - (x \\% R)$$
R 是小于 TableSize 的质数

如果double hashing正确实现，它的探测次数的期望值跟random collision resolution strategy效果差不多，大概就是不同的键拥有完全独立的探测序列且这个序列是一系列随机数

二次探测已经够好了，实际中更简单也更快

# 5 Rehashing
对于二次探测，如果表的元素填的太满，插入可能失败

1. 新建一张大小约为原来 2 倍的哈希表（取不小于 2×TableSize 的最近质数）
2. 遍历原哈希表，找出所有状态为 Legitimate 的元素
3. 用新的哈希函数（基于新 TableSize）将它们插入新表
4. 释放原表空间

如果新建的表的大小为N，在再哈希发生之前，至少已经有 N/2 个key
所以再哈希的时间复杂度$T(N)=O(N)$

什么时候触发rehash呢？
| 触发条件 | 优点 | 缺点 |
|:---:|:---:|:---:|
| 表达到半满时（λ ≥ 0.5）| 性能稳定 | 可能浪费空间 |
| 某次插入失败时 | 最大程度利用空间 | 可能已经性能很差 |
| 达到某个装载因子阈值 | 可灵活调整 | 需要调参 |


在再哈希发生之前，至少已经有 N/2 次插入（表从半满到满）。  
把 O(N) 的再哈希代价**均摊**到这 N/2 次插入上，每次插入的均摊代价只增加了常数。

但注意：**对于交互式系统**，触发再哈希的那次插入会感受到明显卡顿，因为对那次来说，是实实在在的O(N)


# 6 Appendix
以下是ai生成的一个哈希表实现，供参考和练习：

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define MinTableSize 5

typedef enum { Empty, Legitimate, Deleted } KindOfEntry;

typedef struct {
    int Element;
    KindOfEntry Info;
} Cell;

typedef struct {
    int TableSize;
    Cell *TheCells;
} HashTbl;

typedef HashTbl *HashTable;

// ---- 工具函数 ----

int IsPrime(int n) {
    if (n <= 1) return 0;
    if (n <= 3) return 1;
    if (n % 2 == 0 || n % 3 == 0) return 0;
    for (int i = 5; i * i <= n; i += 6)
        if (n % i == 0 || n % (i+2) == 0) return 0;
    return 1;
}

int NextPrime(int n) {
    if (n % 2 == 0) n++;
    while (!IsPrime(n)) n += 2;
    return n;
}

int Hash(int Key, int TableSize) {
    return Key % TableSize;
}

// ---- 核心函数 ----

HashTable InitializeTable(int TableSize) {
    if (TableSize < MinTableSize) {
        fprintf(stderr, "Table size too small\n");
        return NULL;
    }
    HashTable H = malloc(sizeof(HashTbl));
    H->TableSize = NextPrime(TableSize);
    H->TheCells  = malloc(sizeof(Cell) * H->TableSize);
    for (int i = 0; i < H->TableSize; i++)
        H->TheCells[i].Info = Empty;
    return H;
}

int Find(int Key, HashTable H) {
    int CurrentPos = Hash(Key, H->TableSize);
    int CollisionNum = 0;
    while (H->TheCells[CurrentPos].Info != Empty &&
           H->TheCells[CurrentPos].Element != Key) {
        CurrentPos += 2 * (++CollisionNum) - 1;
        if (CurrentPos >= H->TableSize)
            CurrentPos -= H->TableSize;
    }
    return CurrentPos;
}

void Insert(int Key, HashTable H) {
    int Pos = Find(Key, H);
    if (H->TheCells[Pos].Info != Legitimate) {
        H->TheCells[Pos].Info    = Legitimate;
        H->TheCells[Pos].Element = Key;
    }
}

void Delete(int Key, HashTable H) {
    int Pos = Find(Key, H);
    if (H->TheCells[Pos].Info == Legitimate)
        H->TheCells[Pos].Info = Deleted;  // 懒惰删除
}

// ---- 再哈希 ----

HashTable Rehash(HashTable H) {
    int OldSize = H->TableSize;
    Cell *OldCells = H->TheCells;

    // 创建约两倍大的新表
    HashTable NewH = InitializeTable(2 * OldSize);

    // 把旧表中所有有效元素插入新表
    for (int i = 0; i < OldSize; i++)
        if (OldCells[i].Info == Legitimate)
            Insert(OldCells[i].Element, NewH);

    free(OldCells);
    free(H);
    return NewH;
}

// ---- 测试 ----

int main() {
    HashTable H = InitializeTable(7);

    int keys[] = {13, 15, 24, 6, 18, 2};
    int n = sizeof(keys) / sizeof(keys[0]);

    printf("插入元素：");
    for (int i = 0; i < n; i++) {
        printf("%d ", keys[i]);
        Insert(keys[i], H);
    }
    printf("\n");

    printf("TableSize = %d\n", H->TableSize);

    printf("查找 15：");
    int pos = Find(15, H);
    printf(H->TheCells[pos].Info == Legitimate ? "找到，位置 %d\n" : "未找到\n", pos);

    Delete(15, H);
    printf("删除 15 后再查找：");
    pos = Find(15, H);
    printf(H->TheCells[pos].Info == Legitimate ? "找到\n" : "未找到\n");

    // 触发再哈希
    H = Rehash(H);
    printf("再哈希后，TableSize = %d\n", H->TableSize);

    free(H->TheCells);
    free(H);
    return 0;
}
```