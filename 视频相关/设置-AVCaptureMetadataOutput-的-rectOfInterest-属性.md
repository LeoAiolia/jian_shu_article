1.为保证较小的二维码图片能快速扫描，AVCaptureSession 的 sessionPreset 属性 设置为 AVCaptureSessionPreset1920x1080（一般来说AVCaptureSessionPreset640x480就够使用，但是如果要保证较小的二维码图片能快速扫描，最好设置高些）

2.设置rectOfInterest

最开始我按照文档说的按照比例值来设置这个属性，如下:
```
CGSize size = self.view.bounds.size;
CGRect cropRect = CGRectMake(40, 100, 240, 240);
captureOutput.rectOfInterest = CGRectMake(cropRect.origin.x/size.width,
                                         cropRect.origin.y/size.height,
                                         cropRect.size.width/size.width,
                                         cropRect.size.height/size.height);
```
但是发现，好像不对啊，扫不到了，明显不正确，于是猜想： AVCapture输出的图片大小都是横着的，而iPhone的屏幕是竖着的，那么我把它旋转90°呢：
```
CGSize size = self.view.bounds.size;
CGRect cropRect = CGRectMake(40, 100, 240, 240);
captureOutput.rectOfInterest = CGRectMake(cropRect.origin.y/size.height,
                                         cropRect.origin.x/size.width,
                                         cropRect.size.height/size.height,
                                         cropRect.size.width/size.width);
```
OK，貌似对了，在iPhone SE上一切工作良好，但是在4s上，或者换了sessionPreset的大小之后，这个框貌似就不那么准确了， 可能发现超出框上下一些也是可以扫描出来的。 再次猜想： 图片的长宽比和手机屏幕不是一样的，这个rectOfInterest是相对于图片大小的比例。比如iPhone4s屏幕大小是 640x960, 而图片输出大小是 1920x1080. 实际的情况可能就是下图中的效果:

![绿色iphone_灰色视频输出.png](https://upload-images.jianshu.io/upload_images/1613923-19936f1eb38c2235.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

上图中下面的代表iPhone4s屏幕,大小640x960, 上面代表AVCaptureVideoPreviewLayer中预览到的图片位置，在图片输入为1920x1080大小时，实际大小上下会被截取一点的，因为我们AVCaptureVideoPreviewLayer设置的videoGravity是AVLayerVideoGravityResizeAspectFill, 类似于UIView的UIViewContentModeScaleAspectFill效果。

于是我对大小做了一下修正：
```
  CGSize size = self.view.bounds.size;
  // 需要识别二维码的范围
  CGRect cropRect = CGRectMake(40, 100, 240, 240);
  CGFloat p1 = size.height/size.width;
  // 使用了1080p的图像输出
  CGFloat p2 = 1920/1080.0;
  if (p1 < p2) {
    CGFloat fixHeight = self.view.bounds.size.width * 1920.0 / 1080.0;
    CGFloat fixPadding = (fixHeight - size.height)/2;
    metadataOutput.rectOfInterest = CGRectMake((cropRect.origin.y + fixPadding)/fixHeight,
                                               cropRect.origin.x/size.width,
                                               cropRect.size.height/fixHeight,
                                               cropRect.size.width/size.width);
  } else {
    CGFloat fixWidth = self.view.bounds.size.height * 1080.0 / 1920.0;
    CGFloat fixPadding = (fixWidth - size.width)/2;
    metadataOutput.rectOfInterest = CGRectMake(cropRect.origin.y/size.height,
                                               (cropRect.origin.x + fixPadding)/fixWidth,
                                               cropRect.size.height/size.height,
                                               cropRect.size.width/fixWidth);
  }
```
经过上面的验证，证实了猜想rectOfInterest是基于图像的大小裁剪的。
