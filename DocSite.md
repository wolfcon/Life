<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**

- [文档部署系统 MkDocs](#%E6%96%87%E6%A1%A3%E9%83%A8%E7%BD%B2%E7%B3%BB%E7%BB%9F-mkdocs)
  - [官网](#%E5%AE%98%E7%BD%91)
  - [运行机制](#%E8%BF%90%E8%A1%8C%E6%9C%BA%E5%88%B6)
  - [简单部署方式](#%E7%AE%80%E5%8D%95%E9%83%A8%E7%BD%B2%E6%96%B9%E5%BC%8F)
    - [macOS](#macos)
      - [Homebrew](#homebrew)
      - [手动](#%E6%89%8B%E5%8A%A8)
    - [Windows - 待填坑](#windows---%E5%BE%85%E5%A1%AB%E5%9D%91)
    - [Linux - 待填坑](#linux---%E5%BE%85%E5%A1%AB%E5%9D%91)
    - [官方介绍](#%E5%AE%98%E6%96%B9%E4%BB%8B%E7%BB%8D)
  - [使用](#%E4%BD%BF%E7%94%A8)
    - [index.md 是首页](#indexmd-%E6%98%AF%E9%A6%96%E9%A1%B5)
    - [配置](#%E9%85%8D%E7%BD%AE)
      - [位置](#%E4%BD%8D%E7%BD%AE)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# 文档部署系统 MkDocs

## 官网

[官方网址](https://www.mkdocs.org)

[github](https://github.com/mkdocs/mkdocs/)

## 运行机制

MarkDown 文档渲染成 HTML, 並且自动生成目录, 可根据 nav 来配置导航

## 简单部署方式

### macOS

#### Homebrew

```sh
brew install MkDocs
```

#### 手动

1. 安装 `python`, 具体细节就不赘述了, 自己安装吧, 检查一下 `python` 的版本, 这里我用的 `python3` 来安装的, 其他的版本也可以

   ```sh
   $ python3 --version
   Python 3.7.0
   $ pip3.7 --version
   pip 10.0.1 from
   ```

2. 更新 `pip`

   ```shell
   pip3.7 install --upgrade pip
   ```

   如果没有 `pip`, 则需要安装

   ```shell
   python get-pip.py
   ```


3. 安装 `MkDocs`

   ```shell
   pip3.7 install mkdocs
   ```

   检查下版本

   ```shell
   $ mkdocs --version
   mkdocs, version 1.0.4 
   ```


### Windows - 待填坑

### Linux - 待填坑

### [官方介绍](https://www.mkdocs.org/#installation)



## 使用

创建一个新的工程

```shell
mkdocs new demo_project
cd demo_project
```

### index.md 是首页

其余的页面只需要创建对应的 md 文档即可. 页面会抓取首标题为页面标题



### 配置

#### 位置

`mkdocs.yml` 是配置文件

根据不同的主题, 配置文件中的内容会稍有不同. 具体以主题说明为准. 其余的东西在 [官方文档中查看吧](https://www.mkdocs.org/#getting-started)
