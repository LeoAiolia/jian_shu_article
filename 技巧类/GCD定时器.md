使用NSTimer定时器有几个缺点，
1.分线程需要自动管理RunLoop,
2.精确度不太高。
而使用GCD定时器是没有这些缺点的。代码如下：

 

        @interface ViewController ()

        @property (nonatomic,strong) dispatch_source_t timer;

        @end

        @implementation ViewController

        - (void)viewDidLoad {
            [super viewDidLoad];
            //创建一个队列
            dispatch_queue_t queue = dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT, 0);
            //dispatch_source_t 本质上是一个oc对象！！
            self.timer = dispatch_source_create(DISPATCH_SOURCE_TYPE_TIMER, 0, 0, queue);
            //GCD的时间参数
            dispatch_time_t start = DISPATCH_TIME_NOW;
            dispatch_time_t interval = 1 * NSEC_PER_SEC;
            dispatch_source_set_timer(self.timer, start, interval, 0);
            
            //设置定时器的回调
            dispatch_source_set_event_handler(self.timer, ^{
                
                NSLog(@"----%d",[NSThread isMainThread]);
                
            });
            
            //启动定时器
            dispatch_resume(self.timer);
        }    


打印的结果：
2017-06-13 21:55:34.777 GCDTimer[6623:2965563] ----0
说明该定时器是创建在分线程上了，销毁定时器可以调用

    dispatch_source_cancel(_timer);
