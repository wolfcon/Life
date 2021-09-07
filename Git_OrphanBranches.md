---
title: Orphan Branches
date: 2021-9-7
---

# Orphan Branches

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**

- [Orphan Branches](#orphan-branches)
  - [应用情形](#%E5%BA%94%E7%94%A8%E6%83%85%E5%BD%A2)
  - [实现方式](#%E5%AE%9E%E7%8E%B0%E6%96%B9%E5%BC%8F)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## 应用情形

我们一般使用 Git 时, 根节点一般都是一个, 我们也可以创建另一个完全独立的根节点, 可以是 Pages 的源码 / 文档; 也可以是独立分发版本的独立分支, 隐去真正开发提交的分支.

## 实现方式

```bash
git checkout --orphan <new branch name> <commit hash>
```

