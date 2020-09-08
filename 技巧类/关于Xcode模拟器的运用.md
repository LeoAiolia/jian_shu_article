**1.运行时可以让动画变慢**

可以点击模拟器的菜单栏，`debug -> slow Animation `选项，你可以看到xcode的debug 的output中打印了：`Simulator slow-motion animations are now on `，可以使其动画变慢，比如push的时候，如果想让其消失，同样的点击一下就行，在xcode的 debug 的output 中打印了：`Simulator slow-motion animations are now off` ，然后恢复原样。当然也可以用快捷键（command + T）

**2.Color Blended Layers**

App开发中页面有的时候会比较复杂，视图越简单性能一般越快，但是UI不是由开发决定，当页面视图嵌套太多，很容易出现滑动卡顿掉帧的现象.  Color Blended Layers 通过模拟器Debug可以查看视图中颜色混合.如果视图中的颜色混合越多,那么GPU通过混合纹理计算出像素的RGB值需要消耗的时间就越长，GPU的使用率就越高，可以通过减少颜色混合来提升滑动的流畅性.

参考简书：https://www.jianshu.com/p/bdd39a56142a

**3.color off-screen Rendered**

离屏渲染，指的是 GPU 或 CPU在当前屏幕缓冲区以外新开辟一个缓冲区进行渲染操作。过程中需要切换 contexts (上下文环境),先从当前屏幕切换到离屏的contexts，渲染结束后，又要将 contexts 切换回来，而切换过程十分耗费性能。
GPU 产生的离屏渲染主要是当 CALayer 使用圆角，阴影，遮罩等属性的的时候，图层属性的混合体被指定为在未预合成之前不能直接在屏幕中渲染，则过程中需要进行离屏渲染。
实际项目中 CPU 产生的离屏渲染主要由Core Graphics API(核心绘图)的使用导致。

参考简书：https://www.jianshu.com/p/f247f8c13b32

**4.color copied images**

苹果的GPU只解析32bit的颜色格式。 如果一张图片，颜色格式不是32bit，CPU会先进行颜色格式转换，再让GPU渲染。 就算异步转换颜色，也会导致性能损耗，比如电量增多，发热强变大等等。

解决办法：让设计师提供32bit颜色格式的图片。

参考文章：https://www.jianshu.com/p/a46199c41c26

**5.color Misaligned images**
模拟器调试时，打开模拟器的Debug - Color Misaligned Images菜单选项。最快捷，但仅限模拟器上查看。
Instrument性能检测时，选中Core Animation模板，在Display Settings中勾选Color Misaligned Images选项。可针对模拟器和真机，可查看真机上所有应用的像素混合情况。

打开开关后，看到部分视图会有黄色或洋红色（Magenta）的图层标记，代表其像素不对齐。
不对齐：视图或图片的点数(point)，不能换算成整数的像素值（pixel），导致显示视图的时候需要对没对齐的边缘进行额外混合计算，影响性能。
洋红色：UIView的frame像素不对齐，即不能换算成整数像素值。
黄色：UIImageView的图片像素大小与其frame.size不对齐，图片发生了缩放造成。

参考链接：https://www.jianshu.com/p/38cf9c170141

**6.总结**
UI开发是很简单的一件事，但如果能把以上问题都注意到，就变得不那么简单，少年加油吧！
可参考另一篇优秀的文章：https://www.jianshu.com/p/5c968a240e27
