![A9E14108DF763789CB80A64656CBD343.png](https://upload-images.jianshu.io/upload_images/1613923-41679040f49c4dee.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
由上面的结果可知0是最宽，1最窄，但是排序的话如果一个一个去排，虽然也能完成，但是太累了，身为程序员，一定要让程序为我们服务。

    NSMutableDictionary* dic = [NSMutableDictionary dictionaryWithCapacity:0];
    for (NSInteger i = 0; i < 10; i++) {
      NSString* valueString = [NSString stringWithFormat:@"%@",@(i)];
      CGFloat width = [AppDelegate calculateTextLength:IS_FONT(20) givenText:valueString];
      [dic setObject:valueString forKey:@(width)];
    }
    NSArray *keyArr = dic.allKeys;
    NSArray *ascendingArr = [keyArr sortedArrayUsingComparator:^NSComparisonResult(id  _Nonnull obj1, id  _Nonnull obj2) {
      if ([obj1 floatValue] > [obj2 floatValue]) {
        return NSOrderedAscending;
      } else if ([obj1 floatValue] == [obj2 floatValue]) {
        return NSOrderedSame;
      } else {
        return NSOrderedDescending;
      }
    }];
    for (NSInteger i = 0; i<ascendingArr.count; i++) {
      NSNumber *key = ascendingArr[i];
      NSLog(@"--->i:%@ ---->width:%@\n",dic[key], key);
    }

    2018-08-03 17:06:00.002609+0800 ISBaseDemo[4122:423885] --->i:0 ---->width:13.490234375
    2018-08-03 17:06:00.002670+0800 ISBaseDemo[4122:423885] --->i:4 ---->width:13.451171875
    2018-08-03 17:06:00.002697+0800 ISBaseDemo[4122:423885] --->i:8 ---->width:13.34375
    2018-08-03 17:06:00.002721+0800 ISBaseDemo[4122:423885] --->i:9 ---->width:13.333984375
    2018-08-03 17:06:00.002745+0800 ISBaseDemo[4122:423885] --->i:3 ---->width:13.197265625
    2018-08-03 17:06:00.002769+0800 ISBaseDemo[4122:423885] --->i:5 ---->width:13.060546875
    2018-08-03 17:06:00.002793+0800 ISBaseDemo[4122:423885] --->i:2 ---->width:12.6796875
    2018-08-03 17:06:00.002819+0800 ISBaseDemo[4122:423885] --->i:7 ---->width:12.298828125
    2018-08-03 17:06:00.002843+0800 ISBaseDemo[4122:423885] --->i:1 ---->width:10.228515625

由此可知，字符宽度由高到低依次是：0，4，8，9，3，5，2，7，1.

附：

    + (CGFloat)calculateTextLength:(UIFont*)font givenText:(NSString*)text {
      NSDictionary* attribute = @{NSFontAttributeName : font};
      CGSize titleSize =
      [text boundingRectWithSize:CGSizeMake(MAXFLOAT, 0)
                         options:NSStringDrawingTruncatesLastVisibleLine |
       NSStringDrawingUsesLineFragmentOrigin |
       NSStringDrawingUsesFontLeading
                      attributes:attribute
                         context:nil]
      .size;
      CGFloat length = titleSize.width + 1.0f;
      
      return length;
    }
