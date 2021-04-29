---
title: macOS 基础的使用配置
---

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

