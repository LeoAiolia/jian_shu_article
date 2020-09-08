一般编程我们都默认非零值就是TRUE，为零时FALSE。

那么在Objective-C中，又出现了YES、NO。咋回事呢。
```
typedef signed char BOOL
#define YES ((BOOL)1)
#define NO  ((BOOL)0)
```
>oc中明确规定1是YES,0是NO，当你直接拿Int与BOOL比较时，只要不是0和1，得到的结果都是false

看下面的代码

      int a = 8960;
      if (a) {
        ISLog(@"---1---");
      } else {
        ISLog(@"---2---");
      }
      // 这种方式很坑的。注意底部数据的打印
      if (a == YES) {
        ISLog(@"---3---");
      } else {
        ISLog(@"---4---");
      }
      BOOL b = a;
      if (b) {
        ISLog(@"---5---");
      } else {
        ISLog(@"---6---");
      }
  
      if (b == YES) {
        ISLog(@"---7---");
      } else {
        ISLog(@"---8---");
      }
     
      2018-06-08 16:28:51.143529+0800 ISBaseDemo[27344:15315162] -[ISHomePageViewController buttonClick:] [Line 119] ---1---
      2018-06-08 16:28:51.143617+0800 ISBaseDemo[27344:15315162] -[ISHomePageViewController buttonClick:] [Line 127] ---4---
      2018-06-08 16:28:51.143632+0800 ISBaseDemo[27344:15315162] -[ISHomePageViewController buttonClick:] [Line 131] ---5---
      2018-06-08 16:28:51.143643+0800 ISBaseDemo[27344:15315162] -[ISHomePageViewController buttonClick:] [Line 137] ---7---
      
可见，使用 if( a== YES)这种方式是有可能出现意料之外的情况的，但是使用if(a)这种方式并未出现意料外的情况，因此日常代码书写过程中，尽量避免出现if( a== YES)这种情况。

因最近公司搞代码review，使用的是google规范(公司领导比较中意google)，关于google规范中也有同样的说明，可看截图
![图片.png](https://upload-images.jianshu.io/upload_images/1613923-9b27c238aecf31a2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

附：google规范链接：
http://zh-google-styleguide.readthedocs.io/en/latest/google-objc-styleguide/features/#bool
