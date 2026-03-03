---
title: CS70 Note02
date: 2026-03-03 16:52
categories: 'CS70 Notes'
mathjax: true
---



## Direct Proof
>Direct Proof
Goal: To prove P ⇒ Q.
Approach: Assume P
...
Therefore Q
\
其实就是证明P ⇒ Q是真的，这个statement只要p为真q为假时为假，所以我们假设p为真，只要能推出q为真，就说明p为真q为假的情况不存在，那么P ⇒ Q就是真的
## Proof by Contradiction
>Proof by Contradiction
Goal: To prove P.
(Actually $S\Rightarrow P$ ，$S$为数学公理和基本事实)
Approach: Assume ¬P
(Assume that $S\Rightarrow P$ is false,that is,$\neg S \vee P $is false,that is,$S \wedge \neg P$is true. So actually we assume that $S \wedge \neg P$)
...
R
...
¬R
Conclusion: ¬P ⇒ ¬R ∧ R, which is a contradiction. Thus, P.
\
If you are not convinced by the intuitive explanation thus far as to why proof by contradiction works, here
is the formal reasoning: A proof by contradiction shows that S∧¬P ⇒ ¬R ∧ R ≡ False. The contrapositive
of this statement is True ⇒ (S⇒)P, which means that P must be true.

## Proof by Cases
>Proof:There exist irrational numbers $x$ and $y$ such that $x^y$ is rational.
We proceed by cases. Note that the statement of the theorem is quantified by an existential quantifier:
Thus, to prove our claim, it suffices to demonstrate a single x and y such that xy is rational. To do so, let
$x = \sqrt 2$ and $y = \sqrt 2$ Let us divide our proof into two cases, exactly one of which must be true:
(a) ${\sqrt 2}^{\sqrt 2}$is rational, or
(b) ${\sqrt 2}^{\sqrt 2}$is irrational.
Case (a): Assume first that ${\sqrt 2}^{\sqrt 2}$is rational. But this immediately yields our claim, since x and y are
irrational numbers such that $x^y$ is rational.
Case (b): Assume now that ${\sqrt 2}^{\sqrt 2}$is irrational. Our first guess for x and y was not quite right, but now we
have a new irrational number to play with, ${\sqrt 2}^{\sqrt 2}$. So, let’s try setting $x ={\sqrt 2}^{\sqrt 2}$
and $y = \sqrt 2$. Then,
$x^y = ({\sqrt 2}^{\sqrt 2})^{\sqrt{2}} = (\sqrt 2)^2$,
where the second equality follows from the axiom $(x^y)^z = x^{yz}$. But now we again started with two irrational
numbers x and y and obtained rational xy.
Since one of case (a) or case (b) must hold, we thus conclude that the statement of Theorem 2.8 is true.
Before closing, let us point out a peculiarity of the proof above. What were the actual numbers x

# Disc0A
$(P \land Q)\lor R\equiv (P\lor R)\land(Q\lor R)$
$(P \lor Q)\land R\equiv (P\land R)\lor(Q\land R)$

# Disc0B
>Numbers of Friends
Prove that if there are n ≥ 2 people at a party, then at least 2 of them have the same number of
friends at the party. Assume that friendships are always reciprocated: that is, if Alice is friends
with Bob, then Bob is also friends with Alice.
\
(Hint: The Pigeonhole Principle states that if n items are placed in m containers, where n > m, at
least one container must contain more than one item. You may use this without proof.)


Solution:
We will prove this by contradiction. Suppose the contrary that everyone has a different number of friends at the party. Since the number of friends that each person can have ranges from 0 to n−1,we conclude that for every i∈{0,1,...,n−1}, there is exactly one person who has exactly i friends
at the party. 
In particular, there is one person A who has n−1 friends (i.e., friends with everyone),is friends with a person B who has 0 friends (i.e., friends with no one). 
Let $R$ be "B has 0 friends".So $R$ is true.Since A has n-1 friends and friendship is mutual,we get $\neg R$,so $\neg P\Rightarrow \neg R\land R$


>Key 2
Here, we used the pigeonhole principle because assuming for contradiction that ==everyone has a
different number of friends($\neg P$)== gives rise to n possible containers. Each container denotes the number
of friends that a person has, so the containers can be labelled 0,1,...,n−1. The objects assigned
to these containers are the people at the party. However, containers 0, n−1 or both must be empty
since these two containers cannot be occupied at the same time. This means that we are assigning
n people to at most n−1 containers, and by the pigeonhole principle, at least one of the n−1
containers has to have two or more objects i.e. at least two people have to have the same number of friends
R：可以把 n个物品放进最多 n−1 个抽屉(题目条件得到只能n-1个抽屉)，每个抽屉最多放 1 个物品（来自反设 ¬P）；
\
¬R：根据抽屉原理，n个物品放进最多 n−1个抽屉，必然有抽屉放≥ 2 个物品，不可能每个抽屉最多放 1 个（题目允许直接使用的公理）

