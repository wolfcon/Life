---
title: Objective-C 关键字修饰符
---

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