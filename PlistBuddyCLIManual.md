---
title: PlistBuddy 命令行解析 Plist 工具详解
toc: true
---

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**

- [PlistBuddy 命令行解析 Plist 工具详解](#plistbuddy-%E5%91%BD%E4%BB%A4%E8%A1%8C%E8%A7%A3%E6%9E%90-plist-%E5%B7%A5%E5%85%B7%E8%AF%A6%E8%A7%A3)
  - [身世](#%E8%BA%AB%E4%B8%96)
  - [栖身](#%E6%A0%96%E8%BA%AB)
  - [简介](#%E7%AE%80%E4%BB%8B)
    - [How to get?](#how-to-get)
    - [How to set?](#how-to-set)
    - [How to delete?](#how-to-delete)
  - [总结](#%E6%80%BB%E7%BB%93)
    - [基础命令是](#%E5%9F%BA%E7%A1%80%E5%91%BD%E4%BB%A4%E6%98%AF)
    - [替换掉 `Command`:](#%E6%9B%BF%E6%8D%A2%E6%8E%89-command)
    - [替换掉 `Key or Subscript`, 也就是上面命令里的 `Entry`:](#%E6%9B%BF%E6%8D%A2%E6%8E%89-key-or-subscript-%E4%B9%9F%E5%B0%B1%E6%98%AF%E4%B8%8A%E9%9D%A2%E5%91%BD%E4%BB%A4%E9%87%8C%E7%9A%84-entry)
    - [替换掉 `Value`:](#%E6%9B%BF%E6%8D%A2%E6%8E%89-value)
    - [替换掉 Type:](#%E6%9B%BF%E6%8D%A2%E6%8E%89-type)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# PlistBuddy 命令行解析 Plist 工具详解

[TOC]

## 身世

PlistBuddy 是 macOS 系统携带的解析 Plist 文件的好帮手, 如其名.

## 栖身

`/usr/libexec/PlistBuddy`

## 简介

> 官方文档
>
> Usage: PlistBuddy [-cxh] <file.plist>
>
> ​    -c "\<command>" execute command, otherwise run in interactive mode
>
> ​    -x output will be in the form of an xml plist where appropriate
>
> ​    -h print the complete help info, with command guide
>
> Command Format:
>
> ​    Help - Prints this information
>
> ​    Exit - Exits the program, changes are not saved to the file
>
> ​    Save - Saves the current changes to the file
>
> ​    Revert - Reloads the last saved version of the file
>
> ​    Clear [\<Type>] - Clears out all existing entries, and creates root of Type
>
> ​    Print [\<Entry>] - Prints value of Entry.  Otherwise, prints file
>
> ​    Set \<Entry> \<Value> - Sets the value at Entry to Value
>
> ​    Add \<Entry> \<Type> [\<Value>] - Adds Entry to the plist, with value Value
>
> ​    Copy \<EntrySrc> \<EntryDst> - Copies the EntrySrc property to EntryDst
>
> ​    Delete \<Entry> - Deletes Entry from the plist
>
> ​    Merge <file.plist> [\<Entry>] - Adds the contents of file.plist to Entry
>
> ​    Import \<Entry> \<file> - Creates or sets Entry the contents of file
>
> 
>
> Entry Format:
>
> ​    Entries consist of property key names delimited by colons.  Array items
>
> ​    are specified by a zero-based integer index.  Examples:
>
> ​        :CFBundleShortVersionString
>
> ​        :CFBundleDocumentTypes:2:CFBundleTypeExtensions
>
> 
>
> Types:
>
> ​    string
>
> ​    array
>
> ​    dict
>
> ​    bool
>
> ​    real
>
> ​    integer
>
> ​    date
>
> ​    data
>
> 
>
> Examples:
>
> ​    Set :CFBundleIdentifier com.apple.plistbuddy
>
> ​        Sets the CFBundleIdentifier property to com.apple.plistbuddy
>
> ​    Add :CFBundleGetInfoString string "App version 1.0.1"
>
> ​        Adds the CFBundleGetInfoString property to the plist
>
> ​    Add :CFBundleDocumentTypes: dict
>
> ​        Adds a new item of type dict to the CFBundleDocumentTypes array
>
> ​    Add :CFBundleDocumentTypes:0 dict
>
> ​        Adds the new item to the beginning of the array
>
> ​    Delete :CFBundleDocumentTypes:0 dict
>
> ​        Deletes the FIRST item in the array
>
> ​    Delete :CFBundleDocumentTypes
>
> ​        Deletes the ENTIRE CFBundleDocumentTypes array

上面是官方的文档, 下面我们来简要来说明下部分用法

假设是如下结构, 使用 swift 的字典表述法, 真实的数据是和 xml 结构类似

```swift
let info = [
        "Setting": [
            "resolution": "4K",
            "necessary": true,
        ],
        "VersionKey": "1.0",
        "FruitPackage": [
          	"weight": 10,
          	"items": [
            		"apple",
            		"banana",
            		"coconut"
          	]
        ]
    ]
```

### How to get?

```bash
PlistBuddy -c "Print :FruitPackage:items:1" example.plist

# 结果是 banana
```

### How to set?

```bash
PlistBuddy -c "Set :FruitPackage:weight 20" example.plist

# 将水果包装的重量设置为 20
```

### How to delete?

```bash
PlistBuddy -c "Delete :FruitPackage:weight" example.plist

# 将水果包装的重量设置为 20
```

## 总结

其实根据基础文档我们就可以了解到: 

### 基础命令是

 `PlistBuddy -c "<Command> :<Key>:<Key or Subscript> <Value>" example.plist`

### 替换掉 `Command`: 

- Clear [\<Type>] - Clears out all existing entries, and creates root of Type
- Print [\<Entry>] - Prints value of Entry.  Otherwise, prints file
- Set \<Entry> \<Value> - Sets the value at Entry to Value
- Add \<Entry> \<Type> [\<Value>] - Adds Entry to the plist, with value Value
- Copy \<EntrySrc> <\EntryDst> - Copies the EntrySrc property to EntryDst
- Delete \<Entry> - Deletes Entry from the plist
- Merge <file.plist> [\<Entry>] - Adds the contents of file.plist to Entry
- Import \<Entry> \<file> - Creates or sets Entry the contents of file

*注意: [ ] 为可选参数, \< > 为需要自行替换的内容*

### 替换掉 `Key or Subscript`, 也就是上面命令里的 `Entry`: 

Key 和 游标[数组的第几个元素] 用 `:` 分割开, 不带空格.

### 替换掉 `Value`: 

这是一个可选项, 一些命令是没有的.

### 替换掉 Type:

| Type    | Description |
| ------- | ----------- |
| string  | 字符串      |
| array   | 数组        |
| dict    | 字典        |
| bool    | 布尔值      |
| real    | 有理数      |
| integer | 数字        |
| date    | 时间        |
| data    | 二进制数据  |

