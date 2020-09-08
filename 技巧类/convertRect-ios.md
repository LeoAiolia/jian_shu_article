首先看下这个两个方法

    [self.view convertRect:(CGRect) fromView:(nullable UIView *)];
    [self.view convertRect:(CGRect) toView:(nullable UIView *)];

这两行代码，均可划分为三部分

即：源、目标、被操作的对象。fromView后面接的参数是:源，toView后面接的参数是:目标，convertRect后面接的参数永远是:被操作的对象。

作用是：计算源上的被操作的对象相对于目标的frame。举个例子：

    [viewB convertRect:viewC.frame toView:viewA];

该例子中显然viewA是目标，viewC是被操作的对象，那么剩下的viewB自然而然就是源了。作用就是计算viewB上的viewC相对于viewA的frame。

    [viewC convertRect:viewB.frame fromView:viewA];

该例子viewA是源，viewB是被操作的对象，那么viewC就是目标。作用就是计算viewA上的viewB相对于viewC的frame。

若有不对之处，还望指正！O(∩_∩)O谢谢！
