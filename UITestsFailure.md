---
title: UITest Failure - Failed to scroll to visible (by AX action) Button
---

# UITests Failure - Failed to scroll to visible (by AX action) Button

[TOC]

## 如何出现的

当我在使用 UITests 在写 UI 测试的时候, 突然发现有 `Hud` 提示动画的情况, 有很大概率会出现这个错误

```sh
t =    14.12s         Scroll element to visible
    t =    14.15s         Failed: Failed to scroll to visible (by AX action) Button, identifier: 'registerDeviceButton', label: 'Register Device', error: Error kAXErrorCannotComplete performing AXAction 2003 on element AX element pid: 21359, elementOrHash.elementID: 140652851610096.10
    t =    15.15s     Retrying `Tap "registerDeviceButton" Button` (attempt #2)
```

注意看哦. 我们的主角出现了 `Failed: Failed to scroll to visible (by AX action) Button, `

经过我多次的寻因, 发现界面中并没有 `ScrollView`, 为什么会出现 `Scroll element to visible` 的行为呢?

原来是在出现这个操作之前, 我们的界面中出现了 `Animation` 或者 `Hud`, 由此导致了 UITest 的系统找不到我们的目标控件, 尝试 scroll...🤣

## 如何解决

**禁用 Animation 和 Hud 之类的内容**

### 那么我们如何禁用

如果在 App 中直接禁用会影响 App 的功能🧐

### 解决方案

首先我们看看, UITests 是如何启动的, 不知道大家发现了没? UITests 会在设备上新装一个类 App 的应用, 名字是以`测试 App 名开头 + UITests -` 为首. 点开它, 是黑屏, 然后又退出了. 

其实 UITests 是以这个 App 为媒介, 由它挂接启动我们的测试 App. 然后控制测试的.

好了. 至此, 我们的思路来了!

#### Arguments

这个参数是不是很眼熟啊, 入门 C 语言之时, 老师教写 main 函数为入口, 当时最大的疑问, 我想大家与我都有吧? 

```c
int main(int argc, char * argv[]) {
  
}
```

这个函数如果大家不是很深究的话, 一般不会在意 `argv`, 它到底是干嘛的. 当时只关注了, 冒泡排序, 如何写个程序, 能有个跑起来的 `Hello World`. 随着我们不断的成长, 后续肯定知道它是干嘛的了. 毕竟最早没学编程时, 也知道在 Windows 的快捷方式中写个 `-window` 之类的启动参数吧? 🤫

其实这个就是我们的切入点, 因为 UITests 独有的挂载机制.

开始, 给他一个启动参数

```swift
let app = XCUIApplication()
app.launchArguments.append("--UITests")
app.launch()
```

在我们的 App 的 AppDelegate 中

```swift
@UIApplicationMain
class AppDelegate: UIResponder, UIApplicationDelegate {
    func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
        #if DEBUG
        SettingForUITest.disableAnimation()
        #endif
        return true
    }
}

// MARK: 🌈 UITest Setting
class SettingForUITest {
    class func disableAnimation() {
        if CommandLine.arguments.contains("--UITests") {
            UIView.setAnimationsEnabled(false)
            SVProgressHUD.setAnimationsEnabled(false)
            SVProgressHUD.setMaximumDismissTimeInterval(0)
            SVProgressHUD.setMinimumDismissTimeInterval(0)
        }
    }
}
```

我们写一个 `disableAnimation` 的函数来禁用所有可能会引起问题的内容

> 注意, 我们在这里增加了`#if Debug` 用于绝对的过滤掉可能在 Release 中出现的可能

另外, 我们需要注意的是, 这个 `--UITests` 不是一定得是这个参数, 写其他的也可以, 不过在 UITests 和 App 代码中要**一一对应**

