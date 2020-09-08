window有一个属性windowLevel
  
    statusView.windowLevel = UIWindowLevelAlert;

初始化statusView,可在其上添加控件

    statusView = [[UIWindow alloc] initWithFrame:[UIApplication sharedApplication].statusBarFrame];
    statusView.windowLevel = UIWindowLevelAlert;
    statusView.backgroundColor = [UIColor orangeColor];
     //显示statusView，不写该段代码无法显示
    statusView.hidden = NO;

这样该window就会覆盖在statusBar上面。
