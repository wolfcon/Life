# LetsEncrypted配置


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
