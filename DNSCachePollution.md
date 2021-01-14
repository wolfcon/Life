---
title: 万恶的 DNS 污染
---

# 万恶的 DNS 污染

[TOC]

## 简介

说道这万恶的 DNS 污染, 真是深恶痛绝. 🤬

## 解决方案

1. 先去 DNS 查询网站查询 `真正的 ip`

2. 然后记录下来修改 hosts. (本地的 DNS 解析)

   ### macOS / Linux

   `hosts`文件位置都是在 `/etc` 下

   ### Windows

   `win + R` 输入 `drivers` 打开后, 同样在 `etc` 下

## 常用的 DNS 解析地址

```hosts
# 简单粗暴的 github 图片头像解析地址

199.232.96.133      raw.githubusercontent.com
199.232.96.133      gist.githubusercontent.com
199.232.96.133      cloud.githubusercontent.com
199.232.96.133      camo.githubusercontent.com
199.232.96.133      avatars0.githubusercontent.com
199.232.96.133      avatars1.githubusercontent.com
199.232.96.133      avatars2.githubusercontent.com
199.232.96.133      avatars3.githubusercontent.com
199.232.96.133      avatars4.githubusercontent.com
199.232.96.133      avatars5.githubusercontent.com
199.232.96.133      avatars6.githubusercontent.com
199.232.96.133      avatars7.githubusercontent.com
199.232.96.133      avatars8.githubusercontent.com
199.232.96.133      user-images.githubusercontent.com

# 动漫花园的资源站
104.25.61.106				share.dmhy.org
104.25.62.106				share.dmhy.org
172.67.98.15				share.dmhy.org
2606:4700:20::6819:3d6a			share.dmhy.org
2606:4700:20::6819:3e6a			share.dmhy.org
2606:4700:20::ac43:620f			share.dmhy.org
```

## 授人以鱼不如授人以渔

[查询真正的 DNS IP 地址 ipaddress.com](https://www.ipaddress.com/)



> 注意: 如果不想修改 DNS, 而又确认是本地与远程 DNS 不匹配, 那么就去刷一下本地的 DNS 吧.
>
> Windows: `ipconfig /flushdns`
>
> macOS: `sudo killall -HUP mDNSResponder`
>
> Linux: 安装 `nscd` 执行 `nscd restart`
