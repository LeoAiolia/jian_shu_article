一、将系统时间转化为字符串输出
1.获取的时间为整型，可以通过字符串拼接进行输出显示
         
    //获取系统的日期和时间
    NSDate *senddate = [NSDate date];
    NSCalendar *cal = [NSCalendar currentCalendar];
    NSUInteger unitFlags = kCFCalendarUnitSecond|kCFCalendarUnitMinute|kCFCalendarUnitHour|NSCalendarUnitDay|NSCalendarUnitMonth|NSCalendarUnitYear;
    NSDateComponents *conponent = [cal components:unitFlags fromDate:senddate];
    NSInteger year = [conponent year];
    NSInteger month = [conponent month];
    NSInteger day = [conponent day];
    NSInteger hour = [conponent hour];
    NSInteger minute = [conponent minute];
    NSInteger second = [conponent second];
    //可以将通过字符串拼接将其转化为字符串
    NSString *dateString = [NSString stringWithFormat:@"%4ld年%2ld月%2ld日",year,month,day];
    NSString *locationString = [NSString stringWithFormat:@"%2ld:%2ld:%2ld",hour,minute,second]；

2.直接获取字符串形式的时间，这种方式也是比较常见的方式。
      
    NSDate  *date = [NSDate date];
    // NSDateFormatter格式化时间
    NSDateFormatter *formatter = [[NSDateFormatter alloc] init];
    //上下午a星期eeee
    [formatter setDateFormat:@"yyyy年MM月dd日eeee  a"];//MM为了区分分钟用大写
    NSString *dateStr = [formatter stringFromDate:date];
    //此时间就是xxxx年xx月xx日xx(英文周几) am/pm
    //其中星期和上下午也可设置
    //   [formatter setAMSymbol:@"上午"];
    //    [formatter setPMSymbol:@"下午"];
    //   [formatter setWeekdaySymbols:@[@"周日",@"周一",@"周二",@"周三",@"周四",@"周五",@"周六"]];
    [formatter setDateFormat:@"hh:mm:ss"];//hh和HH有12小时与24小时的区别
    NSString *timeStr = [formatter stringFromDate:date];
    NSLog(@"%@   %@",dateStr,timeStr);

二、计算取得授权的软件的access_token什么时候过期

    //dateWithTimeIntervalSinceNow这个方法是从现在算起距离多少秒得到的截止日期
    NSDate *date = [NSDate dateWithTimeIntervalSinceNow:[str integerValue]];//str为秒
    [[NSUserDefaults standardUserDefaults] setObject:date forKey:@"timeOut"];
    [[NSUserDefaults standardUserDefaults] synchronize];
    //接下来判断token是否过期先取出存取的date
    NSDate *date = [[NSUserDefaults standardUserDefaults] objectForKey:@"timeOut"];
    //这个是当前的时间
    NSDate *nowDate = [NSDate date];
    //NSComparisonResult枚举类型NSOrderedAscending升序
    // NSOrderedAscending升序, NSOrderedSame相同, NSOrderedDescending降序
    NSComparisonResult result =  [nowDate compare:date];
    //判断result是升序还是降序就可以知道是否过期了

三、将字符串形式的日期转化为NSDate

1.日期格式为：@“Fri Mar 25 19:43:39 +0800 2016”

    NSDateFormatter *formatter = [[NSDateFormatter alloc] init];
    [formatter setLocale:[[NSLocale alloc] initWithLocaleIdentifier:@"en_US"]];
    [formatter setDateFormat:@"EE MMM dd HH:mm:ss z yyyy"];//与上面的时间对应z对应时区
    NSDate *date = [formatter dateFromString:str];//此时的date就是NSDate形式的
    //若想将日期转化为自己想要的格式，可在采用将日期转化为字符串的方法，如下：
    [formatter setDateFormat:@"yyyy年MM月dd日HH:mm:ss"];
    NSString *timeStr = [formatter stringFromDate:date];
    NSLog(@"date = %@", timeStr);

2.将一个数字字符串如“ 20110826134106”转化为任意的日期时间格式，下面列举两种方法：
第一种 先转换为NSDate，再利用上面的方法转换。

    NSString *string = @"20110826134106";
    NSDateFormatter *inputFormatter = [[NSDateFormatter alloc]init];
    [inputFormatter setLocale:[[NSLocale alloc] initWithLocaleIdentifier:@"en_US"]];
    [inputFormatter setDateFormat:@"yyyyMMddHHmmss"];
    NSDate *inputDate = [inputFormatter dateFromString:string];
    NSLog(@"date = %@", inputDate);
    NSDateFormatter *outputFormatter = [[NSDateFormatter alloc]init];
    [outputFormatter setLocale:[NSLocale currentLocale]];
    [outputFormatter setDateFormat:@"yyyy年MM月dd日HH时mm分ss秒"];
    NSString *str = [outputFormatter stringFromDate:inputDate];
    NSLog(@"testDate:%@", str);
    //两次打印的结果为：
    date =2011-08-26 05:41:06 +0000
    testDate:2011年08月26日13时41分06秒
    //注意：上面的时间是美国时间，下面的没有设置

第二种   直接将字符串截取，然后拼接成日期

    NSString *str=@"20120403000000";
    NSString *dateStr=[NSString stringWithFormat:@"有效期至：%@年%@月%@日",
    [str substringWithRange:NSMakeRange(0,4)],
    [str substringWithRange:NSMakeRange(4,2)],
    [str substringWithRange:NSMakeRange(6,2)]];


附：iOS-NSDateFormatter格式说明：
        
    G:公元时代，例如AD公元
    yy:年的后2位
    yyyy:完整年
    MM:月，显示为1-12
    MMM:月，显示为英文月份简写,如Jan
    MMMM:月，显示为英文月份全称，如Janualy
    dd:日，2位数表示，如02
    d:日，1-2位显示，如2
    EEE:简写星期几，如Sun
    EEEE:全写星期几，如Sunday
    aa:上下午，AM/PM
    H:时，24小时制，0-23
    K：时，12小时制，0-11
    m:分，1-2位
    mm:分，2位
    s:秒，1-2位
    ss:秒，2位
    S:毫秒

常用日期结构：

    yyyy-MM-dd HH:mm:ss.SSS
    yyyy-MM-dd HH:mm:ss
    yyyy-MM-dd
    MM dd yyyy




















