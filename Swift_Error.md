<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**

- [Swift - Error](#swift---error)
  - [Error 并不是简单的 NSError 换层皮](#error-%E5%B9%B6%E4%B8%8D%E6%98%AF%E7%AE%80%E5%8D%95%E7%9A%84-nserror-%E6%8D%A2%E5%B1%82%E7%9A%AE)
  - [要存储 Error 的额外信息怎么办](#%E8%A6%81%E5%AD%98%E5%82%A8-error-%E7%9A%84%E9%A2%9D%E5%A4%96%E4%BF%A1%E6%81%AF%E6%80%8E%E4%B9%88%E5%8A%9E)
    - [enum](#enum)
    - [struct (class)](#struct-class)
  - [如何 print / 使用 UI 展示错误信息](#%E5%A6%82%E4%BD%95-print--%E4%BD%BF%E7%94%A8-ui-%E5%B1%95%E7%A4%BA%E9%94%99%E8%AF%AF%E4%BF%A1%E6%81%AF)
    - [LocalizedError](#localizederror)
    - [CustomStringConvertible / CustomDebugStringConvertible /](#customstringconvertible--customdebugstringconvertible-)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->



# Swift - Error

我们大家都知道 `NSError` 在 Swift 中有的新的类型 - `Error`

## Error 并不是简单的 NSError 换层皮

首先我们来看 Error 的定义

```swift
public protocol Error
```

惊不惊喜, 意不意外. 它是一个协议. 没错! 就是它, 协议在 Swift 中可以算是非常重要的, 它可以以任意的形式表述 Error, 🌰走你

你可以这样

```swift
enum YourOwnError: Error {
    case waller
    case hacker
    case j6me
}
```

```swift
enum YourOwnError: Int, Error {
    case waller = 4
    case hacker = 44
    case J6me = 666
}
```

或者这样

```swift
struct YourOwnError: Error {
    enum Kind {
        case waller
    	case hacker
        case j6me
    }
    
    let code: Int
    let kind: Kind
}
```

再或者这样 (一般使用 enum 和 struct 居多)

```swift
class YourOwnError: Error {
    enum Kind {
        case waller
    	case hacker
        case j6me
    }
    
    let code: Int
    let kind: Kind
    
    init...
}
```

怎么定义我们了解了, 那么问题来了.

## 要存储 Error 的额外信息怎么办

### enum

仅用作预定义好的错误类型, 信息都是提前预设好的, 当然在编码阶段可以随时扩展 `extension` 大法, 走你

### struct (class)

可以在使用时创建自定义错误类型, 上面的例子中有介绍. 

## 如何 print / 使用 UI 展示错误信息

这时候就要使用其他的协议来帮助我们了. 

### LocalizedError

```swift
/// Describes an error that provides localized messages describing why
/// an error occurred and provides more information about the error.
public protocol LocalizedError : Error {

    /// A localized message describing what error occurred.
    var errorDescription: String? { get }

    /// A localized message describing the reason for the failure.
    var failureReason: String? { get }

    /// A localized message describing how one might recover from the failure.
    var recoverySuggestion: String? { get }

    /// A localized message providing "help" text if the user requests help.
    var helpAnchor: String? { get }
}
```

看到这个东西是不是仿佛看到了 `NSError` 的影子. 没错. `Error` 拆分了 `NSError` 的各项职能, 变得更加灵活和更加容易表述.



还有一个需要注意的地方就是 Error 的表述一般使用 `error.localizedDescription`, 这里调用的就是 `LocalizedError` 的 `errorDescription` 属性, 而并不是简单的 `override var localizedDescription`

### CustomStringConvertible / CustomDebugStringConvertible / 

这个是打印字符串的标准协议了. 不多赘述.

```swift
public protocol CustomStringConvertible {

    /// A textual representation of this instance.
    ///
    /// Calling this property directly is discouraged. Instead, convert an
    /// instance of any type to a string by using the `String(describing:)`
    /// initializer. This initializer works with any type, and uses the custom
    /// `description` property for types that conform to
    /// `CustomStringConvertible`:
    ///
    ///     struct Point: CustomStringConvertible {
    ///         let x: Int, y: Int
    ///
    ///         var description: String {
    ///             return "(\(x), \(y))"
    ///         }
    ///     }
    ///
    ///     let p = Point(x: 21, y: 30)
    ///     let s = String(describing: p)
    ///     print(s)
    ///     // Prints "(21, 30)"
    ///
    /// The conversion of `p` to a string in the assignment to `s` uses the
    /// `Point` type's `description` property.
    var description: String { get }
}

public protocol CustomDebugStringConvertible {

    /// A textual representation of this instance, suitable for debugging.
    ///
    /// Calling this property directly is discouraged. Instead, convert an
    /// instance of any type to a string by using the `String(reflecting:)`
    /// initializer. This initializer works with any type, and uses the custom
    /// `debugDescription` property for types that conform to
    /// `CustomDebugStringConvertible`:
    ///
    ///     struct Point: CustomDebugStringConvertible {
    ///         let x: Int, y: Int
    ///
    ///         var debugDescription: String {
    ///             return "(\(x), \(y))"
    ///         }
    ///     }
    ///
    ///     let p = Point(x: 21, y: 30)
    ///     let s = String(reflecting: p)
    ///     print(s)
    ///     // Prints "(21, 30)"
    ///
    /// The conversion of `p` to a string in the assignment to `s` uses the
    /// `Point` type's `debugDescription` property.
    var debugDescription: String { get }
}
```

好了. 以上就是我个人的一些使用经验之谈.
