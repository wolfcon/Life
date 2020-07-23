---
title: macOS 清除 Launchpad 中的无用图标
---

# macOS 清除 Launchpad 中的无用图标

[TOC]

❗️❗️❗️❗️❗️❗️❗️❗️❗️❗️❗️❗️❗️❗️❗️❗️❗️❗️❗️❗️❗️❗️❗️❗️❗️❗️❗️❗️

## ⚠️ 首先备份找到的数据库文件, 然后再操作❗️

❗️❗️❗️❗️❗️❗️❗️❗️❗️❗️❗️❗️❗️❗️❗️❗️❗️❗️❗️❗️❗️❗️❗️❗️❗️❗️❗️❗️

## Terminal

### 寻找数据库文件

```sh
cd /private/var/folders/
sudo find ./ -name com.apple.dock.launchpad
# 根据👆返回的结果下面的目录会不一样. 每个人都不一样. 注意哟🧐
cd mf/34d25p254h30ccbn62xxxb2h0000gn/0/com.apple.dock.launchpad/db
```

### 查找确认删除项

```sh
sqlite3 db "select item_id,title from apps;"
```

### 删除

```sh
sqlite3 db "delete from apps where title='name';"&&killall Dock
```

或者

```sh
sqlite3 db "delete from apps where item_id='id';"&&killall Dock
```

## App

这里我们使用 [DB Browser for SQLite](https://sqlitebrowser.org/dl/)

### 寻找数据库文件

1. 打开 Finder
2. `cmd+shift+G` 输入 `/private/var/folders/`

3. 在右上角搜索框输入 `com.apple.dock.launchpad`

4. 选择搜索范围: `folders`
5. 找到文件夹, 进入 `db` 文件夹下, 找到文件 `db` , 这就是我们需要的数据库文件了🎉

### 查找确认删除项

1. 将 `db` 拖入 `DB Browser for SQLite`
2. 打开 `apps` 表

3. 选择 `Browse Data`
4. 找到删除项

### 删除

1. 选中要删除的项 (*注意在左侧序号处选中*)
2. 右键 `Remove Record`

