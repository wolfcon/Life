---
title: 对于 Mac Catalyst 不支持的控件的解决方案
date: 2021-9-6
---

# 对于 Mac Catalyst 不支持的控件的解决方案

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**

- [对于 Mac Catalyst 不支持的控件的解决方案](#%E5%AF%B9%E4%BA%8E-mac-catalyst-%E4%B8%8D%E6%94%AF%E6%8C%81%E7%9A%84%E6%8E%A7%E4%BB%B6%E7%9A%84%E8%A7%A3%E5%86%B3%E6%96%B9%E6%A1%88)
  - [问题](#%E9%97%AE%E9%A2%98)
  - [探究](#%E6%8E%A2%E7%A9%B6)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## 问题

最近在做 iOS 通用程序时, 遇到了一个错误, 是使用 `UIDatePicker` 引起的, 在 `Inline` 模式下点击左上角的日期, 将会切换至滚动选择器, 也就是 `UIPickerView` 来展示功能, 然而就是它 `UIPickerView` 却在 `MacCatalyst` 中不支持.🥲

以下是报错堆栈信息:

```swift
[General] _UIDatePickerView is not supported when running Catalyst apps in the Mac idiom.
[General] (
	0   CoreFoundation     0x00007fff204a883b __exceptionPreprocess + 242
	1   libobjc.A.dylib    0x00007fff201e0d92 objc_exception_throw + 48
	2   UIKitCore          0x00007fff45382e32 -[UIView(UICatalystMacIdiomUnsupported_Internal) _throwForUnsupportedNonMacIdiomBehaviorWithReason:] + 0
	3   UIKitCore          0x00007fff4508e1c4 -[UIPickerView _didMoveFromWindow:toWindow:] + 191    
```

我们先上解决方法, 后续值得探究一番.

## 解决方法

针对我的问题, 查看报错的堆栈, 可以看到是 UIPickerView 在被移动到前端显示时报的这个错误, 那么我们针对 `UIPickerView`, 让其遇到不支持的方法时, 不 `throw exception`. 当然大家可以看到堆栈中其实是 `UIView` 的分类方法丢出的, 我们也可以直接对 `UIView` 操作. 我这里仅针对 `UIPickerView` 提前调用如下方法:

```swift
// 注意这里初始化 Selector 方法时, 套了 2 层括号是因为 Xcode 默认会扫描不到这个 Objective-C 方法(当然了, 这是私有 API, 能搜到才有鬼了.👻), 多加一层静默掉那个警告罢了.
UIPickerView.perform(Selector(("_setAllowsUnsupportedMacIdiomBehavior:")), with: 1)
```

## 探究

### 问题从何而来

参考文章: 

> [Forbidden Controls in Catalyst: Optimize Interface for Mac](https://steipete.com/posts/forbidden-controls-in-catalyst-mac-idiom/)