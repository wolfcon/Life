---
title: Print & DebugPrint 进阶
---

# Print & DebugPrint 进阶

[TOC]

## 普通用法

### 基本类型

一般我们这样用

```swift
let deagleMaestro = "Niko"
print(deagleMaestro)

// Niko

let deagleJuan = "Dupreeh"
debugPrint(deagleJuan)

// "Dupreeh"
```

debugPrint 与 print 不同的是: **打印东西时会将类型等详细信息包含其中**.

### enum / struct / class

对于 enum, object 和 struct 只需要实现特殊的 `protocol`, 对应关系如下

| Protocol                     | Function   |
| ---------------------------- | ---------- |
| CustomDebugStringConvertible | debugPrint |
| CustomStringConvertible      | print      |

当然这里仅指都实现了的情况下

如果仅实现其中一个的话, 则调用对应方法时会**自动**寻找另外一个进行适配🤪

## 进阶用法

挖坑待填