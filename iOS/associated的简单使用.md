# Associated 的简单使用

Objective-C 的分类(Category)可以添加属性， 但是却不能添加实例变量。使用Associated可以间接地达到这一目的。


## 基本方法
```objective-c
    void objc_setAssociatedObject(id object, const void *key, id value, objc_AssociationPolicy policy);
    id objc_getAssociatedObject(id object, const void *key);
    void objc_removeAssociatedObjects(id object);
```

**objc_setAssociatedObject: 设置关联。**

解释：
- object : 需要设置关联的源对象
- key : 关联的key值，需要保证全局唯一。一般设置方式有以下几种
    1. 声明 static char kAssociatedObjectKey; ，使用 &kAssociatedObjectKey 作为 key 值;
    2. 声明 static void *kAssociatedObjectKey = &kAssociatedObjectKey; ，使用 kAssociatedObjectKey 作为 key 值；
    3. 用 selector ，使用 getter 方法的名称作为 key 值。

- value : 需要关联的值。
- policy : 设置关联的策略。
    ```objective-c
    typedef OBJC_ENUM(uintptr_t, objc_AssociationPolicy) {
            OBJC_ASSOCIATION_ASSIGN = 0,           /**弱引用关联对象，相当于 @property (assign)*/
            OBJC_ASSOCIATION_RETAIN_NONATOMIC = 1, /**强引用关联对象且是非原子性，相当于 @property (nonatomic, strong). */
            OBJC_ASSOCIATION_COPY_NONATOMIC = 3,   /**复制关联对象且是非原子性，相对于 @property (nonatomic, copy)*/
            OBJC_ASSOCIATION_RETAIN = 01401,       /**强引用关联对象且是原子性，相当于 @property (atomic, strong) */
            OBJC_ASSOCIATION_COPY = 01403          /**复制关联对象且是原子性，相对于 @property (atomic, copy) */
        };
    ```

**objc_getAssociatedObject：获取关联key对应的值。**

解释：
- object : 需要设置关联的源对象
- key : 需要设置关联的源对象

**objc_removeAssociatedObjects: 移除所有的关联对象。**

`例子：对NSString的分类，添加name属性。`
```objective-c

//.h文件：
@interface NSString (LCPrint)

@property (nonatomic, copy) NSString *name;

- (void)printName;

@end


//.m文件
#import "NSString+LCPrint.h"
#import <objc/runtime.h>

@implementation NSString (LCPrint)

- (void)setName:(NSString *)strName
{
    objc_setAssociatedObject(self, @selector(name), strName, OBJC_ASSOCIATION_RETAIN_NONATOMIC);
}

- (NSString *)name
{
   //_cmd 是隐藏的参数，表示当前方法的selector
   return objc_getAssociatedObject(self, _cmd);
}

- (void)printName
{
    NSLog(@"string name is %@", self.name);
}

@end


//简单调用

@implementation ViewController

- (void)viewDidLoad {
    [super viewDidLoad];
    // Do any additional setup after loading the view, typically from a nib.
    
    NSString *str = @"hello world";
    str.name = @"longxiaochi";
    
    [str printName];
}

打印：string name is longxiaochi

```

参考：【1】http://www.cocoachina.com/ios/20150629/12299.html