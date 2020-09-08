swift枚举

基本的就不说了，直接说带参数的枚举
```
enum Animal {
    case people(String)
    case other
}
```
使用switch来判断时

```
let a = Animal.people("张三")
switch a {
  case .people(let name):
      print(name)
  case .other:
      print("other")
}
```
相信大多数人都知道这种方式，但是使用if判断呢，这就很有意思了。

```
let a = Animal.people("张三")
// 注意是=而不是==，类似 if let _ = a，是赋值操作。
if case .people(let name) = a {
     print(name)
}

// 只要枚举里带参数了，不带参数类型也需要使用case这样判断，也是=，而使用.other == a,就会报错。有意思。
if case .other = a {
    print(name)
}
```
