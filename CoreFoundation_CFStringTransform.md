---
title: CoreFoundation - String Transform
---

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**

- [CoreFoundation - String Transform](#corefoundation---string-transform)
  - [前言](#%E5%89%8D%E8%A8%80)
  - [CFString](#cfstring)
    - [打开 Playground.](#%E6%89%93%E5%BC%80-playground)
  - [任意字符转成拉丁字符 (`有更深层的意义`)](#%E4%BB%BB%E6%84%8F%E5%AD%97%E7%AC%A6%E8%BD%AC%E6%88%90%E6%8B%89%E4%B8%81%E5%AD%97%E7%AC%A6-%E6%9C%89%E6%9B%B4%E6%B7%B1%E5%B1%82%E7%9A%84%E6%84%8F%E4%B9%89)
    - [深层意义](#%E6%B7%B1%E5%B1%82%E6%84%8F%E4%B9%89)
  - [将普通话转写为拼音](#%E5%B0%86%E6%99%AE%E9%80%9A%E8%AF%9D%E8%BD%AC%E5%86%99%E4%B8%BA%E6%8B%BC%E9%9F%B3)
  - [将平假转写为片假](#%E5%B0%86%E5%B9%B3%E5%81%87%E8%BD%AC%E5%86%99%E4%B8%BA%E7%89%87%E5%81%87)
  - [其他的就不做赘述了.](#%E5%85%B6%E4%BB%96%E7%9A%84%E5%B0%B1%E4%B8%8D%E5%81%9A%E8%B5%98%E8%BF%B0%E4%BA%86)
  - [PS: 其实还有个比较重要的就是 Unicode](#ps-%E5%85%B6%E5%AE%9E%E8%BF%98%E6%9C%89%E4%B8%AA%E6%AF%94%E8%BE%83%E9%87%8D%E8%A6%81%E7%9A%84%E5%B0%B1%E6%98%AF-unicode)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

[TOC]

# CoreFoundation - String Transform

## 前言

> 关于一种语言好不好用，你只需要衡量以下两种指标：
>
> 1. API 的统一性
> 2. String 类的实现质量
>
>
>
> 引用自: Mattt

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

### 打开 Playground. 

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

## 任意字符转成拉丁字符 (`有更深层的意义`)

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

### 深层意义

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

~~我们先忽略~~ `/`, **这时通过对用户输入的文本使用同样的变换，你就可以实现一个通用的搜索，无论搜索文本或内容是什么语言**

## 将普通话转写为拼音

```swift
transform(word, kCFStringTransformMandarinLatin)
```

结果是

> Wisdom / xián zhě / xián zhě / ケンジャ / けんじゃ / 세이지 / ऋषि / الخسحكيم / Φασκόμηλο / шалфей / ปราชญ์

## 将平假转写为片假

```swift
transform(word, kCFStringTransformHiraganaKatakana)
```

结果是

> Wisdom / 贤者 / 賢者 / ケンジャ / ケンジャ / 세이지 / ऋषि / الخسحكيم / Φασκόμηλο / шалфей / ปราชญ์"

## 其他的就不做赘述了.

## PS: 其实还有个比较重要的就是 Unicode

为什么呢? 因为`kCFStringTransformToUnicodeName` 让你可以找出特殊字符的 Unicode 标准名，包括 `Emoji`

🌰走你

```swift
let emoji = "👻💀🤣🐱"
transform(emoji, kCFStringTransformToUnicodeName)
```

结果是 *(我把每个字符换行了, 便于查看)*

> \\N{GHOST}
>
> \N{SKULL}
>
> \N{ROLLING ON THE FLOOR LAUGHING}
>
> \N{CAT FACE}"

