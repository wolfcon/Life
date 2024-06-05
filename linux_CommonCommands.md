---
title: Linux 常用命令
---

#  Linux Common Commands

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**

- [查看 MD5 值](#%E6%9F%A5%E7%9C%8B-md5-%E5%80%BC)
- [获取 Timestamp](#%E8%8E%B7%E5%8F%96-timestamp)
- [Bash 详细输出](#bash-%E8%AF%A6%E7%BB%86%E8%BE%93%E5%87%BA)

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

date +(%Y-%m-%d)
```

## Bash 详细输出

Jenkins uses the shebang, by default

```bash
#!/bin/sh -e
```

-x will prompt those verbose details, if you'd like to reset that you can set the default shebang in your shell build step with something like

```bash
#!/bin/sh -xe
```
