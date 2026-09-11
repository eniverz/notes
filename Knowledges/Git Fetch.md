---
type: command
tags:
  - cli
  - cooperate
done: false
topic:
  - "[[Git]]"
---
# Fetch

> [!note] Preliminaries
> 本地储存四部分内容, [[Git Cache#Git Architecture|本地缓存]]的三部分(workspace, staging area, local repo)以及一个远程仓库分支的引用(使用[[Git Init#设置远程仓库|git remote设置的]])

使用`git fetch`能更新本地的远程分支的引用, 但是不更新本地的local repo. 即, 更新远程commit的索引, 但是这个索引对应的什么内容不更新.

适用于"希望先查看远程改了什么, 在决定如何处理合并"的情况.

通常是把远程的commit记录拉取到本地, 但是不会修改文件树里面的文件.

```bash
git fetch origin
```

如果原来的结果是:
```
GitHub 上的 main：      A──B──C
本地的 origin/main：   A──B
本地的 main：          A──B
```

fetch之后:
```
GitHub 上的 main：      A──B──C
本地的 origin/main：   A──B──C
本地的 main：          A──B
```

