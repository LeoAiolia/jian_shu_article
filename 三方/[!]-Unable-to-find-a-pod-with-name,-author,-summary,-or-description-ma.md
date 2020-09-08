pod 安装完成后，会提示Setup completed，但是pod search afnetworking时，会提示[!] Unable to find a pod with name, author, summary, or description matching `afnetworking`
这是因为 pod setup成功后会生成~/Library/Caches/CocoaPods/search_index.json文件，删除该文件，然后重新搜索就可以了。
终端输入
     
    rm ~/Library/Caches/CocoaPods/search_index.json

然后再执行pod search 就可以了

具体可参考文章：http://www.jianshu.com/p/b5e5cd053464?url_type=39&object_type=webpage&pos=1
