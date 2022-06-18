---
title: 散漫 linux 学习笔记
date created: 2021-08-23
status: note
tags:
  - linux
  - psql
  - postgreSQL
aliases: 散漫 linux 学习笔记
date updated: 2022-01-11 12:44
---

# 散漫 LINUX 学习笔记

快捷通道：

- [[#1 查看当前目录磁盘使用情况]]
- [[#2 psql 基础操作]]
- [[#3 删除重复数据 得到列名]]

## 1.查看当前目录磁盘使用情况

### df & du
`df -h`
`du -sh *`

- `df` 和 `du` 的区别

Linux df（英文全拼：disk free） 命令用于显示目前在 Linux 系统上的文件系统磁盘使用情况统计。
Linux du （英文全拼：disk usage）命令用于显示目录或文件的大小。
du 会显示指定的目录或文件所占用的磁盘空间。

- /dev/vdal 是什么

在 dev 目录下可看到 vdal 文件，文件类型为本地磁盘。通过 `df -h` 可以看到后面“挂载点”一栏显示该磁盘挂载点为"/", `cd /` 移动到该目录即可详细查看占用情况。

- 补充阅读： [df 与 du 不一致情况分析](https://blog.csdn.net/carolzhang8406/article/details/7228248)

### [duf](https://github.com/muesli/duf/releases) (2022.02.01)

[Install Duf with .deb](https://www.linuxcapable.com/how-to-install-duf-disk-usage-utility-on-debian-11-bullseye/)

```
sudo wget https://github.com/muesli/duf/releases/download/v0.8.0/duf_0.8.0_linux_amd64.deb # 记得将版本号替换到最新版本
sudo dpkg -i duf_0.8.0_linux_amd64.deb # 到这里安装完成
```

使用方法： 直接 `duf` 即可。
效果：

![[duf.PNG]]

如果你只想查看本地连接设备的详细信息，而不想看其他的，可执行：
```
duf --only local
```

如果你只想根据大小按特定顺序对输出信息进行排序，可执行：
```
duf --sort size
```

其他：
```
duf /home /some/file 根据参数，则 duf 将仅列出特定的设备和安装点
duf --all 列出所有内容
duf --hide-network 隐藏网络文件系统，与之对应的 --hide-fuse --hide-special --hide-loops --hide-binds
duf --inodes 列出inodes
duf --output mountpoint,size,usage 指定输出的格式 对应的还有(mountpoint, size, used, avail, usage, inodes, inodes_used, inodes_avail, inodes_usage, type, filesystem)
duf --json 以json格式输出
duf --theme light 如果 duf 无法正确检测终端的颜色，可以设置一个主题
duf --help 查看所有 duf 的可用命令
```

### ncdu

[10 款你不知道的 Linux 环境下的替代工具！ - 掘金](https://juejin.cn/post/7035882332535914526)

## 2. psql 基础操作

```
su - postgres
psql 
```

- 查看所有数据库

`\l`

- 切换

`\c <database_name>;`

- 退出

`\q`

- 查看所有数据库大小

`select pg_database.datname, pg_database_size(pg_database.datname) AS size from pg_database;`

- 以 MB 等形式显示某一数据库大小

`select pg_size_pretty(pg_database_size('<database_name>'));`

- 查看数据库的所有表

`\d`

- 查看表的信息

`\d <table_name>;`

- 查看表的大小

`select pg_relation_size('<table_name>');`

- 以 MB 等形式查看表的大小

`select pg_size_pretty(pg_relation_size('<table_name>'));`

- 查看表的总大小，包括索引的大小

`select pg_size_pretty(pg_total_relation_size('<table_name>'));`

- 查看数据库中所有表大小

```
SELECT
table_name,
pg_size_pretty(table_size) AS table_size,
pg_size_pretty(indexes_size) AS indexes_size,
pg_size_pretty(total_size) AS total_size
FROM (
SELECT
table_name,
pg_table_size(table_name) AS table_size,
pg_indexes_size(table_name) AS indexes_size,
pg_total_relation_size(table_name) AS total_size
FROM (
SELECT ('"' || table_schema || '"."' || table_name || '"') AS table_name
FROM information_schema.tables
) AS all_tables
ORDER BY total_size DESC
) AS pretty_sizes;
```



- 索引与表空间请查看 [postgresql 查看数据库、表、表空间（位置大小）、索引的方法](https://blog.csdn.net/silenceray/article/details/55198537)

## 3. 删除重复数据&得到列名

`delete from <table_name> where ctid not in (select max(ctid) from <table_name> group by tableColumn);`

注意：
ctid 是 postgres 中的关键字，不可替换；
max 代表保存最新插入的数据，若要保留最初的数据，替换为 min；
tableColumn 是判断重复与否的列名。

- 得到所有表的列名：

`select column_name from information_schema.columns where table_schema='public' and table_name<>'pg_stat_statements';`

- 得到单独一表的列名：

`select column_name from information_schema.columns where table_schema='public' and table_name='<table_name>';`

## 4.权限设置

修改目录的所属组与所有者：
`chown root: root /home/XXX`
`chown xxx(數字) <文件 or 目錄名>`
`chgrp xxx(數字) <文件 or 目錄名>`
修改目录权限：
`chmod 755 /home/XXX`

-R : 对目前目录下的所有文件与子目录进行相同的权限变更(即以递归的方式逐个变更)

bin 文件的第一个属性用 d 表示。d 在 Linux 中代表该文件是一个目录文件,r 代表可读(read)(4)、 w 代表可写(write)(2)、 x 代表可执行(execute)(1)。排序：(owner/group/others)
（如 id_rsa 文件权限为 `-rw-------` 就是 600）

[chmod 指令](https://www.runoob.com/linux/linux-comm-chmod.html)

## 5.移动

`mv`

- -f：强制覆盖，如果目标文件已经存在，则不询问，直接强制覆盖；
- -i：交互移动，如果目标文件已经存在，则询问用户是否覆盖（默认选项）；
- -n：如果目标文件已经存在，则不会覆盖移动，而且不询问用户；
- -v：显示文件或目录的移动过程；
- -u：若目标文件已经存在，但两者相比，源文件更新，则会对目标文件进行升级；

## 6.手动释放缓存

一般情况下不需要手动释放，但若之前进行频繁操作停止后内存大量被占用以至于开始使用 SWAP，停止后也没有被释放二十一直存储在缓存区（Cache），可能就需要手动恢复了。

```
free -m  # 查看目前内存情况
sync  # 把内存同步到硬盘
echo 1 > /proc/sys/vm/drop_caches  # 释放页缓存
# echo 0 > /proc/sys/vm/drop_caches  # 释放完内存后改回去让系统重新自动分配内存
free -m  # 再次查看内存情况
```

> drop_caches 的值可以是 0-3 之间的数字，代表不同的含义：

0：不释放（系统默认值）
1：释放页缓存
2：释放 dentries 和 inodes
3：释放所有缓存

[linux 如何手动释放 linux 内存](https://www.cnblogs.com/yulia/p/9154600.html)

## curl
[Linux Curl Command 指令與基本操作入門教學](https://blog.techbridge.cc/2019/02/01/linux-curl-command-tutorial/)

打開終端機（terminal）然後，curl 後面加網址，就會在終端機內顯示回傳的 response，可能是 HTML、JSON 或是 XML 等格式，根據輸入的 URL 內容而定。

```
curl https://www.google.com
```

例如我想要下載：黃色小鴨的圖片，可以使用 `-o` 搭配欲下載的檔名和網址

```
curl -o duck.jpg https://im2.book.com.tw/image/getImage?i=https://www.books.com.twhttps://static.coderbridge.com/img/techbridge/images/N00/040/56/N000405619.jpg&v=522ff1cf&w=348&h=348
```

若是使用 `-O` 則可以直接使用下載網址的檔案檔名來命名下載的檔案（N000405619.jpg）：

```
curl -O https://im2.book.com.tw/image/getImage?i=https://www.books.com.twhttps://static.coderbridge.com/img/techbridge/images/N00/040/56/N000405619.jpg&v=522ff1cf&w=348&h=348
```

有可能在下載過程中被中斷，若是想要從中斷的地方繼續的話，可以使用 `-C` 選項：

```
 curl -C - -O http://releases.ubuntu.com/18.04/ubuntu-18.04-desktop-amd64.iso
```

若希望可以跟隨著網址 301/302 redirect 的話，可以使用 `-L` 選項：

```
curl -L http://google.com
```

可以比較沒有使用 -L 的回應：

```
$ curl http://google.com
<HTML><HEAD><meta http-equiv="content-type" content="text/html;charset=utf-8">
<TITLE>301 Moved</TITLE></HEAD><BODY>
<H1>301 Moved</H1>
The document has moved
<A HREF="http://www.google.com/">here</A>.
</BODY></HTML>
```
