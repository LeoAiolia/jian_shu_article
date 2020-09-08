-2016.08.09

---
新版本的cocopods安装需要ruby环境大于2.2.2，但现在大多数的都是2.0.0,自己给电脑升级了一下，遇到不少坑，写篇文章记录一下。

1.安装RVM

RVM:Ruby Version Manager,Ruby版本管理器，包括Ruby的版本管理和Gem库管理(gemset)
安装命令：

    $ curl -L get.rvm.io | bash -s stable  

等待一段时间后就可以成功安装好 RVM。
     
    1.$ source ~/.bashrc 
    2.$ source ~/.bash_profile  

测试是否安装正常
          
    rvm -v  


安装好之后的状态类似于下图

![20130627215551046.png](http://upload-images.jianshu.io/upload_images/1613923-3befedf1c934cc82.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


2、用RVM升级Ruby

     #查看当前ruby版本  
    $ ruby -v  
    ruby 2.0.0  
    #列出已知的ruby版本  
    $ rvm list known  
    #安装ruby 2.2.4  
    $ rvm install 2.2.4  
    
安装完成之后   

    ruby -v    
查看是否安装成功。
文件比较大，可能需要的时间较长，要耐心等待，中间或许还会有错误，我期间安装2.4.0失败了，于是安装了2.2.4，如果你也遇到同样的情况也可以换个版本试一试，祝你好运。

下图是我安装好之后的图片：

![2.png](http://upload-images.jianshu.io/upload_images/1613923-46bc4c4842cc4d4e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)




