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
