---
title: Chapter 6 - Registers and Register Transfers
date: 2026-05-27 23:06:53
mathjax: true
categories: Logic and Computer Design Fundamentals
---

# 1 RTL-based Design Models
传统的状态图 / 状态表设计只适用于小规模电路：当寄存器位数 n 增大时，状态数呈 2ⁿ指数增长，状态表会变得无法处理。
RTL（寄存器传输级）设计是解决复杂时序电路设计的核心方法：
基本构建块：寄存器（计数器、移位寄存器等）+ 组合逻辑电路（加法器、多路选择器等）
核心思想：描述数据在寄存器之间的传输和处理过程，**而非底层门级连接**
优势：抽象度高、接近行为描述、可直接用 Verilog/VHDL 综合成硬件

# 2 Registers
