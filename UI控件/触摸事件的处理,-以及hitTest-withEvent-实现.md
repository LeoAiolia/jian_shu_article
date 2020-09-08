1.发送触摸事件后, 系统会将事件添加到系统UIApplication的事件管理队列中

2.UIApplication会在事件队列的最前端取出事件,然后分发下去,以便处理, 通常会把事件首先分发给KeyWindow处理

3.KeyWindow会在视图层次中找到一个最合适的视图来处理触摸事件,这也是处理事件过程的第一步.

4.找到合适的视图后, 就会调用视图控件的相应方法

    touchesBegan…
    touchesMoved…
    touchedEnded…

5.如果父控件不能接受事件, 那么子控件就不能接受事件.

一个View是如何判断自己为最佳处理点击事件的View

    // recursively calls -pointInside:withEvent:. point is in the receiver's coordinate system 
     - (nullable UIView *)hitTest:(CGPoint)point withEvent:(nullable UIEvent *)event; 

     // default returns YES if point is in bounds 
     - (BOOL)pointInside:(CGPoint)point withEvent:(nullable UIEvent *)event;

-  -hitTest: withEvent:方法的底层实现

此底层实现说明了, 一个view的子控件是如何判断是否接收点击事件的.






 
