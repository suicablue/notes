---
title: SSH 相关指令与操作
date created: 2021-08-22
status: note
tags:
  - SSH
  - 密钥
aliases: SSH 相关指令与操作
date updated: 2022-01-11 12:44
---

## SSH 与服务器

[[instance-fixing-notes|Ssh 相关的东西还有太多要重新巩固]]

强烈推荐 Digital Ocean 的教程：
[SSH Essentials: Working with SSH Servers, Clients, and Keys](https://www.digitalocean.com/community/tutorials/ssh-essentials-working-with-ssh-servers-clients-and-keys)

### 1. 生成 SSH 并添加密钥到 Vultr

1. 下载并安装 PUTTYgen
2. 生成
3. 保留私钥到本地，公钥复制上方框框内的内容
4. 粘贴到 Vultr 公钥处，删除最后的 comment

[Vultr VPS 怎样设置 SSH KEYS 并用之无密码 SSH 登录](https://v.youhuima.cc/vultr-vps%e6%80%8e%e6%a0%b7%e8%ae%be%e7%bd%aessh-keys%e5%b9%b6%e7%94%a8%e4%b9%8b%e6%97%a0%e5%af%86%e7%a0%81ssh%e7%99%bb%e5%bd%95.html)

### 2.删除&重新生成 SSH key

- 删除

`rm -rf .ssh`

- 重新生成

```
ssh-keygen -t rsa -C "邮箱"
```

### 3.将 SSH 公钥复制到远程（Vultr）服务器上

注意，前往不要直接在网页控制面板上 restore SSH key，数据会全部丢失！

一直都觉得去年添加 SSH key 时稀里糊涂，电脑上也没有存备份，一直想要换成自己知道的 SSH key。上一步添加后，并给 Bitvise 添加密钥后发现，依旧无法在本机上通过 SSH 登录，又不能直接在 Vultr 上 restore，注意到 Vultr 提示可以根据它的指南手动更换，主要是这两个指南：
[How to Add and Delete SSH Keys](https://www.vultr.com/docs/how-to-add-and-delete-ssh-keys) - Add SSH Key to Vultr Instance
[Connect to a Server Using an SSH Key](https://www.vultr.com/docs/connect-to-a-server-using-an-ssh-key) - Connect via OpenSSH Client

之后终于可以做到免密登陆了。

`ssh-copy-id -i ~/.ssh/id_rsa.pub -p <SSH-port> root@xxx.xxx.xxx.xxx`

如果变更了 22 端口，需要添加 -p 并标注变更后的端口；
如果 SSH key 存在其他位置，-i 后的位置信息要变更；
ip 和用户名自己更换

这时候终端会显示出你曾经的 SSH key 的 fingerprint，不用管，直接继续操作，并按照指令输入密码

```
Number of key(s) added: 1

Now try logging into the machine, with:   "ssh 'root@xxx.xxx.xxx.xxx'"
and check to make sure that only the key(s) you wanted were added.
```

之后登录确认添加成功：
`ssh -p <SSH-port> example_user@xxx.xxx.xxx.xxx`

用语不精确的地方可以参照这里：
[SSH 简介及两种远程登录的方法](https://blog.csdn.net/li528405176/article/details/82810342)

（添加 SSH key 后.ssh 中出现两个文件，登陆后又会出现两个，不要乱动）

### 4.Bitvise 的 Client key manager 与 Host key manager

二者都在 login 界面

- Client key manager 添加密钥使 bitvise 远程通过 SSH 登录服务器，同时可以导出各种格式的公钥密钥，详情见：[<https://www.bitvise.com/ssh-server-guide-public-key](ssh> server guide public key)

(虽然写的是 public key 但你确实需要导入 private key)

- Host key manager （待补充）

### 5.修改 SSH 默认端口

`sudo nano /etc/ssh/sshd_config`
将 Port 右侧的 22 修改为其他数字，添加或修改的监听端口号最好为 10000~65535 区间之内，防止选择的端口号被系统或者其它软件所占用。
`sudo service ssh restart`

### 6.关闭密码访问

`sudo nano /etc/ssh/sshd_config`
将 PasswordAuthentication 一栏取消注释并替换为 no
`sudo service ssh restart`

### 7.为 SSH key 添加或修改 passphrase

`ssh-keygen -p -f ~/.ssh/id_rsa`
（疑问 为什么我设置 passphrase 后，登录时依旧不需要输入？因为私钥还在服务器上储存吗？）
（应该是因为 client 上的 private 没有更新）

### 8.authorized_keys

本机除了一对密钥外，还有 ~/.ssh/authorized_keys 文件，里面存储多对公钥，用于连接远程机器；若想在本机与另一台机器之间传输文件，另一台远程机的 authorized_keys 中需要另起一行放入本机公钥。

### 9.指纹查询

```
ssh-keygen -l
Enter file in which the key is (/root/.ssh/id_rsa):
```

## 本地 SSH

windows 10 的 SSH 存放位置（dell）：
`C: \Users\dell\.ssh`

1. [[Obsidian-notes|第一次在本机设置 SSH]]
2. [[Obsidian-notes#拓展阅读：Windows 多个 ssh-key 的设置与管理]]
