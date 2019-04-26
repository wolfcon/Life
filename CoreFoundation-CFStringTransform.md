---
title: CoreFoundation - String Transform
---

# CoreFoundation - String Transform

## 前言

其实汉字转拼音 CoreFoundation 已经自带了, 我们可以很轻松的得到拼音. 是不是很带感👻 

废话不多说, 我们看个下我们今天要用到的主角.

## CFString

`CFString` 在 `CoreFoundation` 中对应 `String` 在 `Foundation` 中的位置. `String` 是封装了的 `CFString`, 但是很多的功能没被代入进去. 所以我们要使用 CFString 来进行操作.

`CFString` 今天我们主要用到的是这个方法

```swift
/* Perform string transliteration.  The transformation represented by transform is applied to the given range of string, modifying it in place. Only the specified range will be modified, but the transform may look at portions of the string outside that range for context. NULL range pointer causes the whole string to be transformed. On return, range is modified to reflect the new range corresponding to the original range. reverse indicates that the inverse transform should be used instead, if it exists. If the transform is successful, true is returned; if unsuccessful, false. Reasons for the transform being unsuccessful include an invalid transform identifier, or attempting to reverse an irreversible transform.

You can pass one of the predefined transforms below, or any valid ICU transform ID as defined in the ICU User Guide. Note that we do not support arbitrary set of ICU transform rules.
*/
public func CFStringTransform(_ string: CFMutableString!, _ range: UnsafeMutablePointer<CFRange>!, _ transform: CFString!, _ reverse: Bool) -> Bool
```

👀尖的人估计已经发现了. 这就是从 C 那边拿过来的. 参数都是 `!` 修饰的. 哈哈哈

这里要注意的就是 `CFMutableString`, 我们用到的是它, 而不是 `CFString`. 为什么不用我多说了吧🤣

## 打开 Playground. 

我们来看首先创建一个转换函数.

```swift
func transform(_ word: String, _ transform: CFString) -> String {
    let cfword = NSMutableString(string: word) as CFMutableString
    
    if CFStringTransform(cfword, nil, transform, false) {
        print(cfword)
        return cfword as String
    }
    return ""
}

// 内含 英语/中文/繁体中文/片假/平假/韩语/印地语/阿拉伯语/希腊语/俄语/泰语
let word = "Wisdom / 贤者 / 賢者 / ケンジャ / けんじゃ / 세이지 / ऋषि / الخسحكيم / Φασκόμηλο / шалфей / ปราชญ์"
```

### 任意字符转成拉丁字符 (`有更深层的意义`)

```swift
// 任意字符转写成拉丁文
let word1 = transform(word, kCFStringTransformToLatin)
```

结果是 

> Wisdom / xián zhě / xián zhě / kenja / kenja / seiji / r̥ṣi / ạlkẖsḥkym / Phaskómēlo / šalfej / prāchỵ̒

```swift
// 去除字符中含有的变音及重音标识符
let word2 = transform(word1, kCFStringTransformStripDiacritics)
```

结果是

> Wisdom / xian zhe / xian zhe / kenja / kenja / seiji / rsi / alkhshkym / Phaskomelo / salfej / prachy

#### 深层意义

在应用中存在多语言搜索及检索时, 不需要对单独语言进行处理.

具体我们看🌰, 所有的语言意思都是 `贤者` 的意思

`// 内含 英语/中文/繁体中文/片假/平假/韩语/印地语/阿拉伯语/希腊语/俄语/泰语
let word = "Wisdom / 贤者 / 賢者 / ケンジャ / けんじゃ / 세이지 / ऋषि / الخسحكيم / Φασκόμηλο / шалфей / ปราชญ์"`

比如上面的 `word` -> `word1` -> `word2`, 是经历了 `转拉丁` -> `去重音和变音`

我们再仅需再需一步 `转换为小写`

```swift
let searchIndex = word2.lowercased()
```

结果是

> wisdom / xian zhe / xian zhe / kenja / kenja / seiji / rsi / alkhshkym / phaskomelo / salfej / prachy

这时我们忽略 `/`, **这时通过对用户输入的文本使用同样的变换，你就可以实现一个通用的搜索，无论搜索文本或内容是什么语言**

### 将普通话转写为拼音

```swift
transform(word, kCFStringTransformMandarinLatin)
```

结果是

> Wisdom / xián zhě / xián zhě / ケンジャ / けんじゃ / 세이지 / ऋषि / الخسحكيم / Φασκόμηλο / шалфей / ปราชญ์

### 将平假转写为片假

```swift
transform(word, kCFStringTransformHiraganaKatakana)
```

结果是

> Wisdom / 贤者 / 賢者 / ケンジャ / ケンジャ / 세이지 / ऋषि / الخسحكيم / Φασκόμηλο / шалфей / ปราชญ์"

### 其他的就不做赘述了.



