当我们使用runtime交换方法时，通常使用如下方法

```
static inline BOOL ISExchangeImplementationsInTwoClasses(Class _fromClass,
                                                         SEL _originSelector,
                                                         Class _toClass,
                                                         SEL _newSelector) {
  if (!_fromClass || !_toClass) {
    return NO;
  }
  
  Method oriMethod = class_getInstanceMethod(_fromClass, _originSelector);
  Method newMethod = class_getInstanceMethod(_toClass, _newSelector);
  if (!newMethod) {
    return NO;
  }
  
  BOOL isAddedMethod = class_addMethod(_fromClass, _originSelector,
                                       method_getImplementation(newMethod),
                                       method_getTypeEncoding(newMethod));
  if (isAddedMethod) {
    // 如果 class_addMethod 成功了，说明之前 fromClass 里并不存在 originSelector，
    // 所以要用一个空的方法代替它，以避免 class_replaceMethod 后，
    // 后续 toClass 的这个方法被调用时可能会 crash
    IMP oriMethodIMP = method_getImplementation(oriMethod)
    ?: imp_implementationWithBlock(^(id selfObject){
    });
    const char* oriMethodTypeEncoding =
    method_getTypeEncoding(oriMethod) ?: "v@:";
    class_replaceMethod(_toClass, _newSelector, oriMethodIMP,
                        oriMethodTypeEncoding);
  } else {
    method_exchangeImplementations(oriMethod, newMethod);
  }
  return YES;
}
```
我们知道`class_getInstanceMethod`获取的是实例方法，交换减号方法时，_fromClass和_toClass都传[self class]就行了，但是交换两个加号方法时，传这个参数却没有用，方法交换失败，但是传`object_getClass`交换即可成功。再加上那到底他们有什么区别呢？

先来看下图:
![图片.png](https://upload-images.jianshu.io/upload_images/1613923-33acb503c5f0437d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
当需要交换实例方法时，我们传入 `Class`即可,当时当交换加号方法时，需要传`MetaClss`，可以这样理解，oc里的Class里的加号方法，相当于该类的MetalClas的实例方法，类调用类方法，和对象调用实例方法，其实底层实现都是一样的。类也是对象。

以UIImage为例，
```
Class a1 = objc_getClass("UIImage");
Class a2 = objc_getMetaClass("UIImage");
Class a3 = [self class];
Class a4 = object_getClass(self);

NSLog(@"%p",a1);
NSLog(@"%p",a2);
NSLog(@"%p",a3);
NSLog(@"%p",a4);

2019-03-09 16:01:46.074567+0800 Configuration[8694:652882] 0x1e6229c68
2019-03-09 16:01:46.074607+0800 Configuration[8694:652882] 0x1e6229c90
2019-03-09 16:01:46.074618+0800 Configuration[8694:652882] 0x1e6229c68
2019-03-09 16:01:46.074627+0800 Configuration[8694:652882] 0x1e6229c90
```

>1.objc_getMetaClass    获取MetalClass
>2.objc_getClass             获取Class
>3.object_getClass          获取 object 的isa指针对象。




看开源的objc源码有class的方法
```
+ (Class)class {
return self;
}

- (Class)class {
return object_getClass(self);
}
```
其实object_getClass(obj)与[obj class]的区别了，就两点：
>1、如果是obj实例对象，他们一样；
>2、如果是类对象，class是self，object_getClass是isa

参考链接：https://www.jianshu.com/p/ae5c32708bc6
