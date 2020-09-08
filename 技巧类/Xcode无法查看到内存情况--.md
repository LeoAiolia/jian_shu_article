今天在做项目，发现无法查看内存情况，之前还是可以的，只是打了个僵尸断点。

![无法查看内存.png](http://upload-images.jianshu.io/upload_images/1613923-ea056fdb816cb3db.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

后来去掉后发现可以查看内存了。后来在网上查了一下，终于发现，Xcode6之后，打了僵尸断点情况下，就不能查看内存了。
     解决方法：在项目的edit scheme 设置中，Diagnostics 下的一个 Zommbie Object 这个选项给勾上了。把这个选项去掉，就可以查看到内存使用情况了。

![僵尸断点.png](http://upload-images.jianshu.io/upload_images/1613923-e7a4d01b46c28450.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
    
解决后的正常情况如下：

![可以查看内存.png](http://upload-images.jianshu.io/upload_images/1613923-3c5e44f359a07d29.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
