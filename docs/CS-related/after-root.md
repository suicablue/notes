---
title: root 後手機管理
date created: 2022-01-11 19:07
status: note
tags:
  - root
  - android
  - phone
  - magisk
aliases: root 後手機管理
---

# root 後手機管理

## 常见问题

來源： [Android 玩家必备神器入门：从零开始安装 Magisk - 少数派](https://sspai.com/post/67932)

就目前来看，安装 Magisk 后出现的各类问题大多数情况下都不是因为 Magisk 本身，而是因为安装刷入后又陆续刷入的来源不明的 Magisk 模块或者授予 root 权限的不明应用。如果你同时还想刷入第三方内核的话，建议先刷 Magisk 再刷第三方内核，这样也能知晓问题出在哪一方身上。

因为未知原因导致安装失败也不要怕，在步骤「获取 boot.img」是我们保留了一份原来的镜像，按照最后一步的方法将原来的镜像重新刷回去就能正常开机。

在使用 Magisk 的过程中，可能会出现了 App 闪退或拒绝启动，可以尝试使用 Magisk Hide 来针对这些 app 隐藏 Magisk 的存在。而如果你因为安装了未知模块而翻车无法顺利进入系统，请先冷静下来：解决此类问题有一个万能的命令`adb wait-for-device shell magisk --remove-modules` ，此条指令将会在手机启动过程中生效。

关联阅读：[一日一技 | Magisk 模块「翻车」，没有 TWRP 如何救砖？](https://sspai.com/post/61220)

Magisk App 内容会在之后的文章中更详细的讲述。

## Magisk Hide 開起來還是被檢測到？

來源： [Root 之後躲避銀行偵測 | Magisk – SHXJ MIs BLOG](https://xiaomi.shxj.pw/android-phone/95/)

