ExclusiveTouch的作用是：可以达到同一界面上多个控件接受事件时的排他性,从而避免bug。也就是说避免在一个界面上同时点击多个UIButton导致同时响应多个方法。

全局设置时可在AppDelegate启动应用时添加

      [[UIButton appearance] setExclusiveTouch:YES];

单个UIView内设置所有UIButton 的ExclusiveTouch属性时可用
    
     - (void)setExclusiveTouchForButtons:(UIView *)myView {

          for (UIView * v in [myView subviews]) {

                if([v isKindOfClass:[UIButton class]]) {

                      [((UIButton *)v) setExclusiveTouch:YES];

                 }
                 else if ([v isKindOfClass:[UIView class]]) {
                       [self setExclusiveTouchForButtons:v];
                  }
           }
     }

注意:exclusiveTouch是UIView的属性，默认是NO；xcode9中API如下：

![ExclusiveTouch.png](http://upload-images.jianshu.io/upload_images/1613923-e876055b4182bd1f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
