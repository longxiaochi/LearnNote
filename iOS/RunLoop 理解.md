# Runloop理解

## Runloop 与 线程的关系
- 线程与Runloop 是一一对应的关系，其关系是保存在一个全局的 Dictionary 里。
- 线程刚创建时并没有 RunLoop，如果你不主动获取，那它一直都不会有。
- RunLoop 的创建是发生在第一次获取时，RunLoop 的销毁是发生在线程结束时。
- 只能在一个线程的内部获取其 RunLoop（主线程除外）


## CFRunloop 中存在五个类
- CFRunloopRef
- CFRunloopModelRef
- CFRunloopSourceRef
- CFRunloopTimerRef
- CFRunloopObserverRef

1. 它们之间的关系：CFRunloopRef 包含多个CFRunloopModelRef, CFRunloopModelRef里包含CFRunloopSourceRef、CFRunloopTimerRef、CFRunloopObserverRef。Runloop一次只能运行在一种Model下， 需要切换Model时则需要先退出Runloop，设置Model然后再执行Runloop。 这样做主要是为了分隔开不同组的 Source/Timer/Observer，让其互不影响。

2. 弄清如何使用。

## CFRunloopRef 与 CFRunloopModelRef 的基本结构
```objective-c
struct __CFRunLoopMode {
    CFStringRef _name;            // Mode Name, 例如 @"kCFRunLoopDefaultMode"
    CFMutableSetRef _sources0;    // Set
    CFMutableSetRef _sources1;    // Set
    CFMutableArrayRef _observers; // Array
    CFMutableArrayRef _timers;    // Array
    ...
};
 
struct __CFRunLoop {
    CFMutableSetRef _commonModes;     // 存储Common的ModelName
    CFMutableSetRef _commonModeItems; // Set<Source/Observer/Timer>
    CFRunLoopModeRef _currentMode;    // Current Runloop Mode
    CFMutableSetRef _modes;           // model set集合
    ...
};
```
CFRunloopRef暴露了管理Model的相关接口。CFRunloopModelRef则暴露了管理modelItem相关的接口。



参考链接: [深入理解Runloop](https://mp.csdn.net).
