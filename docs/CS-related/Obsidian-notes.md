---
title: 如何利用 obsidian 高效建立笔记
date created: 2021-08-20
status: draft
tags:
  - obsidian
  - 笔记软件
  - 工具
aliases: obsidian 教学, 如何利用 obsidian 高效建立笔记
date updated: 2022-01-11 12:44
---

## 笔记标题

标题应尽可能详尽地阐释笔记内容（概念导向），比如：

> - 番茄鐘 (x)
> - 使用番茄鐘可以有效提升效率 (o)
>
> [朱琪 - 什麼是 Evergreen Note (長青筆記)？](https://medium.com/pm%E7%9A%84%E7%94%9F%E7%94%A2%E5%8A%9B%E5%B7%A5%E5%85%B7%E7%AE%B1/%E4%BB%80%E9%BA%BC%E6%98%AF-evergreen-note-%E9%95%B7%E9%9D%92%E7%AD%86%E8%A8%98-5f0b2c7b6547)

为了实践，我将标题从没有太多信息的“obsidian”改为了“如何利用 obsidian 建立有记效的连结笔记”，同时决定本篇我们只讲笔记，不回顾基础操作，也不看应用部分。

## metadata

obsidian 的 metadata 与 hugo、hexo 等静态博客一样都是 YAML 格式，metadata 的名称没有限制，名称后输入冒号，冒号后一定有一个空格，空格后输入内容，格式详见 [YAML front matter](obsidian://open?vault=Obsidian%20Help&file=Advanced%20topics%2FYAML%20front%20matter) 。
注意，链接无法使用 markdown 格式，标签不需要标注#。
aliases 是别名。
可以利用 obsidian 的模板官方插件快速建立[[metadata]]。

### 可以尝试使用的 metadata（status）

可以试着将笔记分为三种状态（status）：

- 发芽期
- 培育期
- 长青期

Evergreen Note 认为笔记存在发展时期，需要不断复习巩固，最后才能变为自己的知识。

## 模板

模板是 obsidian 的核心插件，开启后，需要在设置中指定一个文件夹放置模板文件。如果文件夹中仅有一个文件，那么插入模板时将直接插入该模板（即该文件下的所有内容）；如果文件夹下存在多个文件，插入时将询问插入哪一个模板。
模板配合快捷键使用效果最佳，请在设置中为插入模板设置快捷键。

在设置中可指定日期、时间格式。
以日期为例，设置中填入 YYYY-MM-DD_ddd，并在模板文件夹下的文件中放置板块{{date: YYYY-MM-DD_ddd}}，之后在笔记中插入模板时，模板文件中的{{date: YYYY-MM-DD_ddd}}将自动被替换为当天的日期。如[[daily notes]]。

## dataview 插件

dataview 是进阶插件，初步探索 obsidian 的阶段没有必要花太多时间弄明白。
简单来说，dataview 可以根据笔记上的某些要素（如标题、标签、作者）快速生成目录，最常见的有：

- 生成同一作者的笔记目录
- 生成标题中含有相同关键词的笔记目录
- 生成某一标签下的笔记目录

只有在笔记量足够大的前提下，dataview 才能形成具有参考价值的目录。
上述三种使用场景的应用方法请参考 [Obsidian 插件之 Dataview](https://sspai.com/post/68183) 。

注意 dataview 首字母小写。

为了未来有效使用这款强大的插件，现阶段记笔记时可以有意识地使用 tag、将标题写得尽可能详尽并标注 metadata。

## 资料库的数量

一般来说，我们只需要建立一个资料库即可满足笔记记录需求，建立第二个资料库往往意味着第一个资料库中的笔记过于繁杂混乱，我建议整理资料库而不是新建，因为新建的资料库中的笔记无法与之前的资料库笔记互联，如果你想将资料库 A 中的笔记关联到资料库 B，[[]]将失去作用，你只能使用复制 obsidian 网址并使用 markdown 的方法进行连结，这种方法是低效的。（不清楚有没有第三方插件可以解决这个问题）

之前的资料库中所有的设置也需要再次进行设置，尤其是 git 插件。

## 同步到移动端（Sync）的注意事项

这里只阐述付费功能 Sync 的一个应用场景。

如果你一定要建立一个新的资料库，并且使用了 sync 付费功能，将它连接到了远程资料库并同步到其他设备，你需要连接到一个新的远程资料库（一共可以设置五个远程资料库）。
如果你连接到之前的远程资料库，之前的资料库上的笔记将全部导入进新资料库中，造成混乱。
之后，就像第一次将手机和电脑同步起来一样，需要在手机上再次建立同名本地资料库，并将资料库连接至远程资料库。

远程资料库一次只能启动一个，这意味着你只能同时同步一个资料库，除非上一个资料库已经不需要使用，否则不建议这样做。

**请不要随意删除远程资料库**，如果你不再需要某个远程资料库，请确保当前与之连接的本地资料库中没有重要笔记，且每个设备的同步状态都关闭。一旦删除远程资料库，且删除时同步状态未关闭，**与之连接的本地资料库中的笔记也将被全部删除**，这样删除后的笔记**无法找回**。

不再使用 Sync 功能的话，断线即可，删除前请确保笔记安全。

## 利用 Git 插件定时自动备份笔记

在差点因为误删远程资料库丢掉全部笔记之后，我决定将笔记在 github 上进行备份（当然，也可以选择 gitlab、gitee 等，gitee 速度可能在内陆更快）。

### 部署准备

下面以 github 为例，先保证你有一个 github 账号并在电脑上下载 git，不必下载 github desktop，代码照抄稍微修改就好。

### 初步设置

如果你原来搭建过 hexo、hugo 等静态博客，那么这个过程会很熟悉。
如果没有，首先你需要生成 SSH 密钥对。
打开 git bash。
不确定是否创建过的话：
`cd ~/.ssh`
若显示 `No such file or directory`，则表示没有创建过，反之省略这步。

第一次使用 git 需要先配置用户名和邮箱：

```
git config --global user.name <username>
git config --global user.email <useremail">
```

（我的用户名和邮箱与 github 一致，但似乎此处的信息仅用于 git 提交代码时显示身份和联系方式）
（reference: [初次使用 git 配置以及 git 如何使用 ssh 密钥（将 ssh 密钥添加到 github）](https://www.cnblogs.com/superGG1990/p/6844952.html) ）

之后，输入 `ssh-keygen -t rsa -C <useremail>`（更换邮箱）生成密钥对，不要设置 passphrase，否则 obsidian 之后无法自动部署。
生成后，复制公钥：
`cat ~/.ssh/id_rsa.pub`
将这串以 ssh-rsa 开头邮箱结尾的结果复制下来。（如果你之前通过 git 或其他途径生成过密钥对，但没有将公钥添加到 github 中，你同样需要将你的公钥复制下来，记住是以 ssh-rsa 开头、文本较短的那一个）

到 github 点击账户下标，找到 Settings，点击进入。在左侧栏找到 [SSH and GPG keys](https://github.com/settings/keys) ，点击绿色按钮"New SSH key"，在新页面输入刚刚复制的公钥，标题随意。

回到 git 界面，验证 SSH key 是否正常工作：
`ssh -T git@github.com`
若显示：
Hi XXX! You've successfully authenticated, but GitHub does not provide shell access.
则代表 SSH key 工作正常，可以开始把 obsidian 上的笔记往 github 上丢啦~

#### 拓展阅读：Windows 多个 ssh-key 的设置与管理

[Windows 下 Git 多账号配置，同一电脑多个 ssh-key 的管理](https://www.cnblogs.com/popfisher/p/5731232.html)

### 新建 repository 并上传笔记

进入 github，点头像旁的加号，"New repository"。
如果你不想别人看你的笔记，就设置为私人，下面选项不需要选。
之后页面跳转到你新建的 repository，注意到页面上有这么一堆代码：

```
echo "# test" >> README.md
git init
git add README.md
git commit -m "first commit"
git branch -M master
git remote add origin git@github.com:xxx/xxx.git
git push -u origin master
```

在 git 上进入你 obsidian 笔记存放的目录，比方说你存在 D: \obsidian\notes 目录下：
`cd D: \obsidian\notes`
之后，一行行在 git 界面上执行，如果你前面的步骤都没有问题，这里应该不会报错。
没什么问题的话，push 后刷新 github 页面，你的笔记就全都出现在上面了！

#### github 注意事项

对 git 操作不熟悉的话，请尽量不要在 github 页面上添加、删除、修改文件或进行任何变动，远程和本地存在差异的话，push 可能会无法进行。

##### 拓展学习：[[Github-commit]]

### obsidian 插件设置

在第三方插件中下载并启用 Git，上面操作无误的话，这里便会开始自动部署；如果上面设置有误右上角是会报错的，同时页面下方可以看到 git 的状态。
在设置中可以对自动备份时间间隔进行调整，0 代表不备份。

暂不清楚 git 是否可以代替云同步与 sync，理论上，电脑端 push 到远程，手机端从远程 pull 下来笔记（需要手机安装终端模拟器并验证 SSH），手机 push 到远程，电脑 pull 接收，应该是没什么问题的，但我想这需要将自动备份关闭，改为手动备份会比较妥当，否则两边可能出现冲突导致操作失败。
已经在使用 sync 的话，手机应该没有必要开 git 并下载其他辅助程序，在手机对笔记进行修改时，sync 将修改同步到电脑，电脑会自动通过 git 备份到 github。

## 想要发布？转换 wikilinks 为一般静态博客支持的链接
我看过很多使用 Obsidian 在本地写作的人将 wikilinks 关闭，仅使用 markdown 通用语法来达到方便迁移与发布的目的。但随着支持 wikilinks 的应用越来越多、应用间语法转换越来越便捷，似乎没有必要这么麻烦地使用原始语法而抛弃便捷友好的 wikilinks ，抛去初次写作时编辑链接的麻烦先不讲，整个文档的结构很可能在未来发生多次变化，忘记或记不清要更改哪些内部链接会造成很多麻烦，白白损耗效率。

我找到了一些通过不同方式将 wikilinks 转换为静态博客 hugo 可识别的普通 markdown 语法的工具，全部来源于 github ：
- [quartz - issues/16](https://github.com/jackyzha0/quartz/issues/16)  
- [obsidian-export](https://github.com/zoni/obsidian-export)  
- [hugo-wikilinks](https://github.com/milafrerichs/hugo-wikilinks)  
- [zettel-hugo-postmaker](https://github.com/balaji-dutt/zettel-hugo-postmaker)

參考鏈接：
[【Obsidian 使用教學】基礎篇 05 — 如何調整 Obsidian App 設定檔？ 讓 Obsidian App 擁有自己的外觀主題與插件 | by 朱騏 | PM 的生產力工具箱 | Medium](https://medium.com/pm%E7%9A%84%E7%94%9F%E7%94%A2%E5%8A%9B%E5%B7%A5%E5%85%B7%E7%AE%B1/obsidian-%E4%BD%BF%E7%94%A8%E6%95%99%E5%AD%B8-%E5%9F%BA%E7%A4%8E%E7%AF%87-05-%E5%A6%82%E4%BD%95%E8%AA%BF%E6%95%B4-obsidian-app-%E8%A8%AD%E5%AE%9A%E6%AA%94-6ed8d168ae0e)

