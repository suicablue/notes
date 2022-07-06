---
title: Filebrowser 安装与配置（2022）
date created: 2022-06-29 17:22
status: note
tags:
  - filebrowser
aliases: Filebrowser 安装与配置（2022）
---

# Filebrowser 安装与配置（2022）

仅将 Filebrowser 当作网盘使用，为预防可能的权限问题，新建一个新用户 filebrowser 并让它做为之后下载得到的 filebrowser 执行程序、filebrowser.db 与指定根目录的所有者。

## 初步安装与设置

1. 新建 filebrowser 用户

```
sudo adduser -r -s /usr/sbin/nologin -d /srv/mount/filebrowser filebrowser
```
将 /srv/mount/filebrowser 这个路径作为 sshfs 挂载路径，挂载一个 Storage Box 的 subuser 子目录，过程这里省略，记得在参数中设置所有者为 filebrowser，组为 fuse。

之后，这个路径将作为 filebrowser 管理员的初始路径，即之后要设置的 root 路径。

2. 下载程序

```
curl -fsSL https://raw.githubusercontent.com/filebrowser/get/master/get.sh | bash
```
之后 /usr/local/bin 下会出现一个可执行程序 filebrowser，将它移动到其他位置，比方说 /srv/mount/filebrowser_data，设置好权限，进入这个目录。

```
sudo mkdir /srv/mount/filebrowser_data
sudo mv /usr/local/bin/"filebrowser" /srv/mount/filebrowser_data 
sudo chown -R filebrowser /srv/mount/filebrowser_data
sudo chmod -R 755 /srv/mount/filebrowser_data
cd /srv/mount/filebrowser_data
```

3. 设定根目录、创建数据库文件并启动 filebrowser

```
sudo -u filebrowser -H bash
filebrowser -r /srv/mount/filebrowser
```

之后 127.0.0.1:8080 可访问 filebrowser 初始界面，如果想要在本地计算机查看，需要做如下调整：

```
filebrowser config set --address 0.0.0.0
```
之后在计算机浏览器中可访问 `<ip>:<port>` 查看 filebrowser 界面，安全起见后续配置做好后要改回 127.0.0.1。

默认初始界面为登陆界面，默认的用户名和密码都是 admin，至少要从网页或用 filebrowser 指令先更改密码。

没有特别指定的话，数据库文件 filebrowser.db 会在 filebrowser 可执行程序同级目录下生成。

**之后的指令请以 root 或 sudo 执行。**

## 基础反代



```
sudo nano /etc/nginx/conf.d/filebrowser.conf
```

输入下面的内容：

```
server {
    listen 80;
    server_name emample.tld;

    location / {
        proxy_pass http://127.0.0.1:8080;
        proxy_redirect off;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}

server {
    listen              443 ssl;
    server_name         emample.tld;
    ssl_certificate     /etc/letsencrypt/live/emample.tld/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/emample.tld/privkey.pem;
    ssl_protocols       TLSv1 TLSv1.1 TLSv1.2;
    ssl_ciphers         HIGH:!aNULL:!MD5;
}
```
替换 emample.tld 为其他域名，注意证书位置。

```
sudo nginx -t
sudo nginx -s reload
```

没问题的话域名已经可以直接访问 filebrowser 界面了。


## 开机自启

service 内容来源： https://github.com/filebrowser/filebrowser/issues/411

```
sudo nano /etc/systemd/system/filebrowser.service
```

输入以下内容：

```
[Unit]
Description=File browser: %I
After=network.target

[Service]
User=filebrowser
Group=fuse
ExecStart=/srv/mount/filebrowser_data/filebrowser -d /srv/mount/filebrowser_data/filebrowser.db

[Install]
WantedBy=multi-user.target
```

启动程序并设置自动开启：

```
sudo systemctl start filebrowser
sudo systemctl enable filebrowser
sudo systemctl list-unit-files| grep filebrowser  # 看看有无设置成功
```
之后在网页自行根据需求进行配置。

## 后续

无法下载文件

应继续配置 nginx 和 filebrowser ？

https://blog.csdn.net/yimagudao/article/details/89461523

https://github.com/filebrowser/filebrowser/issues/549

https://github.com/filebrowser/filebrowser/issues/400

https://www.reddit.com/r/selfhosted/comments/lz6hah/nginx_reverse_proxy_authrequest_exception_for/

https://github.com/linuxserver/reverse-proxy-confs/blob/master/filebrowser.subdomain.conf.sample

```
server {
    listen 80;
    server_name box.moonblast.boats;

    location / {
        proxy_pass http://127.0.0.1:8080;
        proxy_redirect off;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
    return 301 https://box.moonblast.boats$request_uri;
}

server {
    listen              443 ssl http2;
    server_name         box.moonblast.boats;

    ssl_certificate     /etc/letsencrypt/live/moonblast.boats/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/moonblast.boats/privkey.pem;
    ssl_session_cache   shared:SSL:1m;
    ssl_session_timeout 10m;
    ssl_protocols       TLSv1 TLSv1.1 TLSv1.2;
    ssl_ciphers         HIGH:!aNULL:!MD5;
}
```








