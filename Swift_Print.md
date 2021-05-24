<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**

- [Print & DebugPrint 进阶](#print--debugprint-%E8%BF%9B%E9%98%B6)
  - [普通用法](#%E6%99%AE%E9%80%9A%E7%94%A8%E6%B3%95)
    - [基本类型](#%E5%9F%BA%E6%9C%AC%E7%B1%BB%E5%9E%8B)
    - [enum / struct / class](#enum--struct--class)
  - [进阶用法](#%E8%BF%9B%E9%98%B6%E7%94%A8%E6%B3%95)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->



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
