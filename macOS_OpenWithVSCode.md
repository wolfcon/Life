---
title: 在 Finder 中对文件夹/文件加入 `使用 VSCode 打开`
toc: true
---

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