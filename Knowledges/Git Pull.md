---
type: command
tags:
  - cli
  - cooperate
done: false
topic:
 - "[[Git]]"
---
# Pull

`git pull`相当于[[#Fetch|git fetch]]和[[Git Merge|git merge]]的结合: 首先使用`git fetch origin/master`更新索引, 然后使用`git merge origin/master`将远程的更新合并到本地仓库中

可以设置合并的方式: 使用[[Git Merge#Merge|merge]]或者使用[[Git Merge#Rebase|rebase]](`git pull --rebase`)进行合并.
