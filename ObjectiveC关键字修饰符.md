<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**

- [Objective-C 关键字修饰符](#objective-c-%E5%85%B3%E9%94%AE%E5%AD%97%E4%BF%AE%E9%A5%B0%E7%AC%A6)
- [类型修饰符](#%E7%B1%BB%E5%9E%8B%E4%BF%AE%E9%A5%B0%E7%AC%A6)
  - [`__kindof`](#__kindof)
    - [一般是配合泛型来一起使用](#%E4%B8%80%E8%88%AC%E6%98%AF%E9%85%8D%E5%90%88%E6%B3%9B%E5%9E%8B%E6%9D%A5%E4%B8%80%E8%B5%B7%E4%BD%BF%E7%94%A8)
    - [当然也可以不配合泛型来用](#%E5%BD%93%E7%84%B6%E4%B9%9F%E5%8F%AF%E4%BB%A5%E4%B8%8D%E9%85%8D%E5%90%88%E6%B3%9B%E5%9E%8B%E6%9D%A5%E7%94%A8)
  - [](#)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## Objective-C 关键字修饰符

这里只介绍一些较新, ~~不常见的~~

## 类型修饰符

### `__kindof`

#### 一般是配合泛型来一起使用

直接上🌰:

正常我们会这样使用泛型来让我们后续的编码更加的明确.

```objc
NSMutableArray<UIView *> *views = [NSMutableArray arrayWithCapacity:1];
[views addObject:[UIButton new]];

UIButton *button = views[0];
```

`⚠️ Incompatible pointer types initializing 'UIButton *' with an expression of type 'UIView *'`

这时第四行代码就会报警告了

但是我们如果使用 `__kindof` 来声明泛型的类型, 则就不会出现这样的⚠️.

```objc
NSMutableArray<__kindof UIView *> *views = [NSMutableArray arrayWithCapacity:1];
```

#### 当然也可以不配合泛型来用

🌰: 

我们通过标识符来获得占位视图.

```objc
- (UIView *)placeholderViewWithIdentifier:(NSString *)identifier; 

// 使用时, 我们有自定义的 CircleView
CircleView *circleView = [self placeholderViewWithIdentifier:@"circle"];
```

`⚠️ Incompatible pointer types initializing 'CircleView *' with an expression of type 'UIView *'`

同样, 我们使用 `__kindof` 修饰返回值. ⚠️就消失啦 🎉

```objc
- (__kindof UIView *)placeholderViewWithIdentifier:(NSString *)identifier; 
```



### 
