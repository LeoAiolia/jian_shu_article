#问题来源
在iOS开发中，和货币价格计算相关的，是需要注意计算精度的问题的。即使只是两位小数，也会出现误差。使用float类型运算，是完全不够的。 如： 原始数据： 

    float a = 0.01;
    int b = 99999999;
    double c = 0.0;

1：使用浮点运算

    c = a*b;
    NSLog(@"%f",c);
    NSLog(@"%.2f",c);
输出结果：
            
    2016-09-01 00:42:43.216 MoneyCalculateTest[1367:35625] ------1000000.000000
    2016-09-01 00:42:43.216 MoneyCalculateTest[1367:35625] ------1000000.00

可见使用浮点型来计算是有误差的。 

2.使用类型转换，提高精度

    c = a*(double)b;
    NSLog(@"%f",c);
    NSLog(@"%.2f",c);

输出：

    2016-09-01 00:42:43.216 MoneyCalculateTest[1367:35625] 999999.967648
    2016-09-01 00:42:43.216 MoneyCalculateTest[1367:35625] 999999.97

Double运算的精度是提高了，可是浮点数的数值早已经出现了精度的不准确，即使存储空间足够，同样还是不准确的数值。 

3.通过和NSString的转换，将计算的原始数据转换为纯粹的double类型的数据。

    NSString *objA = [NSString stringWithFormat:@"%.2f", a];
    NSString *objB = [NSString stringWithFormat:@"%.2f", (double)b];
    c = [objA doubleValue] * [objB doubleValue];NSLog(@"%.2f",c);

输出：

    2016-09-01 00:42:43.217 MoneyCalculateTest[1367:35625] 99999999.00

计算的结果还是比较准确的，不过需要做格式化输入和格式化输出的处理。同时使用NSString来转换，这样的写法看起来比较奇怪，但是精确度已经可以达到要求了。 

#系统提供的计算API

事实上， 除了使用字符串来转换提高精确度，系统已经为我们封装好了一套专门用于计算的API，通过NSDecimalNumber提供的计算方式，可以很好的计算出准确的精度的数据，同时不需要使用格式化输出等。其计算的精度是比较高的，这是官方建议的货币计算的API，对乘除等计算都有单独的API接口来提供。
 
   可以将以下封装好的方法写在NString的类别中，这样就可以很方便的调用。 

1.加

    + (NSString *)addNumberMutiplyWithString:(NSString *)multiplierValue secondString:(NSString *)multiplicandValue
    { 
      NSDecimalNumber *multiplierNumber = [NSDecimalNumber decimalNumberWithString:multiplierValue]; 
      NSDecimalNumber *multiplicandNumber = [NSDecimalNumber decimalNumberWithString:multiplicandValue]; 
      NSDecimalNumber *product = [multiplicandNumber decimalNumberByAdding:multiplierNumber];  
    return [product stringValue];
    }

2.减法
    + (NSString *)subNumberMutiplyWithString:(NSString *)multiplierValue secondString:(NSString *)multiplicandValue
    { 
     NSDecimalNumber *multiplierNumber = [NSDecimalNumber decimalNumberWithString:multiplierValue];
     NSDecimalNumber *multiplicandNumber = [NSDecimalNumber decimalNumberWithString:multiplicandValue];
     NSDecimalNumber *product = [multiplicandNumber decimalNumberBySubtracting:multiplierNumber]; 
     return [product stringValue];
    }
3.乘法
    + (NSString *)mutiplyNumberMutiplyWithString:(NSString *)multiplierValue secondString:(NSString *)multiplicandValue
    {
     NSDecimalNumber *multiplierNumber = [NSDecimalNumber decimalNumberWithString:multiplierValue]; 
     NSDecimalNumber *multiplicandNumber = [NSDecimalNumber decimalNumberWithString:multiplicandValue]; 
     NSDecimalNumber *product = [multiplicandNumber decimalNumberByMultiplyingBy:multiplierNumber]; 
     return [product stringValue];
    }

4.除法

    + (NSString *)divisionNumberMutiplyWithString:(NSString *)multiplierValue secondString:(NSString *)multiplicandValue
    { 
     NSDecimalNumber *multiplierNumber = [NSDecimalNumber decimalNumberWithString:multiplierValue];
     NSDecimalNumber *multiplicandNumber = [NSDecimalNumber decimalNumberWithString:multiplicandValue];
     NSDecimalNumber *product = [multiplicandNumber decimalNumberByDividingBy:multiplierNumber];  
     return [product stringValue];
    }
5.简单的应用

    - (void)viewDidLoad
    {
      [super viewDidLoad];
            
      float a = 0.01;
      int b = 99999999;
      double c = 0.0;

      //调用第一种方法来计算
      NSString *str =[self mutiplyNumberMutiplyWithString:[NSString stringWithFormat:@"%f",a] secondString:[NSString stringWithFormat:@"%d",b]];
      NSLog(@"%@",str);
      c = [str doubleValue];

      //调用第二种方法
      NSLog(@"%@",decimalNumberMutiplyWithString([NSString stringWithFormat:@"%f",a], [NSString stringWithFormat:@"%d",b]));
            
    }
    //乘法的封装方法一
    - (NSString *)mutiplyNumberMutiplyWithString:(NSString *)multiplierValue secondString:(NSString *)multiplicandValue
    {
       NSDecimalNumber *multiplierNumber = [NSDecimalNumber decimalNumberWithString:multiplierValue];
       NSDecimalNumber *multiplicandNumber = [NSDecimalNumber decimalNumberWithString:multiplicandValue];
       NSDecimalNumber *product = [multiplicandNumber decimalNumberByMultiplyingBy:multiplierNumber];
            
       return [product stringValue];
    }
    //第二种封装方法，可以直接像函数一样调用该方法
    NSString *decimalNumberMutiplyWithString(NSString *multiplierValue,NSString *multiplicandValue)
    {       
       NSDecimalNumber *multiplierNumber = [NSDecimalNumber decimalNumberWithString:multiplierValue];
       NSDecimalNumber *multiplicandNumber = [NSDecimalNumber decimalNumberWithString:multiplicandValue];
       NSDecimalNumber *product = [multiplicandNumber decimalNumberByMultiplyingBy:multiplierNumber];
       return [product stringValue];          
    }
输出结果

    2016-09-01 02:05:57.714 MoneyCalculateTest[2031:89967] 999999.99
    2016-09-01 02:05:57.715 MoneyCalculateTest[2031:89967] 999999.99



精确度十分OK，因此当我们进行数值计算的时候，优先考虑使用系统的方法来进行计算。 

















