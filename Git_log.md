---
title: Git 工作量统计
date: 2022-3-30
---
- [Git 工作量统计](#git-工作量统计)
# Git 工作量统计

1. 切换至指定库
2. 输入以下 `git` 命令
```bash
# 增删行数及总行数
git log --author="" --pretty=tformat: --numstat | awk '{ add += $1;subs += $2; loc += $1 - $2 } END { printf "added lines: %s, removed lines: %s, total lines: %s\ncommits: ", add, subs, loc }'
# 提交次数
git log --author="" --no-merges | grep -e 'commit [a-zA-Z0-9]*' | wc -l
```