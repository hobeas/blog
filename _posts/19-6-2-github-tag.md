---
title: github tag or release
date:  19-6-2 14:14 +08
tags:  git
---

```bash
# 查看提交历史
git log --pretty=oneline --abbrev-commit

# 查看标签
git tag

# 打标签
git tag <name> <commit id>

# 查看标签信息
git show <tagname>

# 推送标签到远程
git push origin <tagname>

# 推送全部标签到远程
git push origin --tags

# 删除本地标签
git tag -d <tagname>

# 删除远程标签
git push origin :refs/tags/<tagname>
```
