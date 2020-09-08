1.如下model
```
#import <Foundation/Foundation.h>

NS_ASSUME_NONNULL_BEGIN

@interface CSRAttentionModel : NSObject

@property(nonatomic) NSInteger createTime;

@end

NS_ASSUME_NONNULL_END
```

2.数组
```
  NSMutableArray *array = [NSMutableArray arrayWithCapacity:0];
  
  CSRAttentionModel* attentionModel = [[CSRAttentionModel alloc] init];
  attentionModel.createTime = 10;
  [array addObject:attentionModel];
  
  CSRAttentionModel* attentionModel2 = [[CSRAttentionModel alloc] init];
  attentionModel2.createTime = 10;
  [array addObject:attentionModel2];
  
  CSRAttentionModel* attentionModel3 = [[CSRAttentionModel alloc] init];
  attentionModel3.createTime = 8;
  [array addObject:attentionModel3];
  
  CSRAttentionModel* attentionModel4 = [[CSRAttentionModel alloc] init];
  attentionModel4.createTime = 4;
  [array addObject:attentionModel4];
  
  CSRAttentionModel* attentionModel5 = [[CSRAttentionModel alloc] init];
  attentionModel5.createTime = 17;
  [array addObject:attentionModel5];
```

3.根据model的属性createTime降序排序数组
```
NSSortDescriptor* sortDescriptor = [NSSortDescriptor sortDescriptorWithKey:@"createTime" ascending:NO];
NSArray* sortPackageResListArr = [array sortedArrayUsingDescriptors:[NSArray arrayWithObject:sortDescriptor]];
NSLog(@"%@",sortPackageResListArr);
```
5.排序前后对比
![排序前.png](https://upload-images.jianshu.io/upload_images/1613923-74e5370bc4050b46.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/300)

![排序后.png](https://upload-images.jianshu.io/upload_images/1613923-5bec63277f1f1725.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/300)
6.扩展：
若想升序排序，只需要将`ascending:`的值传`YES`即可。
若还有另一个属性`state`,想在此前排序的基础上，在增加一条规则，如：优先按`createTime`降序，其次按`state`升序排序，则可如下：
```
NSSortDescriptor* sortDescriptor1 = [NSSortDescriptor sortDescriptorWithKey:@"createTime" ascending:NO];
NSSortDescriptor* sortDescriptor2 = [NSSortDescriptor sortDescriptorWithKey:@"state" ascending:YES];
NSArray* sortPackageResListArr = [array sortedArrayUsingDescriptors:@[sortDescriptor1,sortDescriptor2]];
```
