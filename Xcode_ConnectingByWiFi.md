---
title: Connecting Problems with devices in WiFi debugging
---

# Connecting Problems with devices in WiFi debugging

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**

- [Connecting Problems with devices in WiFi debugging](#connecting-problems-with-devices-in-wifi-debugging)
  - [Instruction](#instruction)
  - [How to fix it](#how-to-fix-it)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## Instruction

1. Press `shift` + `cmd` + `2` and connect your device by usb cable.

2. Check the box `Connect via network` and connect the same WiFi as your Mac ( LAN is also be a choice. But should be in same network. )

3. Unplug your device.

If you can't see your device in left area, congratulations!

## How to fix it

Just reference in [Apple Developer Forum answer ](https://developer.apple.com/forums/thread/89040#268215022)

> Unfortunately I do not see a "Connect via IP Address" option when I control-click on the device.

Yeah, that can be a bit persnickety. Here’s what I did to get it to show up:

1. I turned off Wi-Fi on my device, just to be sure it wasn’t being seen on the network.
2. I connected it via USB.
3. In Xcode’s *Devices* window, I selected the device on the left.
4. I enabled *Connect via Network* on the right.
5. I disconnected the USB; the device moved to the *Disconnected* section.
6. I control clicked on the device and *Connect via IP Address* shows up in the menu.

I didn’t actually choose the menu item because it wasn’t going to work because, hey, the Wi-Fi was off. But that should at least get it to show up.

> Does that mean the AP is not forwarding multi-casts, or STA-to-STA traffic?

You can test this as follows:

1. Run through the process above.

2. In your Mac, start a Bonjour discovery from Terminal:

   ```terminal
   dns-sd -B _apple-mobdev2._tcp. local.
   ```

3. Turn on Wi-Fi on the device. You should see

   ```terminal
   dns-sd
   ```

   log a line like this:

   ```terminal
   10:20:24.099  Add        2   5 local.               _apple-mobdev2._tcp. 40:33:1a:d7:f4:9b@fe80::4233:1aff:fed7:f49b
   ```

   where

   ```terminal
   40:33:1a:d7:f4:9b@fe80::4233:1aff:fed7:f49b
   ```

   is the Bonjour service name (it seems that Xcode uses the MAC address and the link-local IPv6 for the service name, which is wacky but there you go).

If the service doesn’t show up, Wi-Fi multicast is disabled. In that case you can try the *Connect via IP Address* process described above. If that doesn’t work, it’s likely that all STA-to-STA networking is disabled.
