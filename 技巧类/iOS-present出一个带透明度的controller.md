像封装一个alertViewController等情况，需要看到上个控制器里边的内容，只需要稍加设置即可。

    - (void)action:(id)sender {
     TestViewController * testVC = [[TestViewController alloc] init];
     // self is presenting view controller
     self.definesPresentationContext = YES;   
     testVC.view.backgroundColor = [UIColor colorWithRed:0 green:0 blue:0 alpha:0.5];
     testVC.modalPresentationStyle = UIModalPresentationOverCurrentContext;
     [self presentViewController:testVC animated:YES completion:nil];
    }

```
/*
  Determines which parent view controller's view should be presented over for presentations of type
  UIModalPresentationCurrentContext.  If no ancestor view controller has this flag set, then the presenter
  will be the root view controller.
*/
@property(nonatomic,assign) BOOL definesPresentationContext NS_AVAILABLE_IOS(5_0);

typedef NS_ENUM(NSInteger, UIModalPresentationStyle) {
        UIModalPresentationFullScreen = 0,
        UIModalPresentationPageSheet NS_ENUM_AVAILABLE_IOS(3_2) __TVOS_PROHIBITED,
        UIModalPresentationFormSheet NS_ENUM_AVAILABLE_IOS(3_2) __TVOS_PROHIBITED,
        UIModalPresentationCurrentContext NS_ENUM_AVAILABLE_IOS(3_2),
        UIModalPresentationCustom NS_ENUM_AVAILABLE_IOS(7_0),
        UIModalPresentationOverFullScreen NS_ENUM_AVAILABLE_IOS(8_0),
        UIModalPresentationOverCurrentContext NS_ENUM_AVAILABLE_IOS(8_0),
        UIModalPresentationPopover NS_ENUM_AVAILABLE_IOS(8_0) __TVOS_PROHIBITED,
        UIModalPresentationBlurOverFullScreen __TVOS_AVAILABLE(11_0) __IOS_PROHIBITED __WATCHOS_PROHIBITED,
        UIModalPresentationNone NS_ENUM_AVAILABLE_IOS(7_0) = -1,
};
@property(nonatomic,assign) UIModalPresentationStyle modalPresentationStyle NS_AVAILABLE_IOS(3_2);
```
我们知道设置definesPresentationContext主要是searchController push到下一界面searchBar不消失，设置definesPresentationContext = YES,就能解决这个bug，可以这样理解：
当一个视图控制器以 OverCurrentContext 的方式被呈现时，它会向上寻找一个 definesPresentationContext 属性为真的父视图控制器，并在它之上显示这个视图控制器。

关于definesPresentationContext的理解 可参考 https://www.jianshu.com/p/b065413cbf57
