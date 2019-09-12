---
title: WoW 基本设置命令
---

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

