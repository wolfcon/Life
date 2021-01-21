---
title: Git Shallow Clone
---

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
```

