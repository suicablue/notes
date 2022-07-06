---
title: Linux 用户与组
date created: 2022-06-18 21:09
status: note
tags:
  - null
aliases: Linux 用户与组
---
# Linux 用户与组

[Linux Users and Groups | Linode](https://www.linode.com/docs/guides/linux-users-and-groups/)
## 创建用户
```
sudo useradd -r -s /usr/sbin/nologin -d /path/to/userfolder -m <username>
```

参数：

-r 　建立系统帐号
-s`<shell>`　 　指定用户登入后所使用的shell
-d`<登入目录>` 　指定用户登入时的起始目录
-m 　自动建立用户的登入目录

详解：[Linux useradd命令 | 菜鸟教程](https://www.runoob.com/linux/linux-comm-useradd.html)

## 组
每个用户在被创建时就已经属于一个组（primary group），可以将它们添加到新的组别（secondary groups），至多 15 个。

```
sudo usermod -a -G second_example_group example_user
```

## 查看用户属组

`groups`

直接输入这个命令，可查看当前用户属组；在 `groups` 后输入用户名（用空格分开），可查看指定用户属组。

