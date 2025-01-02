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

# 安装软件包

```bash
sudo apt install postfix dovecot-core dovecot-imapd dovecot-pop3d
```
在安装时会让你选择选项，先选择Internet Site，然后让你填邮件域名，填进去你的域名即可，记得添加mx记录

# 修改配置文件

好了，开始改配置文件了，只要跟着改就没问题

## /etc/postfix/main.cf
```
# 把域名改成你的邮件域名
myhostname = domain.com

# 在末尾追加以下内容
smtpd_sasl_auth_enable = yes
smtpd_sasl_security_options = noanonymous
smtpd_sasl_local_domain = $myhostname
broken_sasl_auth_clients = yes
smtpd_sasl_type = dovecot
smtpd_sasl_path = private/auth
```

## /etc/dovecot/10-master.conf
```conf
# 110行，去注释，添加一点语句
unix_listener /var/spool/postfix/private/auth {
  mode = 0666
  user = postfix
  group = postfix
}
```

## /etc/dovecot/10-auth.conf
```conf
# 10行，去注释，把yes改成no
disable_plaintext_auth = no
```

## 添加用户，为用户设置密码，用户名可改成你自己的
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
# 修改这两行，指定成你的证书和私钥，在27-28行
smtpd_tls_cert_file=/etc/ssl/fullchain.pem
smtpd_tls_key_file=/etc/ssl/privkey.pem
```

## /etc/postfix/master.cf
```conf
# 追加到文件尾，启用smtps
smtpd inet n - y - - smtpd -o smtpd_tls_wrappermode=yes
```

## /etc/dovecot/conf.d/10-ssl.conf
```conf
# 修改这两行，同样指定成你的证书和私钥
ssl_cert = </etc/ssl/fullchain.pem
ssl_key = </etc/ssl/privkey.pem
```

## 重启邮件服务
```bash
systemctl restart postfix dovecot
```

---
好了，现在再试试使用ssl登录，不出意外的话，就能正常使用了

<Comment />
