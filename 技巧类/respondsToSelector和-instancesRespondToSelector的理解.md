

>respondsToSelector 和 instancesRespondToSelector是两个常用的方法,我们经常使用 someObject respondsToSelector,但是对于它和instancesRespondToSelector之间有什么区别?

首先先看两个方法的声明
```
@interface NSObject <NSObject> 
+ (BOOL)instancesRespondToSelector:(SEL)aSelector; 
@end 
@protocol NSObject 
- (BOOL)respondsToSelector:(SEL)aSelector; 
@end
```




我们可以以UIScreen来进行说明
```
@interface UIScreen : NSObject <UITraitEnvironment> 
+ (UIScreen *)mainScreen; 
@property(nullable,nonatomic,strong) UIScreenMode *currentMode 
@end
```
先看结果

    UIScreen* screens = [UIScreen mainScreen];
    if ([screens.class instancesRespondToSelector:@selector(currentMode)]) {
      NSLog(@"[screens.classinstancesRespondToSelector:@selector(currentMode)]能响应");
    }
    if ([screens respondsToSelector:@selector(currentMode)]) {
      NSLog(@"[screens respondsToSelector:@selector(currentMode)]能响应");
    }
    if ([screens.class respondsToSelector:@selector(currentMode)]) {
      NSLog(@"[screens.class respondsToSelector:@selector(currentMode)]能响应");
    }
    if ([UIScreen instancesRespondToSelector:@selector(currentMode)]) {
      NSLog(@"[UIScreen instancesRespondToSelector:@selector(currentMode)]能响应");
    }
    if ([UIScreen respondsToSelector:@selector(currentMode)]) {
      NSLog(@"[UIScreen respondsToSelector:@selector(currentMode)]能响应");
    }

    输出结果：
    [screens.class instancesRespondToSelector:@selector(currentMode)]能响应
    [screens respondsToSelector:@selector(currentMode)]能响应
    [screens.class respondsToSelector:@selector(currentMode)]不能响应
    [UIScreen instancesRespondToSelector:@selector(currentMode)]能响应
    [UIScreen respondsToSelector:@selector(currentMode)]不能响应

这两个方法都是用来判断是否实现了@selector 的方法
根据api我们可以看出

* instancesRespondToSelector是需要类来调用
* respondsToSelector是需要对象来调用

但需要注意的是，类本身，也是一个对象。因此当使用类调用这两个方法时，还是需要注意一下区别的。

当类调用instancesRespondToSelector时，类是类，但当类调用respondsToSelector方法时，类就是对象了（是不是有点绕😈）
通俗的讲就是：
对象A调用respondsToSelector:SELB时，如果返回为YES，则A可以直接调用SELB，即

    [A SELB];

类ClassA调用instancesRespondToSelector:SELB时(可参考👆例子中[UIScreen instancesRespondToSelector:@selector(currentMode)]),如果返回为YES，那么类ClassA的对象可以调用方法SELB,即

    [[ClassA new]  SELB];

接下来看下面这组测试，好好的理解一下吧。

    if ([screens.class instancesRespondToSelector:@selector(mainScreen)]) {
      NSLog(@"[screens.class instancesRespondToSelector:@selector(mainScreen)]能响应");
    } else {
      NSLog(@"[screens.class instancesRespondToSelector:@selector(mainScreen)]不能响应");
    }
    if ([screens respondsToSelector:@selector(mainScreen)]) {
      NSLog(@"[screens respondsToSelector:@selector(mainScreen)]能响应");
    } else {
      NSLog(@"[screens respondsToSelector:@selector(mainScreen)]不能响应");
    }
    if ([screens.class respondsToSelector:@selector(mainScreen)]) {
      NSLog(@"[screens.class respondsToSelector:@selector(mainScreen)]能响应");
    } else {
      NSLog(@"[screens.class respondsToSelector:@selector(mainScreen)]不能响应");
    }
    if ([UIScreen instancesRespondToSelector:@selector(mainScreen)]) {
      NSLog(@"[UIScreen instancesRespondToSelector:@selector(mainScreen)]能响应");
    } else {
      NSLog(@"[UIScreen instancesRespondToSelector:@selector(mainScreen)]不能响应");
    }
    if ([UIScreen respondsToSelector:@selector(mainScreen)]) {
      NSLog(@"[UIScreen respondsToSelector:@selector(mainScreen)]能响应");
    } else {
      NSLog(@"[UIScreen respondsToSelector:@selector(mainScreen)]不能响应");
    }

    输出:
    [screens.class instancesRespondToSelector:@selector(mainScreen)]不能响应
    [screens respondsToSelector:@selector(mainScreen)]不能响应
    [screens.class respondsToSelector:@selector(mainScreen)]能响应
    [UIScreen instancesRespondToSelector:@selector(mainScreen)]不能响应
    [UIScreen respondsToSelector:@selector(mainScreen)]能响应

总结：
* 判断一个类ClassA 是否实现了一个减号方法SELB时，使用

      [ClassA instancesRespondToSelector:SELB];
      [[ClassA new]  respondsToSelector:SELB];

* 判断一个类ClassA 是否实现了一个加号方法SELB时
   
      [ClassA  respondsToSelector:SELB];
 
参考文章： https://www.jianshu.com/p/c37f5d97b07e
