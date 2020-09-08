经常使用cocoapods的人都知道有一个cocoapods的插件，但是有时候刚安装，怎么就是有问题，我今天遇到的问题是，cocoapods安装成功了，但是使用插件不可以，10.11 cocoapods安装命令:

     $ sudo gem install -n /usr/local/bin cocoapods
     
因此，也把路径设置为了/usr/local/bin了，但是使用插件进行安装的时候，就是报错：
     
     env: ruby_executable_hooks: No such file or directory
      
可以推断出是cocopods插件的GEM_PATH路径写错了，通过查资料发现
在终端输入命令：
    
    $ gem env

会显示内容如下图片：

![3.png](http://upload-images.jianshu.io/upload_images/1613923-7a9d47bc7355a9f4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)





找到SHELL PATH:
     - /Users/user/.rvm/gems/ruby-2.2.4/bin
     - /Users/user/.rvm/gems/ruby-2.2.4@global/bin
     - /Users/user/.rvm/rubies/ruby-2.2.4/bin
     - /usr/bin
     - /bin
     - /usr/sbin
     - /sbin
     - /usr/local/bin
     - /Users/user/.rvm/bin

通过一个一个试验，我是在将其路径设置为：

     /Users/user/.rvm/rubies/ruby-2.2.4/bin
     
的时候，便可以使用了。如下图：

![4.png](http://upload-images.jianshu.io/upload_images/1613923-18593089e224aed3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


如果上面方案仍没有解决问题，那么将GEM_PATH里写入 /usr/local/bin 执行cocoapods安装命令: 

     $ sudo gem install -n /usr/local/bin cocoa pods
     
附查看安装清单命令，可查看已安装应用及插件：

      $ gem list


如果有什么新的问题可以给我留言一起探讨。




