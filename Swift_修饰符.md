---
title: Swift 修饰关键字
---

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



