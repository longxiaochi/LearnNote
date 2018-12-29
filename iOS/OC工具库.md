## 常用宏

-  设备高度和宽度
```
    #define SCREEN_WIDTH ([[UIScreen mainScreen] bounds].size.width)
    #define SCREEN_HEIGHT ([[UIScreen mainScreen] bounds].size.height)
    #define ScreenHeight [UIScreen mainScreen].bounds.size.height
    #define ScreenWidth [UIScreen mainScreen].bounds.size.width
    #define SCREEN_MAX_LENGTH (MAX(ScreenWidth, ScreenHeight))
    #define SCREEN_MIN_LENGTH (MIN(ScreenWidth, ScreenHeight))
    #define NavigationTitleWidth ([UIScreen mainScreen].bounds.size.width / 3)
```
- 判断设备型号
```
    #define IS_IPHONE (UI_USER_INTERFACE_IDIOM() == UIUserInterfaceIdiomPhone)
    #define IS_IPHONE_4_OR_LESS (IS_IPHONE && SCREEN_MAX_LENGTH < 568.0)
    #define IS_IPHONE_5 (IS_IPHONE && SCREEN_MAX_LENGTH == 568.0)
    #define IS_IPHONE_6 (IS_IPHONE && SCREEN_MAX_LENGTH == 667.0)
    #define IS_IPHONE_6P (IS_IPHONE && SCREEN_MAX_LENGTH == 736.0)
```

- 其他
```
    //设备的导航栏高度
    #define ScreenNavigationBarHeight 44.0f

    //设备的状态栏高度
    #define ScreenStatusBarHeight [UIApplication sharedApplication].statusBarFrame.size.height

    //底部工具栏高度
    #define ScreenToolBarHeight    49.0f

    //获取IOS系统版本
    #define DeviceVersion [[UIDevice currentDevice] systemVersion].integerValue


    //16进制颜色值转RGB值
    #define RGB_COLOR(_STR_) ([UIColor colorWithRed:[[NSString stringWithFormat:@"%lu", strtoul([[_STR_ substringWithRange:NSMakeRange(1, 2)] UTF8String], 0, 16)] intValue] / 255.0 green:[[NSString stringWithFormat:@"%lu", strtoul([[_STR_ substringWithRange:NSMakeRange(3, 2)] UTF8String], 0, 16)] intValue] / 255.0 blue:[[NSString stringWithFormat:@"%lu", strtoul([[_STR_ substringWithRange:NSMakeRange(5, 2)] UTF8String], 0, 16)] intValue] / 255.0 alpha:1.0])

    //字体大小
    #define FontSize(size)                      [UIFont systemFontOfSize:(size)]
```
