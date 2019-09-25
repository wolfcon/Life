---
title: Lua 中的 and 和 or 的特殊用法
---

# Lua 中的 and 和 or 的特殊用法

[TOC]

## 引言

语言的特殊性, 总是让人捉摸不透. 最近又开始玩 WoW 的了(经典版), 很多插件都有 bug, 遂操起了老本行, 找找🐞.

发现很多在赋值时大量使用了 `and` 和 `or` 的此种操作...心里便难过了起来. 犹如当年的 i++ / ++i / ++(i++) 等等的噩梦...🤕

### 我们先来普及下基础(~~这不是废话么, 谁要你普及~~)

Lua 中的 `假` 仅为 `nil` 或者 `false`

其余一切东西都视为 `真`

## 下面我们来说特殊用法

### 连接 2 个变量时

#### and

若第一个变量为`假`, 则返回它, 否则返回第二个变量

```lua
value = a and b
```

等价于

```lua
if not a then
  value = a
else
  value = b
end
```

🌰: 

```lua
print(3 and 5)
print(0 and 5)
print(nil and 6)
print(false and 6)
print(false or nil)
```

结果为: 

```lua
5
5
nil
false
false
```

#### or

若第一个变量为`真`, 则返回它, 否则返回第二个变量

```lua
value = a and b
```

等价于

```lua
if a then
  value = a
else
  value = b
end
```

🌰：

```lua
print(2 or 7)
print(0 or 3)
print(nil or 9)
print(false or 9)
print(false or nil)
```

结果为

```lua
2
0
9
9
nil
```

### 连接多个变量时

#### and

返回值是 `从左到右第一个为假的值`, 若全都不为假, 则返回最后一个变量

#### or

返回值是 `从左到右第一个为真的值`, 若全都不为真, 则返回最后一个变量