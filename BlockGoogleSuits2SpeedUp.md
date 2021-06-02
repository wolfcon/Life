---
title: 屏蔽 Google API / Recaptcha / Analytics 等以加快网站访问速度
toc: true
---

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**

- [屏蔽 Google API / Recaptcha / Analytics 等以加快网站访问速度](#屏蔽-google-api--recaptcha--analytics-等以加快网站访问速度)
  - [Chrome & Edge](#chrome--edge)
    - [利用 AdblockPlus](#利用-adblockplus)
      - [添加一下屏蔽源, 用于更加广阔的反 adb 的屏蔽](#添加一下屏蔽源-用于更加广阔的反-adb-的屏蔽)
      - [自定义过滤器](#自定义过滤器)
  - [Firefox](#firefox)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->


# 屏蔽 Google API / Recaptcha / Analytics 等以加快网站访问速度

[TOC]

## Chrome & Edge

### 利用 AdblockPlus

#### 添加一下屏蔽源, 用于更加广阔的反 adb 的屏蔽

https://easylist-downloads.adblockplus.org/antiadblockfilters.txt

#### 自定义过滤器

添加以下屏蔽关键字, 直接复制粘贴即可.🎉

分别是

- Twitter Widgets(仅在 hltv.org 的 Domain 下)
- hltv 带有 bet 链接的标签
- hltv 带有 geo class 的标签
- bilibili 带有 pop-live class 的标签
- hltv 顶部菠菜标签
- Google 统计
- Google 人机认证
- Google API

```
platform.twitter.com/widgets.js$domain=hltv.org
hltv.org##[href*="bet"]
hltv.org##[class*="geo"]
bilibili.com##[class*="pop-live"]
hltv.org##[class*="featured-matches-top"]
||google-analytics.com/
||www.google.com/recaptcha/
||apis.google.com/
```

如果需要使用 `Google` 的 `API` 以及 `人机认证` 的话, 暂时禁用 `Ablock` 即可

## Firefox

待填坑. 🤐已经弃用 FF 有 2 年之久
