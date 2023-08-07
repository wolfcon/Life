---
title: macOS 基础的使用配置
---

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**

- [macOS 基础的使用配置](#macos-基础的使用配置)
  - [⚠️❗️Finder 显示隐藏文件](#️️finder-显示隐藏文件)
    - [Terminal](#terminal)
    - [Keyboard shortcuts](#keyboard-shortcuts)
  - [⚠️❗️关闭 Gatekeeper - 允许任意来源的 Application](#️️关闭-gatekeeper---允许任意来源的-application)
  - [制作安装镜像 - createinstallmedia](#制作安装镜像---createinstallmedia)
    - [制作 iso](#制作-iso)
  - [⚠️‼️关闭 SIP (System Integrity Protection)](#️️关闭-sip-system-integrity-protection)
    - [查询 SIP 状态](#查询-sip-状态)
    - [开启/关闭](#开启关闭)
  - [⚠️‼️重置部署状态, 用于新建账户](#️️重置部署状态-用于新建账户)
  - [修改 Hostname](#修改-hostname)

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

## ⚠️‼️重置部署状态, 用于新建账户

前提 - **需要先重启进入恢复模式**: `⌘` + `R`

```bash
rm /Volumes/Macintosh\ HD/var/db/.AppleSetupDone
reboot
```

## 修改 Hostname

```bash
sudo hostname myownhostname
sudo scutil --set LocalHostName $(hostname)
sudo scutil --set HostName $(hostname)
```