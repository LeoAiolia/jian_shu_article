网上已经很多介绍libstdc++.6.0.9.tbd报错如何处理的文章了，但是今天我要写的是关于libstdc++.6.tbd，其实呢这个问题和6.0.9一样，是同一个问题，但是稍微有点小区别，因为libstdc++.6.tbd这个库只是libstdc++.6.0.9.tbd的一个替身。
解决办法如下：
首先前往路径。

    /Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/SDKs/iPhoneOS.sdk/usr/lib/

注意：这个库也有模拟器和真机的区分，要是解决模拟器报错，请前往路径

    /Applications/Xcode.app/Contents/Developer/Platforms/iPhoneSimulator.platform⁩/⁨Developer⁩/⁨SDKs⁩/⁨iPhoneSimulator.sdk⁩/⁨usr/lib/

但是同样的也需要将模拟器的libstdc++.6.0.9.tbd库拷贝到这个目录下。
然后在当前文件夹下，制作刚拷贝进来的libstdc++.6.0.9.tbd的替身，然后将这个替身改一个名字，改为

    libstdc++.6.tbd

![路径.png](https://upload-images.jianshu.io/upload_images/1613923-923aacb58c4cf1b5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


重启Xcode，最好清下缓存，然后重新编译，即可编译通过了。。。


