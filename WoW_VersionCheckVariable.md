<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**

- [魔兽世界插件开发之版本判断](#%E9%AD%94%E5%85%BD%E4%B8%96%E7%95%8C%E6%8F%92%E4%BB%B6%E5%BC%80%E5%8F%91%E4%B9%8B%E7%89%88%E6%9C%AC%E5%88%A4%E6%96%AD)
  - [历史](#%E5%8E%86%E5%8F%B2)
  - [方案](#%E6%96%B9%E6%A1%88)
    - [分 2 个分支来进行开发](#%E5%88%86-2-%E4%B8%AA%E5%88%86%E6%94%AF%E6%9D%A5%E8%BF%9B%E8%A1%8C%E5%BC%80%E5%8F%91)
    - [采用一套代码](#%E9%87%87%E7%94%A8%E4%B8%80%E5%A5%97%E4%BB%A3%E7%A0%81)
  - [WOW_PROJECT_ID](#wow_project_id)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

---
title: 魔兽世界插件开发之版本判断
toc: true
---

# 魔兽世界插件开发之版本判断

[TOC]

## 历史

最近, 魔兽世界开放了经典怀旧服, 热度突然飙升, 虽然怀旧服是在 8.0 的版本上进行的移植, 但是由于需要 `怀旧`, 所以非常多的 `API` 与正式版本不同, 都进行了不同程度的禁止与限制. 由此, 插件的开发让人很迷惑, 到底是采用同一套代码, 还是分 2 个分支单独维护呢?

## 方案

### 分 2 个分支来进行开发

这个没什么好说的, 分成 2 套代码, 分支代码自然不存在, 一删了之.

缺点也很明显, 维护 2 套代码的成本有点多.

### 采用一套代码

这个就是我们今天讲的重头戏, 由于插件接口与正式服类似, 则对应封掉的 `API` 或 `不存在的功能`, 我们只需要通过分支代码区别对待即可. 相信如果进行过 iOS / Android 或者其他多平台/版本适配的开发者对此不会陌生. 那么就祭出我们的今天的头牌!!!

## [WOW_PROJECT_ID](https://wow.gamepedia.com/WOW_PROJECT_ID)

```lua
-- 在怀旧服 API 代码 BNet.lua 中, 玻璃渣给出的定义
WOW_PROJECT_MAINLINE = 1;
WOW_PROJECT_CLASSIC = 2;
WOW_PROJECT_ID = WOW_PROJECT_CLASSIC;
```

其他我们在写插件时该如何做, 相信我不必多说了把哈哈哈.🤪

