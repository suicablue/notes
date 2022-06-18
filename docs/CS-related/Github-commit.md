---
title: Github 回滚代码——利用 commit 进行回溯
date created: 2021-08-22
status: note
tags:
  - Git
  - Github
aliases: 撤回远程 github 误操作, git push 失败解决方法, Github 回滚代码——利用 commit 进行回溯
date updated: 2022-01-11 12:44
---

- [实例手动备份与根据备份重建实例的方法](https://suicablog.cobaltkiss.blue/2021/09/use-github-pages-to-create-your-own-blog-and-deploy-automatically/) 与[[Obsidian-notes|如何利用obsidian高效建立笔记]]中都提到了git与github的相关操作，初学git相关指令并解除github，在很多概念搞不清时很容易出现操作失误。*

回退命令：

```
git reset --hard HEAD^  # 回退到上个版本
git reset --hard HEAD~3  # 回退到前3次提交之前，以此类推，回退到n次提交之前
git reset --hard commit_id  # 退到/进到指定commit
```

之后强制推到远程:
`git push origin HEAD --force`

拓展阅读：
[git 回溯 commit](https://www.jianshu.com/p/7ac00fdc136f)
[github 如何回滚代码 + github 如何回溯代码_少安的砖厂-CSDN 博客_github 如何回滚代码](https://blog.csdn.net/qq_28093585/article/details/101465845)
