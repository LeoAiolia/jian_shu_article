![线程.png](https://upload-images.jianshu.io/upload_images/1613923-6d1c84927ce64586.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
我们调试线程的时候，会发现有些线程会有UMSocial_Thread,WebThread之类的不同于Thred12，Thread20之类的名字，感觉十分高大上，其实很简单，只需要在你需要修改名字的线程里调用一个方法即可。

     [[NSThread currentThread] setName:@"customThreadName"];

在自己封装管理自己的线程时，也可以这样玩。😆
