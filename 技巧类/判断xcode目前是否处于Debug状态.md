有时候我们设置宏定义时

    #ifdef DEBUG
       //do sth1.
    #else 
      //do sth2.
    #endif

但是却发现只走sth2，这是因为你没有设置DEBUG，一般Apple已经为我们设置好了 DEBUG 的宏定义，所以，我们可以直接用，但有时候没有设置，你就要知道如何设置，设置步骤也很简单，如下：
      点击 Build Settings ，然后在搜索框里输入‘macros’，如图：

![屏幕快照 2017-07-06 下午8.05.09.png](http://upload-images.jianshu.io/upload_images/1613923-4c6100d72c953c4f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


如果已经设置过，在 Preprocessor Macros 的 Debug 后面会有 DEBUG=1，如果没有，就手动设置下。
常见的应用场景：
1.

    //put this in prefix.pch
    #ifndef DEBUG
    #undef NSLog
    #define NSLog(args, ...)
    #endif

2.
    #ifdef DEBUG
    #define NSLogDebug(format, ...) \
    NSLog(@"<%s:%d> %s, " format, \
    strrchr("/" __FILE__, '/') + 1, __LINE__, __PRETTY_FUNCTION__, ## __VA_ARGS__)
    #else
    #define NSLogDebug(format, ...)
    #endif

3.swift

    #if DEBUG
      println("I'm running in DEBUG mode")
    #else
      println("I'm running in a non-DEBUG mode")
    #endif

Additionally you will need to set the DEBUG symbol in Swift Compiler - Custom Flags section for the Other Swift Flags key via a -D DEBUG entry. See the following screenshot for an example:
   swift设置的地方稍有不同：
  

![rLan3.png](http://upload-images.jianshu.io/upload_images/1613923-d6758c7c420a2f15.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
