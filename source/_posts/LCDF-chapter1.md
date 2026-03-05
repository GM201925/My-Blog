---
title: Chapter 1 - Digital Systems and Information 
date: 2026-03-05 10:20:30
mathjax: true
categories: Logic and Computer Design Fundamentals
---

## Analog and Digital Signals
- Analog and digital signals are two types of signals carrying information.
- Analog signals are ***continuous*** in both values and time,while digital signals are ***discrete*** in values and time.

## Digital System
{% callout primary fa-regular fa-atom title="Digital-System" %}
1. 输入是离散信号，经过离散信号处理系统(这个系统内部也是离散的信息)，输出离散的信号
2. 见下图
![Test](/img/LCDF-chapter1-1.png)
signal conditioning:信号调理，模拟信号容易受干扰
为什么要A TO D
- 模拟信号易受干扰，数字信号差异较大不易干扰
- 数字信号处理更容易，方法更多
{% endcallout %}

### ADC Converters
***Analog-To-Digital Converters*** converts a signal from analog to digital form.
The result of ADC comprises a string of bytes,e.g.,011,100,101

### Information Representation
Binary values are the most prevalent values in digital systems.They are represented by values or ranges of values of physical quantities.
![Test](/img/LCDF-chapter1-2.png)

{% folding title="Questions" class="blue" open=false %}
{% callout default fa-regular fa-atom title="如果落到0.4-0.6怎么办" %}
这些电压是未定义的，这导致了非法的状态，可能是HIGH，可能是LOW，称为
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



## Number System

A number with *radix r* is represented by a string of digits
$$A_{n-1}A_{n-2}...A_1A_0.A_{-1}...A_{-m}$$
in which $ 0\le A_i<r $.

{% raw %}
$$(Number)_{r} = \sum^{n-1}_{i=-m}A_i\cdot r^i$$
{% endraw %}

![test](/img/LCDF-chapter1-3.png)

### Converting
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



