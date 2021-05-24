<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**

- [WoW 基本设置命令](#wow-%E5%9F%BA%E6%9C%AC%E8%AE%BE%E7%BD%AE%E5%91%BD%E4%BB%A4)
  - [控制台](#%E6%8E%A7%E5%88%B6%E5%8F%B0)
    - [输出 Interface code / art 文件](#%E8%BE%93%E5%87%BA-interface-code--art-%E6%96%87%E4%BB%B6)
  - [游戏参数](#%E6%B8%B8%E6%88%8F%E5%8F%82%E6%95%B0)
    - [全屏泛光](#%E5%85%A8%E5%B1%8F%E6%B3%9B%E5%85%89)
    - [死亡灰色特效](#%E6%AD%BB%E4%BA%A1%E7%81%B0%E8%89%B2%E7%89%B9%E6%95%88)
    - [姓名板距离参数](#%E5%A7%93%E5%90%8D%E6%9D%BF%E8%B7%9D%E7%A6%BB%E5%8F%82%E6%95%B0)
    - [反和谐](#%E5%8F%8D%E5%92%8C%E8%B0%90)
      - [和谐血肉](#%E5%92%8C%E8%B0%90%E8%A1%80%E8%82%89)
      - [和谐图标](#%E5%92%8C%E8%B0%90%E5%9B%BE%E6%A0%87)
      - [和谐文字](#%E5%92%8C%E8%B0%90%E6%96%87%E5%AD%97)
    - [装备自动比较](#%E8%A3%85%E5%A4%87%E8%87%AA%E5%8A%A8%E6%AF%94%E8%BE%83)
    - [修改 tab 选怪的距离 (0-50)](#%E4%BF%AE%E6%94%B9-tab-%E9%80%89%E6%80%AA%E7%9A%84%E8%B7%9D%E7%A6%BB-0-50)
      - [前方](#%E5%89%8D%E6%96%B9)
      - [后方](#%E5%90%8E%E6%96%B9)
    - [设置血液效果等级](#%E8%AE%BE%E7%BD%AE%E8%A1%80%E6%B6%B2%E6%95%88%E6%9E%9C%E7%AD%89%E7%BA%A7)
    - [聊天窗口显示职业颜色](#%E8%81%8A%E5%A4%A9%E7%AA%97%E5%8F%A3%E6%98%BE%E7%A4%BA%E8%81%8C%E4%B8%9A%E9%A2%9C%E8%89%B2)
    - [姓名板显示职业颜色](#%E5%A7%93%E5%90%8D%E6%9D%BF%E6%98%BE%E7%A4%BA%E8%81%8C%E4%B8%9A%E9%A2%9C%E8%89%B2)
      - [友方](#%E5%8F%8B%E6%96%B9)
      - [敌方](#%E6%95%8C%E6%96%B9)
    - [副本重置](#%E5%89%AF%E6%9C%AC%E9%87%8D%E7%BD%AE)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->



# WoW 基本设置命令

[TOC]

## 控制台

在 WoW 快捷方式后加入 `-console`

即可在游戏中开启控制台 (类似 CSGO 的控制台), 在游戏中按 `~` 即可

### 输出 Interface code / art 文件

在控制台中输入

- `exportInterfaceFiles code`
- `exportInterfaceFiles art`

注意 Art 文件可能会比较大.

## 游戏参数

非特殊说明, 命令均是在聊天框输入

### 全屏泛光

聊天框输入

```
/console ffxGlow 0
```

### 死亡灰色特效

聊天框输入

```
/console ffxDeath 0
```

### 姓名板距离参数

聊天框输入

```
/run SetCVar("nameplateMaxDistance", "6e1")
```

### 反和谐

#### 和谐血肉

聊天框输入

```
/console SET overrideArchive "0"
```

#### 和谐图标

解压至 `${World of Warcraft Classic}\Interface` :

 [ICON_Anti.zip](WoW_SettingCommands/ICON_Anti.zip) 

#### 和谐文字

聊天框输入

```
/console SET profanityFilter "0"
```

### 装备自动比较

```
/run SetCVar("alwaysCompareItems", "1")
```

### 修改 tab 选怪的距离 (0-50)

#### 前方

```
/console SET targetNearestDistance "50"
```

#### 后方

```
/console SET targetNearestDistanceRadius "50"
```

### 设置血液效果等级

```
/console violenceLevel 2
```

暴力等级: 0-5 (default: 2)

### 聊天窗口显示职业颜色

```
/console SET chatClassColorOverride "0"
```

### 姓名板显示职业颜色

#### 友方

```
/console ShowClassColorInFriendlyNameplate 1
```

#### 敌方

```
/console ShowClassColorInNameplate 1
```

### 副本重置

```
/run ResetInstances()
```

