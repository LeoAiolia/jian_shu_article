在oc中使用block时很普遍的，但是在使用时总会遇到会遇到各种报错的情况，现记录一下block语法。

    returnType (^blockName)(parameterTypes) = ^returnType(parameters) {...}; 

block作为属性：

    @property (nonatomic, copy, nullability) returnType (^blockName)(parameterTypes); 

作为参数时：

    - (void)someMethodThatTakesABlock:(returnType (^nullability)(parameterTypes))blockName;

方法调用时：

    [someObject someMethodThatTakesABlock:^returnType (parameters) {...}]; 

typedef时：

    typedef returnType (^TypeName)(parameterTypes);
    TypeName blockName = ^returnType(parameters) {...}; 
