---
title: Retina Display Resolution Setting
---

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**

- [Retina Display Resolution Setting](#retina-display-resolution-setting)
  - [开启HiDPI分辨率的方法有3种](#%E5%BC%80%E5%90%AFhidpi%E5%88%86%E8%BE%A8%E7%8E%87%E7%9A%84%E6%96%B9%E6%B3%95%E6%9C%893%E7%A7%8D)
  - [详细步骤](#%E8%AF%A6%E7%BB%86%E6%AD%A5%E9%AA%A4)
    - [1. 启用HiDPI模式](#1-%E5%90%AF%E7%94%A8hidpi%E6%A8%A1%E5%BC%8F)
    - [2. 检测你的显示器](#2-%E6%A3%80%E6%B5%8B%E4%BD%A0%E7%9A%84%E6%98%BE%E7%A4%BA%E5%99%A8)
    - [3. 自定义配置文件](#3-%E8%87%AA%E5%AE%9A%E4%B9%89%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6)
      - [在线工具](#%E5%9C%A8%E7%BA%BF%E5%B7%A5%E5%85%B7)
      - [自己手动加](#%E8%87%AA%E5%B7%B1%E6%89%8B%E5%8A%A8%E5%8A%A0)
    - [4. 下载plist并复制到系统文件夹](#4-%E4%B8%8B%E8%BD%BDplist%E5%B9%B6%E5%A4%8D%E5%88%B6%E5%88%B0%E7%B3%BB%E7%BB%9F%E6%96%87%E4%BB%B6%E5%A4%B9)
      - [10.14 以下 (`High Sierra`)](#1014-%E4%BB%A5%E4%B8%8B-high-sierra)
      - [10.14 以上(``Mojave` & `Catalina` ...)](#1014-%E4%BB%A5%E4%B8%8Amojave--catalina-)
    - [5. 重启 使用RDM切换分辨率](#5-%E9%87%8D%E5%90%AF-%E4%BD%BF%E7%94%A8rdm%E5%88%87%E6%8D%A2%E5%88%86%E8%BE%A8%E7%8E%87)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->





[TOC]

# Retina Display Resolution Setting

## 开启HiDPI分辨率的方法有3种

1. 使用软件  **SwitchResX**
2. 使用自动脚本  [**Enable-HiDPI-OSX**](https://github.com/syscl/Enable-HiDPI-OSX)
3. 手工替换配置文件

第一种方法, 软件要付费, 卸载麻烦
第二第三的原理都是一样的, 就是手工替换配置文件

## 详细步骤

### 1. 启用HiDPI模式

```
sudo defaults write /Library/Preferences/com.apple.windowserver.plist DisplayResolutionEnabled -bool true
```

### 2. 检测你的显示器

`ioreg -l | grep "DisplayProductID" && ioreg -l | grep "DisplayVendorID"`
结果很有可能有很多值, 这里只需要拔掉🖥, 排除法, 就得到值了

```
    | |   |   | |   |       "DisplayProductID" = 41009
    | |   |   | |   |       "DisplayProductID" = 53351
    | |   |   | |   |       "DisplayVendorID" = 1552
    | |   |   | |   |       "DisplayVendorID" = 4268
```

### 3. 自定义配置文件

#### [在线工具](https://comsysto.github.io/Display-Override-PropertyList-File-Parser-and-Generator-with-HiDPI-Support-For-Scaled-Resolutions/)

修改左侧的 DisplayProductID 和 DisplayVendorID 数字填上面生成的

```xml
  <key>DisplayProductID</key>
  <integer>53351</integer>
  <key>DisplayVendorID</key>
  <integer>4268</integer>
```

右侧生成对应的16进制数,是为要使用的文件名

```
DisplayProductID  0xD067 = 53351
DisplayVendorID	  0x10AC = 4268
```

添加想要的分辨率即可

#### 自己手动加

请下载 `PlistEdit Pro` 或者 Xcode 打开 `plist` 文件，展开 `scale-resolutions`，创建新值，类型为 `Data`，值有四组8位的十六进制，每组计算的结果不足八位在前面补 `0`，以下使用 `1600x900` 进行演示

$1600 * 2 = 3200$ 由十进制转换成十六进制得出 `0xc80`, $900 * 2 = 1800$ 由十进制转换成十六进制得出 `0x708`, 注意16进制这里大小写不敏感, `前2组是分辨率`, 后面照抄

该分辨率对应的值为：`<00000c80 00000708 00000001 00200000>`

> 这里需要注意, 如果加的是 `1600x800` 的分辨率. 要添加 `HiDPI` 版本的分辨率. 则需要再添加一个 `3200x1600` 分辨率选项, 否则系统将不会识别 `1600x800` 的 `HiDPI` 分辨率

### 4. 下载plist并复制到系统文件夹

配置好后点击下载
运行指令复制到到 /System/Library/Displays/Contents/Resources/Overrides/

> 10.10 版本及以下为 /System/Library/Displays/Overrides/
```shell
sudo cp ~/Downloads/DisplayProductID-d067.plist /System/Library/Displays/Contents/Resources/Overrides/DisplayVendorID-10ac/DisplayProductID-d067
```
把里面的 ID **DisplayProductID-{{$}} **和 **DisplayVendorID-{{$}}** 换成上面自己生成的

配置结构是这样的

```
./DisplayVendorID-10ac
└── DisplayProductID-d067
```

#### 10.14 以下 (`High Sierra`)

> 一般情况下, 需要禁用 SIP 系统完整保护系统. 🚫

```shell
#关闭 SIP
csrutil disable
reboot

#开启 SIP
csrutil enable
reboot

#查看自己 mac 是否开启(默认开启.)
csrutil status
```

> 这里注意关闭后, 设置完成记得再打开哟. 保护系统文件完整性. 😆

#### 10.14 以上(``Mojave` & `Catalina` ...)

由于加入了保护盘系统. (`Catalina` 更加过, 直接变为 2 个分区, 一个系统区, 一个数据区), 我们再也修改不了系统文件了.

**但是**, 办法还是有的. 

1. 保存好你的配置文件, 记住位置
2. 重启
3. 按住 `cmd(⌘) + R`, 直到进入 `Recovery Mode`
4. 使用 `DiskUtility` 挂载系统盘 (如果使用了加密盘, 可能需要输入密码)
5. 再打开 `Terminal`, 把你保存好的配置文件复制过去吧🤪
6. 复制完之后 `reboot`, 大功告成

### 5. 重启 使用RDM切换分辨率

下载 [RDM](http://avi.alkalay.net/software/RDM/) 重启后运行RDM，在任务栏中找到RDM的logo，点开来切换分辨率。带有⚡️标识的为HiDPI分辨率.
