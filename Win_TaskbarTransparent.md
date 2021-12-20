---
title: Make Taskbar Transparent
date: 2021-12-20
---

# Make Taskbar Transparent

1. `Personalization` -> `Colors` -> `More options` -> `Transparency effects` -> Enabled
2. `Win` + `R` -> open `regedit` 
3. Under the path `\HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\Advanced`, create a new `DWROD (32-bit) Value` and name it `TaskbarAcrylicOpacity`, which value is in range `0-10`. `0` is represented fully transparent(aka: alpha = 0). `10` means opaque (aka: alpha = 1)
4. Restart `Windows Explorer` process in Task Manager. 🎉

