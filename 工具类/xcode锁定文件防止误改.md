我们在用Xcode编程时，不小心修改了系统库文件时总是会弹出如下图的lock框，这样能防止误修改。当我们使用cocoapods管理三方时，也有这样的设定。文件的右上角也有一个🔐的标志。如下图：
![屏幕快照 2017-06-15 下午1.27.07.png](http://upload-images.jianshu.io/upload_images/1613923-403332e522de70c3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

如何让自己在工程中编写好的文件也有这个效果呢？其实很简单，只需要更改读写权限即可，将读与写改为只读即可，可以查看简介，修改权限 
![屏幕快照 2017-06-15 下午1.26.54.png](http://upload-images.jianshu.io/upload_images/1613923-62cb5656b3c9ffe9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
改为只读后xcode右上角自己就会出现🔐的标志。
