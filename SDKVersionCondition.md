---
title: 宏命令判断检测 SDK 版本
---

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**

- [宏命令判断检测 SDK 版本](#宏命令判断检测-sdk-版本)
  - [前言](#前言)
  - [前缀类型](#前缀类型)
    - [__MAC_OS_X](#__mac_os_x)
    - [__IPHONE_OS](#__iphone_os)
  - [后缀类型](#后缀类型)
    - [_VERSION_MIN_REQUIRED](#_version_min_required)
    - [_VERSION_MAX_ALLOWED](#_version_max_allowed)
  - [用法](#用法)
  - [官方解释](#官方解释)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->



# 宏命令判断检测 SDK 版本

## 前言

有时我们在写代码时需要考虑到队友 (使用者) 的 SDK 编译版本, 否则就得让他们忍受编译错误的苦果

比如我们经常遇到

- `新语法糖`
- `新类`
-  ...

将这些写进代码后, 使用老 SDK / IDE 的人就得忍受编译错误的痛苦. 这样的体验无疑是令人招烦的, 当然你可以~~强迫~~让他更换匹配的平台.

那么这时候我们就要祭出我们今天的主角了

## 前缀类型

### __MAC_OS_X

表示 macOS

### __IPHONE_OS

表示 iOS

## 后缀类型

### _VERSION_MIN_REQUIRED

值等于Deployment Target, 检查支持的最小系统版本.

### _VERSION_MAX_ALLOWED

等于Base SDK, 即用于检查 SDK 版本的

## 用法

我们使用的时候只需要把前缀后缀进行组合使用即可达到相应的效果

```objc
// 这里的数字 100100 是 iOS 10.1 , 比如 12.2 就是 120200
#if __IPHONE_OS_VERSION_MIN_REQUIRED >= 100100
	...
#endif
```

```objc
// 这里的数字 101301 是 macOS 10.13.1 , 比如 10.14.4 就是 101404
#if __MAC_OS_X_VERSION_MAX_ALLOWED >= 101301
	...
#endif
```

## 官方解释

```objc
/*      
        An enum available on the phone, but not available on Mac OS X:
        
            #if __IPHONE_OS_VERSION_MIN_REQUIRED
                enum { myEnum = 1 };
            #endif
           Note: this works when targeting the Mac OS X platform because 
           __IPHONE_OS_VERSION_MIN_REQUIRED is undefined which evaluates to zero. 
        

        An enum with values added in different iPhoneOS versions:
		
			enum {
			    myX  = 1,	// Usable on iPhoneOS 2.1 and later
			    myY  = 2,	// Usable on iPhoneOS 3.0 and later
			    myZ  = 3,	// Usable on iPhoneOS 3.0 and later
				...
		      Note: you do not want to use #if with enumeration values
			  when a client needs to see all values at compile time
			  and use runtime logic to only use the viable values.
			  

    It is also possible to use the *_VERSION_MIN_REQUIRED in source code to make one
    source base that can be compiled to target a range of OS versions.  It is best
    to not use the _MAC_* and __IPHONE_* macros for comparisons, but rather their values.
    That is because you might get compiled on an old OS that does not define a later
    OS version macro, and in the C preprocessor undefined values evaluate to zero
    in expresssions, which could cause the #if expression to evaluate in an unexpected
    way.
    
        #ifdef __MAC_OS_X_VERSION_MIN_REQUIRED
            // code only compiled when targeting Mac OS X and not iPhone
            // note use of 1050 instead of __MAC_10_5
            #if __MAC_OS_X_VERSION_MIN_REQUIRED < 1050
                // code in here might run on pre-Leopard OS
            #else
                // code here can assume Leopard or later
            #endif
        #endif
*/
```

