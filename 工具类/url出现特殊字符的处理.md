之前在对url处理时，使用的是
nickName = [nickName stringByAddingPercentEscapesUsingEncoding:NSUTF8StringEncoding];

一直都没问题，直到一个用户，将昵称改为了

 >宝贝，来撩我<>@""&$)( 

这时候,处理的结果是：

>%E5%AE%9D%E8%B4%9D%EF%BC%8C%E6%9D%A5%E6%92%A9%E6%88%91%3C%3E@%22%22&$)(

拼接到url里后，很明显的出现了问题，最后被有特殊字体&$)(
查api发现这个方法在iOS9已经废弃了，推荐使用新的api
stringByAddingPercentEncodingWithAllowedCharacters

为什么这个旧方法会有问题呢，查资料发现

>stringByAddingPercentEscapesUsingEncoding(只对 `#%^{}[]|\"<> 加空格共14个字符编码，不包括”&?”等符号), ios9将淘汰，建议用stringByAddingPercentEncodingWithAllowedCharacters方法

stringByAddingPercentEncodingWithAllowedCharacters这个方法需要传一个 NSCharacterSet 对象 如:[NSCharacterSet  URLQueryAllowedCharacterSet]

```
URLFragmentAllowedCharacterSet         "#%<>[\]^`{|}
URLHostAllowedCharacterSet             "#%/<>?@\^`{|}
URLPasswordAllowedCharacterSet         "#%/:<>?@[\]^`{|}
URLPathAllowedCharacterSet             "#%;<>?[\]^`{|}
URLQueryAllowedCharacterSet            "#%<>[\]^`{|}
URLUserAllowedCharacterSet             "#%/:<>?@[\]^`
```
还有另一种方法，可以自定义你需要过滤的特殊字符
```
+ (NSString*)utf8StringWithString:(NSString*)string {
  if (string == nil) {
    return @"";
  }
  NSString* charactersToEscape = @":/?#[]@!$ &'()*+,;=\"<>%{}|\\^~`";
  NSCharacterSet* allowedCharacters = [[NSCharacterSet characterSetWithCharactersInString:charactersToEscape] invertedSet];
  NSString* encodedUrl = [string stringByAddingPercentEncodingWithAllowedCharacters:allowedCharacters];
  return encodedUrl;
}
```
在之前还有一种方法是
```
+ (NSString*)utf8WithString:(NSString*)string {
  if (string == nil) {
    return @"";
  }
  return (NSString*)CFBridgingRelease(CFURLCreateStringByAddingPercentEscapes(
      kCFAllocatorDefault, (__bridge CFStringRef)string, NULL,
      CFSTR(":/?#[]@!$ &'()*+,;=\"<>%{}|\\^~`"), kCFStringEncodingUTF8));
}
```
但是这个方法在iOS9.0也被废弃了，所以推荐使用stringByAddingPercentEncodingWithAllowedCharacters这种方法，这个方法是iOS7.0出现的，目前主流兼容到8.0的可以放心使用。
相对之前的方式，使用新的方法后，处理结果为
>%E5%AE%9D%E8%B4%9D%EF%BC%8C%E6%9D%A5%E6%92%A9%E6%88%91%3C%3E%40%22%22%26%24%29%28

附： [NSCharacter​Set](https://nshipster.cn/nscharacterset/)

