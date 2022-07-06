---
title: 在 Debian 10 上安装 Filerun
date created: 2022-06-29 15:25
status: note
tags:
  - filerun
  - debian
  - php
  - mariadb
aliases: 在 Debian 10 上安装 Filerun
---

# 在 Debian 10 上安装 Filerun

主要参考: [在Debian10上安装FileRun](https://lala.im/7695.html)

## 安装依赖

### php 7.4

*升级 php 来源*

```
# 下面的应该是 php8.0/8.1 的步骤，7.4 的我不确定
sudo apt install -y lsb-release apt-transport-https ca-certificates wget
sudo wget -O /etc/apt/trusted.gpg.d/php.gpg https://packages.sury.org/php/apt.gpg
echo "deb https://packages.sury.org/php/ $(lsb_release -sc) main" | sudo tee /etc/apt/sources.list.d/php.list
sudo apt update
```

*如果 apt install 时出现多个 php 版本，比方说我需要 7.4 而多出了 8.1*

```
sudo apt install php7.4-fpm php8.1*-
```

**需要补充安装的 pachages**

```
sudo apt install php7.4-gd php7.4-mbstring php7.4-xml php7.4-mysql php7.4-zip php7.4-curl php7.4-ldap php7.4-imagick
```

### 其他

stl-thumb 安装特别提醒：

1. 下载最新版本
2. 在下载了 deb 文件的目录下运行这行命令（注意版本）

    ```
    sudo apt install ./stl-thumb_0.5.0_amd64.deb
    ```

## Mariadb 配置

[mysql数据库密码设置](https://blog.csdn.net/qq_26486949/article/details/88373487)

## nginx 配置

filerun.conf:

```
server {
  listen      80;
  listen      443 ssl http2;
  server_name run.cobaltkiss.blue;
  root        /var/www/filerun;
  index       index.php;
  client_max_body_size 0;

  ssl_certificate     /etc/letsencrypt/live/example.com/fullchain.pem;
  ssl_certificate_key /etc/letsencrypt/live/example.com/privkey.pem;
  ssl_session_timeout 10m;
  ssl_session_cache   shared:ssl_session_cache:10m;
  ssl_protocols       TLSv1 TLSv1.1 TLSv1.2;
  ssl_ciphers         HIGH:!aNULL:!MD5;

  location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php7.4-fpm.sock;
  }
}
```

可能需要修改 conf.d 下的 default 文件：

```
server {
    ###
	
    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php7.4-fpm.sock;
    }
    
    ###
}
```

## 下载安装 Filerun

### 下载

### 页面安装