---
title: 查找命令
date created: 2022-06-28 20:47
status: note
tags:
  - linux
aliases: 查找命令
---

# Linux 查找命令

[搜索 Linux 中的文件和文件夹的四种简单方法 - 知乎](https://zhuanlan.zhihu.com/p/52746102)

## find
1. 查找文件

-name name, -iname name : 文件名称符合 name 的文件。iname 会忽略大小写

```
sudo text find / -iname "xxx"
```
2. 查找目录

```
text find / -type d -iname "xxx"
```

3. 通配符查找

```
text find / -name "*.xxx"
```

4. 查找空目录

```
text find / -empty
```

## grep

遍历当前目录，寻找含有特定字符串的文件 (不要忘记最后那个 dot)：

```
grep -Ril "aside" . 
```