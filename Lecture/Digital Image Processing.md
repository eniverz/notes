---
type: lecture note
tags:
  - lecture
  - algorithm
  - ComputerVision
teacher:
  - Xavier Lagorce
aliases: CS270
done: false
---
# Lecture 01

[[CS270_2026Fall_Lecture_01.pdf]]

![[CS270_2026Fall_Lecture_01.pdf#page=27&rect=90,78,885,418]]

# Lecture 02

[[CS270_2026Fall_Lecture_02.pdf]]

## neighbors of pixels

按照某种空间规则, 位于某个像素周围的其他像素. 用于定义"距离", 定义是否"相邻", 也是滤波,边缘检测,图像分割等操作的基础

假设灰度图中, 一个点的坐标为 `(x,y)`, 使用 `f(x,y)` 表示在 `(x,y)` 的点的值(在灰度图中, 这个值表示亮度). e.g. `p=(12, 13)` 表示一个点的坐标, `f(p)=120` 表示这个点的亮度为120

![[CS270_2026Fall_Lecture_02.pdf#page=28&rect=634,152,842,363|100]]

如图, 假设一个中心元素p周围的元素为:
```
a b c
d p e
f g h
```

那么, 4-neighbors $N_4(p)=\{(x+1,y),(x,y+1),(x-1,y),(x,y-1)\}$ 为:
```
  b  
d p e
  g
```

对角邻居是 $N_D(p)=\{(x+1,y+1),(x+1,y-1),(x-1,y+1),(x-1,y-1)\}$:
```
a   c
  p
f   h
```

8-neighbors $N_8(p)=\{(x+1,y),(x+1,y+1),(x+1,y-1),(x,y+1),(x,y-1),(x-1,y+1),(x-1,y),(x-1,y-1)\}$ 就是周围的所有8个neighbors:
```
a b c
d p e
f g h
```

## neighborhood of pixel

- closed neighborhood 闭邻域: 包含 p 本身的邻域
- open neighborhood 开邻域: 不包含 p 本身的邻域

邻域主要有这些用途：

|用途|邻域发挥什么作用|
|---|---|
|平滑、去噪|用周围像素的均值或中位数估计中心像素|
|边缘检测|比较相邻位置的亮度变化|
|区域生长、分割|从一个像素出发，吸收符合条件的邻居|
|连通分量分析|判断哪些目标像素连在一起，统计有几个对象|
|膨胀、腐蚀等形态学操作|按局部邻域规则扩大或缩小目标区域|

## adjacency

邻接性: 位置靠近的元素, 是否属于同一个连接

首先需要定义用什么邻域, 然后定义可以连接的范围是多少. 定义邻域可以直接使用 $N_4,N_8$ 等, 定义范围使用 $V=[Val_\min,Val_\max]$ 来定义(只有数值在这个区间内的才能连接)

e.g. ![[CS270_2026Fall_Lecture_02.pdf#page=33&rect=157,59,796,198]]

这个题里面, $V=[10,17]$, 也就是说, 11->13->16 是adjacent(假设使用 $N_4$)

- 4-adjacency: $p,q\in V,q\in N_4(p)$, p和q都在范围内, 并且q是p的4-neighbor
- 8-adjacency: $p,q\in V,q\in N_8(p)$, p和q都在范围内, 并且q是p的8-neighbor
- M-adjacency: $p,q\in V, (q\in N_4(p))\text{ or }(q\in N_D(p)\text{ and }N_4(p)\cap N_4(q)\not\in V)$, p和q都在范围内, 并且: 要么q是p的4-neighbor, 要么q是p的对角邻居且不能通过 4-neighbor 连接 (保证路径的唯一性)

![[CS270_2026Fall_Lecture_02.pdf#page=39&rect=57,112,927,440]]

M-adjacency的目的是, 8-adjacency会允许局部三角连接:
```
0  A  B
0  C  0
0  0  D
```

8-adjacency允许 A->B->C (重点是 B->C) , 但是M-adjacecny不允许 B->C, path

## path


