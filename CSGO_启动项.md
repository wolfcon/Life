<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**

- [启动项的奥秘](#%E5%90%AF%E5%8A%A8%E9%A1%B9%E7%9A%84%E5%A5%A5%E7%A7%98)
  - [预加载一部分配置命令](#%E9%A2%84%E5%8A%A0%E8%BD%BD%E4%B8%80%E9%83%A8%E5%88%86%E9%85%8D%E7%BD%AE%E5%91%BD%E4%BB%A4)
    - [其他方法](#%E5%85%B6%E4%BB%96%E6%96%B9%E6%B3%95)
  - [配置启动参数](#%E9%85%8D%E7%BD%AE%E5%90%AF%E5%8A%A8%E5%8F%82%E6%95%B0)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

[TOC]

# 启动项的奥秘

在 CSGO 的世界中, 进入 CSGO 之前可以

- 预加载一部分配置命令. 
- 配置启动参数

那么我们就按照这2个方面主要去大致分解一下

## 预加载一部分配置命令

直接写控制台中的 `exec` 命令即可

譬如我们有一个 `Frank_Training.cfg` , 我们需要在游戏加载时自动执行, 则在启动项上加入 `-exec Frank_Training` 即可. 其中 `.cfg` 可以省略



至于怎么写 `cfg` , 改日在写 

### 其他方法

我们如果不想使用启动项来实现的话, 则可以通过老路数, 将 `执行命令` 加入到游戏必定会自动加载的 `config.cfg` 的最后即可. 

**缺陷**: `config.cfg` 在游戏关闭时, 会被程序重写, 导致我们自己加的那行`失效`, 因为游戏并`不会保留`那一行, 所以我们就得将其设置为`只读`, 即可保留那一行, 不过也有相应的缺陷, 如果在游戏中改了设置, 则并不会被保留.

## 配置启动参数

基础参数

- -novid

  去除游戏开始动画

- -tickrate 128
  自己开本地服务器用的128tick

- -freq 240
  锁定刷新率, 一般不设定这个东西

- -high
  高优先级, CPU 处理优先级最高

- -preload

  预读资源, 会额外耗费内存, 但是进图等会加快一点

- -perfectworld
  进国服, 进入游戏时不弹出地区提示框

- -worldwide
  进国际服, 进入游戏时不弹出地区提示框

- 其他还有不少命令, 但没有太大用处, 就不提了. 有兴趣的可以自行网上搜索
