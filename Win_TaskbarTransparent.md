---
title: Make Taskbar Transparent
date: 2021-12-20
---

# Make Taskbar Transparent

## Windows 10

1. `Personalization` -> `Colors` -> `More options` -> `Transparency effects` -> Enabled
2. `Win` + `R` -> open `regedit` 
3. Under the path `HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\Advanced`, create a new `DWROD (32-bit) Value` and name it `TaskbarAcrylicOpacity`, which value is in range `0-255`. `0` is represented fully transparent(aka: alpha = 0). `255` means opaque (aka: alpha = 1)
4. Restart `Windows Explorer` process in Task Manager. 🎉

> Maybe should set
>
> `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\Advanced`
>
> Add `UseOLEDTaskbarTransparency` and set `0`

## Windows 11 (not work right now)

`HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\DWM` Set `ForceEffectMode` to `1`

`HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\Advanced` Set `UseOLEDTaskbarTransparency` to `1`
