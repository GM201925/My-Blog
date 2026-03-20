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
![](/img/LCDF-chapter2-1.png)
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