---
title: Apt 指令
date created: 2022-06-26 20:48
status: note
tags:
  - apt
  - linux
  - debian
aliases: Apt 指令
---

# Apt 指令

1. 如果我们想安装一个软件包，但如果软件包已经存在，则不要升级它，可以使用 –no-upgrade 选项:

	```
	sudo apt install <package_name> --no-upgrade
	```

2. 如果需要设置指定版本，语法格式如下：

	```
	sudo apt install <package_name>=<version_number>
	```
	
3. 不附加安装某些包（比方说，安装 php7.4-fpm 时总默认一并安装 php8.1 的很多包）

	```
	sudo apt install php7.4-fpm php8.1*-
	```

	这样系统安装时就会忽略 php8.1 相关包。
	
	[How to ignore or skip dependencies while installing packages on Ubuntu](https://www.how2shout.com/linux/use-apt-get-to-ignore-dependencies-on-ubuntu/#:~:text=Therefore%20to%20ignore%20the%20dependencies%20while%20installing%20some,end%20of%20the%20dependency%2C%20you%20want%20to%20exclude.)