在Xcode8上面，默认情况下无法调试iOS7，因为缺乏调试iOS7需要的配置文件。同时在低版本的Xcode上面（8以下），也无法调试iOS10的真机。解决办法如下：

我们在升级Xcode8之前，可以先将调试需要的配置文件拷贝出来，方法finder中前往文件夹

    /Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/DeviceSupport

![屏幕快照 2016-11-11 下午10.32.11.png](http://upload-images.jianshu.io/upload_images/1613923-d1e6e65e9fb350f8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

将旧版本的支持文件拷过来，重启xcode即可。

xcode8中工程的Deployment Target最低是8.0，虽然可以手动将8.0改为7.0，但是总给人一种xcode不支持7.0的赶脚，那怎么才能让xcode8的Deployment Target默认有7.0和7.1呢，步骤如下：
在finder中前往文件夹，打开以下路径:

    /Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/SDKs/iPhoneOS.sdk

用Xcode打开SDKSettings.plist这个文件，加入如下图所示的配置，保存之后重启Xcode，之后在工程的Deployment Target里面就可以选择你所添加的版本了。


![1749624-90b973c68e48ca1e-1.png](http://upload-images.jianshu.io/upload_images/1613923-9cbce95bbe3fdc51.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

如果是Xcode8以下的版本调试适配iOS10，方法是一样的，只不过需要在高版本的Xcode里面把配置文件拷贝出来

如果SDKSettings.plist这个文件提示无法修改的话，可以先讲这个文件拷贝一份到桌面，修改后再覆盖进去即可。至于如何得到这些支持文件，网上有很多大神的分享...








