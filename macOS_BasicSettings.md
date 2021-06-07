---
title: macOS 基础的使用配置
---

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**

- [macOS 基础的使用配置](#macos-%E5%9F%BA%E7%A1%80%E7%9A%84%E4%BD%BF%E7%94%A8%E9%85%8D%E7%BD%AE)
  - [⚠️❗️Finder 显示隐藏文件](#%EF%B8%8Ffinder-%E6%98%BE%E7%A4%BA%E9%9A%90%E8%97%8F%E6%96%87%E4%BB%B6)
    - [Terminal](#terminal)
    - [Keyboard shortcuts](#keyboard-shortcuts)
  - [⚠️❗️关闭 Gatekeeper - 允许任意来源的 Application](#%EF%B8%8F%E5%85%B3%E9%97%AD-gatekeeper---%E5%85%81%E8%AE%B8%E4%BB%BB%E6%84%8F%E6%9D%A5%E6%BA%90%E7%9A%84-application)
  - [制作安装镜像 - createinstallmedia](#%E5%88%B6%E4%BD%9C%E5%AE%89%E8%A3%85%E9%95%9C%E5%83%8F---createinstallmedia)
    - [制作 iso](#%E5%88%B6%E4%BD%9C-iso)
  - [⚠️‼️关闭 SIP (System Integrity Protection)](#%E5%85%B3%E9%97%AD-sip-system-integrity-protection)
    - [查询 SIP 状态](#%E6%9F%A5%E8%AF%A2-sip-%E7%8A%B6%E6%80%81)
    - [开启/关闭](#%E5%BC%80%E5%90%AF%E5%85%B3%E9%97%AD)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# macOS 基础的使用配置

[TOC]

## ⚠️❗️Finder 显示隐藏文件
### Terminal
```bash
defaults write com.apple.finder AppleShowAllFiles -bool true
```
### Keyboard shortcuts
或者使用键盘快捷键 `command(⌘)` + `shift` + `.`

## ⚠️❗️关闭 Gatekeeper - 允许任意来源的 Application

```bash
sudo spctl --master-disable
```

## 制作安装镜像 - createinstallmedia

```bash
sudo /Applications/${Install.app}/Contents/Resources/createinstallmedia --volume /Volumes/${MyVolume} /Applications/${Install.app} --downloadassets --nointeraction
```

### 制作 iso

```bash
hdiutil create -o ./temp -size 8000m -layout SPUD -fs HFS+J
hdiutil attach ./temp.dmg -noverify -mountpoint /Volumes/install_build
sudo /Applications/Install\ macOS\ Catalina.app/Contents/Resources/createinstallmedia  --volume /Volumes/install_build/
hdiutil detach /Volumes/Install\ macOS\ Catalina/
hdiutil convert temp.dmg -format UDTO -o macOSCatalina
mv macOSCatalina.cdr macOSCatalina.iso
```

## ⚠️‼️关闭 SIP (System Integrity Protection)

### 查询 SIP 状态

```bash
csrutil status
```

### 开启/关闭

前提 - **需要先重启进入恢复模式**: `⌘` + `R`

```bash
csrutil enable
csrutil disable
```

