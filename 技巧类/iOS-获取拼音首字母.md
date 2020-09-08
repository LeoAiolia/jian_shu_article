    //获取拼音首字母(传入汉字字符串, 返回大写拼音首字母)
    - (NSString *)firstCharactor:(NSString *)aString
    {
    //转成了可变字符串
    NSMutableString *str = [NSMutableString stringWithString:aString];
    //先转换为带声调的拼音
    CFStringTransform((CFMutableStringRef)str,NULL, kCFStringTransformMandarinLatin,NO);
    //再转换为不带声调的拼音
    CFStringTransform((CFMutableStringRef)str,NULL, kCFStringTransformStripDiacritics,NO);
    //转化为大写拼音
    NSString *pinYin = [str capitalizedString];
    //获取并返回首字母
    return [pinYin substringToIndex:1];
    }
此方法可解决简单的问题，但是对于多音字是有问题的，比如，长春，长沙，长辈，返回的都是Z。而长辈返回的应是c，此问题有更好的解决方法的话可以留言发表自己看法。
