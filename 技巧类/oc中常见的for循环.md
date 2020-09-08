**1.oc中常见的for循环有哪几种？**
>1.for循环
>2.forin
>3.枚举器Block块
>4.dispatch_apply函数
>5.ReactiveCocoa 遍历方法
>6.迭代器模式NSEnumerator

**2.使用方式**
1.for循环
```
//for--数组遍历
 for (int i = 0; i < self.traverseArray.count; i++) {
    NSLog(@"%@",self.traverseArray[i]);
 }

//for--字典遍历
NSArray *dictionaryArray = [self.traverseDictionary allKeys];    
for (int i = 0 ; i < dictionaryArray.count; i++) {
    NSLog(@"key = %@",dictionaryArray[i]);
}
```
2.forin
forin 遍历 又称快速遍历 简单实用 比for 循环等级高些 与for循环最明显的区别就是看不到循环次数及索引情况。数组是有序的 for循环过程中也是有序的，forin遍历过程中是根据数组中数据添加顺序而定的。
```
for (NSString *str in self.traverseArray) {
    NSLog(@"%@",str);
 }
```
3.枚举器
枚举器是一种苹果官方推荐的更加面向对象的一种遍历方式，相比于for循环,它具有高度解耦、面向对象、使用方便等优势
```
// 正序遍历数组
[self.traverseArray enumerateObjectsUsingBlock:^(id  _Nonnull obj, NSUInteger idx, BOOL * _Nonnull stop) {
    NSLog(@"正序%@",obj);
}];
    
// 逆序遍历数组
[self.traverseArray enumerateObjectsWithOptions:NSEnumerationReverse usingBlock:^(id  _Nonnull obj, NSUInteger idx, BOOL * _Nonnull stop) {
     NSLog(@"逆序%@",obj);
}];
    
// 遍历字典
#warning 字典是无序的不存在正序逆序
 [self.traverseDictionary enumerateKeysAndObjectsUsingBlock:^(id key, id value, BOOL *stop) {
     NSLog(@"key:%@->value%@",key,value);
 }];
```
4.dispatch_apply函数
GCD dispatch_apply函数是一个同步调用，block任务执行n次后才返回。该函数比较适合处理耗时较长、迭代次数较多的情况。
```
dispatch_queue_t queue = dispatch_get_global_queue(0, 0); 
dispatch_apply(self.traverseArray.count, queue, ^(size_t insex) {
   NSLog(@"%@",self.traverseArray[insex]);
});
```
5.ReactiveCocoa 遍历方法
ReactiveCocoa 中遍历主要是针对元组RACTuple，对于数组、字典的遍历都会包装成RACTuple进行处理。使用方法先集成ReactiveCocoa [点这里](https://www.jianshu.com/p/badcc5576305)  
集成方法可使用cocopods的podfile加入 pod ‘ReactiveObjC’
使用文件引入中 #import "ReactiveObjC/ReactiveObjC.h"
```
 // 数组遍历
[self.traverseArray.rac_sequence.signal subscribeNext:^(id x) {
    NSLog(@"%@",x);
}];

 // 字典遍历 相当于元组数据
[self.traverseDictionary.rac_sequence.signal subscribeNext:^(id x) {
    // 解包元组，会把元组的值，按顺序给参数里面的变量赋值
    RACTupleUnpack(NSString *key,NSString*value) = x;
    NSLog(@"key=%@ value=%@",key,value);
}];
```
6.迭代器模式
```
    NSEnumerator *enumerator = [arrayM objectEnumerator];
    id object = nil;
    while ((object = [enumerator nextObject]) != nil) {
        NSLog(@"object=%@", object);
    }
```
这里我们可以看到没有下标了，通过nextObject的方法来遍历。这个方法的好处是对于遍历NSDictionary和NSSet代码也比较类似，不便的是对于下标的处理会不方便，另外反向遍历需要用reverseObjectEnumerator方法。

**2.效率如何？**
以上几种遍历方式 在100、10000、100000次遍历所耗时长。
![耗时时间表.png](https://upload-images.jianshu.io/upload_images/1613923-84da636352b59848.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**3.遍历一个可变数组时，如果要对其进行删除操作需要注意什么**
之前参加过一个面试时，面试官问道了这个问题，一般来说，在我们遍历数组时，尽量不要更改这个数组，所以通常我们的做法都是这样
```
    NSMutableArray* arrayM = @[@1,@2,@3,@4,@5,@6,@7,@8,@9].mutableCopy;
    NSArray* array = arrayM.copy;
    for (int i = 0; i < array.count; i++) {
        id object = array[i];
        if (5 == [object intValue]) {
            [arrayM removeObject:object];
        }
    }
```
即：对这个可变的数组进行一份copy，遍历这个copy的数组，然后操作这个可变的数组。
当说出这个方案后，面试官继续问，如果不进行copy，就直接在for循环里删除，又该如何？
当时因为思维受限，一直想不到好的办法，就说那就在删除的时候加判断，防止遍历时崩溃。此时面试官给了一个提示，那倒序遍历呢？当时以为是对我说加判断的反问，没多想，也就没答出来，回来后又想了下
如果倒序遍历数组，此时在删除满足条件的数据，这时候并不会出现越界的现象，如：
```
    NSMutableArray* arrayM = @[@1,@2,@3,@4,@5,@6,@7,@8,@9].mutableCopy;
    for (NSInteger i = arrayM.count - 1; i >= 0; i--) {
        id object = arrayM[i];
        if (5 == [object intValue]) {
            [arrayM removeObject:object];
        }
    }
```
使用这种方式，虽然删除会影响到数组的索引，但是影响的也只是已经遍历过的，未遍历过的元素索引并不会受影响。
**4.关于使用for循环时是否是有序**
```
    NSMutableArray* arrayM = @[@1,@2,@3,@4,@5,@6,@7,@8,@9].mutableCopy;
    NSLog(@"--------1");
    [arrayM enumerateObjectsUsingBlock:^(id  _Nonnull obj, NSUInteger idx, BOOL * _Nonnull stop) {
        NSLog(@"--->obj:%@ -->idx:%@",obj,@(idx));
    }];
    NSLog(@"--------2");
    [arrayM enumerateObjectsWithOptions:NSEnumerationConcurrent usingBlock:^(id  _Nonnull obj, NSUInteger idx, BOOL * _Nonnull stop) {
        NSLog(@"--->obj:%@ -->idx:%@",obj,@(idx));
    }];
    NSLog(@"--------3");
    [arrayM enumerateObjectsWithOptions:NSEnumerationReverse usingBlock:^(id  _Nonnull obj, NSUInteger idx, BOOL * _Nonnull stop) {
        NSLog(@"--->obj:%@ -->idx:%@",obj,@(idx));
    }];
    NSLog(@"--------4");
    dispatch_queue_t globleQueue = dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT, 0);
    dispatch_apply(arrayM.count, globleQueue, ^(size_t size) {
        NSLog(@"--->obj:%@ -->idx:%@",arrayM[size],@(size));
    });
    NSLog(@"--------5");
```
打印结果如下：
```
2019-04-25 15:05:19.974789+0800 ISBaseDemo[13306:805852] --------1
2019-04-25 15:05:19.974812+0800 ISBaseDemo[13306:805852] --->obj:1 -->idx:0
2019-04-25 15:05:19.974821+0800 ISBaseDemo[13306:805852] --->obj:2 -->idx:1
2019-04-25 15:05:19.974829+0800 ISBaseDemo[13306:805852] --->obj:3 -->idx:2
2019-04-25 15:05:19.974836+0800 ISBaseDemo[13306:805852] --->obj:4 -->idx:3
2019-04-25 15:05:19.974843+0800 ISBaseDemo[13306:805852] --->obj:5 -->idx:4
2019-04-25 15:05:19.974849+0800 ISBaseDemo[13306:805852] --->obj:6 -->idx:5
2019-04-25 15:05:19.974856+0800 ISBaseDemo[13306:805852] --->obj:7 -->idx:6
2019-04-25 15:05:19.974863+0800 ISBaseDemo[13306:805852] --->obj:8 -->idx:7
2019-04-25 15:05:19.974870+0800 ISBaseDemo[13306:805852] --->obj:9 -->idx:8
2019-04-25 15:05:19.974877+0800 ISBaseDemo[13306:805852] --------2
2019-04-25 15:05:19.974913+0800 ISBaseDemo[13306:805852] --->obj:2 -->idx:1
2019-04-25 15:05:19.974923+0800 ISBaseDemo[13306:805852] --->obj:5 -->idx:4
2019-04-25 15:05:19.974921+0800 ISBaseDemo[13306:806007] --->obj:3 -->idx:2
2019-04-25 15:05:19.974931+0800 ISBaseDemo[13306:805852] --->obj:6 -->idx:5
2019-04-25 15:05:19.974920+0800 ISBaseDemo[13306:805984] --->obj:1 -->idx:0
2019-04-25 15:05:19.974936+0800 ISBaseDemo[13306:806007] --->obj:7 -->idx:6
2019-04-25 15:05:19.974939+0800 ISBaseDemo[13306:805852] --->obj:8 -->idx:7
2019-04-25 15:05:19.974929+0800 ISBaseDemo[13306:806012] --->obj:4 -->idx:3
2019-04-25 15:05:19.974946+0800 ISBaseDemo[13306:806007] --->obj:9 -->idx:8
2019-04-25 15:05:19.974986+0800 ISBaseDemo[13306:805852] --------3
2019-04-25 15:05:19.974998+0800 ISBaseDemo[13306:805852] --->obj:9 -->idx:8
2019-04-25 15:05:19.975006+0800 ISBaseDemo[13306:805852] --->obj:8 -->idx:7
2019-04-25 15:05:19.975014+0800 ISBaseDemo[13306:805852] --->obj:7 -->idx:6
2019-04-25 15:05:19.975021+0800 ISBaseDemo[13306:805852] --->obj:6 -->idx:5
2019-04-25 15:05:19.975027+0800 ISBaseDemo[13306:805852] --->obj:5 -->idx:4
2019-04-25 15:05:19.975034+0800 ISBaseDemo[13306:805852] --->obj:4 -->idx:3
2019-04-25 15:05:19.975040+0800 ISBaseDemo[13306:805852] --->obj:3 -->idx:2
2019-04-25 15:05:19.975050+0800 ISBaseDemo[13306:805852] --->obj:2 -->idx:1
2019-04-25 15:05:19.975057+0800 ISBaseDemo[13306:805852] --->obj:1 -->idx:0
2019-04-25 15:05:19.975063+0800 ISBaseDemo[13306:805852] --------4
2019-04-25 15:05:19.975087+0800 ISBaseDemo[13306:805852] --->obj:1 -->idx:0
2019-04-25 15:05:19.975098+0800 ISBaseDemo[13306:805852] --->obj:2 -->idx:1
2019-04-25 15:05:19.975106+0800 ISBaseDemo[13306:805852] --->obj:5 -->idx:4
2019-04-25 15:05:19.975105+0800 ISBaseDemo[13306:805984] --->obj:4 -->idx:3
2019-04-25 15:05:19.975113+0800 ISBaseDemo[13306:805852] --->obj:6 -->idx:5
2019-04-25 15:05:19.975116+0800 ISBaseDemo[13306:805984] --->obj:7 -->idx:6
2019-04-25 15:05:19.975120+0800 ISBaseDemo[13306:805852] --->obj:8 -->idx:7
2019-04-25 15:05:19.975123+0800 ISBaseDemo[13306:805984] --->obj:9 -->idx:8
2019-04-25 15:05:19.975127+0800 ISBaseDemo[13306:806014] --->obj:3 -->idx:2
2019-04-25 15:05:19.975182+0800 ISBaseDemo[13306:805852] --------5
```
从打印的数据可知，
>枚举器无参的block块是有序的，传参NSEnumerationReverse 也是有序的，只不过是倒序，而传参NSEnumerationConcurrent的block块和dispatch_apply这种方式是无序的


**5.使用for循环时的注意事项**
>1.该循环内产生大量的临时对象，直至循环结束才释放，可能导致内存泄漏，在循环中创建自己的autoReleasePool，能够及时释放占用内存大的临时变量，减少内存占用峰值。
>2.for in数组时, 同时对其进行删减操会造成崩溃
