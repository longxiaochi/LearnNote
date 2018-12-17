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




https://www.jianshu.com/p/3cbc79424fb8









//内存泄漏与野指针的区别