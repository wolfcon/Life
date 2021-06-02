---
title: Bogon
---

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**

- [macOS 的终端变成 bogon](#macos-的终端变成-bogon)
  - [怨念](#怨念)
  - [破](#破)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

[TOC]

# macOS 的终端变成 bogon

## 怨念

根本忍不了左边是 `bogon`

终端会先向 DNS 服务器查询本地 IP 的方向解析结果, 如果查询不到再显示计算机名. DNS 解析错误, 导致返回给我们的是 bogon, 所以我们要强行设置我们本地的. 

## 破

权限不够, 自行加 `sudo`

1. 临时修改

1. ```shell
   hostname your-hostname
   ```

2. 永久修改( 重启后同样生效 )

2. ```shell
   sudo scutil --set LocalHostName your-hostname
   sudo scutil --set HostName your-hostname
   ```
