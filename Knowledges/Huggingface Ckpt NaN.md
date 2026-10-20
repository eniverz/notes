---
type: skills
tags:
  - code/python
  - error
done: true
topic:
  - "[[Coding]]"
---
使用huggingface的 PreTrainedModel 来写ckpt, 训练保存之后, 加载并使用, 出现计算结果为 NaN 的问题.

原因是 model 在加载的时候会把 non-persistent buffers 给替换掉 torch.empty, 原因是需要把模型在 `meta` device 上创建出来, 加载数据之后才放到cuda device上.

因此需要在 `_init_weights(self, module)` 里面添加:
```python
def _init_weights(self, module):
	if isinstance(module, ClassType):
		data = module.compute_init_value()
		module.buffer.copy_(data)
	...
	super()._init_weights(module)
```

然后确保在 `__init__` 里面有调用 `self.post_init()`

同时, 如果没有使用这个, 可能会导致 init 创建的模型的权重也有问题, 因为`post_init()`会把non-persistent buffer的权重搞爆掉
