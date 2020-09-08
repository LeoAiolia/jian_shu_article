在iOS中，我们经常使用 `NSInteger`  `CGFloat`  `NSTimeInternal`  `int` `long` `long long`等来处理基本变量，而在与C++混编时，经常会遇到`int32_t` `int64_t`类型，这时如果使用类型不当，可能会导致数据溢出的问题，👇看
```
int32_t    是int的别名, 占4个字节
int64_t    是long long的别名, 占8个字节
```
由此可看出我们可使用 `int`  `long long`来处理这两种类型

附：NSInteger 在32位与64位系统的区别
```
#if __LP64__ || (TARGET_OS_EMBEDDED && !TARGET_OS_IPHONE) || TARGET_OS_WIN32 || NS_BUILD_32_LIKE_64
typedef long NSInteger;
typedef unsigned long NSUInteger;
#else
typedef int NSInteger;
typedef unsigned int NSUInteger;
#endif
```

```
在32位系统中
 int        占4个字节
 long       占4个字节
 long long  占8个字节
 
 NSInteger  是int的别名, 占4个字节
 int32_t    是int的别名, 占4个字节
 int64_t    是long long的别名, 占8个字节
 
 在64位系统中
 int        占4个字节
 long       占8个字节
 long long  占8个字节
 
 NSInteger  是long的别名, 占8个字节
 int32_t    是int的别名, 占4个字节
 int64_t    是long long的别名, 占8个字节
 
 
 4字节的整数变量, 它的范围是 -2147483648 ~ 2147483647
 如果不带符号, 它的范围是   0 ~ 4294967295
 
 8字节的整数变量, 它的范围是 -9223372036854775808 ~ 9223372036854775807
 如果不带符号, 它的范围是 0 ~ 18446744073709551615
```

由于long和NSInteger的字节数变了, 所以在兼容的时候可能会导致溢出.

>案例一:  对于一个11位的整数
在64位系统中使用NSInteger或者long类型, 是可以正常存储的.
在32位系统中使用NSInteger或者long类型它就溢出了.
所以要保证某些较大的整数可以正常使用，那么就需要使用long long或者int64_t这样的类型.

>案例二:  在类型转换的时候,
例如int64_t转换成NSInteger, 在64位系统中是正常的, 但在32位系统中就可能会导致溢出.

>总结：
在兼容32位和64位系统，使用int， long long（或者int32_t，int64_t）这样的数据类型比使用NSInteger可靠得多.

处理数据问题一定要谨慎，这种类型的bug一般很不好查，只能靠平常写的时候多注意。
