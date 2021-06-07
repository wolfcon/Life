---
title: Git Shallow Clone
---

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**

- [Git Shallow Clone](#git-shallow-clone)
  - [引言](#%E5%BC%95%E8%A8%80)
  - [Shallow Clone](#shallow-clone)
    - [近期版本](#%E8%BF%91%E6%9C%9F%E7%89%88%E6%9C%AC)
    - [单个分支](#%E5%8D%95%E4%B8%AA%E5%88%86%E6%94%AF)
  - [Unshallow](#unshallow)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# Git Shallow Clone

[TOC]

## 引言

git 库在 clone 下来时, 会将所有提交内容都会 clone 下来, 往往内容会非常大.

## Shallow Clone

### 近期版本

```shell
git clone --depth [depth] [remote-url]
```

如果参数值选择 1, 则仅会 clone 最近的一次提交, 历史记录则不会 clone. 大小会小很多

### 单个分支

```shell
git clone [remote-url] --branch [name] --single-branch [folder]
```



## Unshallow

我们已经使用了 ShallowClone 那么, 再继续 pull 或者 fetch 都拿不到历史了. 如何把这个库再变成一个完全的库呢?

可以使用 `--unshallow` 参数：

```sh
git pull --unshallow
# or
git fetch --unshallow
```

