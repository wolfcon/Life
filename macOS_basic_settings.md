---
title: macOS 基础的使用配置
---

# macOS 基础的使用配置

[TOC]

## ⚠️❗️Finder 显示隐藏文件

```bash
defaults write com.apple.finder AppleShowAllFiles -bool true
```

## ⚠️❗️关闭 Gatekeeper - 允许任意来源的 Application

```bash
sudo spctl --master-disable
```

## 制作安装镜像 - createinstallmedia

```bash
sudo /Applications/${Install.app}/Contents/Resources/createinstallmedia --volume /Volumes/${MyVolume} --applicationpath /Applications/${Install.app}
```

## ⚠️‼️关闭 SIP (System Integrity Protection)

### 查询 SIP 状态

```bash
csrutil status
```

### 开启/关闭

```bash
csrutil enable
csrutil disable
```

