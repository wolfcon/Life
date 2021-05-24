<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**

- [Lua 中的 and 和 or 的特殊用法](#lua-%E4%B8%AD%E7%9A%84-and-%E5%92%8C-or-%E7%9A%84%E7%89%B9%E6%AE%8A%E7%94%A8%E6%B3%95)
  - [引言](#%E5%BC%95%E8%A8%80)
    - [我们先来普及下基础(~~这不是废话么, 谁要你普及~~)](#%E6%88%91%E4%BB%AC%E5%85%88%E6%9D%A5%E6%99%AE%E5%8F%8A%E4%B8%8B%E5%9F%BA%E7%A1%80%E8%BF%99%E4%B8%8D%E6%98%AF%E5%BA%9F%E8%AF%9D%E4%B9%88-%E8%B0%81%E8%A6%81%E4%BD%A0%E6%99%AE%E5%8F%8A)
  - [下面我们来说特殊用法](#%E4%B8%8B%E9%9D%A2%E6%88%91%E4%BB%AC%E6%9D%A5%E8%AF%B4%E7%89%B9%E6%AE%8A%E7%94%A8%E6%B3%95)
    - [连接 2 个变量时](#%E8%BF%9E%E6%8E%A5-2-%E4%B8%AA%E5%8F%98%E9%87%8F%E6%97%B6)
      - [and](#and)
      - [or](#or)
    - [连接多个变量时](#%E8%BF%9E%E6%8E%A5%E5%A4%9A%E4%B8%AA%E5%8F%98%E9%87%8F%E6%97%B6)
      - [and](#and-1)
      - [or](#or-1)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

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
