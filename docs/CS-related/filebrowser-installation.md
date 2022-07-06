---
title: filebrowser 安装与设置
date created: 2021-08-25
status: note
tags:
  - 私有云盘
aliases: filebrowser 安装与设置
date updated: 2022-01-11 12:43
---

## 安装与初步配置

真的瞎折腾了好久，其实原本很简单的……
点开 [官网安装教程](https://filebrowser.org/installation/) ，点击Unix，看到下面的命令：

```
curl -fsSL https://raw.githubusercontent.com/filebrowser/get/master/get.sh | bash filebrowser -r /path/to/your/files
```

第一行很简单，就是根据脚本内容进行安装，安装好后，脚本指示位置会多一个叫做"filebrowser"的文件，这个脚本"filebrowser"存储在 `/usr/local/bin` 目录下，安装成功的提示下也能看到。

之后，你要决定你先把云盘的东西放在哪里，这个位置决定第二行命令的 `/path/to/your/files`。
我因为中间乱操作了一番，有可能你直接跑第二行默认会用你的 IP……你可以先试着跑跑看……
假设你想存到根目录下的 filebrowser 文件夹中……

```
sudo filebrowser -r /filebrowser
```

之后会跳出两行，第一行说没有读取道 config，这是正常的，因为你设置；
第二行跳出一个`<IP>:<port>`的东西，复制下来，粘贴到浏览器里，正常情况下，就能打开云盘了，就这么简单。

正常情况下，你的`<IP>`就是你 VPS 的 IP 地址，如果不是就不可能打开，需要设置 IP 地址:

```
filebrowser config set --address <IP_address>
```

把<IP_address>换成 0:0:0:0；如果需要换端口的话：

```
filebrowser config set --port <port_number>    
```

你想把 [其他设置](https://filebrowser.org/cli/filebrowser-config-set) 现在做了也行，但也可以之后再调整。
现在，再次尝试打开网盘：

```
sudo filebrowser -r /filebrowser
```

记得换掉我的网盘文件存储目录，用你自己的目录；非 root 用户要一直用 sudo，否则无权上传下载文件。

现在你应该可以通过`<IP>:<port>`进入网盘了。

进去之后，用户名和密码都输入 admin，进入界面，记得在设置里把用户名和密码都换掉！
语言也可以换，可以试着上传个东西玩。

不需要使用了记得 Ctrl+C 退出。

明天再做设置。

## filebrowser 相关文件的位置

- filebrowser 文件本身存放在 /usr/local/bin 下面

- filebrowser.db 如果没有用这条指令进行指示：
  `sudo filebrowser -d /path/to/database/filebrowser.db config init`

那么，该文件会存到你目前的位置……比方说你是用用户 test 进行操作的，test 用户文件夹在/home/test 下面，那 database 就在这里；
如果你没有自定义它的存放位置，每次你执行命令时，直接输入 `sudo filebrowser filebrowser.db xxxxxx` 就可以了，如果自定义了位置，每次执行 filebrowser 相关命令都要列出详细位置。

- 文件存放位置，如果第一次登录时用了官方的指令：

`sudo filebrowser -r /path/to/your/files`
那这个位置就会成为第一个 admin 用户与在之后的用户默认进入的目录；

后来我发现，这么操作后 `filebrowser -d filebrowser.db` 以及 `filebrowser` 指令登录都会报错，因为**这些指令没有指定进入的目录**，也没办法指定；也可能是位置没能存放进数据库中；也有可能在界面上修改一下路径就好了……

总之，之后我重建了一下，没有用 -r 指令，用了大多数个人教程都在用的 -d 指令，没有出现这个问题了，虽然还是遇到了点小困扰；

- 登录后指定的可操作路径，可以在界面上手动设置；似乎你什么都不设置的话，同样会进入终端操作时时的目录，比方说，/home/test；

后来我试着修改为 ./filebrowser
原本是希望能存到根目录下新建的 filebrowser 目录下，但是它在/home/test 下新建了个 filebrowser 文件夹……

不知道修改为/filebrowser 是不是就能达到目的了……因为之后我又试着改为/var/lib/pleroma/static ，就刚好可以进入这个目录，没有问题。

## 端口与域名

如果不想输入 IP 地址，只要这个 IP 已经和域名绑定了，输入 域名:端口也可以进入 filebrowser 界面；
如果想要省略端口，可以将端口改为 80 或者 443（看你有无申请 SSL 证书），但是绑到 pleroma 使用的主域名下时出现了问题：因为 80 端口已被占用，输入地址后会默认跳转到 pleroma 的界面；

似乎需要进行 nginx 反代，有人说弄什么 proxy，有人说直接在 pleroma.conf 上改；后者我试了，nginx 出问题了，可能是我参数有误，但我也不想再试一次，我感觉这样不太好，更希望可以像 soapbox 那样单独有一个 nginx 文件，不影响 pleroma 本身。

## 自动开机

今天也就这个弄成功了（
新建一个 service 文件：
`sudo nano /etc/systemd/system/filebrowser.service`
一般建到 etc 或者 bin 下面就行？
编辑这个文档：

```
[Unit]
Description=filebrowser daemon
After=network.target

[Service]
Type=simple
ExecStart=sudo filebrowser -d /path/to/your/filebrowser.db
Restart=always
RestartSec=10

[Install]
WantedBy=multi-user.target
```

之后就是熟悉的 systemctl 操作了：

```
sudo systemctl start filebrowser  # 启动
sudo systemctl enable filebrowser  # 设置开机自启
sudo systemctl list-unit-files| grep filebrowser  # 看看有无设置成功
sudo systemctl daemon-reload  # 每次调整service文件后都要执行这个指令
```

## 疑难杂症
### 2.0 后的 config 到底用来做什么

> The configuration file `.filebrowser.json` should only be used in automatic deployments. We recommend you to use only the database file. The configuration file is more like the flags: used to override some database setting (see [https://docs.filebrowser.xyz/cli/filebrowser](https://docs.filebrowser.xyz/cli/filebrowser)).
> 
> `filebrowser config init` should do the trick.
> 
> Then you can `filebrowser config set ...` anything else you want.

[关于 2.x 版本配置文件的疑问 · Issue #652 · filebrowser/filebrowser · GitHub](https://github.com/filebrowser/filebrowser/issues/652)

### 頻繁 timeout

>It it happening because filebrowser service locks the database file. The workaround is to copy the db file and then run cli using the `-d` flag.

[filebrowser config export or filebrowser config cat always timeout · Issue #1647 · filebrowser/filebrowser · GitHub](https://github.com/filebrowser/filebrowser/issues/1647)