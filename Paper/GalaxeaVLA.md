---
type: paper
tags:
  - VLA
  - EmbodiedAI
  - VLM
  - Robot
  - code/python
  - DeepLearning
  - ComputerVision
publish: company blog
pdf: "[[Paper/PDF/Galaxea_G0_5.pdf]]"
rate: 🌟🌟🌟🌟
done: false
aliases:
  - Galaxea G0.5 Technical Report
  - G0.5
---
> [!note]- paper
![[Paper/PDF/Galaxea_G0_5.pdf]]

## Motivation

目前主流的VLA使用 "VLM as Encoder", 如 [[2504.16054|pi0.5]] 等, VLM 仅用于提取Vision和Language的hidden state, 而连续的动作生成是由一个独立的Action Expert负责的.

虽然这种做法提高了动作执行的连贯性, 但是VLM本身的能力并没有完全发挥出来(如, 思维链, 上下文学习, prompt控制, 等等).

早期的 VLA 自回归模型(如, [[RT-2]] 等)虽然保留了VLM的生成能力, 但是按action dim进行离散化导致action sequence过长, 无法满足高频控制的要求.

因此, 本文希望使用 VLM-as-Actor, 通过结构化的动作压缩分词技术解决计算瓶颈.

## Contribution

1. 统一的cross-embodiment action codec: 通过语义对齐(如, LeftArm, RightArm, Base, ...), 将不同的结构/自由度的机器人连续动作转化为共享的离散token vocabulary. 同时, 模型可以丢弃闲置部分的token, 可以有效提高模型的生成效率
2. 基于AutoRegressive的Native [[CoT]]: 不使用外挂的推理模块, 而是使用模型内部自带的思维链能力, 在输出action token之前自回归生成子任务文本/目标边界/2D轨迹, 增强模型对新场景的泛化能力/语言指令跟随能力/long-horizon多步执行能力
3. Visual Memory: 在Vision Transformer中, 采用了分解的时空注意力机制, 让模型可以吸收过去的frame
4. 无须重训的prompt-driven behavior control: 可以在提示词中修改动词或者添加副词和空间方位约束(如, "用力推进去"), 可以直接改变robot的物理执行策略, 而不需要重新训练新的网络

## Methods





