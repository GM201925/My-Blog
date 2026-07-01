---
title: Chapter 2 - Combinational Logic Circuits
date: 2026-03-18 20:35:00
categories: Logic and Computer Design Fundamentals
mathjax: true
cover: img/gbcop.png

---

## 1 Binary Logic and Gates
### 1.1 Binary Variables
取值0或1的变量
### 1.2 Logical Operations
AND($\cdot$)
OR(+)
NOT($\bar{A}$)

Examples:
Y=A × B is read"Y is equal to A AND B."

详细内容可以看离散数学的笔记
### 1.3 Logic Gates
In the earliest computers,switches were opened and  closed by magnetic fields produced by energizing coils(线圈) in ***relays(继电器)***.The switches in turn opened and closed the current paths.

Later,***vacuum tubes*** that open and close current paths electronically replaced relays.

Today,***transistors*** are used as electronic switches that open and close current paths.

三极管实现逻辑门的图例
![](/img/LCDF/LCDF-chapter2-1.png)
简单来说，当A是高电压时三极管会导通
可能后面会写一些讲原理的...(挖坑)

**Logic Gate Symbols and Behavior**
![](/img/LCDF/LCDF-chapter2-2.png)
此外还有异或门(XOR,不同为1)和同或门(XNOR,相同为1)

逻辑门可以分成基础逻辑门(Basic Logic Gates)、通用逻辑门(Universal Logic Gates)和其他(Other Logic Gates)
基础逻辑门就是与或非，它们三个一起也是通用逻辑门，可以构成各种门电路

通用逻辑门(A gate type that alone can be used to implement all possible Boolean functions)有与非、或非逻辑门，与非门可以表示其他所有门，或非门也是

例如，与非门两个输入都是$x$就能得到$\bar{x}$，有了非门后，与非门接上非门就得到与门，两个输入先非再与非就得到或门

### 1.4 Gate Delay
In actual physical gates,if one or more input changes cause the output to change,the output change does not occur instantaneously.

The delay between an input changes and the resulting output change is the ***gate delay***.
denoted by $t_G$
![](/img/LCDF/LCDF-chapter2-3.png)

### 1.5 Logic Diagrams and Expressions
$$F=X+\bar{Y}Z$$
1. Truth Table
2. Equation $F=X+\bar{Y}Z$
3. Logic Diagram(逻辑电路图)
4. Waveform

- Truth tables and waveforms are unique;equations and diagrams are not.

## 2 Bollean Algebra
Boolean algebra is an algebraic structure defined on the set {0,1} with three binary operators(AND OR NOT) that satisfies the following basic identities.
![](/img/LCDF/LCDF-chapter2-4.png)
注意15条，有些不明显，其实就是$p\lor (q\land r)\equiv (p\lor q)\land (p\lor r)$

### 2.1 Operator Precedence
1. Parentheses
2. NOT
3. AND
4. OR

### 2.2 Advanced Boolean Theorems
#### 2.2.1 Duality Theorem
The dual of an algebraic expression is obtained by:
- interchanging AND and OR
- interchanging 0 and 1
- variables remaining unchanged
>逻辑函数：输入是逻辑变量，输出为0/1的函数
这些全是逻辑函数：
Y=A
Y=A+B
Y=A⋅B
Y=A+B
​Y=AB+ AC

Seek the dual of a function, the operation sequence keep as same as the original function(运算顺序不能变)

例如
$$F=(A+\bar{C})\cdot B+0$$
dual就是$F=(A\cdot \bar{C}+B)\cdot 1$
注意不是$F=A\cdot \bar{C}+B\cdot 1$因为原来的F是先算B乘(A+C非)的

If the function G is the dual of F,then F is also G of duality.
<font color="blue">If the two logical functions F and G are equal,then the duality formula F'and G' are also equal.即如果F=G，那么F的对偶F'=G的对偶G'</font>

例如
$$X(Y+Z)=XY+XZ$$
对偶后
$$X+YZ=(X+Y)(X+Z)$$

#### 2.2.2 Substitution Theorem
{% callout success::Substituion Theorem %}
Any logical equation that contains a variable A, and if all occurrences of A's position are replaced with a logical function F,the equation still holds.
{% endcallout %}
例如
$$X(Y+Z)=XY+XZ$$
我们用F=X+YZ替换X,下式依然成立
$$(X+YZ)(Y+Z)=(X+YZ)Y+(X+YZ)Z$$

#### 2.2.3 Complementary Theorem
$$A\cdot \bar{A} = 0,A+\bar{A} = 1$$
反过来，如果$A\cdot B=0,A+B=1$,那么$B=\bar{A}$

For logic function F,the inverse function of the original function is obtained by
- interchanging AND and OR
- complementing each constant value and literal(字面量，即变量)
- 运算顺序不能变
例如
$F=\bar{A}B+C\bar{D}$->$\bar{F}=(A+\bar{B})(\bar{C}+D)$

我们也可以使用多次德摩根律去得到反函数
$$F=\bar{A}B+C\bar{D}$$
$$\bar{F}=\overline{\bar{A}B+C\bar{D}}$$
$$=\overline{\bar{A}B}\cdot \overline{C\bar{D}}$$
$$=(A+\bar{B})\cdot (\bar{C}+D)$$

## 3 Applications of Boolean Algebra
### 3.1 Boolean Algebraic Proof
{% callout primary::Example %}
1. $A+AB=A \\:\\: \\: \\: (Absorption  \\, Theorem)$
<br>

$A+AB=A\cdot 1+AB=A(1+B)=A$
<br>

2. $AB+\bar{A}C+BC=AB+\bar{A}C\\:\\: \\: \\: (Consensus  \\, Theorem)$

<br>

$AB+\bar{A}C+BC\cdot 1=AB+\bar{A}C+(A+\bar{A})BC$
$=AB+\bar{A}C$
<br>

3. $x+\bar{x}y=(x+\bar{x})(x+y)=x+y\\:\\: \\: \\: (Simplification  \\, Theorem)$

<br>
上面的式子对偶式都成立

4. 证明DeMogran's Laws
例如证明$\overline{x+y}=\bar{x}\cdot \bar{y}$
根据Complementary Theorem
我们只需要证明$A+B=1,AB=0$,那么就有$\bar{A}=B$
即
$x+y+\bar{x}\cdot \bar{y}=1$
而且
$(x+y)\cdot (\bar{x}\cdot \bar{y})=0$
{% endcallout %}

### 3.2 Challenges in Manipulating Boolean Functions
逻辑函数的表达式不是唯一的
而且化简有可能很tricky
![](/img/LCDF/LCDF-chapter2-5.png)

## 4 Canonical Forms
It's useful to specify Boolean functions in a form that:
- has a correspondence to the truth tables 
  (规范形式（如最小项之和或最大项之积）的每一项直接对应真值表中输出为 1（或 0）的那一行)
- allows comparison for equality 
  (如果两个布尔函数都写成相同的规范形式（比如都写成最小项之和，且按相同顺序排列），那么只需要比较它们包含的项是否完全相同，就能判断它们是否相等)
- provides a starting point for optimization

Two common Canonical Forms:
- Sum of Minterms (SOM)
- Product of Maxterms (POM)

例如
|$X$|$Y$|$F$|$\bar{F}$|
|:-:|:-:|:-:|:-:|
|0|0|1|0|
|0|1|0|1|
|1|0|1|0|
|1|1|0|1|

两种方式去表示F的真值表
$F=\bar{X} \bar{Y}+X\bar{Y}$
这表示了所有F为1对应的行，如第一项就是对应第一行，(只有)X=0,Y=0时$\bar{X} \bar{Y}=1,F=1$
即第一项代表：输入0,0,结果为1
这称为最小项之和(SOM)

$F=\overline{\bar{X}Y+XY}=(X+\bar{Y})(\bar{X}+\bar{Y})$
$\bar{X}Y+XY$代表了真值表所有F为0对应的行，所以取反就是F=1

$(X+\bar{Y})(\bar{X}+\bar{Y})$
例如第一项，只有X=0,Y=1才为0，这时F=0，其他情况第一项都为1
即第一项代表：输入0,1,结果为0
第二项只有X=Y=1时才为0，这时对应真值表最后一行，F=0，所以只要不是这两种情况，这两项都为1，F即为1
这称为最大项之积(POM)

***SOM每项反映的是真值表为1的各种情况，POM每项反映的是真值表中0的情况***

### 4.1 Minterms And Maxterms
所有逻辑变量或者它们的非，一起与起来就是最小项

所有逻辑变量或者它们的非，一起或起来就是最大项


例如一个逻辑函数有3个逻辑变量，那么就有$2^3=8$个最小项
$$ABC,\bar{A}BC,\bar{A}\bar{B}C...$$

{% callout info %}
- 每个最小项只有一种输入时为1，其他输入皆为0
  例如$\bar{A}BC$只有A=0,B=C=1时结果为1
  这个最小项我们就把它编号为011(一般按字母顺序来排，即A=0,B=1,C=1时结果为1)
  记作$m_3$(二进制011=十进制3)
- In general, minterms are designated $m_i$, where i corresponds the input combination at which this minterm is equal to 1.

<br>

- 每个最大项只有一种输入时为0，其他输入皆为1
  例如$\bar{A}+B+C$只有A=1,B=0,C=0时结果为0
  这个最大项我们编号为100
  记作$M_4$
{% endcallout %}

***Minterm and Maxterm Relationship***
$$M_i = \bar{m_i}$$
$$m_i = \bar{M_i}$$
因为输入一样，最小项只有在这个输入下为1，其他输入都为0，而最大项刚好相反

### 4.2 Minterms & Maxterms of the Function
- We can implement any function by "ORing" the minterms corresponding to 1 entries in the function table.

|$X$|$Y$|$F$|$\bar{F}$|
|:-:|:-:|:-:|:-:|
|0|0|1|0|
|0|1|0|1|
|1|0|1|0|
|1|1|0|1|

$$F=\bar{X} \bar{Y}+X\bar{Y}=(X+\bar{Y})(\bar{X}+\bar{Y})$$

$$F(x,y)=m_0+m_2=M_1+M_3$$

任何逻辑函数都能写成SOM或POM的形式
写成SOM:
每一项都与上缺失的项，再分配律
例如$F(A,B,C)=AB+C=AB(C+\bar{C})+(A+\bar{A})(B+\bar{B})C$
$=ABC+AB\bar{C}+A\bar{B}C+\bar{A}\bar{B}C+\bar{A}BC=\sum_m(1,3,5,6,7)$

写成POM:
每一项都或上缺失的项，再反用分配律$A+BC=(A+B)(A+C)$

例如$F(x,y,z)=x+\bar{x}\bar{y}$
先反用分配律$x+\bar{x}\bar{y}=(x+\bar{x})(x+\bar{y})$把乘积去掉

$$F=x+\bar{y}+z\bar{z}=(x+\bar{y})+z\bar{z}=(x+\bar{y}+z)(x+\bar{y}+\bar{z})=M_2M_3$$

### 4.3 Canonical Forms for Comparison of Equality
两个函数都化成Canonical Form(SOM/POM)后，相当于直接表示出了这个函数的真值表，所以只要SOM/POM形式一样，两个函数就相等(相同输入结果相同)

### 4.4 Function Complements
例如$F(x,y,z)=\sum_m(1,3,5,7)$
那么
$\bar{F}=\sum_m(0,2,4,6)$
因为这些代表F=0的行

$\bar{F}=\prod_{M} (1,3,5,7)$
因为输入为例如$m_1$即001时F=1，这时F'应该为0
如果输入不是001，011，101，111，F=0
相应地，输入如果不是001，011，101，111，那么F'=1

|$X$|$Y$|$F$|$\bar{F}$|
|:-:|:-:|:-:|:-:|
|0|0|1|0|
|0|1|0|1|
|1|0|1|0|
|1|1|0|1|

$$F=\bar{X} \bar{Y}+X\bar{Y},\bar{F}=(X+Y)(\bar{X}+Y)$$
理解上，我们把F和F'都作为真值表的列，输入0，0时，F=1，F'=0，也就是对F来说，输入0，0时结果为1，但对F'来说，输入0，0结果为0

最小项代表结果为1的行，最大项代表结果为0的行，SOM代表所有为1的行，POM代表所有为0的行，所以F可以用结果为1的1，3行即$m_0+m_2$表示，也可以用结果为0的行来表示$F=(X+\bar{Y})(\bar{X}+\bar{Y})$

那么对F'来说，它也可以用结果为1的行表示，刚好就是F结果为0的行，即输入0，1或者输入1，0，F'=1
$\bar{F}=m_1+m_3$
也可以用结果为0的行来表示
$\bar{F}=M_0M_2$
要注意的是，最大项和最小项下标代表的都是输入的逻辑变量的取值，例如$M_2$和$m_2$都是代表输入$X=1, Y=0$，一个是$\bar X+Y$(即这个输入下取0)，另一个是$X\bar Y$

当然也可以直接用德摩根律加$m_i=\bar{M_i}$证明

{% callout warning %}
如果$F(x,y,z)=\sum_m(1,3,5,7)$
那么
$$F=\prod_M(0,2,4,6)$$
$$\bar{F}=\prod_M(1,3,5,7)=\sum_m(0,2,4,6)$$
{% endcallout %}

对于Canonical Form来说，逻辑电路是“两层”的，如对于SOM，所有输入先分别与，每个最小项的输出再一起或
对于POM，各输入先分别或，每个最大项的输出再一起与

## 5 Standard Forms
SOP和POS，即积之和或者和之积，不要求是最小项之和或最大项之积，例如$AB+A\bar{C}+ABC$

## 6 Circuit Optimization
Optimization(最优化)就是用一种更正式规范的方式去简化逻辑电路，例如简化一个函数的实现

简化电路，我们需要有个标准去评估电路的简单程度

### 6.1 Literal Cost
Literal就是一个变量或者它的非
例如$F=BD+\bar{A}BC+A\bar{C}\bar{D}$
Literal Cost = 8
数一共有多少个变量就好了，这代表的是逻辑电路的输入层，也就是说，在这个标准下，你输入越少，我们就认为这个函数越简单

例如下面这张图，它们的literal cost是一样的，描述的是输入层面，例如左图A,B,C成为与门的三个输入，它们非了之后又成为下面那个与门的三个输入（先不考虑非门）3+3=6
![](/img/LCDF/LCDF-chapter2-6.png)

### 6.2 Gate Input Cost
这个标准考虑函数中所有的输入个数

例如
$$F=(\bar{A}\bar{C}+AC)(B+\bar{D})$$
L(literal cost) = 6
这时我们把所有的第一层输入代价都算了(先不考虑非门)
A非C非与一下得到一个结果，作为下一层输入
A和C与一下得到结果，作为下一层输入
上面两个结果作为或门的两个输入，第二层输入为2

这个或门的输出作为最后一个与门的输入
B和D非或的结果作为最后一个与门的输入
第三层输入为2
G=6+2+2=10,GN=13
如果算非门，非门是共享的，即一个变量的非用一个非门即可，比如$\bar{A}B+\bar{A}C$,L=4,G=6,GN=6+1=7
![](/img/LCDF/LCDF-chapter2-7.png)
上面这种算法可以总结成数完L后，按计算顺序，出一个结果就+1，如上式先算$\bar{A}\bar{C}$,+1,又算$AC$,+1,又算这两者或,+1,又算$B+\bar{D}$,出一个结果,+1,最后算两个括号与,出一个结果输出出去了，不算,共+4

### 6.3 Karnaugh Maps
1. 写成SOM或POM的形式，一般我们都写成SOM
2. 对于SOM，在对应的最小项的格子里写1
3. 开始圈1，用尽可能少的圈去圈尽可能多的相邻的1，圈内不能有0，圈的大小为2的幂次，如2，4，8
4. 化简，一般留下不变的那个项
![](/img/LCDF/LCDF-chapter2-8.png)
这张图解释了卡诺图的原理

![](/img/LCDF/LCDF-chapter2-9.png)
圈一定要尽可能多圈1，否则没法化到最简，还要手工再用定律去化简。

来看一道例题
![](/img/LCDF/LCDF-chapter2-10.png)
首先中间四个直接圈在一起，接下来3,4,9,14分别再用一个大小为2的圈圈起来
化简，找每个圈中不变的那个变量
5,7,13,15:$XZ$
4,5: $\overline{W}X\overline{Y}$
3,7: $\overline{W}YZ$
13,9: $W\overline{Y}Z$
14,15: $WXY$
结果为$XZ+WXY+W\overline{Y}Z+\overline{W}YZ+\overline{W}X\overline{Y}$
等等，好像有些不对啊~
将$XZ$通过乘的方式补上$W,Y$刚好有四项，每一项都会被$WXY+W\overline{Y}Z+\overline{W}YZ+\overline{W}X\overline{Y}$中的一个吸收掉
所以$XZ$是冗余的

为什么呢？画圈本质上就是把这2/4/8项弄到一起化简，画的圈可以有重叠部分是因为$A=A+A$，可以凭空或一个出来，我们看到对于边上的四个小圈，它们两两一起后1就被圈完了，这时还想凑中间四个，那就要再额外或上5,7,13,15的最小项

也就是说，原本是3+4+5+7+9+13+14+15
为了化简这五个圈，变成
3+4+5+7+9+13+14+15+5+7+13+15
即
(4+5)+(3+7)+(9+13)+(14+15)+(5+7+13+15)
可见5,7,13,15根本没必要再多加，如果加了，最后就不是最简，会有冗余项

**所以我们得到另一个原则：要用尽可能少的圈圈尽可能多的1，也就是圈最好大一些，但是如果一个圈内的所有1都被别的圈圈过了，那么这个圈就没必要化简了**

### 6.4 Don't Cares in K-Maps
Don't Cares:输入中不可能出现的(最小)项
对于这些项，它们的结果可以是1，也可以认为是0，因为根本不会出现在输入中
例如，函数F表示BCD码是否超过5，超过5时F的值为1
F(w,x,y,z)如w=x=y=0,z=1,即BCD码0001,即1
F=0
那对于1111的输入，F可以是1也可以是0，因为根本就不会有这个输入
如果我认为1111的输入F=1，那么wxyz就可以写进F的最小项之和中，这可以帮助我们化简

![](/img/LCDF/LCDF-chapter2-11.png)
注意圈X时不要上头，我们的任务只是用尽可能少的圈把1圈完，圈完1后就把剩下的X看作0，所以说如果你的一个圈中的所有1都被别的圈圈了，说明这些1都已经跟别人凑好化简掉了，那这个圈就是无效的

卡诺图也有一些缺陷，比如它的化简结果不是唯一的，为什么呢？后面的算法我们会介绍

### 6.5 Quine-McCluskey Algorithm
***Implicant***: a product that implies the function is true.
e.g. $F=AB+AC$,AB and AC are two implicants.Of course ABC is also a implicant.
因为AB对应到卡诺图上其实是111和110两个格
卡诺图中，只要是大小为2的幂次的，圈内都为1的圈都是implicant

***Prime Implicant***: a product term obtained by combining the maximum possible number of adjacent squares in the map with $2^n$ number of squares.

也就是我们之前提到的“尽可能大的圈”，如$F=AB+AC$中，ABC是implicant但不是prime implicant，画出卡诺图可知AB是prime implicant，它由含两个1的圈化简得到，AC也是prime implicant

***Essential Prime Implicant***: a prime implicant that covers one or more minterms that no combination of other prime implicants are able to include.

也就是我们之前说的，你这个圈里的1不能全被其他圈圈过了。如果这个prime implicant有“独属于自己”的1，那就是essential prime implicant

![](/img/LCDF/LCDF-chapter2-12.png)

***Quine-McCluskey Algorithm***
- Find all prime implicants.
- Include all essential prime implicants in the solution.
- Select a minimum cost set of non-essential prime implicants to cover all minterms not yet covered

这跟我们之前说的是对应的
先找主蕴含项->用尽可能少的圈把1全都圈起来
当然这里“尽可能少”一方面是说圈要大(prime implicants),另一方面是说不要重叠太多(其实对应第三步，用尽可能少的非essential主蕴含项把剩下的最小项给覆盖)

保留essential prime implicants->只留下那些圈内有一些1是自己独有的圈(其实这也对应第三步，因为第三步选剩下的非必要主蕴含项时，有些可能重叠的太多，就不要了)

![](img/LCDF/LCDF-chapter2-13.png)
最后选择的部分，也可以选第二列下面的1和x，所以说结果不是唯一的

### 6.6 Multi-Level Circuit Optimization
最后，我们还可以对卡诺图得到的结果进行进一步代数变形，从而减少门输入代价
![](img/LCDF/LCDF-chapter2-14.png)
![](img/LCDF/chapter2-15.png)

# 7 Other Gate Types
Why?
- Implementation feasibility and low cost 
- Power in implementing Boolean functions 
- Convenient conceptual representation 
![](img/LCDF/chapter2-16.png)
从上到下是
2-2 AOI(2个 2输入与门 → 1个或非门,2-2分别表示与门的输入个数，都是2，如3-2-2 AOI就是三个与门，输入个数分别为3，2，2)
2-2 OAI
2-2 AO
2-2 OA
XOR和XNOR都有交换律、结合律，利用这个性质，给一些数，其中只有一个数字只出现一次其他都出现两次，我们可以这样找到那个只出现一次的数
```Pseudocode
Function Singular(int a[], int Count)   
  value = 0  
  for i = 0, Count-1  
    value = value ⊕ a[i]  
  return value
```

## 7.1 Odd and Even Functions
如果超过两个变量异或起来，就构成了奇函数，卡诺图无法化简
- The 1s of an odd function correspond to minterms having an index with an odd number of 1s. 
- The 1s of an even function correspond to minterms having an index with an even number of 1s.

这是好理解的，因为如果变量输入中有偶数个1，最后一定是0，有奇数个1，最后异或起来一定是1

![](img/LCDF/chapter2-17.png)
可以使用奇函数为一个编码生成偶校验位，比如输入n位，如果有奇数个1，那么输出1，这时这n+1个bit就有偶数个1。

而验证时，将这n个bit输入到奇函数中，检验是否有偶数个1，如果有则输出0，说明没有出现Error

# 8 About Electric Circuits
一些电路的内容，具体原理会不会日后补充，以后再说（
下面内容来自蔡老师的ppt以及ai的回答，可能存在疏漏或错误

## 8.1 Buffer
A buffer is an electronic amplifier used to improve circuit voltage levels and increase the speed of circuit operation. 
The buffer is a gate with the function F = X 

{% callout info::Learn From AI %} 
每个逻辑门的输出端最多只能驱动一定数量的其他逻辑门（这个极限叫扇出系数，比如普通 TTL 门的扇出系数约为 8）。如果超过这个极限：
- 信号会变弱，高低电平不再标准（比如高电平从 3.3V 变成 2V，被后级误判为 0）
- 信号会变慢，上升沿和下降沿变得很陡，导致时序错误
- 严重时会烧坏前级的逻辑门
这时候就需要在中间加一个 buffer，把信号 "放大" 一下，让它能驱动更多的负载。
{% endcallout %}

## 8.2 Wired Output
目前我们介绍过的逻辑门的输出绝对不能直接连在一起！
如图，如果一个是低电压，一个输入高电压，那么右边的就会导通到左边的地，中间没有电阻，直接短路。
当然，严格来说这不是推挽电路，也不是RTL，是一种奇怪的简化版非门。
关于RTL,TTL,CMOS后面会介绍
![](img/LCDF/chapter2-18.png)
![](img/LCDF/chapter2-19.png)

**推挽电路(Push-Pull Circuit)** 是一种常见的电路配置，主要用于信号放大和驱动负载。其基本原理是使用两个不同极性的晶体管（如NPN和PNP）交替工作，一个在导通状态时，另一个处于截止状态，从而实现信号的放大和输出。

### 8.2.1 The 3-State Buffer
![](img/LCDF/chapter2-20.png)
除了能输出 0 和 1 之外，还能输出第三种状态：高阻态（High-Impedance，简称 Hi-Z 或 Z）。输出高阻态时buffer内部导线断开。
如上张图所示，有了三态门之后，我们就可以让多个逻辑门的输出连到总线上，只要给他们都加一个三态门来控制就好了。
- The output of 3-state buffers can be wired together 
- At most one 3-state buffer can be enabled. Resolved output is equal to the output of the enabled 3-state buffer 
- If multiple 3-state buffers are enabled at the same time 
then conflicting outputs will burn the circuit

总线就是一条所有设备共享的公共导线。计算机里的地址总线、数据总线、控制总线，本质上都是由三态缓冲器组成的。
总线的工作规则（绝对不能违反）
- 同一时间，只能有一个设备向总线发送数据
- 这个发送数据的设备，必须打开自己的三态缓冲器（EN=1）
- 其他所有不发送数据的设备，必须关闭自己的三态缓冲器（EN=0），进入 Hi-Z 状态
- 多个设备可以同时从总线上接收数据

## 8.3 RTL/TTL/CMOS
Modern computers use transistors because they are cheap, small, and reliable. Transistors are electrically controlled switches that turn ON or OFF when a voltage or current is applied to a control terminal. 

The two main types of transistors are bipolar junction transistors and metal-oxide-semiconductor field effect transistors (MOSFETs or MOS transistors)

### 8.3.1 Bipolar Junction Transistors
(这里找资料、问ai搞了好久破防了，简单总结一下吧。。。)
三极管有三个极，base，emitter，collector；三极管分为NPN型和PNP型
数字电路中只关心两种状态
以NPN型为例，
- 截止状态，b的电压不大于e，相当于c和e间断路
- 饱和状态，b的电压大于e，一般是超过0.7V，直接看作“导通”，一般是c到e间短路
当然还有倒置状态，下面会涉及，即b的电压不大于e，但b的电压大于c，这时bc可以导通
![](img/LCDF/chapter2-21.png)
万恶之源（
我来试图解释一下这个TTL反相器的原理
当输入为H时，b接VCC，e接H，截止，这时三极管截止。左边的三极管的c接到右边三极管的b，这其实是前级开关，去控制后级开关。
b接VCC高电压，如果导通的话，bc间压降0.7V，右边三极管的be压降又是0.7V，最后b的电压就是1.4V，这是可以成立的，所以右边三极管处于饱和状态，ce间通，输出L

当输入为L，左边三极管通，ce间通了，所以c的电势几乎为0，右边三极管截止，输出的电势几乎等于VCC，输出H

当然这个TTL非门并不是推挽结构，可能是简化的