---
type: lecture note
tags:
  - lecture
  - algorithm
  - ReinforcementLearning
  - math
teacher:
  - 邵子瑜
aliases: SI252
done: false
---
- 随机变量: 样本点映射到实数上的一个函数.
- 事件: 样本点的子集

强化学习: 遍地都是随机变量, $A_t$, $R_t$, $S_t$以及$S_{t+1}$ 都是随机变量

强化学习可以被转化成一个假设: 最大化期望累积reward (maximization of expected cumulative reward)

LOTE: 全概率公式

条件期望: 投影

性质:
- if $X$ and $Y$ are independent, then $E(Y|X)=E(Y)$
	- $X$和$Y$独立, 那么$X$无法给$Y$提供任何的信息, 因此条件期望退化
- For any function $h$, $E(h(X)Y|X)=h(X)E(Y|X)$
	- 给定$X$可以理解成$X$已经确定了, 那么$h(X)$也就固定了, 因此 $E(h(X))=h(X)$ 可以提出来
- $E(Y_1+Y_2|X)=E(Y_1|X)+E(Y_2|X)$
- For any r.v.s $X$ and $Y$, $E(E(Y|X))=E(Y)$
	- Law of Iterated Expectation
	- 里面的期望是针对 $Y$ 展开的, 外层的期望是对 $X$ 展开的
	- 外层的期望把 $X$ 给消掉了
- For any r.v.s $X$, $Y$, $Z$, $E(E(Y|X,Z)|Z)=E(Y|Z)$, $E(E(X|Z,Y)|Y)=E(X|Y)$
	- 类似的, 外层把没有写的随机变量给消掉了.
	- 或者说, 定义 $\hat E(\cdot)=E(\cdot|Z)$, 那么第一个可以写作 $E(E(Y|X,Z)|Z)=\hat E(\hat E(Y|X))=\hat E(Y)=E(Y|Z)$
- for any r.v.s. $X$, $Y$, $Var(Y)=E(Var(Y|X))+Var(E(Y|X))$

Linear Lesast Square Estimate

> [!tip] Theorem
> LLSE of $Y$ given $X$, denoted by $L[X|Y]$, is the linear function $a+bX$ that minimizes $E[(Y-a-bX)^2]$. In fact,
> $$L[Y|X]=E(Y)+\frac{Cov(X,Y)}{Var(X)}(X-E(X))$$


