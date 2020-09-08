ARC情况下，很多人喜欢使用strong来修饰NSString，一般来说，这样是没有什么大问题的，但是，下面来看一个例子：


![屏幕快照 2017-07-06 下午7.52.12.png](http://upload-images.jianshu.io/upload_images/1613923-488e7e1cf080c83a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


str1 是用 strong 修饰的，str2 是用 copy 修饰的。现在让它们两个都等于一个可变字符串 str。
修改 str，输出结果发现，str1 也跟着 str 变了，而 str2 没有变。再结合打印出来的内存地址分析可以发现：

######1.把可变的 str 赋给 strong 修饰的、不可变的 str1，没有产生新对象，二者指向同一个地址，所以 str 的改变会影响 str1。

######2.把可变的 str 赋给 copy 修饰的、不可变的 str2，产生了新对象，二者指向不同的地址，所以 str 的改变不会影响 str2。

为了避免这种情况的发生，NSString的修饰要使用copy




