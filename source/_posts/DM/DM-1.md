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
(一般默认x和y的取值范围是一样的，但是约束x和y的domain其实可以不一样)

Scope of a quantifier:the part of a logical expression to which the quantifier is applied.
$\exists x(P(x)\land Q(x))\lor \forall xR(x)$这里或操作符左右的x是不一样的，比如左边可以存在x=1，这时右边是要任意的x属于论域，这两个x不是一个东西

## 3.3 Logical Equivalences Involving Quantifiers
{% callout success::Definition %}
Statements involving predicates and quantifiers are logically equivalent iff they have the same truth value for every predicate substituted into these statements and for every domain of discourse used for the variables iin the expressions.
{% endcallout %}

$$\forall x(A(x)\land B(x))\equiv \forall xA(x)\land \forall xB(x)$$(The domain is same，这里如果等式右侧A的x的domain和B的x的domain不一样，是不等价的)
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

例 求full DNF
$$(\neg P\lor \neg Q)\rightarrow (P\leftrightarrow \neg Q)$$

$\equiv \neg (\neg P\lor \neg Q)\lor ((\neg P\lor \neg Q)\land (P\lor Q))$ 注意这里是用的两个箭头那个
$\equiv P\lor Q$
自己做，full DNF要化成最小项之和
如果不会了，可以尝试用数逻的那种表达做hh

### 5.3 Prenex Normal Form
所有量词提前，那么这个statement is in prenex normal form
$$Q_1x_1Q_2x_2...Q_nx_nB$$
$Q_i$是全称量词或者存在量词，$B$是不含量词的谓词
没有量词的命题也是prenex normal form

方法：
1. 外面的箭头去掉
2. 量词德摩根律把非放到量词里面
3. **重新命名变量**
   例如$\forall xP(x)\land \exists xS(x)$这两个变量是独立的，它们的domain可能不一样，不能写成$\forall x\exists x(P(x)\land S(x))$而应写成$\forall x\exists y(P(x)\land S(y))$
![](/img/DM/DM-1-1.png)

![](/img/DM/DM-1-2.png)
第二问利用step4最后两条，z、u、v可以随意交换，但不能把v放到x、y前面，因为$\forall y \exists v$和$\exists v\forall y$不一样，不过z和u可以放最前面，因为$\forall x \forall y \forall z$可以随意交换

>为什么step4最后两条是合理的？
例如$\forall xP(x)\land \exists yQ(y)$
我们把$A$看作$\exists yQ(y)$
那么原式等价于$\forall x(P(x)\land \exists yQ(y))$
现在对于括号内的式子，我们再把$P(x)$看作$A$(因为P中没有y)
那么$P(x)\land \exists yQ(y)\equiv \exists y(P(x)\land Q(y))$
所以原式等价于$\forall x(\exists y(P(x)\land Q(y))$
即$\forall x\exists y(P(x)\land Q(y))$
这跟我们上面所说的如何理解嵌套量词是相符的

# 6 Rules of Inference
argument: 一系列proposition(称作premises)，有一个conclusion
proof: 证明某个argument是valid的
valid: 如果preceding statements(propositions)都为真，那么结论是真的，我们称这样的argument是valid的

例 
前提：如果今天是晴天，那么今天是周五；今天是晴天(p)
结论：今天是周五(q)
如果$((p\rightarrow q)\land p)\rightarrow q$是tautology，那么这个argument就是valid的
事实上，这个argument确实是valid的，尽管它看上去并不对(因为它的前提事实上是假的，但我们判断时会假设前提都是真的)

argument form: 形式上的argument，也就是它的前提和结论可能并不是具体的命题，而是命题变量
如果不论命题变量取值是啥，即是什么命题，对应的具体的argument都是valid的，那么称这个argument form是valid的
也就是说argument form更偏向一种模板、规范

比如有这么一个argument form
前提是$p_1,p_2,...,p_n$
结论是$q$
如果$p_1\land p_2\land ... \land p_n\rightarrow q$是tautology，那么这个argument form是valid的

接下来我们介绍一些有用的模板(valid argument forms)，不论命题变量取什么值，只要是这个形式的argument都是valid的，这些模板构成了***Rules of Inference***

|Rule of Inference|Name|
|:-:|:-:|
|$p$<br>$p\rightarrow q$<br>——<br>$\therefore q$|Modus ponens|
|$\neg q$<br>$p\rightarrow q$<br>——<br>$\therefore \neg p$|Modus tollens|
|$p\rightarrow q$<br>$q\rightarrow r$<br>——<br>$\therefore p\rightarrow r$|Hypothetical syllogism|
|$p\lor q$<br>$\neg p$<br>——<br>$\therefore q$|Disjunctive syllogism|
|$p$<br>——<br>$\therefore p\lor q$|Addition|
|$p\land q$<br>——<br>$\therefore p$|Simplification|
|$p$<br>$q$<br>——<br>$\therefore p\land q$|Conjunction|
|$p\lor q$<br>$\neg p\lor r$<br>——<br>$\therefore q\lor r$|Resolution|

## 6.1 Using Rules of Inference to Build Arguments
如何证明一个argument是valid的？
假设前提是真的
利用逻辑等价和推理规则，推出结论是真的

{% callout primary::Example %}
Premises:
It is not sunny this afternoon
We will go swimming only if it is sunny
If we do not go swimming, then we will take a canoe trip
If we take a canoe trip, then we will be home by sunset

Conclusion:
We will be home by sunset

p: "It's sunny this afternoon."
r: "We will go swimming."
s: "We will take a canoe trip."
t: "We will be home by sunset."

那么假设就是
$\neg p$
$r\rightarrow p$
$\neg r\rightarrow s$
$s\rightarrow t$

1. $\neg p$ (Premise)
2. $r\rightarrow p$ (Premise)
3. $\neg r$ (Modus tollens 1,2)
   这里我们已经用了一次推理规则，我们假设前提是真的，根据推理规则，上述三步是一个有效论证，那么我们就得到$\neg r$是真的，这又可以作为我们证明的条件
4. $\neg r\rightarrow s$ (Premise)
5. $s$ (Modus ponens 3,4)
6. $s\rightarrow t$ (Premise)
7. $t$ (Modus ponens 5,6)
{% endcallout %}

如果结论是$p\rightarrow q$这样的形式，我们可以把$p$作为假设
因为$(p_1\land ...\land p_n)\rightarrow (p\rightarrow q)\equiv (p_1\land ...\land p_n\land p)\rightarrow q$
(读者可以自己证明)

## 6.2 Rules of Inference for Quantified Statements
|Rule of Inference|Name|
|:-:|:-:|
|$\forall xP(x)$<br>——<br>$\therefore P(c)$|Universal instantiation|
|$P(c)$ for an arbitrary c<br>——<br>$\therefore \forall xP(x)$|Universal generalization|
|$\exists xP(x)$<br>——<br>$\therefore P(c)$ for some element c|Existential instantiation|
|$P(c)$ for some element c<br>——<br>$\therefore \exists xP(x)$|Existential generalization|
|$\forall x(P(x)\rightarrow Q(x))$<br>$P(a)$, where a is a particular element in the domain<br>——<br>$\therefore Q(a)$|Universal modus ponens|

错在哪？
1. $\forall x\exists yG(x,y)$
2. $\exists y G(a,y)$
3. $G(a,c)$
4. $\forall x G(x,c)$
5. $\exists y\forall x G(x,y)$

c只对应那一个a，并不是随便取一个a都对应的c
比如G(x,y): x+y=0
我随便取一个a，确实$\exists G(a,y)$
于是有一个c，我们知道c=-a，G(a,-a)，要注意a是一个具体的值(我们所谓的任取一个a，这个a都是一个具体的值，只是我们强调它可以是任何值)也就是说这个c只对这一个a成立
但是对任意的x，G(x,-a)都对吗？取x=a+1就错了

# 7 Proofs
|术语|译文|
|:-:|:-:|
|axiom|公理|
|lemma|引理|
|corollary|推论|
|conjecture|猜想|

定理是怎么描述的
1. Implied universal quantifiers
   e.g. For all positive real numbers $x$ and $y$, if $x>y$, then $x^2 > y^2$
2. Implicit implications
   e.g. "The square of an odd integer is odd."
   "For all integer $n$, if $n$ is odd, then $n^2$ is odd."

## 7.1 Methods of Proving Theorems
### 7.1.1 Direct Proofs
对于$P(c)\rightarrow Q(c)$
假设前提P(c)是真的，然后证明Q(c)是真的

我的理解依然是，证明就是证明一个论证是有效论证
$P(c)\rightarrow Q(c)$看似只有结论，但是其实是有前提的，很多我们知道的知识、数学公理都是前提；另一方面，也不能说是没前提，由我们前面的知识，P(c)可以是前提，这时结论变成Q(c)，这时上面说的方法其实就是在证明这是个有效论证

### 7.1.2 Proof by Contraposition
$$p\rightarrow q \equic \neg q\rightarrow \neg p$$
例 “完美数”是它所有除自己外的因数之和等于自身的数，如6=1+2+3，证明“完美数”不是素数

也就是说，对任意正整数s，如果s是完美数，那么s不是素数
这等价于证明对任意正整数s，如果s是素数，那么s不是完美数
如果s是素数，那么除去它自己的因数之和为1，而s不等于1，所以s不是完美数
我们证明了$\neg q\rightarrow \neg p$是真的
由逻辑等价，$p\rightarrow q$是真的
Q.E.D

### 7.1.3 Vacuous Proof
前提$p$是错的，那么$p\rightarrow q$是真的
这看上去和我的理解“证明就是展示一个论证是有效的的过程”有些矛盾

例 证明“对任意实数x，如果x的平方小于0，那么x大于1”
证明：
$P(x): x^2 < 0$
只需证明任意c，$P(c)\rightarrow (c>1)$
for an arbitrary c, $P(c)$ (Additional Premise)
$\neg P(c)$ (Premise 数学公理)
$P(c)\land \neg P(c)$
***爆炸原理(ex contradictione quodlibet)*** 矛盾推出任何命题的形式都是有效论证，即$p\land \neg p\rightarrow q$ is tautology.

上面好像有点复杂，其实可以不把p推q的p作为假设
任意的c，$c^2\ge 0$ (Premise 数学公理)
由implication的定义，$P(c)\rightarrow (c>1)$为真
由UG $\forall x(P(x)\rightarrow (x>1))$

### 7.1.4 Trivial Proof
如果q是真的，那么$p\rightarrow q$是真的
何意味

### 7.1.5 Proof by Contradiction
1. 证明$p$时，可以假设$\neg p$，如果得到了矛盾$q\land \neg q$
即得到$\neg p\rightarrow (q\land \neg q)$
也就是说我们先证明$\neg p\rightarrow (q\land \neg q)$，$\neg p$作为premise
证明完这个，这个又可以成为我们的premise(我们已经证明的东西可以作为premise)
这等价于$T\rightarrow p$
由implication的定义，$p$

2. 证明$p\rightarrow q$，其实上面证明$p$时也是有前提的，如数学公理等，所以也可以理解成$p\rightarrow q$的形式
依旧先证明$\neg (p\rightarrow q)\equiv p\land \neg q$推出矛盾，通常这个矛盾就是$q\land \neg q$(与题意矛盾)
我们已经证明的东西可以作为premise，和上面一样就能得到$p\rightarrow q$是真的


我讲的可能有些啰嗦，因为我试图把“证明就是证明一个论证是有效的”这个理解贯彻到底，所以在解释各种证明方法到底是在干嘛。如果你已经理解了，可以不用看我写的东西。我想大多数人应该都不用看。

证明没有最大的素数
假设有最大的素数s
对于r=2×3×5×...×s
r+1要么是素数要么是合数，如果是素数，那么s不是最大的素数
如果r+1是合数，由算术基本定理，它可以分解为若干个素数的乘积，而这些素数不能是2到s的素数(nk+1不是k的倍数)，所以r+1的因数应该有一个比s大的素数
$q$: s是最大的素数
我们假设$\neg p$得到$q\land \neg q$
所以$p$为真

### 7.1.6 Proofs of Equivalence
怎么证明$p_1...p_n$是等价的
即证明$p_1\leftrightarrow ...\leftrightarrow p_n$
$$p_1\rightarrow ...\rightarrow p_n\equiv (p_1\rightarrow p_2)\land ...\land (p_{n-1}\rightarrow p_n)\land (p_n\rightarrow p_1)$$

### 7.1.7 Proof by Cases
把$p\rightarrow q$拆成$(p_1\lor p_2\lor ...)\rightarrow q\equiv (p_1\rightarrow q)\land ...\land (p_n\rightarrow q)$

例 证明：如果整数n不被2整除也不被3整除，那么$n^2-1$被24整除
$n=6k+1$或$n=6k+5$，然后分别证明即可


