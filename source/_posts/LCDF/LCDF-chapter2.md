---
title: Chapter 2 - Combinational Logic Circuits
date: 2026-03-18 20:35:00
categories: Logic and Computer Design Fundamentals
mathjax: true
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

例如，与非门两个输入都是$x$就能得到$\bar{x}$

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
$(x+y)\cdot (\bar{x}\cdot \bar{y})$
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