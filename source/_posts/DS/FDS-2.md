---
title: Chapter 1 - Algorithm Analysis(b)
date: 2026-03-11 17:43:10
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

# 0 &nbsp;Solution to Exercise

对于程序1，$i=0$，最内层语句执行0次，$i=1$，最内层语句执行1次，以此类推
最内层语句执行$$1+2+...+n-1=O(n^2)$$

对于程序2，注意第二层循环的条件，$i$必须在$0\le i \le n-1$时第二层循环内部的语句才会执行，且重复$n-i$次
因此复杂度为$$n\sum^{n-1}_{i=0}(n-i)=O(n^3)$$
对于$i$的其他取值，判断一下后不会执行内层循环，判断执行$n^3/2 - n$次
故时间复杂度为O(n^3)
# 1 &nbsp;Compare the Algorithms
{% callout primary title="Problem" %}
Given(possibly negative) integers $A_1,A_2...,A_n$,find the maximum value of $\sum\_{k=i}^jA_k$
{% endcallout %}

## 1.1 Algorithm 01
```c
//Algorithm 01
int MaxSubsequenceSum(const int a[],int n)
{
    int thissum = 0;
    int maxsum = 0;
    int i = 0;
    int j = 0;
    int k = 0;
    for(i=0;i<n;++i)//确定起点
    {
        for(j=i;j<n;++j)//j确定终点
        {
            for(k=i;k<=j;++k)//从i项加到j项
            {
                thissum+=a[k];
            }
            if(thissum>maxsum)
            {
                maxsum=thissum;
            }
        }
    }
    return maxsum;
}
```
三层循环总共运行的次数为
$$
\sum_{i=0}^{n-1}\sum_{j=i}^{n-1}\sum_{k=i}^{j}1
$$
$$
\sum_{k=i}^{j}1=j-i+1
$$
$$
\sum_{j=i}^{n-1}(j-i+1)=1+2+...+n-i
$$
$$
=(n-i)(n+1-i)/2
$$
$
\sum_{i=0}^{n-1}(n-i)(n+1-i)
$

$=n^2+n-(2n+1)\sum_{i=0}^{n-1}i+\sum_{i=0}^{n-1}i^2$

$=n^2+n-n(2n+1)(n-1)/2+n(n-1)(2n-1)/6$

>故$T(n)=O(n^3)$

## 1.2 Algorithm 02
```c
/*上面的算法，如i=0，它是这么算的，从第0项加到第1项
又从第0项加到第2项，又从第0项加到第3项……
其实i=0时加到第1项，再算0-2的和时不用再从头开始加
即k那个循环是多余的
*/
//Algorithm 02
int MaxSubsequenceSum(const int a[],int n)
{
    int thissum = 0;
    int maxsum = 0;
    int i = 0;
    int j = 0;
    for(i=0;i<n;++i)//i是子列左端位置
    {
        thissum=0;
        for(j=i;j<n;++j)//j是子列右端位置
        {
            thissum+=a[j];
            if(thissum>maxsum)
            {
                maxsum=thissum;
            }
        }
    }
    return maxsum;
}
```
>$T(n)=O(n^2)$

## 1.3 Algorithm 03
分而治之
![text](/img/DS/DS-2-1.png)
```c
int MaxSubsequenceSum(const int a[],int n)
{
    return MaxSubSum(a,0,n-1);
}

int MaxSubSum(const int a[],int left,int right)
{
    //base case
    if(left==right)
    {
        if(a[left]>0)
        {
            return a[left];
        }
        return 0;
    }
    int mid = (left+right)/2;
    int max_right_sum = MaxSubSum(a,mid+1,right);
    int max_left_sum = MaxSubSum(a,left,mid);

    //接下来算越过中间的最大子列和
    //分别向两边扫
    int max_right_border_sum = 0;
    int right_border_sum = 0;
    for(int i = mid+1;i<=right;++i)
    {
        right_border_sum += a[i];
        if(right_border_sum > max_right_border_sum)
        {
            max_right_border_sum = right_border_sum;
        }
    }

    //往左扫，O(N)
    int max_left_border_sum = 0;
    int left_border_sum = 0;
    for(int i = mid;i>=left;++i)
    {
        left_border_sum += a[i];
        if(left_border_sum > max_left_border_sum)
        {
            max_left_border_sum = left_border_sum;
        }
    }
    int border_sum = max_left_border_sum + max_right_border_sum;
    
    return max(border_sum,max_left_sum,max_right_sum);

}
```
### 1.4 Algorithm 04(on-line algorithm)
```c
int MaxSubsequenceSum(const int a[],int n)
{
    int i = 0;
    int thissum = 0;
    int maxsum = 0;
    for(i=0;i<n;++i)
    {
        thissum+=a[i];
        if(thissum > maxsum)
        {
            maxsum = thissum;
        }
        else if(thissum < 0)
        {
            thissum = 0;
            /*当前子列和为负数，那么再加上
            后面的部分和只会让后面的部分和更小
            最大的情况只可能是后面的部分和*/

            /*
            例如3 -5 2 6先记录max是3，然后thissum
            变成-2，加上2没2大，+2+6没8大
            */
        }
    }
    return maxsum;
}
```
$T(N)=O(N)$
可以用-1 3 -2 4 -6 1 6 -1试试理解
在线处理的特点是每输入一个数据就能做到即时处理，在任何一个地方中止输入，算法都能正确地给出当前的解

# 2 &nbsp;Logarithms in the Running Time
Besides divide-and-conquer algorithms, the most frequent appearance of logarithms centers around the following general rule: An algorithm is O(log n) if it takes constant (O(1)) time to cut the problem size by a fraction (which is usually ). 

On the other hand, if constant time is required to merely reduce the problem by a constant amount (such as to make the problem smaller by 1), then the algorithm is O(n)
(From textbook)

## 2.1 Binary Search
见上一节

## 2.2 Euclid's Algorithm
```c
int gcd(int m,int n)
{
    while(n > 0)
    {
        rem = m % n;
        m = n;
        n = rem;
    }
    return m;
}
```
例如50%15=5,50/15=3
15%5=0,return 5
{% callout info::Theorem 2.1 %} 
If $m\>n$,then $m&nbsp;(mod n)<\frac{m}{2}$
Proof:
There are two cases.
If $n\le m/2$,then the remainder is less than $n$,so it's less than $m/2$
If $n>m/2$,then $m/n=1$,the remainder must be $m-n<m/2$
{% endcallout %}
根据算法，两次循环后，m就会变为m%n，所以两次循环后m至少减半，而我们知道一旦开始循环，一次循环中，m的值就是上一次n的值
如果k次循环后，循环结束之后m=1，说明这次循环n=1，那么余数(rem)一定是0，n变为0(rem)，算法停止
m减半$\frac{k}{2}$次，那么$$\frac{m}{2^{\frac{k}{2}}}=1$$
$$\frac{k}{2}=log_2m$$
所以时间复杂度是$O(logn)$

## 2.3 Exponentiation
比较直接的方法是乘n次x，复杂度为$O(N)$
```c
//Efficient Exponentiation
int pow(int x,int n)
{
    if(n==0)
    {
        return 1;
    }
    if(n==1)
    {
        return x;
    }
    if(n%2==0)
    {
        return pow(x*x,n/2);
    }
    return pow(x*x,n/2) * x;
}
```
n每次调用都会减半，调用$log_2n$次，每次调用要么做一次乘法算x*x，要么做两次乘法(n为奇数的情况再乘x)，所以最多$2logn$次乘法运算
时间复杂度为$O(logN)$

# 3 &nbsp;Exercise
分析下面程序的时间复杂度
```c
int pow(int x,int n)
{
    if(n==0)
    {
        return 1;
    }
    if(n==1)
    {
        return x;
    }
    if(n%2==0)
    {
        return pow(x,n/2)*pow(x,n/2);
    }
    return pow(x,n/2)*pow(x,n/2) * x;
}
```
