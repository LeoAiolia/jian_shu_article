###四舍五入
1.整数

    numberToRound = 4.51;
    int result = (int)roundf(numberToRound);
    NSLog(@"roundf(%f) = %d", numberToRound, result); 
    // roundf(4.510000) = 5
    numberToRound = 10.49;

    result = (int)roundf(numberToRound);
    NSLog(@"roundf(%f) = %d", numberToRound, result); 
    // roundf(10.490000) = 10

    numberToRound = -2.49;
    result = (int)roundf(numberToRound);
    NSLog(@"roundf(%f) = %d", numberToRound, result); 
    //  roundf(-2.490000) = -2

    numberToRound = -3.51;
    result = (int)roundf(numberToRound);
    NSLog(@"roundf(%f) = %d", numberToRound, result); 
    // roundf(-3.510000) = -4
2.保留小数

    CGFloat numberToRound = 4.51;
    NSLog(@"%.1f",numberToRound);
    //  4.5

###进一法
1.整数

    CGFloat numberToRound;
    int result;
    numberToRound = 4.51;
    result = (int)ceilf(numberToRound);
    NSLog(@"ceilf(%f) = %d", numberToRound, result);
    // ceilf(4.510000) = 5
  
    numberToRound = 10.49;
    result = (int)ceilf(numberToRound);
    NSLog(@"ceilf(%f) = %d", numberToRound, result);
    // ceilf(10.490000) = 11
  
    numberToRound = -2.49;
    result = (int)ceilf(numberToRound);
    NSLog(@"ceilf(%f) = %d", numberToRound, result);
    // ceilf(-2.490000) = -2
  
    numberToRound = -3.51;
    result = (int)ceilf(numberToRound);
    NSLog(@"ceilf(%f) = %d", numberToRound, result);
    // ceilf(-3.510000) = -3

2.进一时保留小数

    CGFloat numberToRound = 453210;
    NSLog(@"%.1fW",ceil(numberToRound/1000.0)/10.f);
    // 45.4W

###去尾法
1.整数

    int result;
    CGFloat numberToRound = 4.51;
    result = (int)floorf(numberToRound);
    NSLog(@"floorf(%f) = %d", numberToRound, result);
    // floorf(4.510000) = 4
  
    numberToRound = 10.49;
    result = (int)floorf(numberToRound);
    NSLog(@"floorf(%f) = %d", numberToRound, result);
    // floorf(10.490000) = 10
  
    numberToRound = -2.49;
    result = (int)floorf(numberToRound);
    NSLog(@"floorf(%f) = %d", numberToRound, result);
    // floorf(-2.490000) = -3
  
    numberToRound = -3.51;
    result = (int)floorf(numberToRound);
    NSLog(@"floorf(%f) = %d", numberToRound, result);
    // floorf(-3.510000) = -4

2.去尾时保留小数

      CGFloat numberToRound = 453210;
      // 保留一位
      NSLog(@"%.1fW",floorf(numberToRound/1000.0)/10.f);
      // 45.3W

      CGFloat numberToRound = 45.9210;
      // 保留两位小数点
      NSLog(@"%.2f",floorf(numberToRound*100.0)/100.f);
      // 45.92
这种方式比较简单，但是也很灵活多变，使用时需要多加注意。
###oc方法
    /**
     返回一个字符串，该字符串是进行格式化后的，格式化有三种，四舍五入，进一，去尾

     @param value 需要格式化的value
     @param position 保留的小数点
     @param roundingMode NSRoundPlain,四舍五入 NSRoundDown,去尾 NSRoundUp,进一
     @return 字符串
     */
    + (NSString*)stringWithRoundValue:(CGFloat)value
                           afterPoint:(NSUInteger)position
                         roundingMode:(NSRoundingMode)roundingMode {
      /*
       NSRoundPlain,   // 四舍五入
       NSRoundDown,    // 去尾
       NSRoundUp,      // 进一
      */
      NSDecimalNumberHandler* handler =
      [NSDecimalNumberHandler decimalNumberHandlerWithRoundingMode:roundingMode
                                                             scale:position
                                                  raiseOnExactness:NO
                                                   raiseOnOverflow:NO
                                                  raiseOnUnderflow:NO
                                               raiseOnDivideByZero:NO];
      NSDecimalNumber* ouncesDecimal;
      NSDecimalNumber* roundedOunces;
      
      ouncesDecimal = [[NSDecimalNumber alloc] initWithFloat:value];
      roundedOunces = [ouncesDecimal
                       decimalNumberByRoundingAccordingToBehavior:handler];
      return [NSString stringWithFormat:@"%@", roundedOunces];
    }
