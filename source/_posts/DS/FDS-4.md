---
title: Chapter 3 - Tree
date: 2026-03-28 18:14:41
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

# 1 &nbsp;Preliminaries
{% callout success::Definition %} 
A tree is a collection of nodes.The collection can be empty;otherwise, a tree consists of
1. a root
2. 0 or more nonempty subtrees
{% endcallout %}
一棵树有n个结点，除了root，每个结点上面都有一条边，所以这棵树有n-1条边
{% callout info %} 
结点的度(degree of a node): 这个结点的子树的个数

树的度: 这棵树每个结点的度的最大值

parent: 有子树的结点

children: the roots of the subtrees of a parent

sibilings: children of the same parent

leaf: a node with degree 0

path from $n_1$ to $n_k$: 在树中从$n_1$到$n_k$的序列，由于子树不能有交集，所以这个路径是唯一的

length of path: number of edges on the path

depth of $n_i$: 从根到这个结点的路径长度，根的深度是0，例如根的孩子深度是1(有些也规定为根的深度是1，即全部+1)

height of $n_i$: 从$n_i$到一个叶结点的最长路径长度

height(depth) of a tree: root的height
{% endcallout %}

# 2 &nbsp;Implemention
直接用链表的话，每个结点无法统一，比如根可能有3个child，它的第一个child只有1个child，它的第二个child有5个child，第三个child没有child
如果要统一结点，那么每个结点都有5个指针，对于第三个child就全浪费了

可以使用***FirstChild-NextSibiling Representation***
对每个结点，包括root，它一个指针指向它的第一个child，另一个指针指向它的一个sibiling
![](/img/DS/DS-4-1.png)
表示不唯一，A的firstchild完全可以是C，C的sibilng是B这样

# 3 &nbsp;Binary Trees
A binary tree is a tree in which no node can have more than two children.
**二叉树的子树有左右之分**

性质：
- 一个二叉树第i层最大结点数是$2^{i-1}$
- 深度为k的二叉树的最大总结点数是$2^{k+1}-1$
- 对任何非空二叉树，$n_0$表示叶结点个数，$n_2$是度为2的结点个数，那么$n_0=n_2+1$
>证明：边数，除了root，每个结点的上面都有一条边，边数为$n_0+n_1+n_2-1$，再考虑每个结点下面的边，边数为$2n_2+n_1$，两者相等即得证

## 3.1 Expression Tree
例 已知后缀表达式 ab+cde+**
求中缀表达式
我们遇到操作数就push，遇到operand就以operand为根，pop出两个操作数作为左右结点，这个整体再放到栈里作为一个操作数，重复下去就得到一棵树

对这棵树中序遍历，就得到中缀表达式，后序遍历就得到后缀表达式，前序遍历就得到前缀表达式

ai的实现
```c
#include <stdio.h>
#include <stdlib.h>
#include <ctype.h>

// 1. 定义二叉树节点
typedef struct TreeNode {
    char data;
    struct TreeNode *left;
    struct TreeNode *right;
} TreeNode;

// 创建新节点的辅助函数
TreeNode* createNode(char data) {
    TreeNode* newNode = (TreeNode*)malloc(sizeof(TreeNode));
    newNode->data = data;
    newNode->left = newNode->right = NULL;
    return newNode;
}

// 2. 定义栈（存放的是 TreeNode 的指针）
typedef struct Stack {
    TreeNode* array[100];
    int top;
} Stack;

void push(Stack* s, TreeNode* node) {
    s->array[++(s->top)] = node;
}

TreeNode* pop(Stack* s) {
    return s->array[(s->top)--];
}

// 3. 核心算法：构建表达式树
TreeNode* buildExpressionTree(char postfix[]) {
    Stack s;
    s.top = -1;
    TreeNode *t, *t1, *t2;

    for (int i = 0; postfix[i] != '\0'; i++) {
        // 如果是操作数（字母或数字），建立叶节点入栈
        if (isalnum(postfix[i])) {
            t = createNode(postfix[i]);
            push(&s, t);
        } 
        // 如果是操作符
        else {
            t = createNode(postfix[i]);
            
            // 弹出两个操作数（注意：先弹出来的是右孩子，后弹出来的是左孩子）
            t1 = pop(&s); // 右
            t2 = pop(&s); // 左
            
            // 建立连接
            t->right = t1;
            t->left = t2;
            
            // 把这棵合并后的子树压回栈
            push(&s, t);
        }
    }
    // 最终栈顶剩下的就是整棵树的根节点
    return pop(&s);
}

// 验证：通过中序遍历打印（带括号），看看是否还原了表达式
void inorder(TreeNode* root) {
    if (root) {
        if (root->left) printf("("); // 如果有孩子，说明是操作符，加括号
        inorder(root->left);
        printf("%c", root->data);
        inorder(root->right);
        if (root->right) printf(")");
    }
}

int main() {
    char postfix[] = "ab+c*"; // 对应 (a+b)*c
    printf("后缀表达式: %s\n", postfix);
    
    TreeNode* root = buildExpressionTree(postfix);
    
    printf("还原中缀表达式: ");
    inorder(root);
    printf("\n");
    
    return 0;
}
```

## 3.2 Tree Traversals

```c
//Preorder Traversal
void preorder(Tree* tree)
{
    if(tree)
    {
        visit(tree);
        preorder(tree->left);
        preorder(tree->right);
    }
}

//Postorder Traversal
void postorder(Tree* tree)
{
    if(tree)
    {
        postorder(tree->left);
        postorder(tree->right);
        visit(tree);
    }
}

//Inorder Traversal
void inorder(Tree* tree)
{
    if(tree)
    {
        inorder(tree->left);
        visit(tree);
        inorder(tree->right);
    }
}

//Levelorder Traversal
void levelorder(Tree* tree)
{
    enqueue(tree);
    while(queue is not empty)
    {
        visit(T=dequeue());
        for(each child C of T)
        {
            enqueue(C);
        }
    }
}
```
### 3.2.1 中序非递归遍历
>递归是栈的应用之一，所以我们可以用栈直接实现中序遍历

画图试试👇
![alt text](img/DS/DS-4-2.png)
算法如下
- 遇到一个结点，push进去，然后遍历它的左子树
- 左子树遍历结束后，从栈顶pop出这个结点并访问它
- 然后按它的右指针再去中序遍历该结点右子树（回到第一步）
>对上图
> 1. 遇到a，把a push进去，再到b
> 2. 遇到b依然执行第一步，==把b push进去==，由于左指针空，到第二步，==pop出b==并打印元素，然后b进行第三步，到d
> 3. d再执行第一步，把d push进去，左空，pop出d，右空
> 4. 这时a的第一步结束了，来到第二步，==pop出a==并打印元素
> 5. 来到a的第三步，到c，再对c执行三个步骤。push c，push e，pop出e，pop出c，c的右指针为空，结束a的第三步
```c
//先复习下stack
typedef struct SNode{
    ElementType data;
    struct SNode* next;

}SNode;

//创建栈，即生成dummy head
SNode* Creatstack()
{
    SNode* head = (SNode*)malloc(sizeof(SNode));
    head->next = NULL;
    return head;
}

int Isempty(SNode* head)
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

}

//pop 删除top结点并返回栈顶元素
ElementType Pop(SNode* head)
{
    if(Isempty(head))
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


//二叉树中序非递归遍历
typedef struct TreeNode* Tree
struct TreeNode{
    int element;//方便起见，这里用int
    Tree left;
    Tree right;
};

typedef struct SNode{
    Tree bintree;//注意这里，栈里面是指向树的结点的指针
    struct SNode* next;

}SNode;

void InorderTraversal(Tree bintree)
{
    SNode* stack = Createstack();//创建栈
    while(bintree || !Isempty(stack))
    {
        while(bintree)
        {
            Push(stack,bintree);//push进去
            bintree = bintree->left;
        }

        //循环结束意味着最后一个push进去的结点没有左子树了
        if(!Isempty(stack))
        {
            bintree = Pop(stack);
            //其实top的元素，现在的步骤就是它的步骤
            //如pop出a，说明现在是a的第二步，故stack不空说明仍处在某个结点的步骤中
            printf("%d\n",bintree->element);//第二步，打印中间的
            bintree = bintree->right;
            //进入第三步，对右边的结点重复上面步骤

        }
    }
}

```
>事实上，中序、先序、后序遍历中每个结点只是访问时机不同，所以上面的程序，只要把printf的顺序改变，就变成先/后序遍历

### 3.2.2 层序遍历实现
>对二叉树的遍历实际上是把二维结构线性化的过程，比如访问左儿子后，怎么回到父结点、访问右儿子，需要用到存储结构栈、队列保存暂时未访问的结点

画图试试👇
![alt text](img/DS/4-5.png)
算法：根结点入队，然后
- 从队列取出一个元素（结点的指针）
- 访问该元素所指结点
- 将该元素所指结点的左、右child入队，回到第一步
```c
//层序遍历
typedef struct TreeNode* Tree;
struct TreeNode{
    int element;//方便起见，这里用int
    Tree left;
    Tree right;
};


struct Node{
    Tree bintree;
    struct Node* next;
};

typedef struct{
    struct Node* front;
    struct Node* rear;
}QNode;

QNode* Createqueue()
{
    QNode* q = malloc(sizeof(QNode));
    if(q==NULL)
    {
        puts("创建队列失败");
        return NULL;
    }
    q->front = NULL;
    q->rear = NULL;
    return q;
}

int Isempty(QNode* q)
{
    return q->front==NULL;
}

//Enqueue 
void EnQueue(QNode* q,Tree x)
{
    struct Node* new = malloc(sizeof(struct Node));
    if(new==NULL)
    {
        puts("内存分配失败");
        return;
    }
    new->bintree = x;
    new->next = NULL;
    if(Isempty(q))
    {
        //空队列，rear和front都要指向new所指的
        q->rear = new;
        q->front = new;
    }
    else
    {
        //非空队列，还要连接上两个元素
        q->rear->next = new;
        q->rear = new;

    }
}

//Dequeue 无dummy head
Tree DeQueue(QNode* q)
{
    if(Isempty(q))
    {
        puts("空队列，无法出队");
        return NULL;
    }
    Tree tmp = q->front->bintree;
    struct Node* tmpptr = q->front;
    if(q->front==q->rear)
    {
        //只有一个结点
        q->front = NULL;
        q->rear = NULL;
    }
    else
    {
        q->front = q->front->next;
    }
    free(tmpptr);
    return tmp;
}

//层序遍历函数
void LevelOrderTraversal(Tree bintree)//如上图bintree指向a
{
    if(!bintree)
    {
        return;//空树直接返回
    }
    QNode* q = Createqueue();
    EnQueue(q,bintree);
    Tree tmp;
    while(!Isempty(q))
    {
        tmp = DeQueue(q);
        printf("%d\n",tmp->element);
        if(tmp->left)
        {
            EnQueue(q,tmp->left);
        }
        if(tmp->right)
        {
            EnQueue(q,tmp->right);
        }
    }

}
```
```c
//测试
#include <stdio.h>
#include <stdlib.h>
int main()
{
    struct TreeNode a = { 1 };
    struct TreeNode b = { 2 };
    struct TreeNode c = { 3 };
    struct TreeNode d = { 4 };
    struct TreeNode e = { 5 };
    struct TreeNode f = { 6 };
    a.left = &b;
    a.right = &c;
    b.left = &d;
    b.right = &e;
    e.left = &f;
    LevelOrderTraversal(&a);
    //1 2 3 4 5 6
    return 0;
}


```
### 3.2.3 遍历应用例子
输出二叉树叶结点
```c
void PreOrderPrintLeaves(Tree bintree)
{
    if(bintree)
    {
        if(!bintree->left && !bintree->right)
        {
            printf("%d\n",bintree->element);
        }
        PreOrderPrintLeaves(bintree->left);//左子树
        PreOrderPrintLeaves(bintree->right);//右子树
    }
}
```
求二叉树高度
左、右子树高度的最大值+1
```c
int PostOrderGetHeight(Tree bintree)
{
    if(bintree)
    {
        int HL = PostOrderGetHeight(bintree->left);
        int RL = PostOrderGetHeight(bintree->right);
        int max = (HL>RL)?HL:RL;
        return max+1;
    }
    return 0;
}

```
先序+中序或后序+中序可以确定二叉树
- 根据先序序列第一个结点找到root
- 中序中的根结点左右分别是左子树、右子树
- 对左子树、右子树用相同方法分解
- 如
先序:a b c d e f g h i j
中序:c b e d a h g i j f
> 根为a，左子树为cbed，右子树为hgijf
> 现在对左子树重复操作，左子树先序遍历先遍历b，说明b是左子树的root，类似地可以得到b的左子树是c，右子树是de
> 再由先序序列d、e知右子树d是根，由中序知道e是d的左子树
> 右子树同样操作，就能确定
{% folding title="Exercise" class="purple" %}
**Problem Description**:Suppose that all the keys in a binary tree are distinct positive integers. Given the preorder and inorder traversal sequences, you are supposed to output the level-order traversal sequence of the corresponding binary tree, and find the maximum width of the tree. The width of a level is defined as the number of nodes in that level.

**Input Specification**:Each input file contains one test case. For each case, the first line contains a positive integer $N$ ($\le 30$), which is the total number of nodes in the tree. The second line contains the preorder sequence and the third line contains the inorder sequence. All the numbers in a line are separated by a space.

**Output Specification**:For each test case, first print the level-order traversal sequence in one line. All the numbers must be separated by exactly one space, and there must be no extra space at the end of the line.In the second line, print the total depth and the maximum width of the tree, separated by one space.
Sample Input:
7
1 2 4 5 3 6 7
4 2 5 1 6 3 7
Sample Output:
1 2 3 4 5 6 7
3 4

**解答**
这道题目怎么根据前序+中序确定树很关键。
```c
#include <stdio.h>
#include <stdlib.h>

typedef struct TreeNode{
    int data;
    struct TreeNode* left;
    struct TreeNode* right;
}TreeNode;

TreeNode* buildTree(int* pre, int* in, int n)
// pre是这棵树在前序遍历数组开始的位置，in是这棵树在中序遍历数组开始的位置，n是这棵树总结点数
{
    if(n <= 0)
    {
        return NULL;
    }
    int r = pre[0];
    int i = 0;
    for(i=0;i<n;++i)
    {
        if(in[i]==r)
        {
            break;
        }
    }
    int left = i; //左子树有i个结点
    int right = n - 1 - i;
    TreeNode* root = malloc(sizeof(TreeNode));
    root->data = r;
    root->left = buildTree(pre+1, in, left);
    root->right = buildTree(pre+left+1, in+i+1, right);

    return root;
}

void levelOrderAnalysis(TreeNode* root, int n)
// 层序遍历打印，并统计每层宽度
{
    if(!root)
    {
        return;
    }
    TreeNode* queue[100];
    int front = 0;
    int rear = 0;
    int result[100] = {0}; //储存层序的数，方便打印
    int res_count = 0;
    queue[rear++] = root;
    int maxDepth = 0;
    int maxWidth = 0;

    while(front < rear)
    {
        int size = rear - front;
        // 这层有多少个结点
        if(size > maxWidth)
        {
            maxWidth = size;
        }

        maxDepth++;
        for(int i = 0;i<size;++i)
        {
            // 处理每个结点的子结点
            TreeNode* cur = queue[front++];
            result[res_count++] = cur->data;
            if(cur->left) queue[rear++] = cur->left;
            if(cur->right) queue[rear++] = cur->right;

        }
    }
    for (int i = 0; i < res_count; i++) {
        printf("%d%c", result[i], (i == res_count - 1 ? '\n' : ' '));
    }
    printf("%d %d\n", maxDepth, maxWidth);

}

int main() 
{
    int n;
    if (scanf("%d", &n) != 1) return 0;

    int pre[35], in[35];
    for (int i = 0; i < n; i++) scanf("%d", &pre[i]);
    for (int i = 0; i < n; i++) scanf("%d", &in[i]);

    TreeNode* root = buildTree(pre, in, n);

    levelOrderAnalysis(root, n);

    return 0;
}
```
{% endfolding %}

# 4 Binary Search Tree
斜二叉树(Skewed Binary Trees)
就是斜的一条链表

完美二叉树(Perfect Binary Tree)：所有叶子节点都在同一层，且所有内部节点的度都为2

完全二叉树(Complete Binary Tree)：除最后一层外，其余层都被完全填满，并且最后一层的所有节点都尽可能地靠左排列

## 4.1 Definition
{% callout success::Definition %} 
A binary search tree is a binary tree. It may be empty.  If it is not empty, it satisfies the following properties:
(1)  Every node has a key which is an ***integer***, and the keys are ***distinct***.
(2)  The keys in a nonempty left subtree must be smaller than the key in the root of the subtree.
(3)  The keys in a nonempty right subtree must be larger than the key in the root of the subtree.
(4)  The left and right subtrees are also binary search trees.

{% endcallout %}

## 4.2 Find
```c
typedef struct TreeNode* Tree
struct TreeNode{
    ElementType element;
    Tree left;
    Tree right;
};

Tree Find(Tree bst,ElementType x)
{
    if(!bst)
    {
        return NULL;//树空
    }
    if(x > bst->element)
    {
        return Find(bst->right,x);//右子树为根再找
    }
    else if(x < bst->element)
    {
        return Find(bst->left,x);
    }
    else
    {
        return bst;
    }
}

//迭代实现
Tree IterFind(Tree bst,ElementType x)
{
    while(bst)
    {
        if(x > bst->element)
        {
            bst = bst->right;
        }
        else if(x < bst->element)
        {
            bst = bst->left;
        }
        else
        {
            return bst;
        }
    }
    return NULL;
}

//根据二叉搜索树的特点，最大元素在最右边，
// 最小元素在最左边
Tree FindMax(Tree bintree)
{
    if(!bintree)
    {
        return NULL;
    }
    while(bintree->right)
    {
        bintree = bintree->right;
    }
    return bintree;
}

Tree FindMin(Tree bintree)
{
    if(bintree)
    {
        while(bintree->left)
        {
            bintree = bintree->left;
        }
        return bintree;
    }
    return NULL;
}

//也可以递归
Tree FindMin(Tree bintree)
{
    if(!bintree)
    {
        return NULL;
    }
    else if(bintree->left==NULL)
    {
        return bintree;
    }
    else
    {
        return FindMin(bintree->left);
    }
}
```

## 4.3 Insert
![](img/DS/4-3.png)
寒假预习时的解释：

>我们先来分析怎么插入，以上图为例
空树，先创建一个结点，放Jan
下一个Feb，F跟J比较，比J小，在Jan左边，跟Jan的左子树的根比较，Jan的left为NULL，这时创建结点放Feb
以此类推，我们看看怎么插入Dec
先跟Jan比，比J小
跟F比，比F小
跟A比，比A大
跟Au比，还是大
跟Aug的right比，right是空的，故放在Aug的right
\
上面的过程也可以看作，跟Jan比，比Jan小，一定在Jan的左边，那么问题变成在Jan的左子树插入Dec，返回新的左子树根结点，让它成为Jan的left，最后一步就是在Aug的右子树插入Dec，而右子树为空，所以Dec成为Aug的右子树的根

我们还可以从另一个角度看，首先插入这个元素前，我们要先检查它是不是在BST里，所以查找，当查到最后一个元素时，如果它在，那就不用插入了，如果它不在，那么这最后一个元素的左/右儿子一定是NULL，把它插入到这里就可以了。

就像上例插入Dec，最后一个元素是Aug，Dec比它大，而Aug的右儿子是空的，所以Dec自然地要插入到那里。

但问题是，我们怎么找到这个“最后一个元素”呢？可以借鉴查找的算法，先比较当前结点和插入结点，按照大小关系分别在左/右子树里调用插入函数，如果这个树是空的，说明这时前一次调用时的那个根就是我们要找的“最后一个元素”，上例来看，就是调用``Aug->right = Insert(Aug->right,Dec)``时发现``Aug->right``是空的。所以创建一个结点返回回去就行了

```c
Tree Insert(ElementType x, Tree bst)
{
    if(!bst)
    {
        bst = malloc(sizeof(struct TreeNode));
        bst->left = NULL;
        bst->right = NULL;
        bst->element = x;
    }
    else if(x > bst->element)
    {
        bst->right = Insert(x,bst->right);
    }
    else if(x < bst->element)
    {
        bst->left = Insert(x,bst->left);
    }

    // else x在树里，啥也不用做
    return bst;

}
```

查找和插入的时间复杂度都是$O(D)$,$D$为树的深度

## 4.4 Delete
![我 抄 袭 我 自 己](img/DS/4-4.png)
原结点拷贝最大/最小元素的值，再删掉右子树的最小元素或左子树的最大元素，右子树的最小元素或左子树的最大元素一定不会有左右子树，这就转化成前两种情况

```c
Tree Delete(ElementType x, Tree bst)
{
    if(bst==NULL)
    {
        Error("Element not found");
    }
    else if(x > bst->element)
    {
        bst->right = Delete(x,bst->right);
    }
    else if(x < bst->element)
    {
        bst->left = Delete(x,bst->left);
    }
    else
    {
        //找到了
        if(bst->left && bst->right)
        {
            Tree tmp = FindMin(bst->right);
            bst->element = tmp->element;
            bst->right = Delete(tmp->element,bst->right);
        }
        else
        {
            // 0个孩子或一个孩子
            if(bst->left==NULL)
            {
                Tree tmp = bst;
                bst = bst->right;
                free(tmp);
            }
            else if(bst->right==NULL)
            {
                Tree tmp = bst;
                bst = bst->left;
                free(tmp);
            }
        }
    }
    return bst;
}
```
整个删除过程分为寻找删除结点+执行删除(else部分)，在递归下降查找阶段，设树的高度为h，设找到待删除结点的时间复杂度为T(h)，$T(h) = T(h-1) + O(1)$,可以得到$T(h) = O(d), d$为待删除结点在原树的深度，而$O(d)=O(h)$，之后进入else分支，当有两个孩子的情况最复杂，首先FindMin是$O(h)$，删除部分由于是删除右子树最小元素，找到最小元素$O(h)$，删掉$O(1)$，因此else分支部分时间复杂度为$O(h)$

最坏情况下，$h=N$，平均和最好情况下，$h=O(logN)$