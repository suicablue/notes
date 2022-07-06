---
title: Cloudreve&Aria2 离线下载
date created: 2022-06-28 23:05
status: note
tags:
  - cloudreve
  - aria2
  - aria2c
aliases: Cloudreve&Aria2 离线下载
---

# Cloudreve&Aria2 离线下载注意事项

参考链接：

Systemd 部分: [Aria2下载软件的Linux安装、配置文件编辑、开机启动、浏览器插件连接_lggirls的博客-CSDN博客_aria2 开机启动](https://blog.csdn.net/lggirls/article/details/120685750)

安装与配置部分: [给Cloudreve开启离线下载Aria2功能（Linux版） - 陌罗博客](https://www.mlvlog.com/678.html)

Web 部分 (未测试): [linux 系统安装aria2以及配置web端_快乐如斯的博客-CSDN博客](https://blog.csdn.net/u014418962/article/details/82957465) | [GitHub - straightedge4life/webui-aria2: The aim for this project is to create the worlds best and hottest interface to interact with aria2. Very simple to use, just download and open index.html in any web browser.](https://github.com/straightedge4life/webui-aria2)

注意：

Cloudreve 不仅需要有临时下载目录的读写执行权，还要有 aria2c 本身的执行权。

/usr/bin/aria2c 默认对全部用户开放执行权，所以 service 文件中标记 User 为 filebrowser/Cloudreve 即可。
