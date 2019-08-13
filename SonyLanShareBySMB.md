---
title: SonyTV 与 Windows10 共享文件
---

# SonyTV 与 Windows10 共享文件
[TOC]

## Windows 10

- 打开 `Network and Sharing Center`
- `Advanced shareing settings`
- 勾选 `网络发现`
- 勾选 `分享文件和打印机`
- 勾选 `不需要密码保护`

## Sony TV

- 安装 `ES File Explorer` 或者其他支持 SMB 直接访问分享的 App
- 匿名访问即可食用


## 重中之重

- 在 `Programs and Feature` 中找到 `Windows features`
- **安装 `SMB 1.0/CIFS File Sharing Support`**

*注意: 一定要安装 `SMB1.0`, 否则电视端使用 `ESExplorer` 时会打不开*
