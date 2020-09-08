先说一下原理，每个app都可以配置一个urlScheme，通过在safari里打对应的  

    $urlSchemeName://
即可打开对应的app。
比如：微信的urlScheme是weixin
在safari的地址栏输入 weixin:// 即可打开微信。

捷径打开qq音乐的最近播放列表，首先在捷径里建立如下图所示的两个小步骤
![11.png](https://upload-images.jianshu.io/upload_images/1613923-39b81cc336da9816.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

将图中的URL换成 

    $qqmusic://today?mid=31&k1=2&k4=0

即可,注：$字符不需要输入，只是为了让//后的内容高亮，否则会变成灰色。

通过这种方式，类似的可以建立很多种玩法，如下图：
![222.png](https://upload-images.jianshu.io/upload_images/1613923-d1ccbbb1f711bc95.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

附：

微信扫一扫

```
$weixin://scanqrcode
```
支付宝扫一扫 
```
$alipayqr://platformapi/startapp?saId=10000007
```
支付宝付款 
```
$alipay://platformapi/startapp?appId=20000056
```
支付宝转账 
```
$alipayqr://platformapi/startapp?saId=20000116
```
支付宝收款 
```
$alipays://platformapi/startapp?appId=20000123
```
支付宝记账
```
$alipay://platformapi/startapp?appId=20000168
```
支付宝滴滴 
```
$alipay://platformapi/startapp?appId=20000778
```
支付宝蚂蚁森林
```
$alipay://platformapi/startapp?appId=60000002
```
支付宝转账
```
$alipayqr://platformapi/startapp?saId=20000116
```
支付宝手机充值
```
$alipayqr://platformapi/startapp?saId=10000003
```
播放网易云已下载的音乐 
```
$orpheuswidget://download
```
网易云音乐听歌识曲
```
 $orpheuswidget://recognize
```
常用的 urlScheme 
https://blog.csdn.net/xttxqjfg/article/details/76019824

