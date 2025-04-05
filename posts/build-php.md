---
date: 2025-04-06
title: 编译安装php
category: 教程
tags:
- php
- linux
description: 在linux平台编译安装php8.4
---
# 前言
最近在建站，所以需要用到php，但是ubuntu 22.04上面的php有点旧，是8.1的，所以......编译个php玩玩

# 下载源码
访问[https://www.php.net/downloads.php](https://www.php.net/downloads.php)，然后找到Source Code，找到你需要的php版本，我这里选择了php 8.4.5版本
```bash
wget https://www.php.net/distributions/php-8.4.5.tar.xz
tar -xvf php-8.4.5.tar.xz
cd php-8.4.5
```

# 配置
其实就是执行`./configure`，这里传的参数可能比较多，其实就是打开或者关闭一些功能，或者指定安装路径，按照需要自行调整，我这里给出我的配置，具体配置可以用`./configure --help`命令查看
```bash
# 安装一些依赖，我这里是ubuntu 22.04
sudo apt install build-essential pkgconf libxml2-dev libssl-dev libsqlite3-dev zlib1g-dev libcurl4-openssl-dev libffi-dev libzip-dev libpng-dev libavif-dev libwebp-dev libjpeg-dev libxpm-dev libfreetype-dev libonig-dev libldap-dev libreadline-dev libexpat1-dev libxslt1-dev 

# 配置
./configure --prefix=/usr/local/php/84 --enable-mysqlnd --with-zip --with-xsl --with-expat --enable-sockets --with-readline --enable-pcntl --with-mysqli --enable-mbstring --enable-intl --with-mhash --with-freetype --with-xpm --with-jpeg --with-webp --with-avif --enable-gd --enable-ftp --with-ffi --enable-exif --with-curl --enable-calendar --enable-bcmath --with-zlib --with-openssl --enable-fpm --disable-phpdbg --enable-sysvshm --enable-sysvsem --enable-sysvmsg --with-ldap
```

# 编译
```bash
# nproc命令会自动获取cpu线程数
make -j$(nproc)

# 安装到系统
sudo make install
```

自此，php8.4就编译安装好了


<Comment />
