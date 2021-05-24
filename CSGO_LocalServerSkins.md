<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**

- [CSGO 架设本地服务器 - 专用检视各类皮肤](#csgo-%E6%9E%B6%E8%AE%BE%E6%9C%AC%E5%9C%B0%E6%9C%8D%E5%8A%A1%E5%99%A8---%E4%B8%93%E7%94%A8%E6%A3%80%E8%A7%86%E5%90%84%E7%B1%BB%E7%9A%AE%E8%82%A4)
  - [本文适用于](#%E6%9C%AC%E6%96%87%E9%80%82%E7%94%A8%E4%BA%8E)
  - [本文不适用于](#%E6%9C%AC%E6%96%87%E4%B8%8D%E9%80%82%E7%94%A8%E4%BA%8E)
    - [为什么?](#%E4%B8%BA%E4%BB%80%E4%B9%88)
    - [结论是](#%E7%BB%93%E8%AE%BA%E6%98%AF)
  - [插件平台安装步骤：](#%E6%8F%92%E4%BB%B6%E5%B9%B3%E5%8F%B0%E5%AE%89%E8%A3%85%E6%AD%A5%E9%AA%A4)
    - [补充一点 !!!](#%E8%A1%A5%E5%85%85%E4%B8%80%E7%82%B9-)
    - [创建服务器](#%E5%88%9B%E5%BB%BA%E6%9C%8D%E5%8A%A1%E5%99%A8)
    - [加入服务器](#%E5%8A%A0%E5%85%A5%E6%9C%8D%E5%8A%A1%E5%99%A8)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

[TOC]

# CSGO 架设本地服务器 - 专用检视各类皮肤

## 本文适用于

1. 单机打 BOT。纯自己 YY 一下自己买不到的武器、刀具、手套的各种皮肤(可作为购买前的参考);
2. 建立局域网服务器，和你的局域网内小伙伴一起爽一下自己没有的皮肤，向当初 CS1.6 一样，在一个局域网内都是可以玩的 ~~(整栋宿舍楼走起)~~。

## 本文不适用于

在任何公开服务器里使用自己没有的各种皮肤，包括官方服务器和各大对战平台。

### 为什么?

除了极个别的服务器本身安装了此类插件，问题是这类插件是被 Vavle `禁止`的，下面详细说明这个禁止内容。

CSGO 服务器有两类，一种是本地服务器，Vavle 没有任何约束；另一种是公网服务器，就是你在游戏里选择——社区服务器，里面可以浏览到的; 或者其他的可以通过公网 IP 连接的服务器。第二类服务器架设需要到 Vavle 那里用你的 Steam 账号注册一个 `GSLT(Game Server Login Token)` 的东西才可以上线。而 Vavle 的公网服务器协议禁止使用任何改变用户库存的插件(要不G胖怎么赚钱啊？)，如有违反，BAN 掉你的 GSLT，你的账号再也不能开设任何公网服务器了。同时你的账号也会被 CSGO 游戏封禁11天左右。

但是，社区服务器仍然存在大量的违规服务器，以毛子居多，国内我见过一个，但是需要充 VIP 才能使用....反正这些都不是重点啦。只是给你们交代一下这个东西会不会导致你账号被 BAN 打消疑虑。

### 结论是

本文说的是建立本地或者局域网内服务器，压根跟公网服务器没关系。所以不会被 BAN.

上张图压压惊 ~~众所周知，龙狙是不存在带计数版本的~~

![img](CSGO_LocalServerSkins/dragonLore.png)

此插件可以免费使用任何 CSGO 游戏内所有的皮肤，包含抢、刀、手套，随意搭配计数器版本与否和磨损度；磨损度以 `5%` 为步进随意调节；名字标签。

## 插件平台安装步骤：

- 下载 `Metamod`(注意系统类型)：<http://www.sourcemm.net/downloads.php?branch=stable>

  - [mmsource-1.10.7-git971-windows.zip](CSGO_LocalServerSkins/mmsource-1.10.7-git971-windows.zip)

- 制作 CSGO 专属 VDF：<http://www.sourcemm.net/vdf> 

  >Game：Counter Strike:Global Offensive
  
  >Game folder：为空
  
  >点击Generate metamod.vdf

  - [metamod.vdf](CSGO_LocalServerSkins/metamod.vdf)

- 下载 `Sourcemod` (注意系统类型): <http://www.sourcemod.net/downloads.php?branch=stable>

  - [sourcemod-1.10.0-git6492-windows.zip](CSGO_LocalServerSkins/sourcemod-1.10.0-git6492-windows.zip)

- 将刚才下载的两个压缩包解压到同一目录下

- 最后将刚才下载的 `metamod.vdf` 文件放到 `addons/` 文件夹下，覆盖原文件即可

- 将所得到的 `addons/`, `cfg/` 这两个文件夹复制到游戏服务器的 `csgo/` 文件夹下即可完成插件平台的安装

- 然后下载这个附件一样解压到上述文件夹， <https://ptah.zizt.ru/> 下载最新的。

  - [PTaH-V1.1.2-build18-windows.zip](CSGO_LocalServerSkins/PTaH-V1.1.2-build18-windows.zip)

- 然后去下载 [https://github.com/kaganus/weapons/releases/](https://github.com/kaganus/weapons/releases/) 和 [https://github.com/kaganus/gloves/releases](https://github.com/kaganus/gloves/releases)，点source code(zip)下载。下载完毕以后依旧解压缩到上述文件夹。
  - [weapons-v1.7.0.zip](CSGO_LocalServerSkins/weapons-v1.7.0.zip)
  - [gloves-v1.0.4.zip](CSGO_LocalServerSkins/gloves-v1.0.4.zip)

- 修改 `addons/sourcemod/configs/core.cfg` 文件，找到 `"FollowCSGOServerGuidelines" "yes"`，把 `yes` 改成 `no` 之后保存。

### 补充一点 !!!

解压文件夹目录并不是 `csgo.exe` 所在的文件夹，可以看这张图，注意解压以后 `addons` 和 `cfg` 的要跟图中的一样。

![img](CSGO_LocalServerSkins/file_path.png)

以上文件下载请自行杀毒，我不保证他们的安全性，反正也没啥可执行文件。安装就此完成。下面是启动本地服务器。

### 创建服务器

请在 CSGO 的启动项里加一条 `-insecure`，用了这条之后，你不能加入任何启用了 VAC 反作弊的服务器。但是不用这个参数，这个插件不能启动，因为这个插件本身是设计给独立CS服务器用的(独立 CS 服务器建立游戏压根不需要开启 CSGO 这个游戏，有另一个专门的程序建立，CS1.6也一样)。打其他比如官服或者平台服之前记得去掉这个启动参数，否则没法进入。

如果是局域网联机，请在进入游戏主界面之后，输入 `map 地图名` 新建游戏

### 加入服务器

看一眼你的本地网卡ip地址，告诉你的小伙伴，让他们用 `connect ip` 连你的服务器。此种方法我没有机会测试，不知道是否需求同第一种一样需使用 `-insecure` 参数，也不知道是否可以通过游戏内的社区服务器-局域网游戏来查找加入。

进游戏里，按 `y` 或者 `u` 以后，输入 `!ws` 或者 `!knife` 或者 `!gloves` 弹出选择菜单。此处注意!是英文状态下的感叹号。下同。默认为英语，内置有汉语，但是仅限于皮肤名称，改不改随你(输入!ws以后有选语言的，schinese 是简体中文)我懒得介绍了，很简单一看就会的简单单词。

Enjoy！

> 参考原文链接 <http://ngabbs.com/read.php?&tid=15885087>
