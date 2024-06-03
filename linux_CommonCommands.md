---
title: Linux 常用命令
---

#  Linux Common Commands

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**

- [查看 MD5 值](#%E6%9F%A5%E7%9C%8B-md5-%E5%80%BC)
- [获取 Timestamp](#%E8%8E%B7%E5%8F%96-timestamp)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## 查看 MD5 值

```bash
# md5 字符串
md5Value=$(echo -n "string" | md5sum | cut -d " " -f1)
# md5 文件
md5Value=$(md5sum file | cut -d " " -f1)
```

## 获取 Timestamp 

```bash
date +%s
```