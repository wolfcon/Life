---
title: 文档部署系统 MkDocs
---

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