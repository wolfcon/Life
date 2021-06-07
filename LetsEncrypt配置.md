---
title: LetsEncrypt 配置
---

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**

- [LetsEncrypt配置](#letsencrypt%E9%85%8D%E7%BD%AE)
- [Certbot - 官方推荐的证书生成工具](#certbot---%E5%AE%98%E6%96%B9%E6%8E%A8%E8%8D%90%E7%9A%84%E8%AF%81%E4%B9%A6%E7%94%9F%E6%88%90%E5%B7%A5%E5%85%B7)
  - [生成通配符的证书](#%E7%94%9F%E6%88%90%E9%80%9A%E9%85%8D%E7%AC%A6%E7%9A%84%E8%AF%81%E4%B9%A6)
  - [可能在 Amazon Linux 1 上出现的错误](#%E5%8F%AF%E8%83%BD%E5%9C%A8-amazon-linux-1-%E4%B8%8A%E5%87%BA%E7%8E%B0%E7%9A%84%E9%94%99%E8%AF%AF)
  - [No module named cryptography.hazmat.bindings.openssl.binding](#no-module-named-cryptographyhazmatbindingsopensslbinding)
    - [这个问题参照](#%E8%BF%99%E4%B8%AA%E9%97%AE%E9%A2%98%E5%8F%82%E7%85%A7)
    - [主要解决方法](#%E4%B8%BB%E8%A6%81%E8%A7%A3%E5%86%B3%E6%96%B9%E6%B3%95)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

[TOC]

# LetsEncrypt配置


# Certbot - 官方推荐的证书生成工具
[Certbot 官方 Documents](https://certbot.eff.org/docs/)

## 生成通配符的证书
```shell
sudo ./certbot-auto certonly -d *.site.com --manual --preferred-challenges dns --server https://acme-v02.api.letsencrypt.org/directory
```
之后会生成一个 `DNS TXT record` 需要你在你域名下的 `_acme-challenge.site.com` 去配置
```
Please deploy a DNS TXT record under the name
_acme-challenge.i.cgtn.com with the following value:

*************************************
```
配置完之后, 就会显示生成的证书地址了.

一般是 `/etc/letsencrypt/live/site.com/` 这个地址下
会有 4 个文件:
  - `privkey.pem`  : the private key for your certificate.
  - `fullchain.pem`: the certificate file used in most server software.
  - `chain.pem`    : used for OCSP stapling in Nginx >=1.3.7.
  - `cert.pem`     : will break many server configurations, and should not be used
                 without reading further documentation [User Guide](https://certbot.eff.org/docs/using.html#where-are-my-certificates.).
               

一般情况下, 使用 `fullchain.pem` 和 `privkey.pem` 其他的, 按照需要自行使用.

## 可能在 Amazon Linux 1 上出现的错误
## No module named cryptography.hazmat.bindings.openssl.binding
```
[ec2-user@ip letsencrypt]$ ./letsencrypt-auto
Checking for new version...
Creating virtual environment...
Installing Python packages...
Requesting root privileges to run letsencrypt...
   sudo /home/ec2-user/.local/share/letsencrypt/bin/letsencrypt --no-self-upgrade
Traceback (most recent call last):
  File "/home/ec2-user/.local/share/letsencrypt/bin/letsencrypt", line 7, in <module>
    from letsencrypt.cli import main
  File "/home/ec2-user/.local/share/letsencrypt/local/lib/python2.7/dist-packages/letsencrypt/cli.py", line 21, in <modul
e>
    import OpenSSL
  File "/home/ec2-user/.local/share/letsencrypt/local/lib/python2.7/dist-packages/OpenSSL/__init__.py", line 8, in <modul
e>
    from OpenSSL import rand, crypto, SSL
  File "/home/ec2-user/.local/share/letsencrypt/local/lib/python2.7/dist-packages/OpenSSL/rand.py", line 11, in <module>
    from OpenSSL._util import (
  File "/home/ec2-user/.local/share/letsencrypt/local/lib/python2.7/dist-packages/OpenSSL/_util.py", line 6, in <module>
    from cryptography.hazmat.bindings.openssl.binding import Binding
ImportError: No module named cryptography.hazmat.bindings.openssl.binding
```

### 这个问题参照
[官方链接](https://github.com/certbot/certbot/issues/2544)

### 主要解决方法
由于 `site-packages` 是空的. 只要将 `dist-packages` 中的东西复制过去或者直接软链接过去就可以正常运行了. 

```shell
mv site-packages site-packages_bak
ln -s dist-packages site-packages
```
