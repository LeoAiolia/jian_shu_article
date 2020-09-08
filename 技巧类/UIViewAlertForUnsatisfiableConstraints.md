有时我们经常在xcode控制台上看到这样的信息
![控制台.png](http://upload-images.jianshu.io/upload_images/1613923-3fe9fc6978f4c098.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

Make a symbolic breakpoint at UIViewAlertForUnsatisfiableConstraints to catch this in the debugger. 是给我们的提示信息，告诉我们如何来捕捉这个问题。

1.首先我们创建一个symbolic断点。断点名就是UIViewAlertForUnsatisfiableConstraints 触发断点的操作就写 po [[UIWindow keyWindow] _autolayoutTrace]

![特征断点.png](http://upload-images.jianshu.io/upload_images/1613923-f13b2dd8c4806b65.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这个设置就是发现约束错误的时候自动打印相关的信息。
但是一般我们可能创建视图时约束就发生错误了，这时候打印不出错误信息，我们可以跳过断点，等视图出现了，手动停止，然后控制台 po [[UIWindow keyWindow] _autolayoutTrace]  来打印如图
![手动停止.png](http://upload-images.jianshu.io/upload_images/1613923-1f5d02eda27d3d65.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

打印信息大致如下：
![打印信息.png](http://upload-images.jianshu.io/upload_images/1613923-4a251c7898a5a0d2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这个时候我们再结合之前的提示，也就是橙色字里面的这一句，很重要。

![很重要的一个提示.png](http://upload-images.jianshu.io/upload_images/1613923-22c1c2478c84cb82.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


表明系统帮你把这个多余的约束移除了。约束的相关信息也在上面。你再结合刚才po出来的信息，就能很快找到这个view在哪一层。比如，提示中  0x7fc135408a30 这个内存地址，在我们po出来的层级关系里面是最上面的一层，因为我们这个demo最上面的一层就是蓝色的testview。。。所以我们可以快速判断是这个view上面的某个约束出了问题。。如果同层级的view比较多，难以判断，我们可以通过改变view的颜色，直观地看出来是哪个view。
![改变颜色.png](http://upload-images.jianshu.io/upload_images/1613923-afc0f00eb4881388.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我们也可以打印一下约束的信息

![打印约束信息.png](http://upload-images.jianshu.io/upload_images/1613923-c745a112ef577065.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

再结合代码，便可找到大致有问题的地方了。

2.如果你的视图是使用masonry来约束的，你还可以使用下面👇的方式来查找有问题的视图。。。

首先要明确大致出问题的地方，然后给每个可能有问题的视图设置mas_key，如下：
![设置mas_key.png](http://upload-images.jianshu.io/upload_images/1613923-1251981845a0c398.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
此时运行到有问题的地方后，则会打印如下信息。
![设置mas_key后打印的信息.png](http://upload-images.jianshu.io/upload_images/1613923-be757f2e9286768f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
此时再看到打印的信息，则能很快确定是reprintedLabel出了问题。此时能很清晰的看到问题出在哪里，此时修改代码即可。

参考链接：https://www.jianshu.com/p/a668e6402b59



