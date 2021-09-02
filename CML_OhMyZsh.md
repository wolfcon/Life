---
title: Oh My Zsh 的花式配置
---

# Oh My Zsh 的花式配置

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**

- [Oh My Zsh 的花式配置](#oh-my-zsh-%E7%9A%84%E8%8A%B1%E5%BC%8F%E9%85%8D%E7%BD%AE)
  - [🔨 安装](#🔨 安装)
  - [🌠 主题](#🌠 主题)
  - [其他配置](#%E5%85%B6%E4%BB%96%E9%85%8D%E7%BD%AE)
- [好了, 我们开始搞事情](#%E5%A5%BD%E4%BA%86-%E6%88%91%E4%BB%AC%E5%BC%80%E5%A7%8B%E6%90%9E%E4%BA%8B%E6%83%85)
  - [插件](#%E6%8F%92%E4%BB%B6)
    - [将插件安装在安装目录下的 plugins 下](#%E5%B0%86%E6%8F%92%E4%BB%B6%E5%AE%89%E8%A3%85%E5%9C%A8%E5%AE%89%E8%A3%85%E7%9B%AE%E5%BD%95%E4%B8%8B%E7%9A%84-plugins-%E4%B8%8B)
    - [🛠 配置插件](#%F0%9F%9B%A0-%E9%85%8D%E7%BD%AE%E6%8F%92%E4%BB%B6)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## 🔨 安装

从 [Github](https://github.com/ohmyzsh/ohmyzsh)

从官网 [https://ohmyz.sh/]

## 🌠 主题

自己从 `themes` 里选一个自己喜欢吧🤣

我推荐的一个是 [zsh2000](https://github.com/maverick9000/zsh2000), 是类 powerline 样式的

## 其他配置

[参照官方文档](https://github.com/ohmyzsh/ohmyzsh/wiki/Settings)

# 好了, 我们开始搞事情

## 插件

- [dogsay](https://github.com/txstc55/dogesay) - 打每个命令后都会有个狗头!

  ![狗头](https://raw.githubusercontent.com/txstc55/dogesay/master/dogesay.gif)

其中需要 2 个东西: [lolcat - 给狗头上色](https://github.com/jaseg/lolcat) 和 [figlet - 给狗头说的话变成泡泡字](https://github.com/cmatsuoka/figlet)

### 将插件安装在安装目录下的 plugins 下

目录一般在 `~/.oh-my-zsh/plugins` 

### 🛠 配置插件

打开 `~/.zshrc` 在 `plugins=(git dogesay)` 中添加插件名, 这里我添加了 2 个插件 `git` 和 `dogesay` 

🎉大功告成.