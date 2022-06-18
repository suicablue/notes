---
title: 自管理云盘服务前置调差与安装流程记录
date created: 2022-06-18 20:18
status: note
tags:
  - cloud
  - selfhost
aliases: 自管理云盘服务前置调差与安装流程记录
---

# 自管理云盘服务前置调差与安装流程记录

Heztner 審核嚴格，不清楚是否不能開代理註冊、真實信息填寫多少可通過，大概率只要註冊就需要拍護照、人臉驗證（Idenfy），似乎英文翻譯的駕照也可，身份證據說不行。我驗證時 Idenfy 一直顯示對應不上，後跳轉到人工審核，等了十多分鐘顯示超時，不能再次驗證，又等了幾分鐘收到郵件說審核過了……

- 管理员用户
 - 备份用户（Contabo 对象储存转移）
 - 网盘用户（可读写执行 Nextcloud 下文件）
 - Funkwhale 用户（可读 Nextcloud 下音乐文件并读写 upload 目录）

挂载设置：网盘、funkwhale 的 upload 目录直接通过 sshfp 挂载到 VPS，备份目录不应挂载，通过 rsync 传输即可。

Your authentication works but we do not support interactive logins. 
For more information on how to access your Storage Box please check our Docs: https://docs.hetzner.com/robot/storage-box/access/access-ssh-rsync-borg

sshfs 使用： https://cloud.tencent.com/developer/article/1834822
sftp 移动文件指令： rename 用法类似于 mv
多 ssh 密钥管理： https://www.cnblogs.com/okokabcd/p/9065534.html

Backup： https://docs.nextcloud.com/server/latest/admin_manual/maintenance/backup.html

Upgrade manually： https://docs.nextcloud.com/server/latest/admin_manual/maintenance/manual_upgrade.html

Docker Install： https://hub.docker.com/_/nextcloud

