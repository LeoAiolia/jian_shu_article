本文主要讲的是使用clang命令，他的作用是用来消除特定区域clang的编译警告
一般格式是使用

    #pragma clang diagnostic push
    #pragma clang diagnostic ignored "-Wgnu"
    //消除警告代码
    #pragma clang diagnostic pop
下边的链接是clang的警告信息列表[Which Clang Warning Is Generating This Message?](http://fuckingclangwarnings.com/)

 另外，ignored ""引号中的内容是想要消除的警告的内容，具体获取可以Reveal in Log查看
![屏幕快照 2017-06-03 12.29.16.png](http://upload-images.jianshu.io/upload_images/1613923-7c5ae926bec7b716.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
查看到的内容如下
![FD37B9FC7DAEC7F05D7AAB9EBA563F16.png](http://upload-images.jianshu.io/upload_images/1613923-ec5784623a88213e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
[]中的内容就是该警告的标识，如该警告就是-Wshorten-64-to-32
   则应使用下面
    #pragma clang diagnostic ignored "-Wshorten-64-to-32"
来消除该警告，不过此方法消除警告虽然快速，但是最好还是根据警告的提示来消除该警告，此方法只是在紧急情况下，如马上要上架，但是因为警告较多，可能导致打包无法通过的情况下使用。

另一种情况就是你的工程支持旧版本，但是新版本方法已经废弃，但是虽然你使用安全判断了，但是依然会有警告提示，此时就可以使用该方法，消除废弃方法的警告：
    #pragma clang diagnostic ignored "-Wdeprecated"

参考链接：
1.http://www.cnblogs.com/dsxniubility/p/4757760.html
2.http://www.cnblogs.com/qiutangfengmian/p/5644133.html
