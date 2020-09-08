## 1、问题描述

由于ZSH 的支持代码高亮，还有命令的提示功能，于是就安装了 oh my ZSH ，安装完成之后发现，原有的bash 指令还可以用，类似 于代码审核的工具arcanist在系统自带的Terminal里不能使用了，但是使用iterm2可以用，如`arc lint`   、`arc diff`  、`arc commit`
##解决办法
使用terminal
```
vim ~/.bash_profile
// 如下图1：找到bash_profile里失效指令的路径
// export PATH=$PATH:/Users/student/Arcanist/arcanist/bin
// 添加进： zshrc，如图2：
vim ~/.zshrc
// 最后：
source ~/.zshrc
```

![图片1.png](https://upload-images.jianshu.io/upload_images/1613923-583e129fe15b65d9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/520)

![图片2.png](https://upload-images.jianshu.io/upload_images/1613923-0e0faf85e900d0b4.png?imageMogr2/strip%7CimageView2/2/w/520)
