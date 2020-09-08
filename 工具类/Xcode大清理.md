1.移除对旧设备的支持
影响：可重新生成；再连接旧设备调试时，会重新自动生成。

    ~/Library/Developer/Xcode/iOS DeviceSupport
注意：这个只是对旧设备的支持，并非xcode里面的兼容设备的支持文件，路径不一样，那个路径是

    /Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/DeviceSupport

这个支持文件删除了可以重写生成，但是那个没有了就无法生成了。不过可以手动移动。可参考文章：http://www.jianshu.com/p/21af658b6e83

2.移除DerivedData
影响：可重新生成；会删除build生成的项目索引、build输出以及日志。重新打开项目时会重新生成，大的项目会耗费一些时间。

路径：
    
    ~/Library/Developer/Xcode/DerivedData

3.移除Archives
影响：不可恢复；Adhoc或者App Store版本会被删除。建议备份dSYM文件夹

路径：

    ~/Library/Developer/Xcode/Archives





