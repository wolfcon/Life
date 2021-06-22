---
title: 万恶的 DNS 污染之 Github 等正常访问
---

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**

- [万恶的 DNS 污染之 Github 等正常访问](#%E4%B8%87%E6%81%B6%E7%9A%84-dns-%E6%B1%A1%E6%9F%93%E4%B9%8B-github-%E7%AD%89%E6%AD%A3%E5%B8%B8%E8%AE%BF%E9%97%AE)
  - [📄 简介](#-%E7%AE%80%E4%BB%8B)
  - [✅ 解决方案](#-%E8%A7%A3%E5%86%B3%E6%96%B9%E6%A1%88)
  - [🍪 常用的 DNS 解析地址](#-%E5%B8%B8%E7%94%A8%E7%9A%84-dns-%E8%A7%A3%E6%9E%90%E5%9C%B0%E5%9D%80)
  - [🎣 授人以鱼不如授人以渔](#-%E6%8E%88%E4%BA%BA%E4%BB%A5%E9%B1%BC%E4%B8%8D%E5%A6%82%E6%8E%88%E4%BA%BA%E4%BB%A5%E6%B8%94)
  - [🔨 测试](#-%E6%B5%8B%E8%AF%95)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# 万恶的 DNS 污染之 Github 等正常访问

[TOC]

## 📄 简介

说道这万恶的 DNS 污染, 真是深恶痛绝. 🤬

## ✅ 解决方案

1. 先去 DNS 查询网站查询 `真正的 ip`

2. 然后记录下来修改 hosts. (本地的 DNS 解析)

   ### macOS / Linux

   `hosts`文件位置都是在 `/etc` 下

   ### Windows

   `win + R` 输入 `drivers` 打开后, 同样在 `etc` 下

## 🍪 常用的 DNS 解析地址

```hosts
# github 加速

199.232.69.194      github.global.ssl.fastly.net

# github
140.82.113.4        github.com
140.82.113.5        api.github.com

# 简单粗暴的 github 图片头像解析地址

185.199.108.133      raw.githubusercontent.com
185.199.108.133      gist.githubusercontent.com
185.199.108.133      cloud.githubusercontent.com
185.199.108.133      camo.githubusercontent.com
185.199.108.133      media.githubusercontent.com
185.199.108.133      avatars.githubusercontent.com
185.199.108.133      avatars0.githubusercontent.com
185.199.108.133      avatars1.githubusercontent.com
185.199.108.133      avatars2.githubusercontent.com
185.199.108.133      avatars3.githubusercontent.com
185.199.108.133      avatars4.githubusercontent.com
185.199.108.133      avatars5.githubusercontent.com
185.199.108.133      avatars6.githubusercontent.com
185.199.108.133      avatars7.githubusercontent.com
185.199.108.133      avatars8.githubusercontent.com
185.199.108.133      user-images.githubusercontent.com

# 动漫花园的资源站
104.25.61.106								share.dmhy.org
104.25.62.106								share.dmhy.org
172.67.98.15								share.dmhy.org
2606:4700:20::6819:3d6a			share.dmhy.org
2606:4700:20::6819:3e6a			share.dmhy.org
2606:4700:20::ac43:620f			share.dmhy.org
```

## 🎣 授人以鱼不如授人以渔

[查询真正的 DNS IP 地址 👀 ipaddress.com](https://www.ipaddress.com/)



> 注意: 如果不想修改 DNS, 而又确认是本地与远程 DNS 不匹配, 那么就去刷一下本地的 DNS 吧.
>
> Windows: `ipconfig /flushdns`
>
> macOS: `sudo killall -HUP mDNSResponder`
>
> Linux: 安装 `nscd` 执行 `nscd restart`

## 🔨 测试

兄弟们! 最终要的是测试啊. 🤩

在`终端/cmd/bash` 中 `ping` 你需要的地址吧, 看是否已经解析✅
