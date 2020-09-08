项目中使用GPUImage做小视频，需要接入人脸识别，因此需要对AVCaptureVideoDataOutput的代理方法

    - (void)captureOutput:(AVCaptureOutput *)captureOutput didOutputSampleBuffer:(CMSampleBufferRef)sampleBuffer fromConnection:(AVCaptureConnection *)connection;

中进行特殊的处理，查看GPUImage的源码发现GPUImageVideoCamera这个类里面有一个方法，

    - (void)processVideoSampleBuffer:(CMSampleBufferRef)sampleBuffer;
这个方法即是在此代理中调用的，此方法已暴露给开发者。
因为项目中的GPUImage使用的是Framework的形式，故首先不考虑修改这个三方，因此采用了继承的方法，重写这个方法，拿到了CMSampleBufferRef，但是处理的sdk使用时需要传入图像的RGB数据，查资料，找到方法：

        // 实现预览效果不断设置Image
        CVImageBufferRef cvImageBufferRef = CMSampleBufferGetImageBuffer(sampleBuffer);
        // 转换类型
        CVPixelBufferRef cvPixelBufferRef = cvImageBufferRef;
        // 如果想要对数据进行修改就必须对向前数据进行锁定
        CVPixelBufferLockBaseAddress(cvPixelBufferRef, kCVPixelBufferLock_ReadOnly);
        // 处理图像数据
        // 图像出来的原始数据是 R G R A 每个像素 4 个字节 32 位的数据
        // 获取宽高
        size_t width = CVPixelBufferGetWidth(cvPixelBufferRef);
        size_t height = CVPixelBufferGetHeight(cvPixelBufferRef);
        // 获取指向数据内容的指针
        unsigned char *pImageData = (unsigned char *)CVPixelBufferGetBaseAddress(cvPixelBufferRef);

这个PImageData就包含了RGB数据，但是调试的数据却不是，后阅读GPUImage源码发现了问题，
![屏幕快照 2017-07-05 下午9.26.29.png](http://upload-images.jianshu.io/upload_images/1613923-3d9dc4c1597ea452.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

    [videoOutput setVideoSettings:[NSDictionary dictionaryWithObject:[NSNumber numberWithInt:kCVPixelFormatType_32BGRA] forKey:(id)kCVPixelBufferPixelFormatTypeKey]];
需要设置videoOutPut的输出方式。之后还需要修改GPUImageVideoCamera

的captureAsYUV属性为NO，然后发现可以正常录制视频了，不过因为使用GPUImage默认是YUV，转成RGB会有点小bug，可自行修改即可。
链接GPUImage源码地址：https://github.com/BradLarson/GPUImage
