---
title: git 提示信息和处理
date: 20-5-12 17:37 +08
---

# 合并无关历史

提示

```sh
fatal: refusing to merge unrelated histories
```

确定是该分支，则允许合并

```sh
git pull --allow-unrelated-histories
```
