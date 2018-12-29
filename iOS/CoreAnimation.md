## CALayer 与 UIView 关系
- 与UIView类似，也是一些被层级关系树管理的矩形快。包含一些图片、文本、背景色，管理子图层的位置。与UIView不同的是CALayer 不处理用户的交互(因为它不清楚具体的响应链)。
- 使用图层
    1. 引用QuartzCore框架到Build Phases
    2. 创建一个CALayer,添加到视图对应图层的子图层。
    ```Objective-c
    - (void)viewDidLoad {
        [super viewDidLoad];
        
        CALayer *layer = [CALayer layer];
        layer.frame = CGRectMake(50.0f, 50.0f, 100.0f, 100.0f);
        layer.backgroundColor = [UIColor blueColor].CGColor;
        
        [self.view.layer addSublayer:layer];
        self.view.backgroundColor = [UIColor whiteColor];
    }
    ```
- 一个视图只有一个与之对应的图层，但是图层可以添加无数个子图层。一般来说，我们都只是简单地操作视图，没有必要给他们关联的图层添加子图层。 使用视图的好处是，可以使用CALayer底层特性的同时，也能使用UIView 高级的API

- 图层中设置图片
    ```objective-c
    - (void)viewDidLoad {
        [super viewDidLoad];
        
        UIImage *image = [UIImage imageNamed:@"IU"];
        self.view.layer.contents = (__bridge id)image.CGImage;
        self.view.contentMode = UIViewContentModeScaleAspectFit;
        
        self.view.backgroundColor = [UIColor whiteColor];
    }
    ```
   注意： 设置的图片发生了拉伸， 我们可以通过设置UIview 的contentMode来调整(UIViewContentModeScaleAspectFit)，而对于图层则可以通过设置其contentsGravity的值调整。

- contentsScale 属性是支持高分辨率Retina|Hi-DPI 屏幕机制的一部分。它用来判断绘制图层的时候应该为寄宿图创建的空间大小，和需要显示的图片拉伸度。(如果设置了contentsGravity，则不起作用了，kCAGravityCenter除外),当使用代码的方式来处理寄宿图时，要手动设置图层的contentsScale属性。否则会显示不正确
代码：layer.contentsScale = [UIScreen mainScreen].scale;

- UIView 的 clipsToBounds属性可以用来决定是否显示超出边界的内容， CALayer对应的属性叫做 masksToBounds