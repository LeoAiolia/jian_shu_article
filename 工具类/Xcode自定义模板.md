Xcode系统模板的路径是

    /Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/Library/Xcode/Templates/

文件夹里面有文件模板File Templates和工程模板Project Templates，分别对应创建文件时的选项和创建工程时的选项：

![创建文件.png](http://upload-images.jianshu.io/upload_images/1613923-a7114b36e18d472d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我们用的最多就是

    File Templates/Source/Cocoa Touch Class.xctemplate

里面的模板。里面长的是这样的

![Cocoa Touch Class.xctemplate.png](http://upload-images.jianshu.io/upload_images/1613923-eab442f561e45977.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

例如我们需要改UIViewController的模板，就需要改其中的UIViewControllerObjective-C、 UIViewControllerSwift、UIViewControllerXIBObjective-C、UIViewControllerXIBSwift。他们的区别从名字上就能看出来，就是OC与Swift，是否用Xib的区别。


在创建文件的时候，如果想使用你自定义的模板选项，如：

![选择自定义模板.png](http://upload-images.jianshu.io/upload_images/1613923-631e2e036885d1b9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

可以在File Templates下新建一个Custom Template文件夹，把系统的Cocoa Touch Class.xctemplate复制进去，然后进行修改就可以了。不会影响系统的默认模板的样式。

本文参考：http://www.cocoachina.com/ios/20170419/19087.html
我的自定义模板：https://github.com/LeoAiolia/Custom-templete
