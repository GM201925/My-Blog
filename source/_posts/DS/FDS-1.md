---
title: Chapter 1 - Algorithm Analysis(a)
date: 2026-03-07 14:48:16
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
# 1 &nbsp;Algorithm Analysis
## 1.1 Definition
{% callout success [fa-style] [fa-icon] [extra-classes...]::Definition %}
An algorithm is a finite set of instructions that,if followed,accomplishes a particular task.
{% endcallout %}
In addition,all algorithms must satisfy the following criteria:
{% tabs %}

<!-- tab Input -->

There are zero or more quantities that are externally supplied.

<!-- tab Output -->

At least one quantity is produced.

<!-- tab Definiteness -->

Each instruction is clear and unambiguous.

<!-- tab Finiteness -->

算法在有限步骤后会结束

<!-- tab Effectiveness -->

每个指令要足够基础，原则上，一个人用纸和笔就能执行，每个指令要是可行的(feasible)
{% endtabs %}
{% callout warning::Note %} 
程序是用编程语言写成的，而且不一定是有限步后结束的，比如操作系统
算法可以用自然语言、流程图、伪代码等描述
{% endcallout %}

## 1.2 What to Analyze
程序的运行时间可能和运行的机器或编译器有关，我们主要分析***time and space complexities(machine & compiler-independent)***
{% callout primary::Assumptions %} 
1. Instructions are executed sequentially.
2. Each instruction is simple,and takes exactly ***one time unit***.
3. Integer size is fixed and we have infinite memory.
{% endcallout %}
我们通常考虑平均时间复杂度或最坏情况下的时间复杂度，这里的最坏情况是指**最坏的输入**下，程序的时间复杂度是多少
```c
void add(int a[][MAX],int b[][MAX],int c[][MAX],int rows,int cols)
{
    for(int i = 0;i<rows;++i) 
    //执行rows+1次
    //(最后一次判断也算for循环这个语句执行一次，不过这个其实不重要)
    {
        for(int j = 0;j<cols;++j) //执行rows(cols+1)次
        {
            c[i][j] = a[i][j] + b[i][j]; //执行rows·cols次
        }
    }
}
```
一共执行了多少**次**语句，就是时间复杂度

$T(rows,cols)=2rows\cdot cols+2rows+1$

```c
float rsum(float list[],int n)
{
    if(n) //1次
    {
        return rsum(list,n-1) + list[n-1]; //1次
    }
    return 0; //1次
}
```
每个rsum都有2次，直到第二个参数为0，共2n+2次，如n=2，2次(其实是先判断一次，然后进到rsum(1)，rsum(1)返回结果后有个return那才是第二次)到rsum(1)，再2次到rsum(0)，rsum(0)又有2次

## 1.3 Asymptotic(渐进的) Notation
我们数一共执行了多少次语句，其实是想预测当$n$增长时，运行时间会如何增长，以此来比较两个程序的时间复杂度
例如一个程序执行$n^2$次，另一个程序执行$n+3$次，当$n=1,2$时看上去第二个程序执行的次数更多，但对于足够大的n，第二个程序的执行次数更少，运行时间更短
{% callout success::Definition %}
如果存在正数c和n，使得当N>n时，$T(N)\le c\cdot f(N)$
那么$T(N)=O(f(N))$

如果存在正数c和n，使得当N>n时，$T(N)\ge c\cdot f(N)$
那么$T(N)=\Omega(f(N))$

如果$T(N)=\Omega(f(N))$且$T(N)=O(f(N))$，那么$T(N)=\Theta(f(N))$
{% endcallout %}

当我们说一个程序的时间复杂度是$O(N)$时，我们也可以说复杂度是$O(N^2)$，但我们总选择最小的那个，也就是说其实我们是在说程序的复杂度是$\Theta(N)$

### 1.3.1 Rules of Astmptotic Notation

如果$T\_1(N)=O(f(N))$而$T_2(N)=O(g(N))$,那么
1. $T\_1(N)+T\_2(N)=max(O(f(N)),O(g(N)))$
2. $T\_1(N)*T\_2(N)=O(f(N)*g(N))$

如果$T(N)$是k次多项式，那么$T(N)=\Theta(N^k)$

$log^kN=O(N)$
{% callout warning ::Note %}
当比较两个程序的时间复杂度时，我们认为N是无限大的
{% endcallout %}

```c
void add(int a[][MAX],int b[][MAX],int c[][MAX],int rows,int cols)
{
    for(int i = 0;i<rows;++i) 
    // 1
    {
        for(int j = 0;j<cols;++j) // 2
        {
            c[i][j] = a[i][j] + b[i][j]; // 3
        }
    }
}
```
1. $\Theta(rows)(次？)$
2. $\Theta(rows\cdot cols)$
3. $\Theta(rows\cdot cols)$
   
$T(rows,cols)=\Theta(rows\cdot cols)$

for循环的运行时间至多是循环内部的语句的运行时间乘循环次数
嵌套for循环：所以循环的循环次数相乘，再乘最里面的语句的运行时间
```c
for(int i = 0;i<n;++i)
{
    for(int j  = 0;j<n;++j)
    {
        a+=1;
    }
}
//n*n*1，复杂度为O(N^2)
```
条件语句的运行时间不超过判断的运行时间加上各种情况里最长的运行时间

### 1.3.2 Recursion
```c
long int Fib(int n)
{
    if(n<=1)
    {
        return 1;
    }
    else
    {
        return Fib(n-1) + Fib(n-2);
        //1+T(N-1)+T(N-2)
    }
}
```
时间复杂度和空间复杂度是多少？
时间复杂度$T(N)=T(N-1)+T(N-2)+2$
令$T(N)+2=Q(N)$
那么$$Q(N)=Q(N-1)+Q(N-2)$$
(用高中学习的数列递推方法)斐波那契数列通项是$$F(n)=\frac{1}{\sqrt{5}}((\frac{1+\sqrt{5}}{2})^n-(\frac{1-\sqrt{5}}{2})^n)$$
那么$T(N)=O((\frac{1+\sqrt{5}}{2})^N)$
{% callout info::空间复杂度 %}
递归算法的空间复杂度等于递归树的最大深度
例如计算F(4)
1. 调用F(4)，入栈，深度为1
2. F(4)需要先算F(3)，入栈，深度=2
3. F(3)需要先算F(2)，入栈，深度=3
4. F(2)需要先算F(1)，入栈，深度=4
5. F(1)返回1，出栈，深度=3
6. F(2)接着调用F(0)，入栈，深度=4
...
对于F(N),最长的一条调用路径是F(N)->F(N-1)->...->F(1)
路径长度为N，故空间复杂度为$O(N)$
{% endcallout %}

### 1.3.3 Master Theorem
我们考虑这样一种递归算法的时间复杂度
$$T(n)=aT(\frac{n}{b})+f(n)$$
$a$是把原问题拆成的子问题个数
$\frac{n}{b}$是子问题规模，每个小任务是原问题的几分之几
$f(n)$是除递归调用外的额外工作量
$$T(n)=aT(\frac{n}{b})+f(n)=a(aT(\frac{n}{b^2})+f(\frac{n}{b}))+f(n)=...$$
$$T(n)=a^kT(\frac{n}{b^k})+\sum_{i=0}^{k-1}a^if(\frac{n}{b^i})$$
当子问题规模降到1时，即$k=log_bn$进入base case，T(1)我们一般是知道是多少的，于是由换底公式
$$T(n)=n^{log_ba}T(1)+\sum_{i=0}^{log_bn-1}a^if(\frac{n}{b^i})$$
```c
int BS(int arr[],int left,int right,int x)
{
    int mid = (left+right)/2;
    if(left>right)
    {
        return -1;
    }
    if(arr[mid]==x)
    {
        return mid;
    }
    else if(arr[mid]>x)
    {
        return BS(arr,left,mid-1,x);
    }
    else if(arr[mid]<x)
    {
        return BS(arr,mid+1,right,x);
    }    
}
```
{% callout primary ::二分查找 %}

$T(1)=1,T(n)=T(\frac{n}{2})+1$(最后加几无所谓，判断、返回等等可能都会加，但都是O(1))，可以看到，我们每次规模减半，所以b=2，每次只再来一次调用，也就是原问题进入到一个子问题，所以a=1
最后可以得到$T(n)=O(logn)$
{% endcallout %}
对于主定理，假设$f(n)=n^d$，我们有个结论，若$n^{log_ba}>n^d$，那么$T(n)=O(n^{log_ba})$
若$f(n)$大，$T(n)=O(n^d)$
若一样大，即$log_ba=d,T(n)=O(n^dlogn)$

## 1.4 Exercise
最后来一道练习，求下面两个程序的时间复杂度
有点难，答案也许会在下一篇文章公布
```c
//程序1
for(int i = 0;i<n;++i)
{
    for(int j = 0;j<i;++j)
    {
        x+=1;
    }
}

//程序2
for(int i = 0;i<n*n*n/2;++i)
{
    for(int j = n;j>i;--j)
    {
        for(int m = 0;m<n;++m)
        {
            x+=1;
        }
    }
}
```
{% folding class="purple" title="Hint" open=false %}
当比较两个程序的时间复杂度时，我们认为N是无限大的
{% endfolding %}

[回到目录](https://fuyukawa.netlify.app/categories/Data-Structure/)