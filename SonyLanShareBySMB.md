---
title: SonyTV 与 Windows10/11 共享文件
---

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**

- [SonyTV 与 Windows10/11 共享文件](#sonytv-与-windows1011-共享文件)
  - [Windows 10/11](#windows-1011)
  - [Sony TV](#sony-tv)
  - [Windows 11 的问题](#windows-11-的问题)
  - [重中之重](#重中之重)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->



# SonyTV 与 Windows10/11 共享文件
[TOC]

## Windows 10/11

- 打开 `Network and Sharing Center`
- `Advanced shareing settings`
- 勾选 `网络发现`
- 勾选 `分享文件和打印机`
- 勾选 `不需要密码保护`

## Sony TV

- 安装 `ES File Explorer` 或者其他支持 SMB 直接访问分享的 App
- 匿名访问即可食用


## Windows 11 的问题

在 Windows 11 中, 可能遇到使用 smb 拿不到文件详细信息, 比如最后编辑时间等, 无法排序. 可使用 FTP 原位替代即可, 操作同 smb 的安装(额外会在 IIS 中设置)

## 重中之重

如果升级了系统和 ESExplorer, 以下步骤就无需处理了.

- ~~在 `Programs and Feature` 中找到 `Windows features`~~
- ~~**安装 `SMB 1.0/CIFS File Sharing Support`**~~

~~*注意: 一定要安装 `SMB1.0`, 否则电视端使用 `ESExplorer` 时会打不开*~~
