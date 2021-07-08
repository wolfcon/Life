---
title: iOS Simulator 101
---

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**

- [iOS Simulator 101](#ios-simulator-101)
  - [Live Development/Debugging](#live-developmentdebugging)
    - [Tools - InjectionIII](#tools---injectioniii)
    - [Getting Started](#getting-started)
      - [Loading InjectionIII bundle](#loading-injectioniii-bundle)
      - [Configuration for Injection](#configuration-for-injection)
      - [Adding Linker Flags](#adding-linker-flags)
      - [Other More Useful Cases](#other-more-useful-cases)
  - [Screen Recording](#screen-recording)
    - [Use QuickTime Player](#use-quicktime-player)
    - [Use Command Line Tool](#use-command-line-tool)
      - [Start](#start)
      - [End](#end)
  - [Simulate Remote Notification](#simulate-remote-notification)
    - [Use Command Line Tool](#use-command-line-tool-1)
      - [Acquire Device ID](#acquire-device-id)
      - [Push](#push)
    - [Drag `Notification Simulation File` to `Simulator`](#drag-notification-simulation-file-to-simulator)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# iOS Simulator 101

[TOC]

## Live Development/Debugging

### [Tools - InjectionIII](https://github.com/johnno1962/InjectionIII)

Code injection allows you to update the implementation of functions and any method of a class, struct or enum incrementally in the iOS simulator without having to rebuild or restart your application. 

### Getting Started

#### Loading InjectionIII bundle

```swift
#if DEBUG
Bundle(path: "/Applications/InjectionIII.app/Contents/Resources/iOSInjection.bundle")?.load()
//for tvOS:
Bundle(path: "/Applications/InjectionIII.app/Contents/Resources/tvOSInjection.bundle")?.load()
//Or for macOS:
Bundle(path: "/Applications/InjectionIII.app/Contents/Resources/macOSInjection.bundle")?.load()
#endif
```

#### Configuration for Injection

1. using default method.

   ```swift
   @objc func injected() {
   		// injection method
   }
   ```

2. using notification: `INJECTION_BUNDLE_NOTIFICATION` (this is notification name)

#### Adding Linker Flags

```
-Xlinker -interposable
```

#### Other More Useful Cases

Just look into [this document](https://github.com/johnno1962/InjectionIII/blob/main/README.md)

## Screen Recording

### Use QuickTime Player

Just record mac Screen!

> Right click `QuickTime` app in Dock and `Record`.
>
> Stop in top menu bar right corner.

### Use Command Line Tool

#### Start

```shell
xcrun simctl io booted recordVideo videoName.mov
```

#### End

Just `ctrl` + `c`

## Simulate Remote Notification

### Use Command Line Tool

#### Acquire Device ID

```shell
 xcrun simctl list | grep Booted 
```

#### Push

```shell
# 40F9EEBF-98BE-4203-99BB-C64D95E4D7A4 is your deivce ID
xcrun simctl push 40F9EEBF-98BE-4203-99BB-C64D95E4D7A4 com.bundle.identifier PushNotificationPayload.apns
```

⚠️ `PushNotificationPayload.apns` is a Notification Simulation File(JSON payload). You can create it in `watchOS Resource`

`com.bundle.identifier` is needed to replace with your app bundle identifier. **Or**

- remove it from command line. 

- Add `"Simulator Target Bundle": "com.bundle.identifier",` into `Notification Simulation File(JSON payload)` on the top level.

  ```json
  {
      "Simulator Target Bundle": "com.bundle.identifier",
      "aps": {
          "alert": {
              "body": "Test message",
              "title": "Optional title",
              "subtitle": "Optional subtitle"
          },
          "category": "myCategory",
          "thread-id": "5280"
      },
      
      "customKey": ""
  }
  ```

### Drag `Notification Simulation File` to `Simulator`

 `Notification Simulation File(JSON payload)`  should be had this key-value `"Simulator Target Bundle": "com.bundle.identifier",` on the top level.
