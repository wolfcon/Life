---
title: Mac Catalyst 中强行使用不支持的 API
date: 2021-9-2
---

# Mac Catalyst 中强行使用不支持的 API

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**

- [前言](#%E5%89%8D%E8%A8%80)
- [解决方案](#%E8%A7%A3%E5%86%B3%E6%96%B9%E6%A1%88)
  - [Dynamic](#dynamic)
  - [Plugin Extension](#plugin-extension)
    - [创建 Bundle](#%E5%88%9B%E5%BB%BA-bundle)
    - [嵌入 Target](#%E5%B5%8C%E5%85%A5-target)
    - [创建通用 `Protocol`](#%E5%88%9B%E5%BB%BA%E9%80%9A%E7%94%A8-protocol)
    - [创建真正实现功能的 AppKit 类](#%E5%88%9B%E5%BB%BA%E7%9C%9F%E6%AD%A3%E5%AE%9E%E7%8E%B0%E5%8A%9F%E8%83%BD%E7%9A%84-appkit-%E7%B1%BB)
    - [调用方法](#%E8%B0%83%E7%94%A8%E6%96%B9%E6%B3%95)
- [后记](#%E5%90%8E%E8%AE%B0)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## 前言

Mac Catalyst 这个从 iOS 平台过渡到 macOS 的中间方案, 肯定从底层来看, API 肯定是和 AppKit 共享一套 Runtime, 然而, AppKit 中的大多数 API 均被标记为 MacCatalyst 不可用😅. 那么本文将横空出世, 来总结一下我最近遇到的类似问题以及整理一下解决方案, 话不多说, 🏂

## 解决方案

共有 2 种解决方案, 一种是通过 [`Dynamic`](https://github.com/mhdhejazi/Dynamic) 这个库来去做, 另外一种是通过植入 macOS 版的插件来去实现.

### Dynamic

这个不多说了, 去看 [`API`](https://github.com/mhdhejazi/Dynamic#examples) 吧

注意: 对于一些简单的调用, 推荐使用这个库, 如果说是非常复杂的情况, 还是建议使用第二种, 创建 macOS 版的插件更好一些.

### Plugin Extension

#### 创建 Bundle

`File` -> `New` -> `Target...` -> `macOS` -> `Framework & Library` -> `Bundle`

#### 嵌入 Target

在 `主Target ` 中添加该 `bundle`, 并将 `platform filters` 修改为 `MacCatalyst`

#### 创建通用 `Protocol`

由于我们不能在 iOS 的开发环境中直接调用 macOS 的类, 所以我们曲线救国, 创建一个通用 Protocol, 我们在 iOS 的环境中仅调用这个 Protocol 的方法, 不去直接操纵类.

*注意: 这里创建的 Protocol `Target Membership` 需要`同时勾选` `主 Target` 和 `Bundle`*

```swift
@objc(WeChatLauncher)
protocol WeChatLauncher: NSObjectProtocol {
    init()
    
    func launchWeChat()
}
```

#### 创建真正实现功能的 AppKit 类

```swift
import AppKit

class MacLauncher: NSObject, WeChatLauncher {
    required override init() {
        super.init()
    }
    
    func launchWeChat() {
        let config = NSWorkspace.OpenConfiguration()
        
        NSWorkspace.shared.openApplication(at: URL(fileURLWithPath:  "/Applications/WeChat.app"), configuration: config) { app, error in
        }
    }
}
```

#### 调用方法

```swift
import Foundation
#if !os(macOS)
import UIKit
#endif

class PluginHelper {
    static func presentWeChat4Pasting() {
#if targetEnvironment(macCatalyst)
        // 1. 这里 fileName 就是创建 bundle 时填的 Product Name + Bundle Extension. (可以在 Products 中看到)
        let bundleFileName = "WeChatPlugins.Mac.bundle"
        guard let bundleURL = Bundle.main.builtInPlugInsURL?
                                    .appendingPathComponent(bundleFileName) else { return }
        guard let bundle = Bundle(url: bundleURL) else { return }

        // 2. 加载 bundle 以及获取插件的类, 类名是插件 [ProductName.类名] 的形式
        let className = "WeChatPlugins.MacLauncher"
        guard let pluginClass = bundle.classNamed(className) as? WeChatLauncher.Type else {
            return
        }

        // 3. 初始化类, 并调用
        pluginClass.init().launchWeChat()
#elseif os(iOS)
        UIApplication.shared.open(URL(string: "WeChat://")!) { flag in
            
        }
#endif
    }
}
```

其实如果这里插件只实现了一个类的话, 也可以在 Bundle 的 `info` 中指定 `Principal class` 为 `$(PRODUCT_MODULE_NAME).MacLauncher` (格式为: `$(PRODUCT_MODULE_NAME).类名`)

那么我们获取类时就使用如下代码替代 2 中的代码:

```swift
guard let pluginClass = bundle.principalClass as? WeChatLauncher.Type else { return }
```

## 后记

Apple 显然是想将 macOS 跟 iOS 在不久的将来进行大一统, 现在的 MacCatalyst 只是一个临时蹩脚方案, 其实我们现在使用的 `UIKit` 已经是一个外壳了, 真正的核心代码都在 `UIKitCore` 这个库中了. 至于其中的些许联系, 你或许可以在[另外一篇文章中窥得一二](iOS_MacCatalystCapabilityExtension.md).