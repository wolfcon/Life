---
title: Watchdog - Exception Codes: 0x8badf00d
---

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents** 

- [Watchdog - Exception Codes: 0x8badf00d](#watchdog---exception-codes-0x8badf00d)
  - [Exception Codes: 0x8badf00d](#exception-codes-0x8badf00d)
  - [Watchdog termination](#watchdog-termination)
    - [`watchdog` 会监测应用的性能.](#watchdog-会监测应用的性能)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# Watchdog - Exception Codes: 0x8badf00d

[TOC]

## Exception Codes: 0x8badf00d

These are watchdog timeout crash reports, which you can recognise by the infamous "ate bad food" (`0x8badf00d`) exception code, as shown in Listing 1.



**Listing 1**  Watchdog timeout crash report

```
`Exception Type: 00000020 Exception Codes: 0x8badf00d`
```

The most common cause for watchdog timeout crashes in a network application is synchronous networking on the main thread. There are four contributing factors here:

1. synchronous networking — This is where you make a network request and block waiting for the response.

2. main thread — Synchronous networking is less than ideal in general, but it causes specific problems if you do it on the main thread. Remember that the main thread is responsible for running the user interface. If you block the main thread for any significant amount of time, the user interface becomes unacceptably unresponsive.

3. long timeouts — If the network just goes away (for example, the user is on a train which goes into a tunnel), any pending network request won't fail until some timeout has expired. Most network timeouts are measured in minutes, meaning that a blocked synchronous network request on the main thread can keep the user interface unresponsive for minutes at a time.

   Trying to avoid this problem by reducing the network timeout is not a good idea. In some situations it can take many seconds for a network request to succeed, and if you always time out early then you'll never make any progress at all.

4. watchdog — In order to keep the user interface responsive, iOS includes a watchdog mechanism. If your application fails to respond to certain user interface events (launch, suspend, resume, terminate) in time, the watchdog will kill your application and generate a watchdog timeout crash report. The amount of time the watchdog gives you is not formally documented, but it's always less than a network timeout.

One tricky aspect of this problem is that it's highly dependent on the network environment. If you always test your application in your office, where network connectivity is good, you'll never see this type of crash. However, once you start deploying your application to end users—who will run it in all sorts of network environments—crashes like this will become common.

## Watchdog termination

为了防止一个应用占用过多的系统资源, 苹果设计了一个 `watchdog` 的机制.

### `watchdog` 会监测应用的性能.

| Life cycle | timeout     |
| ---------- | ----------- |
| Launch     | 20s         |
| Resume     | 10s         |
| Suspend    | 10s         |
| Quit       | 6s          |
| Background | 由于后台任务不同的场景决定 |

如果超出了该场景所规定的运行时间, `watchdog` 就会 `强行退出` 这个应用的进程.




