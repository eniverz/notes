---
type: skills
tags:
  - code/python
  - DeepLearning
done: false
topic:
  - "[[Coding]]"
---
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

