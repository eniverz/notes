---
type: skills
tags:
  - code/python
  - DeepLearning
  - LLM
done: true
topic:
  - "[[Coding]]"
  - "[[PyTorch]]"
---
# Normal Files

## open

使用 `open()` 读取和写入文件，搭配 `with` 可以在操作结束后自动关闭文件。处理文本时，通过 `encoding="utf-8"` 指定编码。

```python
# 读取整个文件
with open("example.txt", "r", encoding="utf-8") as f:
    content = f.read()
    print(content)

# 写入文件：不存在则创建，存在则覆盖
with open("example.txt", "w", encoding="utf-8") as f:
    f.write("你好，Python！")

# 追加内容
with open("example.txt", "a", encoding="utf-8") as f:
    f.write("\n这是新增的一行。")
```

常用打开模式：

| 模式              | 用途                               |
| --------------- | -------------------------------- |
| `"r"`           | 读取，文件必须存在；默认模式                   |
| `"w"`           | 写入，不存在则创建，存在则覆盖原有内容              |
| `"a"`           | 追加到文件末尾，不存在则创建                   |
| `"x"`           | 创建新文件，已存在则报错                     |
| `"rb"` / `"wb"` | 读取 / 写入二进制文件，例如图片；不指定 `encoding` |

读取大文件时，可以逐行处理，避免一次性加载全部内容：

```python
with open("example.txt", encoding="utf-8") as f:
    for line in f:
        print(line.rstrip("\n"))
```

## shutil

`shutil` 是 Python 内置模块，适合复制、移动文件，以及操作整个目录。

```python
import shutil

# 复制文件，并尽量保留修改时间、权限等元数据
shutil.copy2("example.txt", "backup.txt")

# 移动文件到已有目录，也可以同时修改文件名
shutil.move("backup.txt", "data/backup.txt")

# 复制整个目录，包括子目录和文件；目标目录默认必须不存在
shutil.copytree("data", "data_backup")

# 删除整个目录及其中的所有内容
shutil.rmtree("data_backup")
```

常用函数：

| 函数 | 用途 |
| --- | --- |
| `shutil.copy(src, dst)` | 复制文件内容和权限；目标可以是文件路径或已有目录 |
| `shutil.copy2(src, dst)` | 在 `copy()` 的基础上，尽量保留时间戳等元数据 |
| `shutil.move(src, dst)` | 移动文件或目录；目标为已有目录时，移入该目录 |
| `shutil.copytree(src, dst)` | 递归复制整个目录 |
| `shutil.rmtree(path)` | 递归删除目录及其内容 |

如果复制目录时允许目标目录已存在，可以使用 `dirs_exist_ok=True`，同路径的文件会被覆盖：

```python
shutil.copytree("data", "data_backup", dirs_exist_ok=True)
```

# Json

使用json管理config

```python
import json # builtin module

with open("file/path", "r") as f:
	cfg = json.load(f)

with open("file/path", "w") as f:
	json.dump(cfg, f, indent=4)
```

如果希望直接输出到str, 可以使用dumps:
```python
import json

cfg = {...}
json_str: str = json.dumps(cfg, ...)

# load from str
cfg = json.loads(json_str)
```

# Yaml

需要安装 `pyyaml` 或者说 `pyyaml-include`

使用:
```python
import yaml

with open("file/path", "r") as f:
	cfg = yaml.safe_load(f)

with open("file/path", "w") as f:
	yaml.safe_dump(cfg, f, sort_keys=False)
```

相似的, 可以使用str来 load/dump. 而且不需要其他的函数, 直接使用这个函数即可

```python
import yaml

cfg = {...}
yaml_str = yaml.safe_dump(cfg, ...)

cfg = yaml.safe_load(yaml_str, ...)
```

# Toml

toml的读取可以使用python自带的 `tomllib` , 但是写入文件需要安装第三方库 `tomli-w`

```python
import tomllib
import tomli_w

toml_str = """
...
"""

data = tomllib.loads(toml_str)

with open("file/path", "rb") as f:
	cfg = tomllib.load(f)

toml_str = tomli_w.dumps(data)

with open("file/path", "wb") as f:
	tomli_w.dump(data, f)
```

注意, toml必须要使用二进制文件读取. 如果希望读取一个普通的文本文件, 可以先read再loads:
```python
with open("file/path", "r") as f:
	data = tomllib.loads(f.read())
```

# Pickle

使用pickle保存和读取Python对象, 比如dict、list、set等

```python
import pickle # builtin module

with open("file/path.pkl", "rb") as f:
	cfg = pickle.load(f)

with open("file/path.pkl", "wb") as f:
	pickle.dump(cfg, f)
```

如果希望直接输出到bytes, 可以使用dumps:
```python
import pickle

cfg = {"name": "example", "epochs": 10}
pickle_bytes: bytes = pickle.dumps(cfg)

# load from bytes
cfg = pickle.loads(pickle_bytes)
```

注意, pickle使用二进制格式, 文件需要以 `rb` / `wb` 模式打开, 不指定encoding。`dumps()` 返回的是bytes, 不是str。

只加载可信来源的pickle数据, 因为反序列化时可能执行代码。

# Torch

使用 `torch.save()` / `torch.load()` 保存和读取Tensor、模型参数或训练checkpoint, 常用扩展名是 `.pt` / `.pth`。需要安装 `torch`。

可以直接传入文件路径, 不需要先open:
```python
import torch

data = {"features": torch.randn(3, 4), "labels": torch.tensor([0, 1, 0])}
torch.save(data, "data.pt")

data = torch.load("data.pt", map_location="cpu", weights_only=True)
```

保存模型时, 推荐保存 `state_dict()`。它包含模型参数和持久化buffer, 加载时需要先创建相同结构的模型:
```python
import torch

model = torch.nn.Linear(4, 2) # 示例模型, 实际使用时换成自己的模型
torch.save(model.state_dict(), "model.pth")

model = torch.nn.Linear(4, 2)
state_dict = torch.load("model.pth", map_location="cpu", weights_only=True)
model.load_state_dict(state_dict)
model.eval() # 推理时切换到评估模式
```

如果需要继续训练, 可以将模型、优化器状态和epoch一起保存。下面假设 `model`、`optimizer` 和 `epoch` 已在训练代码中定义:
```python
torch.save({
	"model": model.state_dict(),
	"optimizer": optimizer.state_dict(),
	"epoch": epoch,
}, "checkpoint.pth")

# 恢复前先创建相同结构的model, 并用其参数创建对应optimizer
checkpoint = torch.load("checkpoint.pth", map_location="cpu", weights_only=True)
model.load_state_dict(checkpoint["model"])
optimizer.load_state_dict(checkpoint["optimizer"])
start_epoch = checkpoint["epoch"] + 1 # 假设在一个epoch结束后保存
model.train()
```

这是基本的训练checkpoint; 如果使用学习率调度器或AMP的GradScaler, 还需要保存和恢复它们的状态。[模型保存与加载教程](https://docs.pytorch.org/tutorials/beginner/saving_loading_models.html)

`weights_only=True` 限制反序列化允许的类型, 并不是只读取字典中的模型权重; 上面的Tensor、基础类型和优化器状态仍可以一起加载。PyTorch 2.6起, 未传入 `pickle_module` 时默认使用该设置, 示例中显式写出以便理解。

如果已有文件通过 `torch.save(model, path)` 保存了整个模型对象, 加载通常需要 `weights_only=False`, 且原模型类必须可导入。只对可信来源的文件这样操作。[序列化说明](https://docs.pytorch.org/docs/stable/notes/serialization.html)

# NumPy

使用 `np.save()` / `np.load()` 保存和读取单个数组, 文件格式为 `.npy`, 可以保留数组的shape和dtype。需要安装 `numpy`。

可以直接传入文件路径:
```python
import numpy as np

data = np.arange(12, dtype=np.float32).reshape(3, 4)
np.save("data.npy", data)

data = np.load("data.npy")
```

如果需要将多个数组保存到同一个文件, 可以使用 `np.savez()`, 格式为 `.npz`。使用关键字参数为数组命名, 读取时按名称取出:
```python
import numpy as np

features = np.random.randn(3, 4)
labels = np.array([0, 1, 0])
np.savez("dataset.npz", features=features, labels=labels)

with np.load("dataset.npz") as data:
	features = data["features"]
	labels = data["labels"]
```

`.npz` 加载后返回类似字典的对象, 使用 `with` 可以在读取完毕后关闭文件。需要压缩保存时, 换成 `np.savez_compressed()`, 读取方式相同:
```python
np.savez_compressed("dataset_compressed.npz", features=features, labels=labels)

with np.load("dataset_compressed.npz") as data:
	features = data["features"]
	labels = data["labels"]
```

如果需要与其他工具交换文本数据, 可以使用 `np.savetxt()` / `np.loadtxt()` 读写TXT或CSV。下面以二维数值数组为例:
```python
import numpy as np

data = np.array([[1.2, 3.4], [5.6, 7.8]])
np.savetxt("data.csv", data, delimiter=",", fmt="%.6f")

data = np.loadtxt("data.csv", delimiter=",", dtype=np.float64, ndmin=2)

# 读取带一行表头的CSV, 并只取前两列
data = np.loadtxt("table.csv", delimiter=",", skiprows=1, usecols=(0, 1), ndmin=2)
```

`savetxt()` 支持一维或二维数组; `fmt="%.6f"` 保留小数点后6位, 整数可以使用 `fmt="%d"`。文本不会自动保留原数组的dtype, 需要完整保留数组信息时使用 `.npy`。[savetxt文档](https://numpy.org/doc/stable/reference/generated/numpy.savetxt.html)

如果数值表格含有缺失值, 可以使用 `np.genfromtxt()`, 下面将空字段和 `NA` 读取为 `np.nan`:
```python
data = np.genfromtxt(
	"table.csv",
	delimiter=",",
	skip_header=1,
	dtype=float,
	missing_values="NA", # 空字段默认也视为缺失值
	filling_values=np.nan,
	ndmin=2,
)
```

`np.load()` 默认 `allow_pickle=False`, 上面的数值数组无需修改此参数。含Python对象的object数组需要 `allow_pickle=True` 才能加载, 此时只应读取可信来源的文件。[load文档](https://numpy.org/doc/stable/reference/generated/numpy.load.html)

## Big File with MemMap

对于较大的数值文件, 可以使用内存映射, 像操作数组一样按需访问磁盘数据, 不必先将整个文件读入内存。`np.memmap` 用于映射磁盘文件; 如果数据已经是内存中的bytes, 则使用下面的 `BytesIO` 或 `np.frombuffer()`。

已有 `.npy` 文件可以通过 `np.load(..., mmap_mode="r")` 读取, shape和dtype从文件头自动获取:
```python
import numpy as np

data = np.load("data.npy", mmap_mode="r") # 只读映射
batch = data[:2] # 基本切片仍共享映射数据
batch_copy = data[:2].copy() # 复制到独立的内存数组

# 修改已有文件并写回磁盘
data = np.load("data.npy", mmap_mode="r+")
data[0] = 0
data.flush()
```

如果需要创建 `.npy` 文件并分块写入, 可以使用 `np.lib.format.open_memmap()`:
```python
import numpy as np

data = np.lib.format.open_memmap(
	"large.npy", mode="w+", dtype=np.float32, shape=(10000, 128)
)
for start in range(0, 10000, 1000):
	data[start:start + 1000] = np.zeros((1000, 128), dtype=np.float32)
data.flush()

data = np.load("large.npy", mmap_mode="r")
batch = data[:1000]
```

实际使用时, 将循环中的零数组换成每批计算或读取的数据即可。`open_memmap()` 会写入 `.npy` 文件头, 后续无需手动指定shape和dtype。[open_memmap文档](https://numpy.org/doc/stable/reference/generated/numpy.lib.format.open_memmap.html)

如果文件是没有 `.npy` 文件头的原始二进制数据, 可以直接使用 `np.memmap()`, 读写时需要约定相同的dtype、shape和数据排列顺序:
```python
import numpy as np

data = np.memmap("data.dat", mode="w+", dtype="<f4", shape=(3, 4))
data[:] = np.arange(12).reshape(3, 4)
data.flush()

data = np.memmap("data.dat", mode="r", dtype="<f4", shape=(3, 4))
batch = data[:2]

# 跳过第一行: offset单位是字节, 每行4个float32, 共16字节
remaining = np.memmap("data.dat", mode="r", dtype="<f4", offset=16, shape=(2, 4))
```

`"<f4"` 表示小端float32。原始二进制文件不会保存这些数组信息; `.npy` 文件应使用 `np.load(..., mmap_mode=...)` 或 `open_memmap()`, 由它们处理文件头。

常用映射模式:

| mode | 用途 |
| --- | --- |
| `"r"` | 只读已有文件 |
| `"r+"` | 读写已有文件, 修改会写回文件 |
| `"w+"` | 创建或覆盖文件, 创建时需要指定shape |
| `"c"` | 写时复制, 修改不写回文件 |

`flush()` 用于将修改写回文件, 不会关闭映射。NumPy的memmap没有公开的 `close()` 方法, 切片也可能继续持有映射; 使用完后需释放数组及其视图的引用。[memmap文档](https://numpy.org/doc/stable/reference/generated/numpy.memmap.html)

如果 `.npy` 数据已经在内存中, 可以使用 `io.BytesIO` 读写, 不需要创建磁盘文件:
```python
import io
import numpy as np

data = np.arange(12, dtype=np.float32).reshape(3, 4)
buffer = io.BytesIO()
np.save(buffer, data)
npy_bytes = buffer.getvalue() # 包含.npy文件头的完整字节数据

data = np.load(io.BytesIO(npy_bytes))

# 如果直接复用原buffer, 读取前先将位置移回开头
buffer.seek(0)
data = np.load(buffer)
```

如果内存中只有原始数组字节, 可以使用 `np.frombuffer()` 创建视图, dtype和shape需要自行指定:
```python
import numpy as np

data = np.arange(12, dtype="<f4").reshape(3, 4)
raw_bytes = data.tobytes() # 只有数组数据, 不包含shape或dtype

view = np.frombuffer(raw_bytes, dtype="<f4").reshape(3, 4)
data_copy = view.copy() # 独立、可修改的数组

# bytearray可修改, 数组视图与它共享内存
raw_buffer = bytearray(raw_bytes)
writable_view = np.frombuffer(raw_buffer, dtype="<f4").reshape(3, 4)
writable_view[0, 0] = 100 # 同时修改raw_buffer中的对应字节
```

`frombuffer()` 本身不复制底层数据; bytes不可修改, 因而由它创建的视图只读。需要独立数据时使用 `.copy()`。带 `.npy` 文件头的字节数据使用上面的 `np.load(BytesIO(...))` 读取。[frombuffer文档](https://numpy.org/doc/stable/reference/generated/numpy.frombuffer.html)

# Safetensors

使用 `.safetensors` 保存命名Tensor, 需要安装 `safetensors`, 并安装对应的 `torch` 或 `numpy`。文件保存张量数据及可选的字符串metadata, 模型结构和配置需要另外保存。

## Torch

适合保存Tensor字典或普通模型的state_dict。Tensor需要是稠密、连续的; 模型存在共享权重时使用下一节的模型接口。

```python
import torch
from safetensors.torch import save_file, load_file

model = torch.nn.Linear(4, 2)
weights = {name: tensor.contiguous() for name, tensor in model.state_dict().items()}
save_file(weights, "weights.safetensors", metadata={"description": "linear weights"})

model = torch.nn.Linear(4, 2)
weights = load_file("weights.safetensors", device="cpu")
model.load_state_dict(weights)
model.eval()
```

`load_file()` 返回整个文件的Tensor字典, `device` 指定这些Tensor的设备, 不会自动移动已有model。`metadata` 的键和值都必须是字符串。[Torch API](https://huggingface.co/docs/safetensors/api/torch)

直接传入模型, 适合包含共享权重的情况, 比如embedding和输出层共用权重。加载前仍需创建相同结构及共享关系的模型。

```python
import torch
from safetensors.torch import save_model, load_model

model = torch.nn.Linear(4, 2) # 实际使用时换成自己的模型
save_model(model, "model.safetensors")

model = torch.nn.Linear(4, 2)
load_model(model, "model.safetensors", strict=True)
model.eval()
```

`save_model()` 保存的是权重, 不包含模型类代码。它可能省略共享权重的重复名称, 因此应与 `load_model()` 配套使用。[共享Tensor说明](https://huggingface.co/docs/safetensors/torch_shared_tensors)

## Numpy

使用NumPy数组读写, 文件内容仍然是命名张量, 可以与支持对应dtype的其他框架交换。

```python
import numpy as np
from safetensors.numpy import save_file, load_file

weights = {
	"weight": np.arange(8, dtype=np.float32).reshape(2, 4),
	"bias": np.zeros(2, dtype=np.float32),
}
save_file(weights, "numpy_weights.safetensors")

weights = load_file("numpy_weights.safetensors")
weight = weights["weight"]
```

返回值为 `dict[str, np.ndarray]`。例如普通NumPy未原生支持的BF16权重, 可以改用Torch接口读取。[NumPy API](https://huggingface.co/docs/safetensors/api/numpy)

如果需要直接生成或读取内存中的完整文件内容, 可以使用 `save()` / `load()`。NumPy接口也提供同名方法。

```python
import torch
from safetensors.torch import save, load

weights = {"weight": torch.arange(8, dtype=torch.float32).reshape(2, 4)}
data_bytes: bytes = save(weights)
weights = load(data_bytes) # 返回CPU上的Tensor字典
```

这种方式需要持有完整bytes, 大文件优先使用下面的文件按需读取接口, 避免先调用 `f.read()` 读完整文件。[Torch API](https://huggingface.co/docs/safetensors/api/torch)

## safe_open

用于大权重的数据的读取, 防止一次性读取太多导致内存爆掉.

可以先查看名称和metadata, 然后只读取需要的Tensor:

```python
from safetensors import safe_open

with safe_open("weights.safetensors", framework="pt", device="cpu") as f:
	names = list(f.keys())
	metadata = f.metadata()
	weight = f.get_tensor("weight")
```

`framework="pt"` 返回Torch Tensor, `framework="numpy"` 返回NumPy数组。默认使用文件内存映射, 由访问触发数据读取; `get_tensor()` 获取的是一个完整张量。

如果需要遍历大文件, 可以每次处理一个Tensor, 不把全部结果收集到dict或list中:

```python
from safetensors import safe_open

with safe_open("weights.safetensors", framework="pt", device="cpu") as f:
	for name in f.keys():
		tensor = f.get_tensor(name)
		# 在这里处理tensor, 例如转换后写入另一个分片文件
		del tensor
```

这是本地文件的按需读取; 不需要先把整个文件转成bytes。`del` 只移除当前引用, 不保证立即清空操作系统的文件缓存。[safe_open用法](https://huggingface.co/docs/safetensors/index)

如果单个Tensor也很大, 使用 `get_slice()` 获取可切片对象, 再按第一维分批读取。下面假设文件中包含二维的 `embedding.weight`:

```python
from safetensors import safe_open

with safe_open("large_model.safetensors", framework="pt", device="cpu") as f:
	view = f.get_slice("embedding.weight")
	rows, hidden_dim = view.get_shape() # 查看形状, 无需加载完整张量
	block_rows = 1024

	for start in range(0, rows, block_rows):
		end = min(start + block_rows, rows)
		block = view[start:end, :]
		# 在这里处理block; 如果需要GPU计算, 再将当前块移动到GPU
		# block = block.to("cuda:0")
		del block
```

先调用 `get_tensor()` 再切片, 会先取得完整张量; 对大张量应直接切 `get_slice()` 的结果。`block_rows` 根据可用内存调整, 避免将所有block重新拼接成完整Tensor。移动到GPU或转换dtype时仍会产生相应的数据分配和复制。[切片读取接口](https://github.com/huggingface/safetensors/blob/main/bindings/python/py_src/safetensors/__init__.pyi)

如果需要将完整权重载入model, 可以逐个读取并复制到已有参数中。下面适用于普通稠密模型, 要求文件与model的state_dict名称及shape完全一致, 且model已分配实际存储, 不是meta模型。

```python
import torch
from safetensors import safe_open

# model已按正确结构创建, 并放在目标设备上
targets = model.state_dict() # 其中Tensor与model的参数、buffer共享存储

with safe_open("weights.safetensors", framework="pt", device="cpu") as f:
	if set(f.keys()) != set(targets):
		raise ValueError("文件权重名称与模型不一致")
	for name, target in targets.items():
		if tuple(f.get_slice(name).get_shape()) != tuple(target.shape):
			raise ValueError(f"shape不匹配: {name}")

	with torch.no_grad():
		for name, target in targets.items():
			tensor = f.get_tensor(name)
			target.copy_(tensor) # 复制到目标设备, 并按target的dtype转换
			del tensor

model.eval()
```

这种方式减少同时持有的源Tensor引用, 完整模型的参数存储仍需容纳全部权重。若模型本身超出单设备容量, 还需要分片加载、设备分配或offload; 逐张量读取不会自动实现这些功能。通过 `save_model()` 去除了重复键的共享权重文件, 使用前面的 `load_model()` 配套加载。
