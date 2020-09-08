最近xcode老出现这种问题


![ScanningForWorkingCopies.png](http://upload-images.jianshu.io/upload_images/1613923-7058a141758e846b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


顶部一直有Scanning for Working Copies 的菊花在转圈，怎么也转不完，虽然对平时工作也没发现有啥影响，但是看着确实心里不安。
网上搜了一下，发现这个东西和版本控制有关。出现这种现象，此时无法进行代码的版本管理操作的
解决办法：

1》在XCode->Preferences->Source Control里将Enable Source Control的对勾去掉，然后关掉项目

2》重新打开项目,这时"scanning for working copies"消失了，再将Enable Source Control的对勾打上。

如果使用其他工具，如source Tree，是不用打开这个选项的
