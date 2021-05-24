<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**

- [Swift 修饰关键字](#swift-%E4%BF%AE%E9%A5%B0%E5%85%B3%E9%94%AE%E5%AD%97)
  - [函数修饰符](#%E5%87%BD%E6%95%B0%E4%BF%AE%E9%A5%B0%E7%AC%A6)
    - [1. @discardable](#1-discardable)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->



# Swift 修饰关键字

## 函数修饰符

### 1. @discardable

```swift
@discardable
func foo() -> Int
```

如果不用 `@discardable` 修饰带返回值的函数, 在不使用返回值时, 我们通常这样写

```swift
_ = foo()
```

但是如果加了 `@discardable` , 则我们可以这样写

```swift
foo()
```

是不是方便了很多. 如同 `Objective-C`



