---
title: CocoaPods 自走手册
---

[TOC]

# CocoaPods 自走手册

我们从两个方面来去介绍

- [使用库 🍦](#使用)
- [制作库 👍](#制作)

## 使用

先挖个坑. 稍后整理一下填

## 制作

挖坑, 稍后整理

### 最后一步

要想一个Pods依赖库真正可用，还需要做最后一步操作，将我们刚才生成的podspec文件上传到CocoaPods官方的Specs仓库中，链接为： <https://github.com/CocoaPods/Specs> 

现在已经不提供直接 PR 到这里了. 需要试用 Pod 命令来实现这个功能 (上古时期的上传方法...😶)

#### 检测账户 

```shell
pod trunk me
```

#### 如果没有注册过, 先注册自己的机器 

```shell
pod trunk register 472730949@qq.com 'Frank' --description='iMacPro'
```

之后会受到一封验证📧, 记得查看📮

#### 使用这个命令进行上传 

```shell
pod trunk push MyPods.podspec --allow-warnings
```

#### 添加管理账户

```shell
pod trunk add-owner jiyou@qq.com
```

注意: 这里是以邮箱, 来管理的