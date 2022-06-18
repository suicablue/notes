---
title: MKDocs 的安装与部署（搭配 Obsidian ）
date created: 2021-12-22 17:33
status: draft
tags:
  - 笔记
  - 博客
  - MKDocs
  - Material
  - git
  - github
aliases: MKDocs 的安装与部署（搭配 Obsidian ）
date updated: 2022-01-11 05:34
---

# MKDocs 的安装与部署（搭配 Obsidian ）

阅读本文前您需要了解基础的 Obsidian 和 git 的基础操作。

如果您对个人 wiki 页面制作、笔记发布感兴趣，又或者您认为传统的静态博客写作不适合您，不妨试试操作简便功能丰富的 MKdocs 。参考本文操作步骤，您可以实现 Obsidian 本地编辑笔记并通过 MKdocs 和 GitHub 发布到网页的效果，效果参考： [suica's notes](https://suicablog.cobaltkiss.blue/notes/)

<!--more-->

# 一. 部署与发布的准备
1.  安装 git 并配置
若要将本地的文档传输到远程，并将远程的改动同步到本地，我们需要在电脑上安装 [git](https://git-scm.com/downloads) ，选择您使用的系統版本下载，安装过程中在 "Adjusting your PATH environment" 处建议选择 "GIt from the command line and also from 3rd-party software" ，对安装和 git 基础操作不熟悉可以参考 [[Git][教學] 01 - Git 介紹與在Windows系統下安裝](https://progressbar.tw/posts/1) 这篇教程进行学习。

确保此时已有一个 github 账号，没有的话就快速注册一个，您也可以使用 gitlab 或国内速度更快的 gitee ，本文以 gitHub 为例。在您想要储存文档的目录下右键打开 "git bash" ，输入以下指令：
```
git init
git config user.name "您的 github 用户名"
git config user.email "您的 github 邮箱"
# 此处使用 github 的用户名和邮箱以确保每次 commit 都是使用您的 github 身份进行
```

您也可以进行全局配置，如果您只使用一个 github 账号，之后每次在本地新建仓库时就不需要再用 `git comfig` 进行配置了：
```
git config --global user.name "您的 github 用户名"
git config --global user.email "您的 github 邮箱"
```

在 Window 10 中，git 的全局配置储存在 "C:\Users\用户名\.gitconfig" 中，git 的单独仓库配置储存在每个仓库目录的隐藏文件夹 ".git" 中的 “config” 文件中，全局配置优先于局部配置。

生成一对 ssh 密钥：
```
ssh-keygen -t rsa -C your@example.com -b 4096
```

邮箱处填写您的任意一个邮箱即可。如果不需要 passphrase 的话一路回车即可，生成的密钥对保存在 "C:\Users\用户名\.ssh" 中，"id_rsa.pub" 为公钥，之后我们要将它上传至 github ，"id_rsa" 为密钥，请不要上传泄露到任何地方。

由于国内访问 github 网络不稳定，建议给 git 设置代理，如果电脑上开着代理软件（如 clash 、小火箭等），在里面找到 HTTP/SOCKS5 代理端口，之后在文本编辑器中修改 ssh 配置（位置： "C:\Users\dell\.ssh\config" ，没有 config 文件则新建）：
```
Host github.com
  ProxyCommand connect -H 127.0.0.1:端口号 %h %p
# 将端口号替换为刚刚找到的端口号
# -H 为HTTP代理端口 -S 为SOCKS5代理端口
```

2. fork # **[foam-mkdocs-template](https://github.com/Jackiexiao/foam-mkdocs-template)** 仓库并进行修改
