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
Poly_ptr Add( Poly_ptr a, Poly_ptr b )
{
    //两个多项式(链表)已按次数递减排序
    //我们先创建出结果多项式
    Poly_ptr dummy_head = createNode(0,0);
    

    bc*a+d-
}
```
乘法
```c

```