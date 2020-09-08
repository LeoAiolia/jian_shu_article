在 AppDelegate 的 didFinishLaunchingWithOptions 方法中加入以下代码就可以了。

    #if DEBUG
    id overlayClass = NSClassFromString(@"UIDebuggingInformationOverlay");
    [overlayClass performSelector:NSSelectorFromString(@"prepareDebuggingOverlay")];
    #endif      

运行后，用两个手指头在状态栏上同时点击下就可以显示出这个调试的悬浮

参考链接：http://www.cocoachina.com/ios/20170712/19817.html
