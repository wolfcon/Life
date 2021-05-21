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

 `Notification Simulation File(JSON payload)`  should be have this key-value `"Simulator Target Bundle": "com.bundle.identifier",` on the top level.
