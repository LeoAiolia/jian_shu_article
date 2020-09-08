####前言
在 iOS中可以直接调用某个对象的消息方式有两种：
一种是performSelector:withObject；
再一种就是NSInvocation。
第一种方式比较简单，能完成简单的调用。但是对于>2个的参数或者有返回值的处理，那performSelector:withObject就显得有点有心无力了，那么在这种情况下，我们就可以使用NSInvocation来进行这些相对复杂的操作
#####NSInvocation的基本使用
#####方法签名类

    // 方法签名中保存了方法的名称/参数/返回值，协同NSInvocation来进行消息的转发
    // 方法签名一般是用来设置参数和获取返回值的, 和方法的调用没有太大的关系
    //1、根据方法来初始化NSMethodSignature
    NSMethodSignature  *signature = [ViewController instanceMethodSignatureForSelector:@selector(run:)];

#####根据方法签名来创建NSInvocation对象

    - (void)viewDidLoad {
      [super viewDidLoad];

      //方法
      SEL selector = @selector(run);
      //初始化方法签名(方法的描述)   
       NSMethodSignature *signature = [[self class] instanceMethodSignatureForSelector:selector];
      // NSInvocation : 利用一个NSInvocation对象包装一次方法调用（方法调用者、方法名、方法参数、方法返回值）
      NSInvocation *invocation = [NSInvocation invocationWithMethodSignature:signature];
      //设置调用者
      invocation.target = self;
      //设置调用方法
      invocation.selector = selector;
      //设置参数
      NSUInteger object = 5;
      //参数从2开始，index为0表示target，1为_cmd 
      [invocation setArgument:&object atIndex:2];
      //调用方法
      [invocation invoke];
    }
    -(void)run:(NSInteger)num{
      NSLog(@"run");
    }

#####NSInvocation实现多参数的封装

 系统的NSObject提供的performSelector的方法只提供了最多两个参数的调用,我们可以使用NSInvocation封装一个多个参数的performSelector方法.

    #import <Foundation/Foundation.h>

    @interface NSObject (MutiPerform)

    -(id)performSelector:(SEL)Selector withObjects:(NSArray *)objects;
    @end

#

    #import "NSObject+MutiPerform.h"

    @implementation NSObject (MutiPerform)

    -(id)performSelector:(SEL)selector withObjects:(NSArray *)objects{

        //初始化方法签名
        NSMethodSignature *signature = [[self class] instanceMethodSignatureForSelector:selector];
    
        // 如果方法selector不存在
        if(signature == nil){

            // 抛出异常
            NSString *reason = [NSString stringWithFormat:@"%@方法不存在",NSStringFromSelector(selector)];
            @throw [NSException exceptionWithName:@"error" reason:reason userInfo:nil];
         }
    
        NSInvocation *invocation = [NSInvocation invocationWithMethodSignature:signature];

        invocation.target = self;
        invocation.selector = selector;
    
        //参数个数signature.numberOfArguments 默认有一个_cmd 一个target 所以要-2
        NSInteger paramsCount = signature.numberOfArguments - 2;
    
        // 当objects的个数多于函数的参数的时候,取前面的参数
        //当当objects的个数少于函数的参数的时候,不需要设置,默认为nil
        paramsCount = MIN(paramsCount, objects.count);
    
        for (NSInteger index = 0; index < paramsCount; index++) {
        
            id object = objects[index];

            // 对参数是nil的处理
            if([object isKindOfClass:[NSNull class]]) continue;
        
            [invocation setArgument:&object atIndex:index+2];

        }
        //调用方法
        [invocation invoke];

        // 获取返回值
        id returnValue = nil;
    
        //signature.methodReturnLength == 0 说明给方法没有返回值
        if (signature.methodReturnLength) {

            //获取返回值
            [invocation getReturnValue:&returnValue];
        }

        //可以通过signature.methodReturnType获得返回的类型编码，因此可以推断返回值的具体类型
        return returnValue;
    }



