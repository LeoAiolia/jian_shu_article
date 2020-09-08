首先看原图：
![
![Uploading 屏幕快照 2017-06-04 下午3.50.00_754738.png . . .]
](http://upload-images.jianshu.io/upload_images/1613923-8284e30e81e4dd32.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


主要有三种实现方式：
1.UIToolBar：

    //只需要在想要此效果的地方加上toobar即可实现
    UIToolbar *toolbar = [[UIToolbar alloc] initWithFrame:CGRectMake(0, 0, screenSize.width, screenSize.height)];
    toolbar.barStyle = UIBarStyleBlack;
    toolbar.translucent = YES;
    [self.imageView addSubview:toolbar];

![屏幕快照 2017-06-04 下午3.50.00.png](http://upload-images.jianshu.io/upload_images/1613923-3fe7c8f979e76b75.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

2.UIBlurEffect：
在 iOS8.0 之后，苹果新增了一个类 UIVisualEffectView，基本使用和UIToolBar类似，需要注意的是，UIVisualEffectView 是一个抽象类，不能直接使用，需通过其子类UIBlurEffect,UIVibrancyEffect来实现
   
    UIBlurEffect *effect = [UIBlurEffect effectWithStyle:UIBlurEffectStyleDark];
    UIVisualEffectView *effectView = [[UIVisualEffectView alloc] initWithEffect:effect];
    effectView.frame = CGRectMake(0, 0, screenSize.width, screenSize.height);
    [self.imageView addSubview:effectView];

![屏幕快照 2017-06-04 下午3.51.02.png](http://upload-images.jianshu.io/upload_images/1613923-8df5d8bea8ef8ce7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

UIVibrancyEffect：生动的效果
    
    UIBlurEffect *effect = [UIBlurEffect effectWithStyle:UIBlurEffectStyleDark];
    UIVibrancyEffect *effect2 = [UIVibrancyEffect effectForBlurEffect:effect];
    UIVisualEffectView *effectView = [[UIVisualEffectView alloc] initWithEffect:effect2];
    effectView.frame = CGRectMake(0, 0, screenSize.width, screenSize.height);
    //通过改变contentView中subView，可以实现不同更加生动的效果
    UIView *yellowView = [[UIView alloc] init];
    yellowView.backgroundColor = [UIColor yellowColor];
    yellowView.frame = effectView.frame;
    [effectView.contentView addSubview:yellowView];

    [self.imageView addSubview:effectView];

![屏幕快照 2017-06-04 下午3.50.13.png](http://upload-images.jianshu.io/upload_images/1613923-db75a83b8d8cbbf4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
如果使用

    yellowView.backgroundColor = [UIColor yellowColor];
效果如下：

![屏幕快照 2017-06-04 下午3.57.40.png](http://upload-images.jianshu.io/upload_images/1613923-4aebddbc1effb011.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


3.第三方库：
LBBlurredImage 方式实现

    //需要#import "UIImage+ImageEffects.h"
    self.imageView.image = [[UIImage imageNamed:@"1"] applyDarkEffect];


![屏幕快照 2017-06-04 下午3.50.27.png](http://upload-images.jianshu.io/upload_images/1613923-c9387b75a11665b4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

而使用该库给出的方法：
       
    [self.imageView setImageToBlur:self.imageView.image completionBlock:^{
         NSLog(@"The LBBlurred image has been set");
    }];
会发现图片会有一瞬间是原图，这是因为
![屏幕快照 2017-06-04 下午3.18.07.png](http://upload-images.jianshu.io/upload_images/1613923-825619e9f4187fcc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
该方法使用了异步队列，然后进行图片的重绘，重绘完成之后才在主队列更新，如果不想要这个闪一下的效果，可以去掉异步，都在主队列就没问题了。
Demo参考地址：https://github.com/CSRDemo/UIBlurEffectDemo
参考文章：http://www.cocoachina.com/ios/20170531/19392.html
欢迎留言探讨。
