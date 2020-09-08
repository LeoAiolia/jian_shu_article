1.打开终端，cd到跟文件夹。
2.输入命令行
```
find . "(" -name "*.m" -or -name "*.mm" -or -name "*.cpp" -or -name "*.h" -or -name "*.rss" -or -name "*.swift" ")" -print | xargs wc -l
```
点击回车就会出现结果了.
3.终端显示结果如下：
```
      17 ./demo/AppDelegate.h
      39 ./demo/ViewController.m
      16 ./demo/main.m
      51 ./demo/AppDelegate.m
      17 ./demo/ViewController.h
     140 total
```
每一行前面的数字表示,某一个.m或者.h文件中的代码行数
最后显示的140 total就是项目所有的代码行数
