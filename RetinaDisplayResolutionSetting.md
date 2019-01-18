---
title: Retina Display Resolution Setting
---



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

[在线工具](https://comsysto.github.io/Display-Override-PropertyList-File-Parser-and-Generator-with-HiDPI-Support-For-Scaled-Resolutions/)
修改左侧的 DisplayProductID 和 DisplayVendorID 数字填上面生成的

```
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

### 3a. 自己手动加

请下载 `PlistEdit Pro` 或者 Xcode 打开 `plist` 文件，展开 `scale-resolutions`，创建新值，类型为 `Data`，值有四组8位的十六进制，每组计算的结果不足八位在前面补 `0`，以下使用 `1600x900` 进行演示

$1600 * 2 = 3200$ 由十进制转换成十六进制得出 `0xc80`, $900 * 2 = 1800$ 由十进制转换成十六进制得出 `0x708`, 注意16进制这里大小写不敏感, `前2组是分辨率`, 后面照抄

该分辨率对应的值为：`<00000c80 00000708 00000001 00200000>`

### 4. 下载plist并复制到系统文件夹

配置好后点击下载
运行指令复制到到 /System/Library/Displays/Contents/Resources/Overrides/

> 10.10 版本及以下为 /System/Library/Displays/Overrides/
>
```shell
sudo cp ~/Downloads/DisplayProductID-d067.plist /System/Library/Displays/Contents/Resources/Overrides/DisplayVendorID-10ac/DisplayProductID-d067
```
把里面的 ID **DisplayProductID-{{$}} **和 **DisplayVendorID-{{$}}** 换成上面自己生成的

配置结构是这样的

```
./DisplayVendorID-10ac
└── DisplayProductID-d067
```

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

### 5. 重启 使用RDM切换分辨率

下载 [RDM](http://avi.alkalay.net/software/RDM/) 重启后运行RDM，在任务栏中找到RDM的logo，点开来切换分辨率。带有⚡️标识的为HiDPI分辨率.