---
title: 生物识别验证
toc: true
---

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**

- [生物识别验证](#生物识别验证)
  - [简介](#简介)
    - [NSFaceIDUsageDescription](#nsfaceidusagedescription)
    - [Biometrics](#biometrics)
  - [LocalAuthentication](#localauthentication)
    - [LAContext](#lacontext)
      - [func canEvaluatePolicy](#func-canevaluatepolicy)
      - [func evaluatePolicy](#func-evaluatepolicy)
        - [死锁问题](#死锁问题)
        - [连续验证失败!](#连续验证失败)
      - [var biometryType](#var-biometrytype)
    - [LAError](#laerror)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# 生物识别验证

[TOC]

## 简介

iOS 现阶段有 3 种本地验证方式, 分别是

- Passcode
- TouchID
- FaceID

macOS 现阶段有 2 种本地验证方式, 分别是

- Password
- TouchID

比如 `KeyChain` 的调用在获取 API 中有参数可以自动带有生物识别验证方式, 这里不做赘述, 这里我们仅仅讨论自定义的验证机制.

### NSFaceIDUsageDescription

在使用 `FaceID` 时, 我们要向 App 的 `Info.plist` 中添加 `NSFaceIDUsageDescription` 的权限描述.

### Biometrics

在 `iOS 11` 之后 `TouchID` 的相关参数及方法全部都被替换成 `Biometrics` 了, 也就说 `Biometrics` 包含了 `TouchID` 和 `FaceID`, 具体由系统去判断, 因为现在并没出现两者同时出现的迹象, 而且 Apple 貌似并不想让他们同时出现.

## LocalAuthentication

我们用到的就是这个库

```swift
import LocalAuthentication
```

### LAContext

这个类是我们主要操作的对象. 下面我们来介绍一下, 我们一般会用到的验证方法

#### func canEvaluatePolicy

```swift
    /// Determines if a particular policy can be evaluated.
    ///
    /// @discussion Policies can have certain requirements which, when not satisfied, would always cause
    ///             the policy evaluation to fail - e.g. a passcode set, a fingerprint
    ///             enrolled with Touch ID or a face set up with Face ID. This method allows easy checking
    ///             for such conditions.
    ///
    ///             Applications should consume the returned value immediately and avoid relying on it
    ///             for an extensive period of time. At least, it is guaranteed to stay valid until the
    ///             application enters background.
    ///
    /// @warning    Do not call this method in the reply block of evaluatePolicy:reply: because it could
    ///             lead to a deadlock.
    ///
    /// @param policy Policy for which the preflight check should be run.
    ///
    /// @param error Optional output parameter which is set to nil if the policy can be evaluated, or it
    ///              contains error information if policy evaluation is not possible.
    ///
    /// @return YES if the policy can be evaluated, NO otherwise.
    @available(iOS 8.0, *)
    open func canEvaluatePolicy(_ policy: LAPolicy, error: NSErrorPointer) -> Bool
```

在调用 `evaluatePolicy` 之前必须调用这个方法, 来判断是否支持预设的验证条件

#### func evaluatePolicy

```swift
/// Evaluates the specified policy.
    ///
    /// @discussion Policy evaluation may involve prompting user for various kinds of interaction
    ///             or authentication. Actual behavior is dependent on evaluated policy, device type,
    ///             and can be affected by installed configuration profiles.
    ///
    ///             Be sure to keep a strong reference to the context while the evaluation is in progress.
    ///             Otherwise, an evaluation would be canceled when the context is being deallocated.
    ///
    ///             The method does not block. Instead, the caller must provide a reply block to be
    ///             called asynchronously when evaluation finishes. The block is executed on a private
    ///             queue internal to the framework in an unspecified threading context. Other than that,
    ///             no guarantee is made about which queue, thread, or run-loop the block is executed on.
    ///
    ///             Implications of successful policy evaluation are policy specific. In general, this
    ///             operation is not idempotent. Policy evaluation may fail for various reasons, including
    ///             user cancel, system cancel and others, see LAError codes.
    ///
    /// @param policy Policy to be evaluated.
    ///
    /// @param reply Reply block that is executed when policy evaluation finishes.
    ///              success Reply parameter that is YES if the policy has been evaluated successfully or
    ///                      NO if the evaluation failed.
    ///              error Reply parameter that is nil if the policy has been evaluated successfully, or it
    ///                    contains error information about the evaluation failure.
    ///
    /// @param localizedReason Application reason for authentication. This string must be provided in correct
    ///                        localization and should be short and clear. It will be eventually displayed in
    ///                        the authentication dialog subtitle for Touch ID or passcode. The name of the
    ///                        calling application will be displayed in title, so it should not be duplicated here.
    ///
    ///                        This parameter is mostly ignored by Face ID authentication. Face ID will show
    ///                        generic instructions unless a customized fallback title is provided in
    ///                        localizedFallbackTitle property. For that case, it will show the authentication
    ///                        reason so that the instructions can be made consistent with the custom button
    ///                        title. Therefore, you should make sure that users are already aware of the need
    ///                        and reason for Face ID authentication before they have triggered the policy evaluation.
    ///
    /// @warning localizedReason parameter is mandatory and the call will throw NSInvalidArgumentException if
    ///          nil or empty string is specified.
    ///
    /// @warning Applications should also supply NSFaceIDUsageDescription key in the Info.plist. This key identifies
    ///          a string value that contains a message to be displayed to users when the app is trying to use
    ///          Face ID for the first time. Users can choose to allow or deny the use of Face ID by the app before
    ///          the first use or later in Face ID privacy settings. When the use of Face ID is denied, evaluations
    ///          will fail with LAErrorBiometryNotAvailable.
    ///
    /// @see LAError
    ///
    /// Typical error codes returned by this call are:
    /// @li          LAErrorUserFallback if user tapped the fallback button
    /// @li          LAErrorUserCancel if user has tapped the Cancel button
    /// @li          LAErrorSystemCancel if some system event interrupted the evaluation (e.g. Home button pressed).
    @available(iOS 8.0, *)
    open func evaluatePolicy(_ policy: LAPolicy, localizedReason: String, reply: @escaping (Bool, Error?) -> Void)
```

##### 死锁问题

这里需要特别注意的是, 不能在 `reply` 回调里再次调用 `canEvaluatePolicy` 方法, 否则会造成 **死锁**

##### 连续验证失败!

连续验证失败超过预定次数后, 将不会再调用 `reply` 回调. 这点需要注意

#### var biometryType

其中包含一个用于判断当前机器是拥有何种 `biometryType` 的属性. 

```swift
    /// The device does not support biometry.
    
    /// The device supports Touch ID.
    
    /// The device supports Face ID.
    
    /// Indicates the type of the biometry supported by the device.
    ///
    /// @discussion  This property is set when canEvaluatePolicy has been called for a biometric policy.
    ///              The default value is LABiometryTypeNone.
@available(iOS 11.0, *)
open var biometryType: LABiometryType { get }
```

它是在调用 `canEvaluatePolicy` 方法后, 被设值

类型如下: 

- none
- touchID
- faceID

```swift
@available(iOS 11.0, *)
public enum LABiometryType : Int {

    /// The device does not support biometry.
    @available(iOS 11.2, *)
    case none

    /// The device does not support biometry.
    @available(iOS, introduced: 11.0, deprecated: 11.2, renamed: "LABiometryType.none")
    public static var LABiometryNone: LABiometryType { get }

    /// The device supports Touch ID.
    case touchID

    /// The device supports Face ID.
    case faceID
}
```



### LAError

```swift
/// Authentication was not successful, because user failed to provide valid credentials.
        case authenticationFailed

        
        /// Authentication was canceled by user (e.g. tapped Cancel button).
        case userCancel

        
        /// Authentication was canceled, because the user tapped the fallback button (Enter Password).
        case userFallback

        
        /// Authentication was canceled by system (e.g. another application went to foreground).
        case systemCancel

        
        /// Authentication could not start, because passcode is not set on the device.
        case passcodeNotSet

        
        /// Authentication could not start, because Touch ID is not available on the device.
        @available(iOS, introduced: 8.0, deprecated: 11.0, message: "use LAErrorBiometryNotAvailable")
        case touchIDNotAvailable

        
        /// Authentication could not start, because Touch ID has no enrolled fingers.
        @available(iOS, introduced: 8.0, deprecated: 11.0, message: "use LAErrorBiometryNotEnrolled")
        case touchIDNotEnrolled

        
        /// Authentication was not successful, because there were too many failed Touch ID attempts and
        /// Touch ID is now locked. Passcode is required to unlock Touch ID, e.g. evaluating
        /// LAPolicyDeviceOwnerAuthenticationWithBiometrics will ask for passcode as a prerequisite.
        @available(iOS, introduced: 9.0, deprecated: 11.0, message: "use LAErrorBiometryLockout")
        case touchIDLockout

        
        /// Authentication was canceled by application (e.g. invalidate was called while
        /// authentication was in progress).
        @available(iOS 9.0, *)
        case appCancel

        
        /// LAContext passed to this call has been previously invalidated.
        @available(iOS 9.0, *)
        case invalidContext

        
        /// Authentication could not start, because biometry is not available on the device.
        @available(iOS 11.0, *)
        public static var biometryNotAvailable: LAError.Code { get }

        
        /// Authentication could not start, because biometry has no enrolled identities.
        @available(iOS 11.0, *)
        public static var biometryNotEnrolled: LAError.Code { get }

        
        /// Authentication was not successful, because there were too many failed biometry attempts and
        /// biometry is now locked. Passcode is required to unlock biometry, e.g. evaluating
        /// LAPolicyDeviceOwnerAuthenticationWithBiometrics will ask for passcode as a prerequisite.
        @available(iOS 11.0, *)
        public static var biometryLockout: LAError.Code { get }

        
        /// Authentication failed, because it would require showing UI which has been forbidden
        /// by using interactionNotAllowed property.
        @available(iOS 8.0, *)
        case notInteractive
```

这些错误就不细说了.

主要赘述一下 `notInteractive`: 有时候在后台调起 App 时, 如果申请的时机早于 `UIApplicationDidBecomeActiveNotification` 之前, 则有可能会报这个错误. 换而言之: 并不能在后台调用验证功能, 其实想想也明白, 如果在后台调用, 弹出验证, 用户哪知道是什么 App 弹出的.🤪
