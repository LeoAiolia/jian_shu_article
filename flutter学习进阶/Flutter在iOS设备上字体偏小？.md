在使用了flutter开发后，设计发现iOS设备上字体偏小，但是各种查也没发现是什么问题，最后看到了这篇文章：

https://blog.csdn.net/studying_ios/article/details/106199143

然后尝试将字体跟随系统给禁止掉，在给设计去看ui，字体就不偏小了。

由于我们是swift与flutter混合开发，使用了flutterBoost，在app的builder处是这样的

```
builder: FlutterBoost.init()
```
导致无法renten MediaQuery，最后是这样解决的

```
builder: (BuildContext context, Widget child) {
       return MediaQuery(
           data: MediaQuery.of(context).copyWith(textScaleFactor: 1.0, boldText: false),
           child: Builder(builder: (BuildContext context) {
             return widget.builder(context, child);
          }),
    );
 },

widget.builder 外部传参：FlutterBoost.init()
```

