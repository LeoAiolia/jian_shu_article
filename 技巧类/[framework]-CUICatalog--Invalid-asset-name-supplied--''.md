这个问题其实是老问题，产生原因就是因为在使用的时候 [UIImage imageNamed:]时，图片不存在或者传入的图片名为nil或者是@""

可使用特征断点Symbolic Breakpoint.来查找这个问题。
![20160910101102224.png](http://upload-images.jianshu.io/upload_images/1613923-a59a1527c8af50cd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

1.在Xcode的Breakpoint Navigator点击加号, 选择Add Symbolic Breakpoint.

2.右键选择Breakpoint选择Edit Breakpoint, 在Symbol填入+[UIImage imageNamed:], 在Condition填入

     [(NSString *)$arg3 length] == 0
或者

    $arg3 == nil
可以自己尝试po $arg1,po $arg2试试看.

3.运行程序, 直到程序进入断点. 打开Debug Navigator观察调用栈, 最顶部的一定是+[UIImage imageNamed:], 点击调用栈下一条, 能够看到有调用到imageNamed的代码, 就是name为nil的地方.


