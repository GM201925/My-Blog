---
title: Chapter 6 - Counting
date: 2026-04-21 11:58:32
categories: Discrete Mathematics
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
# 1 The Basic of Counting
## 1.1 Basic Counting Principles
***The Sum Rule***
If a first task can be done in $n_1$ ways and a second task in $n_2$ ways, and if these tasks cannot be done at the same time, then there are $n_1 + n_2$ ways to do **one of these tasks**. 
也就是说你可以做第一个任务，有x种选择/方法，也可以做第二个任务，有y种，但不能同时做两个任务。
那么你做其中一个任务，即任务一或任务二，就一共有x+y种选择/方法

对应到集合，就是说两个disjoint且finite的集合的并集的元素个数等于两个集合的元素个数之和

{% callout primary::Example 1 %} 
Counting the number of elements in A,
A = {length 10 bit strings with 0-streak of length exactly 5}. 

集合A可以拆成这几个不相交的子集：
A1 = {000001\*\*\*\*}
A2 = {1000001\*\*\*}
A3 = {\*1000001\*\*}
A4 = {\*\*1000001\*}
A5 = {\*\*\*1000001}
A6 = {\*\*\*\*100000}

最后相加它们的元素个数即可
{% endcallout %}

***The Product Rule***
Suppose that a procedure can be broken down into two tasks. 
If there are $n_1$ ways to do the first task and $n_2$ ways to do the second after the first task has been done, then there are $n_1n_2$ ways to complete the procedure. 

对应到集合，就是笛卡尔积的元素个数等于两集合元素个数乘积

{% callout primary::Example 2 %} 
How many functions are there from a set with  m elements to one with n elements? 

A有m个元素，B有n个元素，f从A到B，A的每个元素都要对应一个B中的元素，有n种选择，第一步，对应A中第一个元素，第二步，对应A中第二个元素……故答案为$n^m$

How many **one-to-one** functions are there from a set with m elements to one with n elements? 

one-to-one意味着两个A中不同的元素不能对应B中同一个元素
当$m > n$时，不可能有one-to-one函数
当$m \le n$时，第一步，将A中第一个元素对应到B的一个元素，n种，第二步，将A中第二个元素对应到B的一个元素，这时不能选之前选过的，所以有(n-1)种……
答案为$n(n-1)(n-2)...(n-m+1)$
{% endcallout %}

{% callout primary::Example 3 %} 
Counting Subsets of a Finite Set     
If |A|=$n$, then |P(A)|=$2^n$.  

我们用集合的bit string representation
集合A就是111...1(n个1)
A的所有子集其实就是长度为n的bit string有多少个
比如000...0(n个0)就是空集，100...0(1个1)就表示只含有A中第一个元素的集合
所以答案为$2^n$

Choose three different numbers from the integers between 1 to 300 such that the sum of the three integers can be divisible by 3. How many ways are there?  

A = {x |   x(mod 3) = 1 }
B = {x |   x(mod 3) = 2 }
C = {x |   x(mod 3) = 0 }
其中$1\le x \le 300$
那么A,B,C中各有100个元素

一、从A中选三个元素，C(3,100)
二、从B中选三个元素，C(3,100)
三、从C中选三个元素，C(3,100)
四、从A,B,C中各选一个元素，$100^3$

这四个“任务”不能同时做，但由题意我们需要做其中一个，所以一共的方法数就是这四种情况的方法数之和

{% endcallout %}

## 1.2 The Inclusion-Exclusion Principle (Subtraction Rule) 
$$\mid A\cup B \mid = \mid A \mid + \mid B\mid - \mid  A\cap B\mid$$

How many positive integers not exceeding 100 are divisible by neither 4 nor 6? 
先算被4整除或被6整除的一共多少个
被4整除： 25个
被6整除： 16个
被4和6整除： 即被12整除，8个
被4或被6整除： 25+16-8=33个
故答案为100-33=67个

## 1.3 Tree Diagrams
To use trees in counting, we use a branch to represent 
each possible choice. We represent the possible outcomes 
by the leaves. 
![](img/DM/3-1.png)

# 2 The Pigeonhole Principle
{% callout info::Theorem 1 %} 
***The Pigeonhole Principle***
If k is a positive integer and k+1 or more objects are placed into k boxes, then there is at least one box containing two or more of the objects. 
{% endcallout %}

例如367个人中一定至少有两个人生日是一样的，因为一年最多366天
如果让你把6个球放到5个盒子里，那么一定会至少有一个盒子里有2个及以上的球

Among any group of 11 integers, there are two integers a and b such that 10 | a-b.
为什么？因为一个数除以10的余数是0到9，10种，现在有11个数，一定至少有两个数除以10有相同的余数，那么它们作差一定是10的倍数
pigeons: 11 integers
pigeonholes: the possible remainders when an integer is divided by 10

{% callout primary::Example 4 %} 
In a party of 2 or more people, there are 2 people with the same number of friends in the party. (Assuming you can’t be your own friend and that friendship is mutual.)

pigeons: the n people
pigeonholes: the possible number of friends
每个人可以有0/1/2/.../n-1个朋友

看上去好像是n个pigeon和n个pigeonhole
但朋友是相互的，如果有人有n-1个朋友，那么不可能有人有0个朋友，也就是0和n-1不能同时存在

所以一个人的朋友数只有n-1种可能，所以一定有两个人的朋友数是一样的
{% endcallout %}

{% callout info::Theorem 2 %} 
***The Generalized Pigeonhole Principle ***
If N objects are placed into k boxes, then there is at least one box containing at least $\left \lceil N/k \right \rceil$ objects. 
{% endcallout %}


{% callout primary::Example 5 %} 
Show that among any n+1 positive integers not exceeding 2n there must be an integer that divides one of the other integers. 

任何正整数可以写成$2^kq$的形式，其中$q$是奇数
在1到2n之间的正整数，奇数q只有n种可能，所以对于任何n+1个正整数，一定有两个正整数的q是相同的
设$a_i = 2^{k_1}q,a_j = 2^{k_2}q$
那么其中大的一个数一定是小的那个数的倍数
{% endcallout %}

{% callout primary::Example 6 %} 
During 11 weeks football games will be held at least 1 game a day, but at most 12 games be arranged each week. Show that there must be a period of some number of consecutive days during which exactly 21 games must be played.

设$x_i$为第i天的比赛数
而$a_i$是前i天总的比赛数，即对前i天比赛数求和
那么$1\le a_1\le ... \le a_{77}\le 12x11=132$
设$c_i = a_i +21$
那么$22\le c_1\le ... \le c_{77}\le 132+21=153$
所有的a和c一共有154个数，但它们可能的取值只能是1到153
又a之间不会相等，因为a递增，c之间也一样
那么一定存在$i,j$使得$a_i = c_j = a_j + 21$
那么从第$j$天到第$i$天，有21场比赛

【练习1】如果是22场呢？还存在吗
设$c_i = a_i +22$
那么$23\le c_1\le ... \le c_{77}\le 132+22=154$
仍存在，此时a和c一共有154个数，它们可能的取值只能是1到154
如果这154个数中有相等的两个数，那么存在$i,j$使得$a_i = c_j = a_j + 22$，证明完毕
如果没有，说明a和c没有相等的数，那么一定有一个数是22，且这个数只能出现在a中，如$a_j=22$，那么前j天打了22场比赛，证明完毕

我们还可以引入$a_0=0$，那么$c_0=22$
156个数，可能的取值只能是0到154，证明完毕，其实这跟上面是一样的，因为可能$a_j=c_0=a_0+22$

【练习2】如果是23场呢？还存在吗
对于$a_1$到$a_24$，一定存在两个数，它们除以23的余数相等
即存在s,t使得$a_t=a_s+23k$
若k=1，证明完毕
若k>1，那么$a_24> 46$，一样的，对于$a_24$到$a_47$，一定存在两个数，它们除以23的余数相等
即存在s,t使得$a_t=a_s+23k$
若k=1，证明完毕
若k>1，那么$a_47> 46+46=92$，一样的，对于$a_47$到$a_70$，一定存在两个数，它们除以23的余数相等
即存在s,t使得$a_t=a_s+23k$
若k=1，证明完毕
若k>1，那么$a_70> 92+46=138$，矛盾！
所以前面不可能一直k>1，一定有k=1的情况，证明完毕
所以哪怕是24场，也一样
{% endcallout %}

{% callout primary::Example 7 %} 
【1】Suppose that there are n arbitrary integers. Show that there exist some consecutive integers such that the sum of these integers is the multiple of n.

前i项和记为$a_i$
若$a_i$中有n的倍数，证明完毕
若没有，它们除以n的余数只能是1到n-1，由鸽巢原理有两个数余数相同，它们作差就是n的倍数，证明完毕

这跟引入$a_0=0$是一样的

【2】Every sequence of $n^2+1$ distinct integers contains a subsequence of length $n + 1$ that is either strictly increasing or strictly decreasing. 
给每个元素 a_i 赋值：(inc_i, dec_i)，其中 inc_i 是从 a_i 开始的最长递增子序列长度，dec_i 是最长递减子序列长度
假设没有长度 n+1 的子序列，则 inc_i≤n，dec_i≤n → 最多 n×n=n² 种不同标签
鸽子：n²+1 个元素 → 必有两个元素标签相同，设为$a_i,a_j$
矛盾：若 $a_i < a_j$，则 inc_i≥inc_j+1；若 $a_i>a_j$，则 dec_i≥dec_j+1
结论：假设不成立，原命题得证

【3】Assume that in a group of six people, each pair of individuals consists of two friends or two enemies. Show that there are either three mutual friends or three mutual enemies in the group. 
任取一个人 A，他和其他 5 个人的关系只有 "朋友" 或 "敌人" 两种
鸽子：5 个人，鸽巢：2 种关系 → 至少⌈5/2⌉=3 个人和 A 是朋友/敌人
不妨设 A 和 B、C、D 是朋友：
若 B、C、D 中有任意两个是朋友（如 B 和 C）→ A、B、C 是 3 个互相朋友
若 B、C、D 中没有任何两个是朋友 → 他们 3 个互相是敌人
同理，若 A 和 3 个人是敌人，结论也成立

The Ramsey number R(m,n), where m and n are
positive integers greater than or equal to 2, denotes
the minimum number of people at a party so that
there are either m mutual friends or n mutual
enemies, assuming that every pair of people at the
party are friends or enemies.
如本例中，R(3,3) = 6，当然我们没证明1到5不行，留给读者构造反例（
{% endcallout %}




# 3 Permutations and Combinations

# 4 Generalized Permutations and Combinations

## 4.1 r-permutation with repetition 
什么叫repetition？
就是一个元素可以被选多次，下例中，字符串第一个位置选了a，下一个位置仍可以选a

The number of r-permutations of a set of $n$ objects with repetition allowed is $n^r$. 

例如，一个长度为10的字符串，由二十六个英文小写字母组成，这样的字符串有多少种
$26^10$

## 4.2 n-Permutation with limited repetition 
什么叫limited repetition？
如下例中，我第一个位置选S，第二个位置还可以选S，但是，我的S一共只有三个

The number of different permutations of n objects, where there are n1 indistinguishable objects of type1, …, and $n_k$ indistinguishable objects of type k, is $n!/(n_1!n_2!…n_k!)$

将SUCCESS重排，能得到多少个不重复的字符串？
七个位置，选三个放S，这是无序的，即C(7,3)，下一步在剩下的4个位置放C,...
结果是C(7,3) C(4,2) C(2,1) C(1,1)

另一种方法是先假设所有元素都不同，有
n!种排列；但相同元素内部的排列其实是同一种，所以要除以每种相同元素的阶乘。
即先随便排，比如第一个位置7种选法，第二个位置6种...
但这样，比如S1UCCES2S3 和S2UCCES1S3 是一样的，对于S就重复了6次因为S1S2S3自己排就有3!种
所以答案为7!/(3!2!1!1!)

这类问题实际上就是解决，我要全排列，但是有些元素没有区别，它们内部是无序的，但又不是大家都无序
There are 50 students in a class. If two of the leading group are elected as a monitor and a vice monitor, then how many are there?
我们当然可以先P(50,2)，再乘C(48,5)

也可以看作先从50个选7个全排列，不是特殊身份的5个人内部是没区别的，比如选了7个人，叫ABCDEFG，排列的第一个位置和第二个位置是特殊身份，那么
ABCDEFG和ABGFEDC就是一种排列

How many bit strings of length 10 are there that contain exactly two 0s, eight 1s?
先2个0，8个1随便排列，10!种，1和0内部无序，除以2!,8!

## 4.3 r- Circle Permutation 
The number of r- Circle permutations of a set of n objects is P(n,r)/r .
自己画图就可以知道，12345，按这种顺序，23451等5种都是一种

## 4.4 Combinations With Repetition
和之前一样，repetition就是说一个元素被选了，它下一次还可以被选，如下例中，选三个蛋糕，我可以选三次A蛋糕，也可以一个A蛋糕两个B蛋糕

There are C (n-1+r, r) r-combination from a set with n elements when repetition of elements is allowed

{% callout primary::Example %} 
Suppose that a cake shop provides 2 different kinds of cakes. How many different ways can 3 cakes be chosen?

什么意思呢，我们要选三个蛋糕，记作星星，来自两个种类，所以用一个杠分开
n-1 根竖线把空间分成 n 个格子（对应 n 个不同元素）
r 个星星代表总共选 r 个元素
第 i 个格子里的星星数 = 第 i 个元素被选中的次数
比如 \*\*|\* 对应：元素 1 选 2 个，元素 2 选 1 个

总共有n-1+r个位置，把r放上去，剩下的放杠就行，所以答案是C(4,3)=4
杆左边A类型，杆右边B类型
\*\*\*|
\*\*|\*
\*|\*\*
|\*\*\*

How many solutions are there to the equation
$$x_1+x_2+x_3+x_4=16$$
$x_i$是非负整数

其实这玩意有点像隔板法，即相同的球放到不同的盒子，当然这里允许盒子是空的，不过高中时我也在一个视频中学习过解决方法，先每个盒子放一个球，再在空隙中插三个板，答案是C(19,3)

如果按我们学的方法做，答案是C(16+4-1,16)，是一样的，这里解释为4个不同的元素，即$x_i$，可以重复选，选16个，$x_i$被选中m次就代表$x_i=m$

【追问1】如果要求每个x都大于1呢
即每个x大于等于2，那么等价于找$x_1+x_2+x_3+x_4=8$，每个加2就行了，C(8+4-1,8)

【追问2】如果找$x_1+x_2+x_3+x_4\le 16$呢
我们可以引入$x_5$
$x_1+x_2+x_3+x_4+x_5=16$
这样就行了，有C(5+16-1,16)种
{% endcallout %}

## 4.5 Distributing objects into boxes 
孩子们，我回来了
### 4.5.1 不同的球放不同的盒子
The number of ways to distribute n 
distinguishable objects into k distinguishable boxes so that ni objects are place into box i, i=1,2,…,k, equals n!/(n1!n2!…nk!) 

证明方法就是分步，第一步n个里选n1个，直接放到对应的唯一那个box去了，之后一直放，再化简就得到上式

高中时，笔者学到的方法是，先分组再分配，而分组对应着不同的球放相同的盒子，然而这里却只展示了一种特殊情况，即每个盒子放的元素给固定死了

当然通法确实不存在，因为先分组再分配，你要关心怎么分组，这要分类讨论
{% callout primary::Example %} 
How many ways are there to distribute hands of 5 cards to each of four players from the standard deck of 52 cards? 

不同的盒子：四个玩家+剩余牌堆（桌子）
不同的球：52张牌
只不过规定好了，52=5+5+5+5+32

按上面说的，答案为52!/(5!5!5!5!32!)
也可以先分组再分配，先分成5组
C(52,5) C(47,5) C(42,5) C(37,5) C(32,32)/4!
再乘P(4,4)，注意不能随便分，桌子必须给32张
{% endcallout %}

### 4.5.2 不同的球放相同的盒子
纯分组
分组要注意除以组中元素个数一样的组的数量的阶乘，正如上例除以4!

{% callout primary::Example %} 
How many ways are there to put four different employees into three indistinguishable offices, when each office can contain any number of employees? 

分组
0+0+4 1种
0+1+3 C(4,1)
0+2+2 C(4,2)/2!
1+1+2 C(4,2)就够了，当然也可以C(4,2) C(2,1) C(1,1)/2!

这里如果是分组分配，对0+0+4应用之前说的，
C(4,4) C(0,0) C(0,0)/2!，再乘P(3,3)，结果是对的，但是分组的意义就会很奇怪，变成1/2组了，可见对于先分组再分配问题这虽然是个通法，尽管分组的意义有时会奇怪，其实是因为我们允许了空堆存在，事实上这并不是代表组有多少个，而是每 1 个真实分法，对应多少个重复的有序堆的倒数


所以纯分组时，这个方法是寄了的，会算出0.5，不过大多数情况是对的，其实面对有空堆的情况，比如上面，直接看作分成1堆就行了

我们也可以换个角度
1. 三个公司都不空，只能1+1+2，C(4,2)
2. 两个公司不空，一个空，1+3或2+2，C(4,2)/2!+C(4,1)
3. 只有一个公司不空，1种
{% endcallout %}

### 4.5.3 相同球放不同盒子
隔板法

### 4.5.4 相同球放相同盒子
枚举
比如6本相同的书放到3个相同的盒子
6=
0+0+6
0+1+5
0+2+4
0+3+3
1+1+4
1+2+3
2+2+2




