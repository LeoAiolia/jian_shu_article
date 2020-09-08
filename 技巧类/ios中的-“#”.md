可参考[Xcode Dev](http://blog.xcodev.com/)的文章，"#"的迷雾
http://blog.xcodev.com/posts/mists-of-the-sharp/
1.#
宏展开（即宏替换）后，#可以立即把其后的宏替换部分原封不动地进行字符串化.

    #define TEST(x) printf("square of " #x " is %d.\n",(x)*(x))
    void main() { 　　 int y =4;
    // #x被替换成字符串"y" 　　 TEST(y); // printf("square of " "y" " is %d.\n",(y)*(y))
    // #x被替换成字符串"6-3" TEST(6-3); // printf("square of " "6-3" " is %d.\n",(6-3)*(6-3))
    // #x被替换成字符串"y+3" TEST(y+3); // printf("square of " "y+3" " is %d.\n",(y+3)*(y+3)) }
输出结果：

    square of y is 16. square of 6-3 is 9. square of y+3 is 49.

2.## 
预处理拼接符，或者称其为宏拼接符
用于类似函数的宏的替换部分，还可以用于类似对象的宏的替换部分。##放在宏的替换部分的前面，用于宏展开（即宏替换）后，立即将宏中位于##右边的宏替换部分与该宏中位于##左边的部分相拼接至一个整体。

    // 宏定义
    #define XNAME(n) x##n 
    // 宏调用 int XNAME(4) = 1； 
    // 宏展开（即宏替换）后，我们得到： int x4 = 1;
    // 这也就体现出了##对其左右部分（即左x和右4）的拼接作用，最终拼接成x4

#
单例的宏定义一般如下：

    // @interface
    #define singleton_interface(className) \
    + (className *)shared##className;

    // @implementation
    #define singleton_implementation(className) \
    static className *_instance; \

    + (id)allocWithZone:(NSZone *)zone \
    { \
        static dispatch_once_t onceToken; \
        dispatch_once(&onceToken, ^{ \
            _instance = [super allocWithZone:zone]; \
        }); \
        return _instance; \
    } \

    + (className *)shared##className \
    { \
        static dispatch_once_t onceToken; \
        dispatch_once(&onceToken, ^{ \
            _instance = [[self alloc] init]; \
        }); \
        return _instance; \
    }\

可以看到，上面的+ (ClassName *)shared##ClassName;就用到了##的宏拼接作用

3.##在宏定义中可以放在__VA_ARGS__之前表示可变参数可以为空，否则的话可变参数至少为一个了。

    #define CSLOG(format, ...) NSLog(format, ##__VA_ARGS__)
    CSLOG(@"Don't make an error!");

上面代码中CSLOG中只有一个参数，如果不加##，则MLOG至少需要两个参数，在Xcode里将会出现编译错误。
