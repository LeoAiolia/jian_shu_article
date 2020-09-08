一、从终端进入Finder
终端cd到文件件下，想快速打开finder，可以使用命令

    $open .
二、从finder快速打开终端
1.系统设置
进入系统偏好设置->键盘->快捷键->服务。
在右边新建位于文件夹位置的终端窗口上打勾。
![1.png](https://upload-images.jianshu.io/upload_images/1613923-f7870fc7be177dce.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这时

![2.png](https://upload-images.jianshu.io/upload_images/1613923-3ff8dbdfe5e4ba2a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

但是这依然不是最方便的
2.使用Go2Shell

在Mac App Store中下载 Go2Shell 软件，下载后在”应用程序“里找到Go2Shell软件，按住Cmd键，将其拖动到Finder的工具栏上。
![3.png](https://upload-images.jianshu.io/upload_images/1613923-304570e3dfd6aabb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

打开Go2Shell设置的方法是：在终端中输入命令 

    $open -a Go2Shell --args config

然后回车。

优点：可以不需要选中文件夹，直接点Go2Shell按钮就可以打开终端。
缺点：每次都是在新的终端窗口中打开，而不是在新的终端标签中打开。

![5.png](https://upload-images.jianshu.io/upload_images/1613923-86acea575599a35f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
