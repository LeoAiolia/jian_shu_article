疑问点：scrollview上放一个button，此时如果滑动的起始位置位于button上，则会优先响应了button的点击事件，导致没有滑动的效果，如何让其优先滑动，如果是点击再响应button的点击事件呢？

解决方案：

    self.scrollView.canCancelContentTouches = YES;
    self.scrollView.delaysContentTouches = NO;

设置scrollview的这两个属性，
接着创建一个scrollView的分类，实现两个与上面属性配套的方法


    #import "MainScrollView.h"
    #import "HBSignView.h"  // 自定义的view，实现画板功能

    @implementation MainScrollView
    //当设置canCancelContentTouches=YES时，触摸事件响应前会调用该方法
    - (BOOL)touchesShouldCancelInContentView:(UIView*)view {
      if ([view isKindOfClass:[UIButton class]]) {
        return YES;
      }
      return [super touchesShouldCancelInContentView:view];
    }
    //在触摸事件开始相应前调用
    - (BOOL)touchesShouldBegin:(NSSet*)touches
                     withEvent:(UIEvent*)event
                 inContentView:(UIView*)view {
      if ([view isKindOfClass:[HBSignView class]] ||
          [view isKindOfClass:[UIButton class]]) {
        return YES;
      }
      return NO;
    }




参考链接：https://www.jianshu.com/p/c57ed75e563c
