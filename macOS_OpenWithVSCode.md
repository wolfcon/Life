---
title: 在 Finder 中对文件夹/文件加入 `使用 VSCode 打开`
toc: true
---

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**

- [在 Finder 中对文件夹/文件加入 `使用 VSCode 打开`](#%E5%9C%A8-finder-%E4%B8%AD%E5%AF%B9%E6%96%87%E4%BB%B6%E5%A4%B9%E6%96%87%E4%BB%B6%E5%8A%A0%E5%85%A5-%E4%BD%BF%E7%94%A8-vscode-%E6%89%93%E5%BC%80)
  - [Getting Started](#getting-started)
  - [Modify](#modify)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# 在 Finder 中对文件夹/文件加入 `使用 VSCode 打开`

## Getting Started

1. Open `Automator` 
2. Press `⌘`+`N` to create a new  `Quick Action`
3. Receives current `files or folders` in `Finder.app`
4. `drag` `Utilities`->`Run shell Script` to right action area
5. Pass input: `as arguments`
6. Write the script
7. Save your `action`

Scrip is down below:

```shell
for f in "$@"
do
	open -a "Visual Studio Code" "$f"
done
```

## Modify

- Open `System Preferences`.
- Select `Keyboard`
- in `Shortcuts`
- Find your `action`
- Right click `action` to modify with `Automator`
