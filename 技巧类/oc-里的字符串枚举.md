1.当需要使用特定的字符串作为参数、
2.使用字典作为参数传递，或者作为返回值时，需要提供字典的key

现在使用字符串枚举即可解决这样的问题。
可参考系统的API `NSKeyValueObserving.h`

```
// 使用NS_STRING_ENUM宏，定义了一个枚举类型
typedef NSString * NSKeyValueChangeKey NS_STRING_ENUM;

FOUNDATION_EXPORT NSKeyValueChangeKey const NSKeyValueChangeKindKey;
FOUNDATION_EXPORT NSKeyValueChangeKey const NSKeyValueChangeNewKey;
FOUNDATION_EXPORT NSKeyValueChangeKey const NSKeyValueChangeOldKey;
FOUNDATION_EXPORT NSKeyValueChangeKey const NSKeyValueChangeIndexesKey;
FOUNDATION_EXPORT NSKeyValueChangeKey const NSKeyValueChangeNotificationIsPriorKey;

// 使用泛型，声明了change参数用到的key，是在NSKeyValueChangeKey的枚举范围中
- (void)observeValueForKeyPath:(nullable NSString *)keyPath ofObject:(nullable id)object change:(nullable NSDictionary<NSKeyValueChangeKey, id> *)change context:(nullable void *)context;
```
