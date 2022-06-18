---
title: 修复 Win10 默认照片查看器
date created: 2021-08-22
status: note
tags:
  - win10
aliases: 修复 Win10 默认照片查看器
date updated: 2022-01-11 12:44
---

快捷键 Win+R 打开「运行」，输入"regedit"，进入如下目录：

```
计算机\HKEY LOCAL MACHINE\SOFTWARE\Microsoft\Windows Photo Viewer\Capabilities\FileAssociations
```

再改目录下点击鼠标右键，新建字符串值。
如果希望照片查看器打开".jpg"格式图片，则输入".jpg"，数值数据填入"PhotoViewer.FileAssoc.Tiff"。
以此类推，另照片查看器支持多种格式。

參考鏈接：
[Fetching Title#c6lh](http://www.windows764.org/news/13945.html)
