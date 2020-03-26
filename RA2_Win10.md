---
title: Origin红警2（含尤里的复仇）的Win10补丁和汉化（简体中文版）
---

[TOC]

# Origin红警2（含尤里的复仇）的Win10补丁和汉化（简体中文版）- Win10 黑屏补丁

## 🧻 将下载的补丁文件夹下所有文件复制并替换 

- [游戏黑屏补丁(针对显卡渲染)](RA2_Win10/仅Win10补丁不含汉化.zip)
- [简体汉化资源文件 - RA2](RA2_Win10/RA2汉化.zip)
- [简体汉化资源文件 - Yuri's Revenge](RA2_Win10/尤里的复仇汉化.zip)
- [DX 的 3D 渲染关闭与开启的注册表文件](RA2_Win10/黑屏备用.zip)

## 🔧 修改 exe 可执行程序的兼容性属性

就是这 5 个货!

![5exe](RA2_Win10/5exe.jpg)

分别打开没一个程序的兼容性设置, 修改如下选项

- Compatibiity mode - **XP SP3**
- Reduced color mode - **16-bit**
- Disable fullscreen optiomizations
- Change high DPI settings - **Override high DPI scaling behavior, Scaling performed by: Application**

![Compatibility.png](RA2_Win10/Compatibility.png)

## 🎉 完结撒花

终于!!!👏

- 可以切屏
- 不卡顿
- 不黑屏
- 点击选项不卡死
- 支持高分辨率

> 小瑕疵：主界面切屏会丢失按钮名称（遭遇战界面切屏不会丢失按钮，但是点击遭遇战进入后会丢失界面），可以点击。并且点一下就恢复正常（推荐点击最底部的按钮）

## PS：

1. 补丁中的黑屏备用请看食用说明，不要随意使用。

2.  测试中独显1080TI不用替换应用程序运行也不卡顿，核显在运行尤里的复仇会卡顿 

3. 补丁中的应用程序是原版程序，使用了改渲染的工具修改了红警的渲染方式，

   取自国外大神 原链接：http://www.stuffhost.de/files/cnc/CnCPatcher.html

>  本文内容经过整理, 部分参考原始作者：[鲁汀LT](https://www.bilibili.com/read/cv357537)