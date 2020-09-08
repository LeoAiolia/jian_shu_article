用来dump目标文件的class信息的工具。它利用Objective-C语言的runtime的特性，将存储在mach-O文件中的@interface和@protocol信息提取出来，并生成对应的.h文件。

## 安装步骤

1、下载地址：[http://stevenygard.com/projects/class-dump/](http://stevenygard.com/projects/class-dump/)

2、打开终端输入

    open /usr/local/bin



3、把dmg文件中的class-dump文件复制到/usr/local/bin

4、更改权限：终端输入

    sudo chmod 777 /usr/local/bin/class-dump



到这儿就安装完成了。

显示class-dump的用法和版本

    class-dump --help



## 使用方法

1、首先下载一个ipa文件，更改文件为zip格式，然后解压之后得到.app的目标文件

![image.jpeg](http://upload-images.jianshu.io/upload_images/1613923-639b564aa9c3da11.jpeg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

2、用终端输入命令class-dump -H [.app文件的路径] -o [输出文件夹路径]

    class-dump -H /Users/mac/Desktop/Payload/Kt.app -o /Users/mac/Desktop/Payload



就可以得到所有的.h文件了（在输出的文件夹里）
