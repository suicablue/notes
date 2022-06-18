---
title: adb 調試
date created: 2022-01-11 17:28
status: note/draft/post/diary
tags:
  - adb
  - windows
  - android
  - wireless debugging
aliases: adb 調試, magisk 的安裝與 root 權限獲得
date updated: 2022-01-11 05:32
---

# adb 調試

## adb 的windows 命令啓用

前段時間發現安卓國產備用機上的 AppOpsX 無法使用了（似乎一重新開機關機就會這樣），必須結合 pc 進行 adb 調試。但是鏈接手機到電腦後，終端輸入 adb 命令報錯顯示該命令不存在，但我記得我之前分明在電腦上操作過。

應該是沒有將 adb.exe 加入系統變量的緣故。

如果不想加入，終端操作前先 cd 到安裝了 adb.exe 的目錄，之後再執行 adb 相關命令即可。

更多請參考：
[Windows 操作系统下的 ADB 环境配置 - 少数派](https://sspai.com/post/40471)

## magisk 的安裝與 root 權限獲得

### TWRP支持
之前已經刷過了機換了系統，解开 Bootloader 鎖（必須步驟），但是一直每搞懂網絡上的 magisk 安裝教程。明明 github 上有 apk 文件下載，爲何我還要再在電腦上做系列操作刷入什麼東西？

我記得我之前曾從刷機包裏提取第三方 boot.img 文件，後來怎樣我忘記了。今天讀[每个 Android 玩家都不可错过的神器（一）：Magisk 初识与安装 - 少数派](https://sspai.com/post/53043)這篇文章時，通過文章內鏈接[twrp - Devices](https://twrp.me/Devices/)得知我的安卓設備有 TWRP 支持不需要很麻煩地提取第三方的文件：

- 從 TWRP 中下載對應設備最新版本的 img 後綴文件
- 执行 `adb reboot bootloader` 进入 Bootloader 界面
- 执行 `fastboot boot TWRP.img` 进入临时 TWRP
- 在 TWRP 中刷入你下载的 Magisk 安装包

最後一個步驟讓我困惑 —— 哪裏有 Magisk.zip 安裝呀，github 上的壓縮包是源碼，安卓不是 apk 格式直接下載嘛？

### magisk.zip 的下載

直到我看到[Magisk Manager Apk现在可以通过TWRP直接刷入手机+附root办法-右手网](https://www.uso.cn/post/view/56993)這篇文章中這一段才終於解惑：

> 在这之前，想要安装Magisk最常见的办法就是，先安装Manager app，然后再从Manager app中下载最新版本的可刷入ZIP文件——这个文件含有安装Magisk所必需的二进制文件和脚本，最后再用TWRP这样的刷机软件刷入这个ZIP，最终实现Magisk的安装，root手机并管理root权限。现在呢，用户**只需下载最新版本的Manager APK，并将它的扩展名改为ZIP**，这样一来，TWRP就可以识别它为可刷入文件，然后你就可以将其刷入手机。这个原理其实很简单，APK文件会遵循ZIP文件格式，所以只需要一些小改动，APK文件就可以同时拥有两个“身份”——安卓安装包文件，以及TWRP可识别的可刷入ZIP文件。

### 通过 TWRP 安装 Magisk

之後根據[通过 TWRP 安装 Magisk 操作指南 – MIUI历史版本](https://miuiver.com/install-magisk-via-twrp/)這篇教程進行上面提到的「在 TWRP 中刷入你下载的 Magisk 安装包」的後續步驟（此時手機上已經出現 TWRP 界面）：

- TWRP 主界面依次点击“高级” -> “ADB Sideload” -> “滑动按钮开始 Sideload”
- 接着在电脑端运行下面命令刷入 Magisk 安装包（注意改動版本號） `adb sideload Magisk-v23.0.zip`
- 待刷入命令运行完成后，用 `adb reboot` 命令重启手机

之後手機上多出 Magisk 應用程序，打開後裏面能看到 Magisk 版本號即成功。

### 永久導入 Magisk
一些教程提到雖然 root 成功了，但一重啓手機 root 狀態就會消失，一些[每个 Android 玩家都不可错过的神器（一）：Magisk 初识与安装 - 少数派](https://sspai.com/post/53043)還提到了要用 magisk manager 進行一些步驟……但是官方 github 頁面裏[MagiskManager](https://github.com/topjohnwu/MagiskManager)已經不作爲單獨項目存在了。於是我暫時認爲一切提到 magisk manager 的教程已經不適合當下，在[Android 玩家必备神器入门：从零开始安装 Magisk - 少数派](https://sspai.com/post/67932)這篇文章中找到了直接用 magisk 永久下載 magisk 的方法：

>如果想要谨慎一点，比如说修改的镜像文件是从网上下载的，想先试试看能否正常启动，则可以用命令：`fastboot boot` 。这样顺利启动系统后即可暂时拥有 Magisk ，不过重启后就会失效。确认没有问题后，再打开 Magisk App 中选择安装 > 直接安装，来「永久」写入 Magisk。
>
>最终，当我们**能在 App 中看到「当前」一栏的版本号且重启不会消失时**，恭喜你，安装成功。

這樣 root 的過程就結束啦，之後升級也直接在 app 內通過同樣的直接安裝方式即可升級到最新版本。撒花！

後續手機系統維護請看： [[after-root|root 後手機管理]]