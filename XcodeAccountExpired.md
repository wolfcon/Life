---
title: Xcode 账户失效问题
toc: ture
---

# Xcode 账户失效问题

有木有遇到过. Xcode 在上传包时报账户登录会话失效? 

🤪 恭喜你找对地方了. 这是Xcode 的诡异问题.

## 解决方法

在 Xcode 中删除账户, 然后再 Terminal 中输入

```bash
defaults write com.apple.dt.Xcode DVTDeveloperAccountUseKeychainService -bool NO
```

再次登录 Xcode 账户.

这样就成功解决使用 `xcodebuild` CI 自动上传包时报错的问题了.🎉

>  Reference Article: [xcode 9.3 session expires every time i close and re-open Xcode](https://stackoverflow.com/questions/49675844/xcode-9-3-session-expires-every-time-i-close-and-re-open-xcode) from Stack Overflow