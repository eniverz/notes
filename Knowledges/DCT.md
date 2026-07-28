---
type: skills
tags:
  - math
  - algorithm
  - code/python
done: true
topic:
  - "[[Coding]]"
  - "[[PyTorch]]"
---
# DCT

离散余弦变换, Discrete Cosine Transformer, 将连续的时域信号转换成余弦的频域中的一种转换方法.

这种方法和[[FFT#DFT|离散傅立叶变换]]类似, 但是这种方法并没有用到虚数部分, 完全使用余弦函数进行频域的分解(将一个实数的信号分解成多个余弦函数的加权求和)

## Mathematical Principles

DCT 有多种变体, 其中最常用的是 DCT-II 型. 在实际应用中, 除非特别说明, 通常所提到的 DCT 指的都是 DCT-II.

### 1D Discrete Cosine Transform

对于长度为 $N$ 的一维离散信号序列 $x[n]$, 其 DCT-II 变换公式定义如下:

$$
X[k] = \sum_{n=0}^{N-1} x[n] \cos \left[ \frac{\pi}{N} \left( n + \frac{1}{2} \right) k \right], \quad k = 0, \dots, N-1
$$

对应的逆变换 (IDCT) 用于从频域系数恢复原始信号:

$$
x[n] = \frac{1}{N} X[0] + \frac{2}{N} \sum_{k=1}^{N-1} X[k] \cos \left[ \frac{\pi}{N} \left( n + \frac{1}{2} \right) k \right]
$$

在实际的数值计算库 (如 SciPy 和 MATLAB) 中, 为了保证变换是正交的 (Orthogonal), 通常会引入归一化系数. 正交化后的 DCT 具有 $X^T = X^{-1}$ 的性质, 这对于信号重建非常重要.

### 2D Discrete Cosine Transform

二维 DCT 主要用于图像处理. 由于 DCT 具有可分离性 (Separability), 二维变换可以通过连续执行两次一维变换来实现: 先对图像的每一行进行一维 DCT, 然后对结果的每一列进行一维 DCT.

$$
X[u, v] = \sum_{x=0}^{M-1} \sum_{y=0}^{N-1} I[x, y] \cos \left[ \frac{\pi}{M} \left( x + \frac{1}{2} \right) u \right] \cos \left[ \frac{\pi}{N} \left( y + \frac{1}{2} \right) v \right]
$$

其中 $I[x, y]$ 是原始图像在 $(x, y)$ 处的像素值, $X[u, v]$ 是频域系数. 低频系数 (代表图像的平坦区域) 集中在矩阵的左上角, 而高频系数 (代表边缘和细节) 分布在右下角.

## PyTorch Implementation

在具身智能 ( #EmbodiedAI ) 和机器人学习领域, 动作序列 (Action Sequence) 的生成与预测是一个核心问题. 传统的逐帧预测方法往往容易产生抖动或不连贯的动作, 这对于需要平滑控制的机器人硬件来说是致命的. 为了解决这一问题, 离散余弦变换 (DCT) 被引入到动作空间的建模中. 通过将时域上的动作轨迹转换为频域表示, 模型可以更有效地学习动作的整体趋势, 并天然地保证生成轨迹的平滑性. `pytorch-dct` 库则为在 [[PyTorch]] 深度学习框架中实现这一机制提供了高效且可微分的工具.

### The pytorch-dct Library

PyTorch 虽然功能强大, 但在早期版本中并未原生提供完善的 DCT 变换接口. `pytorch-dct` 是一个基于 PyTorch 实现的第三方库, 旨在填补这一空白. 它的核心价值在于完全使用 PyTorch 的张量运算 (Tensor Operations) 来实现 DCT 和 IDCT, 这意味着整个变换过程是完全可微分的 (Differentiable).

#### Library Features and Mechanics

该库的设计理念是复刻 `scipy.fftpack` 的接口行为, 使得熟悉 SciPy 的用户可以无缝迁移. 由于底层基于 PyTorch, 它可以利用 GPU 进行加速, 并且可以作为神经网络层的一部分参与反向传播. 这对于端到端的深度学习模型至关重要. 在实现上, 它通常利用快速傅里叶变换 (FFT) 的算法技巧来加速 DCT 的计算, 从而避免了 $O(N^2)$ 的直接矩阵乘法开销, 将复杂度降低至 $O(N \log N)$.

#### Basic Usage

使用 `pytorch-dct` 非常直观. 它支持对多维张量的指定维度进行变换. 在默认情况下, 它计算的是 DCT-II 型变换.

```python
import torch
import torch_dct as dct

# 创建一个模拟的张量, 假设 shape 为 (Batch, Time, Dimension)
# 例如: batch_size=16, 序列长度=100, 动作维度=7 (如 7 轴机械臂)
x = torch.randn(16, 100, 7)

# 对时间维度 (维度 1) 进行 DCT 变换
# norm='ortho' 保证正交性, 使得能量守恒
X_freq = dct.dct(x, dim=1, norm='ortho')

# 对频域系数进行 IDCT 逆变换以恢复原信号
x_reconstructed = dct.idct(X_freq, dim=1, norm='ortho')

# 验证重构误差
error = (x - x_reconstructed).pow(2).mean()
print(f"Reconstruction error: {error.item()}")
```

### DCT for Action Sequences in Embodied AI

在具身智能中, 机器人通常需要规划未来的一系列动作, 例如 $T$ 步的关节角度或末端执行器位姿. 直接在高维时域空间中预测这些序列面临着两大挑战. 首先是时序一致性难以为继, 模型生成的动作容易在相邻时间步之间发生突变. 其次是数据的稀疏性, 实际上有效的动作轨迹往往位于低维流形上, 高频分量通常对应着噪声或不必要的抖动.

#### Spectral Representation of Trajectories

利用 DCT, 我们可以将长度为 $T$ 的动作序列 $a_{1:T}$ 映射到频域. 由于物理世界的惯性和电机限制, 机器人的有效运动主要集中在低频部分. 因此, 我们可以截断高频系数, 仅保留前 $K$ 个低频系数来近似整条轨迹. 这种表示方法极大地压缩了状态空间, 使得模型只需要预测少量的频域系数即可重建出一条长时程且平滑的轨迹.

#### Mathematical Formulation

假设动作序列表示为矩阵 $A \in \mathbb{R}^{T \times D}$, 其中 $T$ 是时间步长, $D$ 是动作维度. 我们对时间维度应用 DCT 变换得到频谱矩阵 $\mathcal{A}$:

$$
\mathcal{A} = \text{DCT}(A) \in \mathbb{R}^{T \times D}
$$

在训练阶段, 模型 (如 [[2212.09748|DiT]] 或 [[Deep Learning#Multi-Layer Network|MLP]]) 的目标是预测 $\mathcal{A}$ 的低频部分. 在推理阶段, 模型输出预测的频域系数 $\hat{\mathcal{A}}$, 然后通过逆变换恢复动作序列:

$$
\hat{A} = \text{IDCT}(\hat{\mathcal{A}})
$$

这种方法不仅起到了低通滤波 (Low-pass Filtering) 的作用, 实际上还充当了一种强归纳偏置 (Inductive Bias), 强制模型生成平滑的运动.

### Implementation of Action DCT Encoding

在实际的代码实现中, 我们通常会封装一个编码器-解码器结构来处理动作序列的变换与截断. 下面的代码展示了如何在 PyTorch 中实现针对动作序列的 DCT 处理流程, 这类逻辑常用于 Diffusion Policy 等算法中.

#### Action Encoder and Decoder

这里我们定义两个函数. `encode_actions` 负责将时域动作转换为频域并进行可选的低频截断. `decode_actions` 负责将频域系数补零 (如果进行了截断) 并还原回时域.

> [!tip]- code
> ```python
> import torch
> import torch_dct as dct
> 
> class ActionDCTProcessor:
>     def __init__(self, action_horizon, keep_coeffs=None):
>         """
>         action_horizon: 动作序列的总长度 (T)
>         keep_coeffs: 保留的低频系数数量 (K). 如果为 None, 则不截断.
>         """
>         self.action_horizon = action_horizon
>         self.keep_coeffs = keep_coeffs if keep_coeffs else action_horizon
> 
>     def encode(self, actions):
>         """
>         输入 actions: (Batch, T, D)
>         输出 coeffs: (Batch, K, D)
>         """
>         # 1. 对时间维度 (dim=1) 进行 DCT
>         coeffs_all = dct.dct(actions, dim=1, norm='ortho')
>         
>         # 2. 截断: 仅保留前 keep_coeffs 个系数
>         # 低频系数通常位于索引较小的位置
>         coeffs_kept = coeffs_all[:, :self.keep_coeffs, :]
>         
>         return coeffs_kept
> 
>     def decode(self, coeffs):
>         """
>         输入 coeffs: (Batch, K, D)
>         输出 actions: (Batch, T, D)
>         """
>         batch_size, k, d = coeffs.shape
>         device = coeffs.device
>         
>         # 1. 补零 (Zero Padding)
>         # 如果进行了截断, 需要将丢弃的高频部分用 0 填充回去
>         if k < self.action_horizon:
>             padding = torch.zeros(
>                 (batch_size, self.action_horizon - k, d), 
>                 device=device
>             )
>             coeffs_padded = torch.cat([coeffs, padding], dim=1)
>         else:
>             coeffs_padded = coeffs
> 
>         # 2. 逆 DCT 恢复时域信号
>         actions_reconstructed = dct.idct(coeffs_padded, dim=1, norm='ortho')
>         
>         return actions_reconstructed
> 
> # 示例应用
> processor = ActionDCTProcessor(action_horizon=100, keep_coeffs=20)
> 
> # 模拟一批动作数据 (Batch=16, Time=100, Dim=7)
> raw_actions = torch.randn(16, 100, 7)
> 
> # 编码: 压缩数据, 仅保留 20 个低频特征
> encoded_coeffs = processor.encode(raw_actions)
> print(f"Encoded shape: {encoded_coeffs.shape}") # (16, 20, 7)
> 
> # 解码: 恢复为 100 步的动作序列
> decoded_actions = processor.decode(encoded_coeffs)
> print(f"Decoded shape: {decoded_actions.shape}") # (16, 100, 7)
> ```

### Benefits in Training

在上述实现中, 神经网络只需要学习输出 `(Batch, 20, 7)` 的张量, 而不是 `(Batch, 100, 7)`. 输出维度的降低显著减轻了模型的学习负担. 此外, 由于高频噪声已被物理性地移除, 模型不会去拟合演示数据中的手抖或传感器噪声, 从而提高了在真实机器人上部署时的鲁棒性. 这种策略在模仿学习 (Imitation Learning) 和强化学习 (Reinforcement Learning) 的动作空间建模中已被证明是非常有效的.

## Application in Image Compression

DCT 最经典的应用场景是图像压缩. 利用 DCT 的能量集中特性, 我们可以通过保留左上角的低频系数, 丢弃右下角的高频系数, 来实现有损压缩. 这种方法可以在大幅减少数据量的同时, 保持图像在视觉上的主要特征.

> [!example]- frequency domain filtering
> 下面的示例代码模拟了一个简化的压缩过程. 我们将对整张图像进行 DCT 变换, 然后将幅值较小的系数直接置零 (模拟量化和高频截断), 最后重建图像.
> 
> ```python
> import cv2
> import numpy as np
> import matplotlib.pyplot as plt
> 
> def dct_compress_demo(image_path, keep_ratio=0.1):
>     # 读取图像并转换为灰度图
>     img = cv2.imread(image_path, cv2.IMREAD_GRAYSCALE)
>     if img is None:
>         print("Image not found.")
>         return
> 
>     h, w = img.shape
>     # 转换为 float32 以满足 cv2.dct 的要求
>     img_float = np.float32(img)
> 
>     # 全图 DCT (实际 JPEG 使用的是 8x8 分块 DCT, 这里为了演示简化为全图)
>     dct_coeff = cv2.dct(img_float)
> 
>     # 简单的压缩策略: 对系数的绝对值进行排序, 仅保留幅值最大的部分
>     # 将系数展平并求绝对值
>     flat_coeffs = np.abs(dct_coeff.flatten())
>     # 确定阈值
>     threshold_index = int(len(flat_coeffs) * (1 - keep_ratio))
>     threshold = np.partition(flat_coeffs, threshold_index)[threshold_index]
> 
>     # 创建掩膜: 小于阈值的系数置为 0
>     mask = np.abs(dct_coeff) > threshold
>     compressed_dct = dct_coeff * mask
> 
>     # 逆 DCT 恢复图像
>     idct_img = cv2.idct(compressed_dct)
>     
>     # 将数据转换回 uint8 用于显示
>     idct_img = np.clip(idct_img, 0, 255)
>     idct_img = np.uint8(idct_img)
> 
>     return img, idct_img
> 
> # 使用示例 (请替换为实际图片路径)
> # original, compressed = dct_compress_demo('path_to_image.jpg', keep_ratio=0.1)
> ```
> 
> 在上述代码中, `keep_ratio=0.1` 意味着我们丢弃了 90% 的频域信息. 由于被丢弃的主要是对视觉影响较小的高频细节, 重建后的图像通常仍然能清晰辨认.

### Block Processing

实际的工程应用 (如 JPEG) 并非对全图做一次 DCT, 而是将图像切割成 8x8 的小块分别处理. 这种分块处理有多个好处. 首先, 局部相关性更强, 能量压缩效率更高. 其次, 计算复杂度大幅降低. 在 Python 中, 可以使用简单的循环或 `sklearn.feature_extraction.image.extract_patches_2d` 等工具来实现分块, 然后对每个块分别调用 `cv2.dct`.

## Summary

DCT 是数字信号处理领域的基石之一. 它通过将数据从空间域转换到频率域, 并将能量集中在少数低频系数上, 为数据压缩和特征提取提供了极其高效的手段. 对于 Python 开发者而言, `scipy.fftpack` 提供了理论验证的便利, 而 `cv2.dct` 则提供了工程落地的高效实现. 理解 DCT 的数学原理及不同库之间的实现差异, 是进行高级图像处理算法开发的基础.
