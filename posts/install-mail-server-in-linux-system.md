---
date: 2025-1-3
title: 用postfix和dovecot在ubuntu 24.04搭建一个邮件系统
category: 教程
tags:
- mail
- linux
- postfix
- dovecot
description: 使用postfix和dovecot在ubuntu 24.04搭建一个邮件系统
---
# 系统版本
系统版本：ubuntu 24.04  
理论上deb系系统都能装
> [!WARNING]
> 下面的所有操作均在root用户操作

# 安装软件包

```bash
apt install postfix dovecot-core dovecot-imapd dovecot-pop3d
```
> 在安装时会让你选择选项，先选择Internet Site，然后让你填邮件域名，填进去你的域名即可，记得去dns提供商那边添加mx记录

> [!WARNING]
> 如果你正在使用wsl安装postfix，有可能在安装postfix时会出现`newaliases: fatal: bad string length 0 < 1: mydomain =`报错，检查main.cf的大概37行`myhostname =`，看看这行的后面是不是带个点，如果有就删掉这个点，然后用apt install postfix再次安装，然后就安装上了

# 修改配置文件

好了，开始改配置文件了，只要跟着改就没问题

## /etc/postfix/main.cf
```conf
# 把域名改成你的邮件域名
myhostname = domain.com

# 在末尾追加以下内容
home_mailbox = Maildir/
smtpd_sasl_auth_enable = yes
smtpd_sasl_security_options = noanonymous
smtpd_sasl_local_domain = $myhostname
broken_sasl_auth_clients = yes
smtpd_sasl_type = dovecot
smtpd_sasl_path = private/auth
```

## /etc/dovecot/conf.d/10-master.conf
```conf
# 大概110行，先把原来有的去掉注释，然后在里面加上原来没有的，不要去错注释了

# Postfix smtp-auth
unix_listener /var/spool/postfix/private/auth {
  mode = 0666
  user = postfix
  group = postfix
}
```

## /etc/dovecot/conf.d/10-auth.conf
```conf
# 大概10行，去注释，把yes改成no
disable_plaintext_auth = no
```

## /etc/dovecot/conf.d/10-mail.conf
```conf
# 大概30行，改成如下内容
mail_location = maildir:~/Maildir
```

## 添加用户，为用户设置密码，用户名可改成你自己的，如果你原本就有你自己的账号那可以不用新建
```bash
useradd -m user
passwd user
```

## 重启邮件服务
```bash
systemctl restart postfix dovecot
```

---

好了，基本的已经配置完了，现在你就可以尝试使用邮件客户端连接服务器了，要关闭ssl再登录

---
# 配置ssl

## /etc/postfix/main.cf
```conf
# 修改这两行，指定成你的证书和私钥，大概在27-28行
smtpd_tls_cert_file=/etc/ssl/fullchain.pem
smtpd_tls_key_file=/etc/ssl/privkey.pem
```

## /etc/postfix/master.cf
```conf
# 追加到文件尾，启用smtps
smtps inet n - y - - smtpd -o smtpd_tls_wrappermode=yes
```

## /etc/dovecot/conf.d/10-ssl.conf
```conf
# 找到并修改这两行，同样指定成你的证书和私钥
ssl_cert = </etc/ssl/fullchain.pem
ssl_key = </etc/ssl/privkey.pem
```

## 重启邮件服务
```bash
systemctl restart postfix dovecot
```

---

好了，现在再试试使用ssl登录，不出意外的话，就能正常使用了

---

# 注意事项
## 如何查看日志文件？
日志文件在/var/log/mail.log，如果没有这个文件，检查`rsyslog`是否安装，如果没有安装，请安装它，然后重启`postfix`和`dovecot`服务即可

---

<Comment />
