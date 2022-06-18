---
title: Git 学习
date created: 2021-08-22
status: note
tags:
  - Git
  - Github
aliases: Git 学习
date updated: 2022-01-11 12:44
---

# Git 学习

[Git 更新远程仓库代码到本地 - 前路有光，初心莫忘 - 博客园](https://www.cnblogs.com/sxy370921/p/11734612.html) <https://www.cnblogs.com/sxy370921/p/11734612.html>)

## [[Github-commit|Github 回滚代码——利用 commit 进行回溯]]

## 退出 git bush Vim 编辑器

[在 git bush 中如何退出 vim 编辑器 - 简书](https://www.jianshu.com/p/1bb48c844ac4)
一般远程和本地库有冲突时，pull 后会跳出 git 编辑器，最下方有两条提示栏，最下一个为空白时表示正处于命令模式，按下“o”、"i"、"a"都可以进入编辑模式。在命令模式下键盘 “Shift-Ctrl-;” 打入冒号 “:” ，输入指令，比如 "wq" 或 "ZZ" 保存并退出或 "q!" 不保存直接退出，enter 确认。

## 多 git 账号 windows 操作

如果兩個帳號一個公鑰就不難，用了兩對密鑰的話，要在系統 ssh 努力下建一個 config 文件區分 host ，用第二個帳號密鑰 remote 時要注意修改 host 名稱。

## Failed to connect to github.com port 443: Timed out

这个问题在国内常见，与 Github 服务器处在国外有关，建议设置代理，方法如下：
存有 git 全局设置的文件一般在 windows C 盘 - 用户 - <用户名> - .gitconfig ，可以直接在里面进行更改，也可以在 git 中输入以下指令：

```
git config --global http.proxy http://127.0.0.1:<port>
git config --global https.proxy http://127.0.0.1:<port>

```

注意 http 和 https 都要指定， port 处填写代理端口号，全局设置文件中其他的设置都可以删除。

## subtree 指令

### git subtree 實現效果（如圖）

![[gitsubtree.png]]

### 爲什麼用 subtree 而不用 submodule

`git submodule` 只能從其他倉庫下載、拉取子庫，之後 parents 本地庫會在根目錄出現一個 .gitmodule 文件，並在指定位置出現一個文件夾，遠程目錄除 .gitmodule 外新增文件夾點擊繪製借條轉到子庫， `git remote` 無法將遠程 parents 庫中的子庫文件夾下載到其他本地機器。在 parents 庫本地所做的命令無法影響到遠程子庫。

`git subtree` 操作後 parents 庫也會生成子庫文件夾，該文件夾無倫遠程還是本地都獨立於子庫，對這個文件夾的改動可以推送到遠程子庫，遠程子庫的改動也可以推送到 parents 庫或由 parents 庫拉取遠程子庫的改動。

### submodule 相關命令

#### 在 parents 倉庫添加子庫

```
git subtree add   --prefix=<prefix> <repository> <ref>
```

prefix 處爲子庫存放在 parents 庫的位置，若存放在根目錄則爲 `subrepository name` ，若存放在其他路徑則爲 `folder name/…/subrepository-name` 。

red 處填寫想要拉取倉庫的分支名，若加上 `--squash` ，則會將遠程子庫所有 commit 匯聚爲一條 commit 歷史信息，如果加上這個參數，記得之後每次在 parents 倉庫拉取都加上，否則容易出現版本混亂。

另外，--prefix=<prefix> 的意義如下，（因爲 subtree 指令都在源倉庫執行）意味着可以在源倉庫指定一個路徑和子倉庫進行關聯，在源倉庫 push 和 pull 都只影響到這一個路徑：

> Defines the path in the repository to the subtree you want to manipulate. It is mandatory for all commands.

舉例說明：

```
git subtree add --prefix=sub/libpng https://github.com/test/libpng.git master --squash
```

之後可執行 `git status` 與 `git log` 查看詳情。

#### 將 parents 本地倉庫改動推送到遠程 parents 庫正常 push 即可

#### 從子庫拉取更新

```
git subtree pull  --prefix=<prefix> <repository> <ref>
```

舉例說明：

```
git subtree pull --prefix=sub/libpng https://github.com/test/libpng.git master --squash
```

#### 推送修改到子庫

```
git subtree push  --prefix=<prefix> <repository> <ref>
```

這裏一般不加 `--squash` 。

_debug 事件：前幾次 push 一直失敗 ，報錯爲 `permission denied` ，搜不到解決辦法。後來想到可能因爲我在兩個帳號之間進行操作且只有一臺機器，在後面提到的 `git clone add` 操作時我依舊用了默認的 `git@github.com` 這個 host name ，而因爲涉及第二個帳號，我應該用第二個帳號的 host name （在系統 ssh 文件夾的 config 文件中設置），於是 我直接修改了項目 git 目錄下的 config 中第二個 remote 下的 host name ，推送成功。_

#### 簡化命令

通過 `git remote add -f <remotename> git@github.com:<username>/<subrepository name>.git` 把子庫地址作爲一個 remote 方便記憶。注意如果本地有兩個賬戶虛修改 hostname ，假設賬戶二的 hostname 是 <git@github2.com> ，那麼要將 <git@github.com> 替換。

簡化後常用命令如下：

```
git subtree add --prefix=sub/libpng libpng master --squash
git subtree pull --prefix=sub/libpng libpng master --squash
git subtree push --prefix=sub/libpng libpng master
```

#### 參考鏈接

1. [git subtree 教程 - SegmentFault 思否](https://segmentfault.com/a/1190000012002151)
2. [Git subtree: the alternative to Git submodule | Atlassian Git Tutorial](https://www.atlassian.com/git/tutorials/git-subtree)
3. [W3docs - Git Subtree](https://www.w3docs.com/learn-git/git-subtree.html)

## 撤銷未提交的本地修改

[git丢弃本地修改的所有文件（新增、删除、修改）](https://blog.csdn.net/leedaning/article/details/51304690)
