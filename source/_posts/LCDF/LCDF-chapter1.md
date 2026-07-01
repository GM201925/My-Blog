---
title: Chapter 1 - Digital Systems and Information 
date: 2026-03-05 10:20:30
mathjax: true
categories: Logic and Computer Design Fundamentals
cover: img/gbcop.png

---
## 0 写在前面
数逻的笔记学习了[Isshiki学长的笔记](https://note.isshikih.top/cour_note/D2QD_DigitalDesign)
本文在亮色模式下阅读体验也许更好

## 1 Analog and Digital Signals
- Analog and digital signals are two types of signals carrying information.
- Analog signals are ***continuous*** in both values and time,while digital signals are ***discrete*** in values and time.

## 2 Digital System
{% callout primary fa-regular fa-atom title="Digital-System" %}
1. 输入是离散信号，经过离散信号处理系统(这个系统内部也是离散的信息)，输出离散的信号
2. 见下图
![Test](/img/LCDF-chapter1-1.png)
signal conditioning:信号调理，模拟信号容易受干扰
为什么要A TO D
- 模拟信号易受干扰，数字信号差异较大不易干扰
- 数字信号处理更容易，方法更多
{% endcallout %}

### 2.1 ADC Converters
***Analog-To-Digital Converters*** converts a signal from analog to digital form.
The result of ADC comprises a string of bytes,e.g.,011,100,101

### 2.2 Information Representation
Binary values are the most prevalent values in digital systems.They are represented by values or ranges of values of physical quantities.
![Test](/img/LCDF-chapter1-2.png)

{% folding title="Questions" class="purple" open=false %}
{% callout default fa-regular fa-atom title="如果落到0.4-0.6怎么办" %}
这些电压是未定义的，这导致了非法的状态，可能是HIGH，可能是LOW，称为<font color="DarkViolet">floating </font>
{% endcallout %}

{% callout success fa-regular fa-atom title="为什么输入输出的评判范围不一样" %}
信号传输过程中可能会受干扰，所以input容忍度更高
传输过程中会有噪声，为了保证信号在传输中受到干扰的影响要小，所以output要严格
例如，输入了2V，那么可能原来是5V，受到干扰变成2V，所以我们仍要认为它是高电平；之所以要求输出高电平的电压值比输入判断为1的电压值更高，是为了留出<font color="DarkViolet">噪声容限(noise margin) </font>。信号在传输过程中会受到干扰而衰减，如果输出只输出刚好能被识别为1的电压，那么一点点干扰就会让接收端误判为0。因此，发送端必须输出一个足够高的电压，即使经过干扰衰减，到达接收端时仍然高于判断为1的门限，从而保证数据传输的可靠性。

The tolerable ranges for input signal levels are wider,to ensure that the circuits to function correctly in spite of variations and undesirable "noise" voltages.
The difference between the tolerable output and input ranges is called the <font color="DarkViolet">noise margin </font> (High=2.7-2=0.7V,Low=0.8-0.5=0.3V)

{% endcallout %}

{% callout warning fa-regular fa-atom title="为什么使用二进制" %}
数学理论上，三进制最优，但大多数真实的物理器件是二值的，开关的开和关，晶体管的导通和不导通……
此外，二进制使用的电路、空间更少，成本低
{% endcallout %}
{% endfolding %}



## 3 Number System

A number with *radix r* is represented by a string of digits
$$A_{n-1}A_{n-2}...A_1A_0.A_{-1}...A_{-m}$$
in which $ 0\le A_i<r $.

{% raw %}
$$(Number)_{r} = \sum^{n-1}_{i=-m}A_i\cdot r^i$$
{% endraw %}

![test](/img/LCDF-chapter1-3.png)

### 3.1 Converting
{% callout success fa-solid fa-shield-halved title="十进制转二进制" %}
整数部分：不断除以2，取余数，直到商为0，再倒着读
例 
10/2=5.......0
5/2=2......1
2/2=1......0
1/2=0......1
10的二进制表示为1010

小数部分：不断乘2，每次取整数（然后抛弃整数再乘2），直到积为1.0
例 
0.875\*2=1.75
0.75\*2=1.5
0.5*2=1.0
原理
$$(...abc)_2=c\cdot 2^0+b\cdot 2^1+a\cdot 2^2+...$$
$$(0.abc...)_2 = a\cdot 2^{-1}+b\cdot 2^{-2}+c\cdot2^{-3}+...$$
{% endcallout %}
我们会发现，整数一定能转成二进制(因为10的自然数次幂均可转换成二进制)，但小数不一定能转成二进制，如0.1，根据我们上面的方法，永远都不会乘积为1.0
>Therefore,the fractional part of decimal cannot always be represented exactly in binary.
{% callout primary fa-solid fa-shield-halved title="二、八、十六进制互相转换" %}
八/十六进制转二进制：每位数字用三/四位二进制表示即可
例 $(312.64)_8=(011 /001 /010.110 /100)_2=(11001010.110100)_2$

二进制转八/十六进制：
整数部分，从右往左三/四个一截断，不够补0，然后每段分别转换
小数部分，从左往右截断
例 
$(10110.11)\_2=(010/110.110)_2=(26.6)\_8$
$(10110.11)\_2=(0001/0110.1100)\_2=(16.C)\_{16}$

八进制和十六进制互相转：以二进制作桥梁
{% endcallout %}

## 4 Binary Codes
Given n binary digits(called ***bits***),a binary code is a mapping from a set of represented elements to a subset(子集) of the $2^n$ binary numbers.
{% folding title="More" class="purple" open=false %}
例如，我们可以用000代表red，001代表orange等等，我们可以用四位二进制数表示1到10这10个数字等

编码vs代码
code is a system of rules to convert information—such as a letter, word, sound, image, or gesture—into another form
代码是由程序员使用特定编程语言编写的一系列指令，这些指令能够被计算机理解并执行
(以上信息来自ai/互联网，可能有误)
{% endfolding %}

### 4.1 Binary Codes for Decimal Digits
#### 4.1.1 Binary Coded Decimal(BCD)
BCD is a ***<font color="blue">weighted</font>*** code,which is also called the 8,4,2,1 code.
- Example: 1001(9)=1000(8)+0001(1)
- <font color="blue">Invalid code</font>:1010,1011,1100,1101,1110,1111
{% folding  class="purple" open=false:: Why Use BCD code? %}
1. 绝对精确的表示，例如，6.1我们可以表示为0110.0001，而6.1用二进制表示总是不精确的
2. 对于十进制输入->二进制处理->十进制输出的场景非常高效，比如输入12，如果不用BCD码，机器要先算1*10+2=12，再用1100存起来，输出时又要把1100转换成12（很多步除法），而BCD码输入时直接00010010，输出时直接4个4个一拆，输出12，不用十进制二进制来回转换
3. 当然，BCD码需要的空间也更多
{% endfolding %}
运算，如5+8，0101+1000，列竖式，得到1101，这在二进制是13>9，在BCD是无效的，需要+6，即加上0110（原理大概是，BCD码中有6个二进制数是用不到的，+6强行进位）得到1 0011，即0001|0011

作为练习，计算1897+2905得到的BCD码，方法是用1897的BCD码与2905的BCD码，每个数分别相加，比如最终结果的个位，就应该是0111(7)+0101(5)得到1100，再加0110得到1|0010,保留0010，向前进一位，接着就是1+1001+0000...最后结果是0100100000000010

事实上，我们可以直接算出1897+2905=4802，然后用BCD码表示4802即可

#### 4.1.2 Excess 3 Code
BCD码每个向后移3位(+3)，如BCD码中3是0011，余三码中0就是0011，类似地，9就是1001+0011(3)=1100
好处是十进制下能进位的两个数，在余三码下相加也会进位。另外，0011取反是1100刚好就是9，1跟8，2跟7也是，它们的表示一个是另一个的反码(至于有什么具体作用，后面我知道了再更新吧hh)

#### 4.1.3 8,4,-2,-1 Code
|Decimal|8,4,-2,-1|
|:-:|:-:|
|0|0000|
|1|0111|
|2|0110|
|3|0101|
|4|0100|
|5|1011|
|6|1010|
|7|1001|
|8|1000|
|9|1111|

依然0和9刚好是反码关系
### 4.2 One-hot Code
只有一个bit为1代表一个状态，比如表示四种颜色，那么可以0001表示红色，0010表示蓝，0100表示绿，1000表示黄，其他都是非法的

"使用这种编码的好处是，决定或改变状态机目前的状态的成本相对较低，容易设计也容易检测非法行为等。
但是相对应的，缺点是信息表示率较低，非法状态非常多而有效状态很少。"(引自学长笔记)
### 4.3 ASCII Character Codes
***American Standard Code for Information Interchange*** is used to represent information sent as character-based data.It uses **7-bits** to represent 94 graphic printing characters and 34 non-printing characters(like '\n').

如A的编码是1000001
<font color="blue">The eighth and most significant bit(MSB) in ASCII are used to hold parity(相同)(即最高位是校验位)</font>

### 4.4 Gray Code
Two successive value differ in only one bit.
|Decimal|Binary|Gray Code|
|:-:|:-:|:-:|
|0|000|000|
|1|001|001|
|2|010|011|
|3|011|010|
|4|100|110|
|5|101|111|
|6|110|101|
|7|111|100|

上表所示的是binary reflected Gray code
反射体现在哪里？

我们先不看最高位，前四个是00,01,11,10
后四个是10,11,01,00
就像镜面反射一样，只不过上面的四个最高位是0，后面四个最高位是1

这是格雷码的一种经典构造方法
1位：0,1
2位:先写1位的，原来的+镜像反射的
0,1,1,0
前半部分最左边加0，后半部分最右边加1
00,01,11,10
3位同理

#### 4.4.1 Conversion from Binary Code to Gray Code
格雷码并不是唯一的，只要满足任意两个相邻的编码(包括第一个和最后一个，即循环相邻)之间有且只有一位二进制数发生变化

Converting a binary code to a specific Gray Code(binary relfected Gray Code) can be obtained by adding each adjacent(邻近的) pair of binary code bits to get the next Gray code bit(Discard carry).

即保留最高位，后面每位和前一位异或
这等价于二进制数和二进制数右移1位(最高位补0)异或

例如110
最高位1
第二位1异或1为0
第三位0异或1为1
101
或者110和011异或，结果也是101

## 5 Error-Detection
有时候可能发送的是00000010
传输中受到noise影响
接受到的是00001010，有一个0变成1

Redundancy(冗余码，即加入额外信息用来校验),in the form of extra bits, can be incorporated into binary code words to detect and correct errors

Error Detection Techniques:
- Single Parity Check
- Cyclic Redundancy Check
- Checksum

Parity(奇偶校验) is an extra bit appended onto the code word to make the number of 1's odd or even.
0101010
Even parity:偶校验，要添加一个parity bit(奇偶校验位)使得1的个数是偶数，故添加一个1
Odd parity:添加一个0

比如对于奇校验01010100,如果接收的是11010100,这时1的个数是偶数，说明有一个0变成了1