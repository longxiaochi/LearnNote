# OC 属性修饰符解释

## 简介
在ios5之前是MRC，内存需要程序员进行管理，ios5之后推出了ARC,不需要程序员手动管理内存，但对于非OC类，还是需要进行手动管理的，如c框架的对象。

oc主要的属性修饰符有以下几种：
- strong
- weak
- assign
- copy
- retain
- readwrite\readonly (读写权限)
- nonatomic\atomic (安全策略)

** 在MRC 和 ARC 的访问修饰符：**
    
    MRC: assign/ retain/ copy/  readwrite、readonly/ nonatomic、atomic  等。
    ARC: assign/ strong/ weak/ copy/ readwrite、readonly/ nonatomic、atomic  等。


ARC中apple会自动在需要进行内存管理的地方添加retain/release, 当对象的引用计数，即retainCount为0时，会自动释放内存，从而实现内存的自动管理。


## 详述
1. strong
    - 特点：强引用，指向并拥有对象,其修饰的对象的引用计数会+1。该对象的引用计数不为0则不会被销毁，可强制复制为nil来销毁它。
    - 用途：oc非基本数据类型的默认修饰符。

2. weak
    - 特点：弱引用，指向但不拥有对象，其修饰的对象引用计数不会增加。 当对象的引用计数为0时，其修饰的变量会自动赋值为nil, 即不会形成野指针。
    - 用途：
        - 修饰代理(delegate). 
        - 在block中，self常用弱引用形式。__weak typeof(self) weakSelf = self;

3. assign
    - 特点：其修饰的对象引用计数不会增加。
    - 用途：主要用于修饰基本的数据类型，如NSInteger、CGFloat等， 以及c的基本类型(int，float, bool)等。
    - 疑问：为什么assign主要用于修饰的基本类型？修饰oc对象会怎样？
        ```
        原因: 
        1.assign 修饰的对象引用计数不增加，但当对象被释放后，指针的地址依然存在，会造成野指针。 这点需要与weak作区分, weak修饰的对象的被释放后，指针地址会自动赋值为nil.
        2. 基本数据类型存放在栈中，系统会自动处理，不会造成野指针。
        ```

4. copy
    - 特点: 会重新复制出一个新的对象，原对象的引用计数不会发生改变，新的对象的引用计数加1.
    - 用途：主要用于有可变对象的非可变对象。如：NSString、NSArray、NSDictionary 等。
    

5. retain
    - 特点: 主要用在MRC模式下，其修饰的对象引用计数，即retainCount会+1。对象的引用计数不为0则不会被销毁,不会造成野指针。
    - 用途：retain一般用来修饰非NSString 的NSObject类和其子类。
    - 注意：retain只能修饰oc对象，不能修饰非oc对象，比如说CoreFoundation对象就是C语言框架，它没有引用计数，也不能用retain进行修饰。
     

6. readwrite\readonly (读写权限)
   - readwrite: 表示可读写。其修饰的对象可以被读取和修改。也是oc属性修饰符中默认值。
   - readonly: 表示只读。 其修饰的对象的只能被读取，而不能被修改。
   - 用法： 如果某属性对外部是只读的，而内部可写。则可以在.h文件中定义为readonly,在.m文件中定义为readwrite。

7. atomic\nonatomic (安全策略)
   - atomic: 原子性，代表着多线程安全，其在getter和setter加了锁(sychronize)。加锁会影响一定的性能，并要注意atomic 只能保证对象的读写这个操作是线程安全的，但不能保证整个程序运行过程都是安全的。如：
        ```
        在A线程中对变量var赋值
        var = 10;  //这个线程安全的
        
        刚赋值完成，A释放锁。 这时B线程又对var进行了赋值，
        var = 12; //这也是线程安全的

        B赋值完成后， 这时A线程读取到的var值就是12. 但原本希望的值是10。
        所以atomic 只是针对对象的赋值和取值过程是线程安全的，其他场景保证不了。(这种情况就要使用sychronize来对过程加锁了)

        ```

   - nonatomic: 非原子性，即不是线程安全。 由于访问过程不需要经历锁的操作，直接访问地址。所以其性能会比atomic要好。 在oc对象属性修饰多数情况下使用的都是nonatomic.


## 注意
1. oc默认属性修饰符：
    - 基本类型：atomic、 assign、 readwrite
    - 非基本类型：atomic、 strong、 readwrite

2. __unsafe_unretain、__strong、__weak、__autoreleasing 这四个属性的区别。

    -  __unsafe_unretain：修饰的对像引用计数不会增加，当对象被释放时，对象的指针地址依然存在，所以有可能出现野指针。

    -  __weak：修饰的对像引用计数不会增加，当对象被释放时，对象的指针地址会被赋值为nil，所以不会出现野指针。 __weak 是在iOS5之后才出现的，而__unsafe_unretain在ios5之前就有。weak 算是__unsafe_unretain的演变，现在基本上都是使用weak。

    - __strong：主要注意的是循环引用问题。

    -  __autoreleasing: 局部变量用__autoreleasing修饰后，其指向的对象，在当前autorelease pool结束之前不会被回收.

3. 用以alloc/new/copy/mutableCopy为前缀的方法名创建的对象，是自己创建并持有的。其他的会默认放入autoreleasepool中。


 [1]: https://www.jianshu.com/p/3cbc79424fb8
 [2]: https://www.jianshu.com/p/ec2c854b2efd









