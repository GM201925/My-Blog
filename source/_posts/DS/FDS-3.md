---
title: Chapter 2 - Lists,Stacks,Queues
date: 2026-03-14 15:11:04
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
# 0 &nbsp;Solution to Exercise

# 1 &nbsp;Abstract Data Type(ADT)
{% callout success title="Definition" %} 
Data Type = {Obejcts}$\cup${Operations}
An ADT is a data type that is orgnized in such a way that the specification(说明) on the objects and specification of the operations on the objects are separated from the representation of the objects and the implementation on the operations.
{% endcallout %}
即对于数据对象的说明、对操作的说明和数据如何表示、操作如何实现是分割开的，如下图
![test](/img/DS/DS-3-1.png)
数据对象是m x n的矩阵，矩阵由m x n个三元组构成，我们只知道这个矩阵是个什么东西，但是如何表示的，比如把三元组按顺序存到一维数组里还是三元组构成一个结构用结构数组存？还是用链表？我们不关心
同理，各种操作是怎么实现的，用什么语言，我们也不关心
ADT仅仅是说明了数据对象是什么，可以对它们进行哪些操作，即***specification***，而不关心***representation & implementation***

# 2 &nbsp;The List ADT
线性表的ADT描述：
数据对象集：n个元素构成的有序序列
操作集：
- 初始化
- 根据位置找元素
- 找某个元素第一次出现的位置
- 插入
- 删除
- 返回线性表的长度

## 2.1 Array Implementation of Lists
```c
typedef struct LNode* List;
struct LNode{
    ElementType data[MAX];
    int last;//长度为last+1
};

//初始化
List makeempty()
{
    List Lptr;
    Lptr = (List)malloc(sizeof(struct LNode));
    Lptr->last = -1;
    return Lptr;
}

//根据位置找元素 返回长度
struct LNode L;
List Lptr;
L.data[i];
Lptr->last;//长度

//找某个元素第一次出现的位置
int Find(List Lptr,ElementType x)
{
    int i = 0;
    while(i<=Lptr->last && Lptr->data[i]!=x)
    {
        ++i;
    }
    if(i>Lptr->last)
    {
        return -1;
    }
    else
    {
        return i;
    }
}
```

Insertion and Deletion
```c
//在下标i的项后面插入元素x,即data[i+1]=x;
void Insert(List Lptr,int i,ElementType x)
{
    if(Lptr->last==MAX-1)
    {
        puts("表满");
        return;
    }
    if(i < 0 || i > Lptr->last )
    {
        puts("位置invalid");
        return;
    }
    //下标i+1往后的都往后挪一个位置
    int j = 0;
    for(j=Lptr->last;j>=i+1;--j)
    {
        Lptr->data[j+1]=Lptr->data[j];
    }
    /*O(N)*/
    Lptr->data[i+1]=x;
    Lptr->last++;
}

//删除下标为i的元素，即删掉data[i]
void Delete(List Lptr,int i)
{
    if(i < 0 || i > Lptr->last )
    {
        puts("位置invalid");
        return;
    }  
    int j = 0;
    for(j=i;j<Lptr->last;++j)
    {
        Lptr->data[j]=Lptr->data[j+1];
    }
    /*O(N)*/
    Lptr->last--;
}
```
Insertion and deletion take O(N) time

## 2.2 Linked Lists
{% callout info::Dummy Head %}
A node that doesn't include data and points to the actual head of the linked list.
{% endcallout %}
```c
//节点
typedef struct LNode* List;
struct LNode{
    ElementType data;
    List next;
};

//求表长 有dummy head,它不计入表长
int Length(List head)
{
    int i = 0;
    List move = head;
    while(move->next != NULL)
    {
        ++i;
        move=move->next;
    }
    return i;
}

//查找第k个元素，dummy head是第0个元素
List FindKth(List head,int k)
{
    List move = head;
    int i = 0;
    while(i<k)
    {
        ++i;
        move=move->next;//这里要保证move不是NULL
        if(move==NULL)
        {
            /*下次循环开始前，判断move是不是NULL*/
            return NULL;
        }
    }
    return move;
}

List FindX(List head,ElementType x)
{
    List move = head->next;
    while(move!=NULL)
    {
        if(move->data==x)
            return move;
        move=move->next;
    }
    return NULL;//没找到
}
```
Insertion and Deletion
```c
//插入 在第i-1个结点后插入值为x的新结点
List Insert(int i,List head,ElementType x)
{
    //先找第i-1个结点，若i=1也没问题
    List p = FindKth(head,i-1);
    if(p==NULL)
    {
        puts("Invalid i");
        return NULL;
    }
    List s = (List)malloc(sizeof(struct LNode));
    s->data = x;
    s->next = p->next;
    p->next = s;
    return head;
}

//删除 删除第i个结点
List Delete(int i,List head)
{
    List p = FindKth(head,i-1);
    if(p==NULL)
    {
        puts("Invalid i");
        return NULL;
    }
    List tmp = p->next;//tmp指向被删除结点
    p->next = tmp->next;
    free(tmp);
    return head;
}

```
## 2.3 Doubly Linked Circular Lists
![test](/img/DS/DS-3-2.png)

## 2.4 Applications
### 2.4.1 The Polynomial ADT
Objects: $P(x)=a_1x^{e_1}+...+a_nx^{e_n}$; a set of ordered pairs of $(e_i,a_i)$ where $a_i$ is the coefficient and $e_i$ is the exponent.

我们可以用数组实现，例如$x^3+2x+1$可以用``[1,2,0,1]``表示
但是遇到$x^{1000}+x^2+1$意味着要用一个长度为1001的数组表示

用链表实现
```c
typedef struct poly_node* Poly_ptr;
struct poly_node
{
    int coefficient;//系数
    int exponent;//幂
    Poly_ptr next;
};
```
我们以次数递减排序
例如$x^3+x+1$
dummy_head->(1,3)->(1,1)->(1,0)

下面我们实现一下加法和乘法
```c
Poly_ptr createNode(int coef, int exp) 
{
    Poly_ptr newNode = (Poly_ptr)malloc(sizeof(struct poly_node));
    newNode->coefficient = coef;
    newNode->exponent = exp;
    newNode->next = NULL;
    return newNode;
}
Poly_ptr poly_add( Poly_ptr a, Poly_ptr b )
{
    //两个多项式(链表)已按次数递减排序
    //我们先创建出结果多项式
    Poly_ptr dummy_head = createNode(0,0);
    Poly_ptr move = dummy_head;

    //接着用两个指针分别读多项式a和b
    Poly_ptr pa = a->next; //a,b有dummy head
    Poly_ptr pb = b->next;

    while(pa!=NULL && pb!=NULL)
    {
        /*有点像Merge Sort*/
        if(pa->exponent > pb->exponent)
        {
            //复制a的结点到结果多项式中
            move->next = createNode(pa->coefficient,pa->exponent);

            move = move->next;
            pa = pa->next;
        }
        else if(pa->exponent < pb->exponent)
        {
            move->next = createNode(pa->coefficient,pa->exponent);
            move = move->next;
            pb = pb->next;
        }
        else
        {
            //指数相等，系数相加
            //要注意相加后系数可能为0
            int sum = pa->exponent + pb->exponent;
            if(sum != 0)
            {
                move->next = createNode(sum,pa->exponent);
                move = move->next;
            }
            pa = pa->next;
            pb = pb->next;
        }
    }

    //若a还有剩下的项
    while(pa != NULL)
    {
        move->next = createNode(pa->coefficient,pa->exponent);
        move = move->next;
        pa = pa->next;
    }

    while(pb != NULL)
    {
        move->next = createNode(pb->coefficient,pb->exponent);
        move = move->next;
        pb = pb->next;
    }

    return dummy_head;
}
```

乘法
让我们先考虑下多项式乘法的做法，即多项式a中的每一项分别与多项式b相乘，得到的多项式再相加
```c
typedef struct poly_node* Poly_ptr;
struct poly_node
{
    int coefficient;//系数
    int exponent;//幂
    Poly_ptr next;
};

Poly_ptr createNode(int coef, int exp) 
{
    Poly_ptr newNode = (Poly_ptr)malloc(sizeof(struct poly_node));
    newNode->coefficient = coef;
    newNode->exponent = exp;
    newNode->next = NULL;
    return newNode;
}

Poly_ptr poly_add( Poly_ptr a, Poly_ptr b );//实现见上

/*
=====================
single_multiply
=====================
单项乘以多项式：返回新多项式头结点（dummy head）
*/
Poly_ptr single_multiply(int coef, int exp, Poly_ptr b)
{
    Poly_ptr dummy_head = createNode(0,0);
    Poly_ptr move = dummy_head;
    Poly_ptr pb = b->next;

    while(pb != NULL)
    {
        int new_coef = coef * b->coefficient;
        if(new_coef != 0)
        {
            int new_exp = exp + pb->exponent;
            move->next = createNode(new_coef,new_exp);
            move = move->next;
        }

        pb = pb->next;
    }
    return dummy_head;
}

// 释放整个多项式（包括头结点）
void freePoly(Poly_ptr head) {
    Poly_ptr curr = head;
    while (curr) {
        Poly_ptr temp = curr;
        curr = curr->next;
        free(temp);
    }
}

Poly_ptr poly_multiply(Ploy_ptr a, Poly_ptr b)
{
    Poly_ptr result = createNode(0,0);
    Poly_ptr pa = a->next;
    while(pa != NULL)
    {
        Poly_ptr temp = single_multiply(pa->coefficient,pa->exponent,b);
        //临时多项式，加到结果上

        Poly_ptr newResult = poly_add(result,temp);
        freePoly(result); 
        //这时result变成悬空指针，result变量仍存在，只是指向的内存块(地址)被释放了
        freePoly(temp);

        //由于我们的add函数不会破坏输入的多项式
        //只能稍微复杂一些
        result = newResult;
        pa = pa->next;
    }
    return result;
}
```

### 2.4.2 Radix Sort
我们先来讲桶排序(Bucket Sort)
例如我们有n个整数要排序，范围是[0,m-1]
那么我们可以建一个大小为m的数组``count``
遍历输入的数$ai$,接着``count[ai]++``
扫描``count``数组，输出的结果就是排序结果

复杂度为$O(M+N)$(遍历输入O(N),扫描输出O(M))

问题是m很大时怎么办呢

我们对64, 8, 216, 512, 27, 729, 0, 1, 343, 125进行排序
最后应排成0,1,8,27,64,125,216,343,512,729

如果让你来排，你会怎么做？
也许我们会先找三位数，三位数找到百位最大的729，然后是512，343……
或者从一位数找起，0最小，然后1，8，两位数中27小于64因为十位2小于7；如果要排的数中有26,28,那么我们知道28更大因为个位上的8更大

好了，这有什么用呢？记住上面排序时人的想法，其实跟基数排序很像

基数排序就是按位来排，根据基数N，从最低有效位开始，每个位进行桶排序

例如64, 8, 216, 512, 27, 729, 0, 1, 343, 125
十进制数，基数为10，那么我们按个位->十位->百位的顺序来排
**第一轮 按个位排序**
建10个桶(0-9)，把数按个位放进对应的桶里
|   0   |   1   |  512  |  343  |  64   |  125  |  216  |  27   |   8   |  729  |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
|   0   |   1   |   2   |   3   |   4   |   5   |   6   |   7   |   8   |   9   |

这里最好不要用数组，有可能要排序的数的某位数都一样，所以需要n*n的空间(n为基数，如十进制n=10，代表0-9)
可以每个桶是一个链表，元素放在桶里就等于元素插入对应链表，这样空间是O(N)

**第二轮 按十位排序** 
**如果有数十位相等，要按第一轮的顺序排，因为这时要比较个位，所以第一轮大的数就应该大**

| 0<br>1<br>8 | 512<br>216 | 125<br>27<br>729 |       |  343  |       |  64   |       |       |       |
| :---------: | :--------: | :--------------: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
|      0      |     1      |        2         |   3   |   4   |   5   |   6   |   7   |   8   |   9   |

十位数相同的数，从上到下按第一轮的顺序

**第三轮 按百位排序**
百位数相同的数，从上到下按第二轮的顺序
| 0<br>1<br>8<br>27<br>64 |  125  |  216  |  343  |       |  512  |       |  729  |       |       |
| :---------------------: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
|            0            |   1   |   2   |   3   |   4   |   5   |   6   |   7   |   8   |   9   |

Then we are done !

时间复杂度为$O(p(n+b))$
其中p为轮数，即位数，如十进制三位数p=3
n为元素个数
b为桶的数量，如十进制按位划分，b=10
二进制比如是32位整数，可以8位一划分，8位8位看，b=256
例如10000000 00000001
第一轮00000001放到1号桶

每轮在做的就是：
从0号桶开始遍历每个桶(b)，遇到一个元素就放在对应桶里，共n次，故p(n+b)

虽然当b和n同阶，p为常数是复杂度是线性的，但运行时间的常数因子可能高

1. 链表的内存不连续：缓存命中率低（CPU 访问连续内存比跳着访问快很多）；
2. 指针操作开销：每个元素要存指针，插入删除要操作指针，比数组的 “连续读写” 慢；
3. 多轮遍历：即使 p=3，每一轮都要遍历所有元素和所有桶，而快排的log n因子其实不大（比如 n=1e6，log₂n≈20，3*(n+2048) vs 20n，实际中链表的开销可能让 3n 比 20n 还慢）。
### 2.4.3 Multilists

### 2.4.4 Sparse Matrix Representation
挖坑(

### 2.4.5 Cursor

# 3 &nbsp;The Stack ADT
{% callout success::Stack %}
*栈(stack)* 是限制插入和删除只能在一个位置上进行的线性表，该位置是表的末端，叫做栈的*顶(top)*
插入数据叫*入栈(push)* 删除数据叫*出栈(pop)*
特点是Last-In-First-Out(LIFO)
{% endcallout %}

## 3.1 Linked List Implementation (with a dummy head)
栈的链式存储实际上是单向链表，栈顶指针top在链表的头处
> 如果表尾作top，top指针指向表尾，则删除时找不到前面一个结点，故单向链表只有表头(dummy head)能做top

```c
typedef struct SNode{
    ElementType data;
    struct SNode* next;

}SNode;

//创建栈，即生成dummy head
SNode* Create_stack()
{
    SNode* head = (SNode*)malloc(sizeof(SNode));
    head->next = NULL;
    return head;
}

int Is_empty(SNode* head)
{
    return (head->next==NULL)
}//空的返回1

//push
void Push(SNode* head,ElementType x)
{
    SNode* new = malloc(sizeof(SNode));
    if(new==NULL)
    {
        puts("error");
        return;
    }
    new->data = x;
    new->next = head->next;
    head->next = new; 
    /*插入到head后面*/
}

//pop 删除top结点并返回栈顶元素
ElementType Pop(SNode* head)
{
    if(Is_empty(head))
    {
        puts("Empty Stack");
        return ERROR;
    }
    SNode tmp = head->next;
    head->next = tmp->next;
    ElementType pop_element = tmp->data;
    free(tmp);
    return pop_element;
}
```
频繁使用malloc和free是expensive的(为什么? Ask AI)
我们可以再建一个栈作为空间的回收站

## 3.2 Array Implementation
```c
typedef struct{
    ElementType* stack_array;
    unsigned int stack_size; //栈的容量
    int top;//top=-1表示栈空
}STACK;

STACK* create_stack(unsigned int max)
{
    STACK* s;
    s = (STACK*)(malloc(sizeof(STACK)));

    s->stack_array = (ElementType*)malloc(sizeof(ElementType)*max);
    if(s->stack_array == NULL)
    {
        free(s);
        puts("error");
        return;
    }

    s->top = -1;
    s->stack_size = max;
    return s;
}

//销毁栈
void dispose_stack(STACK* s)
{
    if(s)
    {
        free(s->stack_array);
        free(s);
    }
}
//Push
void Push(STACK* p,ElementType x)
{
    //判断栈是否满
    if(p->top == p->stack_array-1)
    {
        puts("栈满");
        return;
    }
    p->stack_array[++(p->top)] = x;
}

//Pop
ElementType Pop(STACK* p)
{
    //检查栈是否空
    if(p->top == -1)
    {
        puts("栈空");
        return ERROR;//ERROR是ElementType的特殊值
    }
    return p->stack_array[p->(top--)];
}
```
>The stack model must be well encapsulated.  That is, no part of your code, except for the stack routines, can attempt to access the Array or TopOfStack variable.
\
你的代码里，只有栈自己的操作函数（比如 push、pop、top、create_stack 这些，教材里叫 “stack routines”），才能直接碰两样东西：
存元素的数组（Array，对应教材里的 stack_array）；
记录栈顶的变量（TopOfStack，对应教材里的 top_of_stack）。
除了这些函数之外，其他任何代码（比如你的主函数 main、或者其他业务逻辑函数），都绝对不能直接去读写这两个变量。

