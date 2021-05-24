<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**

- [CocoaPods 自走手册](#cocoapods-%E8%87%AA%E8%B5%B0%E6%89%8B%E5%86%8C)
  - [使用](#%E4%BD%BF%E7%94%A8)
  - [制作](#%E5%88%B6%E4%BD%9C)
    - [最后一步](#%E6%9C%80%E5%90%8E%E4%B8%80%E6%AD%A5)
      - [检测账户](#%E6%A3%80%E6%B5%8B%E8%B4%A6%E6%88%B7)
      - [如果没有注册过, 先注册自己的机器](#%E5%A6%82%E6%9E%9C%E6%B2%A1%E6%9C%89%E6%B3%A8%E5%86%8C%E8%BF%87-%E5%85%88%E6%B3%A8%E5%86%8C%E8%87%AA%E5%B7%B1%E7%9A%84%E6%9C%BA%E5%99%A8)
      - [使用这个命令进行上传](#%E4%BD%BF%E7%94%A8%E8%BF%99%E4%B8%AA%E5%91%BD%E4%BB%A4%E8%BF%9B%E8%A1%8C%E4%B8%8A%E4%BC%A0)
      - [添加管理账户](#%E6%B7%BB%E5%8A%A0%E7%AE%A1%E7%90%86%E8%B4%A6%E6%88%B7)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->


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
