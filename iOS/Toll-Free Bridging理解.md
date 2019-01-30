## Toll-Free Bridging是什么？
在Core Foundation中Foundation中，有一些类型是可以交换使用的。

## 生命周期
- Foundation对象是可以通过ARC进行管理的，而Core Foundation则需要调用CFRetain,CFRelease等方法手动管理生命周期。那么bridge的时候生命周期是如何移交的呢？

我们需要告诉编译器如何管理生命周期。通过以下三个关键字来控制：
- __bridge: 进行OC指针和CF指针之间的转换，不涉及对象所有权转换。
  - Foundation对象是由ARC管理的（这里不考虑MRC的情况），你不需要手动retain和release。
  - Core Foundation的指针是需要手动管理生命周期。
    - OC -> CF，所有权在Foundation，不需要手动管理
        ```
        NSString * str = [NSString stringWithFormat:@"%ld",random()];
        CFStringRef cf_str = (__bridge CFStringRef)str;
        NSLog(@"%ld",(long)CFStringGetLength(cf_str));
        ```
    - CF -> OC，所有权在CF，需要手动管理内存
        ```
        CFStringRef cf_str = CFStringCreateWithFormat (NULL, NULL, CFSTR("%d"), rand());
        NSString * str = (__bridge NSString *)cf_str;
        NSLog(@"%ld",(long)str.length);
        //这一行很有必要，不然会内存泄漏
        CFRelease(cf_str);
        ```
- __bridge_retained:将一个OC指针转换为一个CF指针，同时移交所有权，意味着你需要手动调用CFRelease来释放这个指针。这个关键字等价于CFBridgingRetain函数。
    - 例子：
        ```
        NSString * str = [NSString stringWithFormat:@"%ld",random()];
        CFStringRef cf_str = (__bridge_retained CFStringRef)str;
        NSLog(@"%ld",(long)CFStringGetLength(cf_str));
        CFRelease(cf_str);
        ```
- __bridge_transfer:将一个CF指针转换为OC指针，同时移交所有权，ARC负责管理这个OC指针的生命周期。这个关键字等价于CFBridgingRelease
    - 例子：
       ```
        CFStringRef cf_str = CFStringCreateWithFormat(NULL, NULL, CFSTR("%d"), rand());
        NSString * str = (__bridge_transfer  NSString *)cf_str;
        NSLog(@"%ld",(long)str.length);
      ```
## 小结：
总结一句话，所有权在`Foundation`，则不需要手动管理内存；所有权在`CoreFoundation`，需要调用CFRetain/CFRelease来管理内存。

!(link)