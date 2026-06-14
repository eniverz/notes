---
type: skills
tags:
  - code
  - math
  - algorithm
  - DeepLearning
  - MachineLearning
done: true
topic:
  - "[[Coding]]"
aliases:
  - EMA
---
# Exponential Moving Average

**指数移动平均 (EMA)** 是一种平滑技术，它通过维护一个模型的两个副本来解决训练不稳定的问题：

1.  **在线网络 (Online Network):** 正常接收梯度并快速更新的网络。
2.  **目标网络 (Target Network):** 从不接收梯度，其权重是“在线网络”过去所有权重的一个**指数加权平均**。

它的工作机制是，在每次更新“在线网络”后，都通过以下公式极其缓慢地更新“目标网络”：
$$w_t=\tau w_{t-1}+(1-\tau)w'_t$$