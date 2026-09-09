---
type: command
tags:
  - cli
  - cooperate
done: true
topic:
 - "[[Git]]"
---
# Preliminaries

## (可选) 设置SSH

![[SSH Login#Generate Key]]

然后将ssh pub key添加到github

# Clone

将一个远程的代码仓库下载到本地, 包含版本信息(`.git`文件夹):
```bash
git clone <remote_url> <local_folder>
```

其中:
- `remote_url`是远程仓库的地址, 可以是`https://`的url, 也可以是`git@github.com:<username>/<repo>`这种的ssh路径
- 如果不指定`local_folder`, 那么默认创建一个`repo_name`的folder. 如果已经存在, 那么git clone会失败. 指定`local_folder`之后会将repo下载到`local_folder`中(创建新的文件夹)

可以指定下载的[[Git Branch|分支]]:
```bash
git clone <remote_url> -b <branch> --single-branch
```
使用`--single-branch`可以避免一次性将全部的分支都拉取.

可以递归拉取, 一次性clone全部的[[Git Submodule|子模块]]:
```bash
git clone <remote_url> -r
```

## Clone only folder

```bash
git clone --depth 1 --filter=blob:none --sparse <url/for/repo> --branch <branch_name>
cd <repo>
git sparse-checkout set <path/to/folder>
```

参数解释:
- `--depth 1`: 只把最新的一个commit记录下载到本地, 之前的记录不管. 后续如果需要其他的记录可以通过 [[#Fetch]] 来获取
- `--filter=blob:none`: 把没有checkout的文件过滤掉, 只把远程的commit记录拉下来. 然后通过`sparse-checkout`来下载.
- `--sparse`: 启用sparse checkout. 目的是为了优化磁盘占用. 如果不使用这个, 那么一开始可能会直接下载所有的文件(需要clone整个文件夹)
- `--branch <branch_name>`: 指定分支
- `sparse-checkout`: 稀疏checkout, 只把某一个路径下的文件检出, 而不是把整个branch检出

# Fetch
