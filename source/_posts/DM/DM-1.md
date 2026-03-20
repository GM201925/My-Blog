---
title: Chapter 1 - Logic and Proofs
date: 2026-03-13 17:45:21
categories: Discrete Mathematics
mathjax: true
---
{% callout primary title="写在前面" %}
离散数学的笔记对[翁老师分享的前辈的笔记](https://mem.ac/course/dm/)有所参考，所以放在前面，文章中讲的不详细的地方，大家可以去阅读学长的笔记
{% endcallout %}

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

{% folding class="blue" title="术语对照" %}
参考这就来了(
- conjunction 合取
- disconjunction 析取
- exclusive or 异或
- precedence 优先级
{% endfolding %}

# 1 Propositional Logic
## 1.1 Proposition
{% callout success title="Definition" %} 
A proposition is a declarative sentence that is either true or false,but not both.
- Beijing is the captial of China.
- He shouted, "Stop!"(光看这句话我们不知道真假，但是在那一刻地点那一时刻，他要么说了，要么没说，一定可以确定真假)
- x+1=2(这不是命题，因为我们无法判断真假，注意与上一个区别，上一个是我们不知道真假，但可以确定真假，譬如问一下在场的人，而这一个无法确定，因为我们不知道x的取值)
- Open the door.(祈使句，不是陈述句)
Anyway,it doesn't matter.
{% endcallout %}

Propositional Logic is the area of logic that deals with propositions.
- <font color="blue">Propositional variables</font>:$p,q,r,s,...$ each represents a proposition like "Today is Friday."
- <font color="blue">Truth value</font>:T(if a proposition is true),F
- <font color="blue">Logical operators(Connectives) </font>:be used to form ***compound propositions*** from existing propositions

## 1.2 Logical Operators
- Negation $\neg p$
- Conjunction $p\land q$(p和q都真才为真)
- Disjunction $p \lor q$(p和q至少有一个为真即为真)
- Exclusive OR $p\oplus q$(p和q真值不同时为真)
### 1.2.1 Conditional Operator
{% callout success title="Definition" %}
Let $p$ and $q$ be propositions.The conditional statement(implication)$p\rightarrow q$,is the proposition "if $p$,then $q$."

The conditional statement is false only when $p$ is true and $q$ is false.
{% endcallout %}
{% folding blue::Equivalent Forms %}
- If p,then q
- p implies q
- q if p
- q when p
- q whenever(every time that,每当，只要) p
- p is sufficient condition for q
- ***p only if q***
  (只有q为真时，p才能是真的，也就是说***p can't be true when q isn't true,q为假时p为假(事实上，这个命题就只规定了这一件事)***，所以p为真q为假时这个命题为假；如果p是假的，那么q是真是假这个命题都是对的，因为这个命题只要求p是真的时，q必须是真的)
- 让我们举个例子，我去上课only if我醒了，即只有我醒了，我才会去上课，但这并不说明我一定去上课了，其实就是在说我醒了是我去上课的必要条件，一定要我醒了，我才能去上课，而不是如果我醒了我就去上课(如果你因此更晕了，那就记住它或者忘掉我所说的orz)
- $q$ unless $\neg p$(q是真的，除非p是假的，也就是p为真时q一定为真，p为假时这个命题一定正确，因为没说p是假的时对q有什么要求，而p是假的但q是真的时这个命题就是假的，因此它与implication等价)
{% endfolding %}

Converse of Conditional Statement $p\rightarrow q$:
$q\rightarrow p$
二者不一定等价

Inverse of Conditional Statement $p\rightarrow q$:
$\neg p\rightarrow \neg q$
二者不一定等价

Contrapositive of Conditional Statement $p\rightarrow q$:
$\neg q\rightarrow \neg p$
二者等价
When two compound propositions always have the same truth value in all possible cases we call them ***equivalent***

### 1.2.2 Biconditional Operator
$p\leftrightarrow q$ is the position "p if and only if q."
即$p\leftrightarrow q\equiv (p\rightarrow q)\land (q\rightarrow p)$

p和q同时为真或同时为假时，$p\leftrightarrow q$为真

如果$p\leftrightarrow q$是永真式，那么称p和q是逻辑等价的，其中p和q是复合命题
注意一定要求是***永真式***，如果说：如果$p\leftrightarrow q$是真的，那么p和q等价，这样是不对的
例如，p:今年是2026年，q:我今天喝了水
那么在2026年，有一天我喝了水，那么那一天$p\leftrightarrow q$就是真的，我就可以得到今年是2026年等价于我今天喝了水，即如果我今天喝了水，那么今年是2026年，这是不合理的，(而且根据上面的定义，这意味着我今天喝水和今年是2026年一定同时成立或同时不成立)我们必须要求$p\leftrightarrow q$是恒真的，即p和q只可能同时为真或者同时为假，不能有其他情况，而这个例子中，完全可以是今年是2027年而我今天喝了水，$p\leftrightarrow q$就是假的了
{% callout warning %} 
如果你还是很晕，那么记住下面这点：
我们说复合命题p和复合命题q等价，只是在陈述“p和q的真值一定一样”或“$p\leftrightarrow q$永远是真的”，$p\equiv q$并不是一个复合命题，虽然$p\equiv q$确实可能为真可能为假，但更多时候我们把它看作一种关系的描述“p和q的真值一定一样”

另一方面，$p\leftrightarrow q$是真的只是说明p是真的，q是真的，或p是假的，q是假的，它就像是真值表中某一行的各种值，并没有说明“p和q只能同时真或同时假”，而$p\equiv q$更多是一种对p和q真值的限制的陈述
{% endcallout %}

### 1.2.3 Precedence of Logical Operators
|Operator|Precedence|
|:-:|:-:|
|$\neg$|1|
|$\land$ $\lor$|2|
|$\rightarrow$ $\leftrightarrow$|3|

## 1.3 Translation
{% callout primary %}
You can access the Internet from campus only if you are a computer science major or you are not a freshman.
Let
a:"you can access the Internet from campus"
c:"you are a cs major"
f:"you are a freshman"

$a\rightarrow c\lor \neg f$
{% endcallout %}

## 1.4 Consistent System Specifications
{% callout success title="Definition" %}
A list of propositions is ***consistent*** if it is possible to assign truth values to the proposition variables so that each proposition is true.
{% endcallout %}

# 2 Propositional Equivalences
## 2.1 Classification of Compound Propositions
- Tautology:compound proposition that is always true
- Contradiction:compound proposition that is always false
- Contingency neither a tautology nor a contradiction
For example,$p\lor \neg p$ is a tautology

## 2.2 Logical Equivalences
{% callout success title="Definition" %}
The compound propositions $p$ and $q$ are called ***logically equivalent*** if $p\leftrightarrow q$ is a tautology.
Notation:$p\equiv q$
{% endcallout %}

|Name|Equivalence|
|:--:|:---:|
|Identity laws|$p\land T\equiv p$<br>$p\lor F\equiv p$|
|Domination laws|$p\lor T\equiv T$<br>$p\land F\equiv F$|
|Idempotent laws(幂等律)|$p\lor p\equiv p$<br>$p\land p\equiv p$|
|Commutative laws|$p\lor q\equiv q\lor p$<br>$p\land q\equiv q\land p$|
|Associative laws|$(p\lor q)\lor r\equiv p\lor (q\lor r)$<br>$(p\land q)\land r\equiv p\land (q\land r)$|
|<font color="blue">Distributive laws</font>|$p\lor (q\land r)\equiv (p\lor q)\land (p\lor r)$<br>$p\land (q\lor r)\equiv (p\land q)\lor (p\land r)$|
|De Morgan's laws|$\neg (p\land q)\equiv \neg p\lor \neg q$<br>$\neg (p\lor q)\equiv \neg p\land \neg q$|
|<font color="blue">Absorption laws</font>|$p\lor (p\land q)\equiv p$<br>$p\land (p\lor q)\equiv p$|
|<font color="red">Implication law</font>|$p\rightarrow q\equiv \neg p\lor q$|
|Equivalence law|$p\leftrightarrow q\equiv (p\rightarrow q)\land(q\rightarrow p)$|

## 2.3 Propositional Satisfiability
A compound proposition is satisfiable if there is an assignment of truth values to its variables that make it true.When no such assignments exist,the compound proposition is unsatisfiable.

# 3 Predicates and Quantifiers
## 3.1 Predicate
{% callout success::Definition %}
A predicate (propositional function) is a statement that contains variables.Once the values of the variables are specified,the function has a truth value.
{% endcallout %}
- $P(x)=$"$x>3$",and $P(3)$ is false
- $Q(x,y)=$"$x$ is the $y$'s best friend"
## 3.2 Quantifiers
Predicate Logic:the area of logic that deals with predicate and quantifiers.

### 3.2.1 Universal Quantification
{% callout success::Definition %} 
A universal quantification of $P(x)$,denoted by $\forall x P(x)$,is the statement "P(x) for all values of x in the domain."

Domain:range of the possible values of the variable x
An element for which P(x) is false is called a counterexample(反例) of $\forall x P(x)$
{% endcallout %}


{% callout primary::Example %}
1. The truth value of$\forall x(x^2>x)$ is false if the domain consists of all real numbers.

2. Express the following statement as a universal quantification:
"All lions are fierce."

Let Q(x) denote the statement "x is fierce."
(1) Assuming that the domain is the set of all lions.
$$\forall xQ(x)$$

(2) Assuming that the domain is the set of all creatures.
P(x): "x is a lion."
$$\forall x(P(x)\rightarrow Q(x))$$
{% endcallout %}

### 3.2.2 Existential Quantification
{% callout success::Definition %} 
An existential quantification of P(x),denoted by $\exists x P(x)$,is the statement "There exists an element x in the domain such that P(x)."
{% endcallout %}
{% callout primary::Example %}
Express the following statement as an existential quantification.
"Some real numbers are rational numbers."

Let Q(y): y is a rational number
(1) Assuming that the domain is the set of all real numbers.
$$\exists yQ(y)$$
(2) Assuming that the domain is the set of all complex numbers.Let R(y): y is a real number
$$\exists y(R(y)\land Q(y))$$
{% endcallout %}
存在唯一：$\exists !$

### 3.2.3 Quantifiers with Restricted Domains
{% callout primary::Example %}
Domain: the real numbers
$\forall x<0 (x^2>0)$ means for every real number $x$ with $x$ > 0,$x^2>0$
It's logically equivalent to $\forall x(x<0\rightarrow (x^2>0))$

$\exists y>0(y^2=2)\equiv \exists y((y>0)\land (y^2=0))$
{% endcallout %}

### 3.2.4 Precedence of Quantifiers
The quantifiers have higher precedence than all logical operators from propositional calculus.
$\forall xP(x)\lor Q(x)\equiv (\forall xP(x))\lor Q(x)$

### 3.2.5 Binding Variables
When a quantifier is used on the variable x,we say that this occurence of the variable is ***bound(受约束的)***.

A variable neither quantified nor specified with a value is said to be ***free***.

***All the variables in a propositional function must be quantified or set equal to a particular value to turn it into a proposition.***
例如$\exists x (x+y)=1$中x被存在量词作用，是约束变元，y是自由变元，当然这不是命题，可以是谓词$Q(y):\exists x (x+y)=1$，根据上面所述，y也必须被赋值或者被量词作用才能变成命题
(一般默认x和y的取值范围是一样的，不清楚，问了几个ai这么说的)

Scope of a quantifier:the part of a logical expression to which the quantifier is applied.
$\exists x(P(x)\land Q(x))\lor \forall xR(x)$这里或操作符左右的x是不一样的，比如左边可以存在x=1，这时右边是要任意的x属于论域，这两个x不是一个东西

## 3.3 Logical Equivalences Involving Quantifiers
{% callout success::Definition %}
Statements involving predicates and quantifiers are logically equivalent iff they have the same truth value for every predicate substituted into these statements and for every domain of discourse used for the variables iin the expressions.
{% endcallout %}

$$\forall x(A(x)\land B(x))\equiv \forall xA(x)\land \forall xB(x)$$(The domain is same)
$$\exists x(A(x)\lor B(x))\equiv \exists xA(x)\lor \exists xB(x)$$

x is not occurring in A
|Equivalence|Proof|
|:-:|:-:|
|$\neg \forall xP(x)\equiv \exists x\neg P(x)$||
|$\neg \exists xP(x)\equiv \forall x\neg P(x)$||
|$\forall xP(x)\lor A\equiv \forall x(P(x)\lor A)$|分A为T或F两种情况证明|
|上式任意或存在选一个，与变成或或者或变成与，四种情况都成立||
|$\forall x(A\rightarrow P(x))\equiv A\rightarrow \forall xP(x)$|$\forall x(A\rightarrow P(x))\equiv \forall x(\neg A\lor P(x))$<br>$\equiv \neg A\lor \forall xP(x)$|
|上式任意变成存在仍成立||
|$\forall x(P(x)\rightarrow A)\equiv \exists xP(x)\rightarrow A$|先去掉箭头，再利用第三行|
|上式任意、存在互换仍成立||

## 3.4 Translation
{% callout primary::Example %}
All lions are fierce.
Some lions do not drink coffee.


P(x):x is a lion
Q(x):x is fierce
R(x):x drinks coffee
Assume the domain consists of all creatures.

(1)$\forall x(P(x)\rightarrow Q(x))$
(2)$\exists x(P(x)\land \neg R(x))$
注意不能是$\exists x(P(x)\rightarrow \neg R(x))$，这样的话并不是说有些狮子不喝咖啡，事实上如果x不是狮子，这个命题也是真的，就不是原来的意思了
同理(1)中不能用与，那样就变成所有的生物都是狮子而且很凶狠
{% endcallout %}

## 3.5 Nested Quantifiers
Two quantifiers are nested if one is within the scope of the other.
$\forall x \exists yC(x,y)$可理解为遍历论域中每个x，都要存在一个y满足C(x,y)

这里$\exists yC(x,y)$是在$\forall x$的scope里的,体现在C(x,y)里的x受前面量词的论域限制

也可以理解成$\forall xP(x)$，其中$P(x)$为$\exists y C(x,y)$，P(x)里的x在量词的scope里

{% callout primary::Example %}
Everyone has exactly one best friend.
Domain:All people

B(x,y): "y is the best friend of x."

$$\forall x(\exists y(B(x,y)\land \forall z((z\ne y)\rightarrow \neg B(x,z))))$$
注意x后的括号要一直括到最后，否则B(x,z)里的x就没被量词作用，也没赋值，就不是命题
同理y,z右侧的括号也要括到最后
也可以写成嵌套量词
$$\forall x \exists y \forall z(B(x,y)\land ((z\ne y)\rightarrow \neg B(x,z)))$$
过程是先遍历论域中的x，对每一个x要找到一个y满足：遍历论域中的z，$Q(x,y,z)$即$B(x,y)\land ((z\ne y)\rightarrow \neg B(x,z))$成立
{% endcallout %}

### 3.5.1 The Order of Quantifiers
The order of nested quantifiers matters if quantifiers are of different types.
{% callout primary::Example %}
论域：实数
$\exists x\forall y(x+y=1)$
存在一个x，使得对所有y，x+y=1，这是假命题

$\forall y\exists x(x+y=1)$
对任意y，都存在一个x使得x+y=1，这是真命题
{% endcallout %}

### 3.5.2 Negating Nested Quantifiers
Successively apply the rules for negating statements involving a single quantifier.
这时这样理解的好处就凸显了
$\forall x \exists yC(x,y)$可理解为遍历论域中每个x，都要存在一个y满足Q(x,y),这里$\exists yQ(x,y)$是在$\forall x$的scope里的，<font color="blue">也可以理解成$\forall xP(x)$，其中$P(x)$为$\exists y C(x,y)$，P(x)里的x在量词的scope里</font>

例如$\neg (\forall x\exists y(xy=1))$
等价于$\exists x(\neg \exists y(xy=1))$
等价于$\exists x\forall y(\neg (xy=1))$
等价于$\exists x\forall y(\neg (xy\ne 1))$

# 4 Functionally Complete
{% callout success::Definition %}
A set of logical operators is called ***functionally complete*** if every compound proposition is logically equivalent to a compound proposition involving only this set of logical operators.
{% endcallout %}
比如与、或、非构成的逻辑操作符集合就是functionally complete的
与非也可以(跟数逻联动了)

# 5 Propositional Normal Forms
Literal(字面量):$p$或者$\neg p$

Disjunctions (conjunctions) with ***one*** or more literals as disjuncts (conjuncts) are called disjunctive (conjunctive) clauses.

例如
$q\lor r$是析取子句
$p$是析取子句，也可以是合取子句
$p\lor q\land r$不是clause(子句)

## 5.1 Normal Forms(CNF&DNF)
A <font color="red">conjunction</font> with one or more<font color="red"> disjunctive clauses</font> is said to be in conjunctive normal form.
$$(A_1\lor A_2\lor ...)\land ...\land (B_1\lor B_2\lor ...)$$

A <font color="red">disjunction</font> with one or more<font color="red"> conjunctive clauses</font> is said to be in disjunctive normal form.

例如
$p\land (q\lor r)$是CNF
$p$如果看作disjunctive clause就是CNF，看作conjunctive clause就是DNF
$\neg q\land p$是CNF(两个disjunctive clause)，也可以是DNF(看作一个conjunctive clause)

### 5.2 How to Obtain Normal Forms
1. 利用$p\rightarrow q\equiv \neg p\lor q$和$p\leftrightarrow q\equiv (p\rightarrow q)\land (q\rightarrow p)$和$p\leftrightarrow q\equiv (p\land q)\lor (\neg p\land \neg q)$去掉箭头
2. 德摩根律去掉非
3. 利用分配律统一符号