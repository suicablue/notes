---
title: 彻底删除旧的 git 提交记录
date created: 2022-06-18 18:16
status: note
tags:
  - git
  - branch
aliases: 删除可能含有敏感信息的 git commit
---

# 彻底删除旧的 git 提交记录

## 含有隐患的操作
刚刚 Obsidian 自动提交 commit 意外将我一可能含有敏感信息的修改提交了，这可不好，就算可以用新的记录覆盖先前的，含有敏感信息的记录仍然留在 commits 中，可以被其他人看到。

实际上，之前我就曾不慎将本应用来自己记录的含有 api key 的笔记提交了，尽管已经被新的无敏感信息的记录覆盖，Mailgun 还是检测到了信息疏漏，将我的账号暂时关闭了，后将仓库删除重建并重设 api key 才恢复。

有没有什么办法可以不删除仓库，将除最新一次提交外之前的记录都彻底删掉呢？有的。

## 删除方法

参考：[本地操作git删除GitHub上的历史提交记录（亲测有效）_StomRin的博客-CSDN博客_git删除历史提交记录](https://blog.csdn.net/Mr_m_jia_bao/article/details/122753680)

```
# 新建一个分支 "latest_branch" 并转换到该分支
git checkout --orphan latest_branch

# 提交内容到这个分支
git add -A  

# 提交新的 commit 更改
git commit -am “commit message”（或 git commit -m “commit message”

# 删除旧分支 "main"
git branch -D main

# 将"latest_branch" 重命名为 "main"
git branch -m main

# 强制推送改动
git push -f origin master
```

之后可以看到 github 中 commit 只剩下了最新一个。

## 其他方法参考

[【GIT技巧】清除历史记录中的敏感信息 - 知乎](https://zhuanlan.zhihu.com/p/25990989)

[Git移除敏感数据_阳君的博客-CSDN博客](https://blog.csdn.net/y550918116j/article/details/49913631)

[快速删除Git中的敏感数据_机智的程序员小熊的博客-CSDN博客](https://coding3min.blog.csdn.net/article/details/104708390?spm=1001.2101.3001.6650.2)

[don't use the obsolete BFG or git filter-branch](https://stackoverflow.com/a/61034210)

## 后续操作

