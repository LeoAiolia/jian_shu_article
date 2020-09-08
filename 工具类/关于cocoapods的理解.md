做iOS开发的人都知道，使用cocoapods来管理三方库十分的方便，而且目前有一个很火的概念，就是使用cocoapods来进行组件化开发，发布自己的cocoapods，可以是私有，也可以是公开的，很方便，方法有很多种，关于发布自己pod库推荐一个连接：http://www.code4app.com/blog-847095-1887.html

很长一段时间对于这个cocoapods，我都感觉很神秘，这些流程玩的多了，突然间就有了一点自己的体会，记录下来，供后来者学习与探讨。要看懂这篇文章不需要你有很深的cocopods使用技巧，只需要你玩过，或者探索或尝试发布过自己的pod库即可。

之前一直学习如何用组件化开发，学习了使用cocoapods发布了自己的pod库，每次都需要打tag，发布podspec,更新本地索引文件,等各种复杂的操作，但有一个疑问，实际开发中需要这么麻烦吗？如何保证开发效率呢，如何确保修改pod库文件后能实时看运行效果呢，毕竟开发不是一次就很完美的，而且网上的各种教程都是建立一个自己的pod私有库，一个组件一个私有库，对于个人开发很不友好，我就一直在思考，如何将发布pod库，打tag，推送等步骤给省略，做到真正的即时修改，即时看效果。本文主要就是为了解决这些问题而存在的。

首先看下，我们正常的一个pod库都有哪些东西。
1.一个.podspec文件
2.你自己写的库

开始，我对于这个.podspec很不理解，为什么需要这个文件，我们平时使用三方的时候并没有写过关于.podspec的文件，只需要在podfile文件里写pod 'AFNetworking' 然后pod install即可了，其实还有这种写法

    pod 'ISCommon', :path => '../podspec'

哈哈，我们的重头戏来了，这个podspec究竟是干什么的呢，发布过pod库的都知道，这个podspec里面是这样的

![image.png](https://upload-images.jianshu.io/upload_images/1613923-db5a544f1530d489.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


它里面记录了你的pod库信息，当你发布你的pod时，你会把你的podspec文件上传给cocoapods，而且你的pod文件也是寄存在公共的网站上，这时你安装cocoapods在你电脑上时，你电脑里会下载所有公共的podspec文件，然后pod install时会找到相应的podspec文件，根据文件的信息去拉取pod所在网站上的代码，然后安装。

同样的，我们发布一个私有库时，我们会将我们的pod库和podspec文件放在只有经过我们允许后，才能访问的到的网站上，这时我们安装时，只有可以访问的这个podspec文件和pod库的才能安装成功，其他人则会安装失败。

同样的，你当你本地新建一个podspec文件后，在podfile文件里指定你的podspec文件的地址，并同时根据podspec里的配置信息能正确的找到对应的pod库，不管你是否发布在了cocoapods上面，你都能安装成功，即你开发pod库时，可以在 你的库后面指定

    :path => 'podspec的路径'

然后pod install即可，此时你安装后你的pod文件夹下会是这样的

![image.png](https://upload-images.jianshu.io/upload_images/1613923-c442f2c462ab4a4f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

开发完成可以直接发布你的pod库，然后将:path去掉即可完成你的开发。

注意：podspec里的s.source_files 指向实际的pod库文件地址，且pod库需要在podspec的同级目录下，否则可能出现找不到的问题

参考文章：https://www.jianshu.com/p/oZfb8s
附Demo:https://github.com/LeoAiolia/ModulTest
