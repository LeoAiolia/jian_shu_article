1.如何在oc工程中调用swift文件
一般需要引入"工程名-swift.h",也不是绝对的，具体取决于buildSetting>Objective-c Generated Interface Header Name的设置。如下图 ：


![oc引入swift头文件.png](http://upload-images.jianshu.io/upload_images/1613923-99f9ee26b12b6f70.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
2.在swfit工程中使用调用oc
需要在桥接文件中导入oc头文件。当在swift工程中新建oc文件时，会提示你是否创建桥接文件，也可以自己创建桥接文件，然后在build setting>Objective-c Bridging Header里设置。如下图：

![swift的桥接文件.png](http://upload-images.jianshu.io/upload_images/1613923-e18f1beb9b0d757d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

参考链接：http://www.kittenyang.com/swiftandoc/
