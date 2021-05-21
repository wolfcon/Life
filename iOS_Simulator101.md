<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [iOS Simulator 101](#ios-simulator-101)
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

---
title: iOS Simulator 101
---

# iOS Simulator 101

[TOC]

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