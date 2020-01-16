#

## 前言：

## 性能优化

#### 1. 什么是CPU和GPU

- CPU (Central Processing Unit，中央处理器 )
  - 对象的创建和销毁、对象属性的调整、布局计算、文本的计算和排版、图片的格式转换和解码、图像的绘制 (Core Graphics)
- GPU (Graphics Processing Unit，图形处理器 ) 
  - 纹理的渲染
    ![](https://user-gold-cdn.xitu.io/2019/5/13/16ab093ceb760691?w=1214&h=386&f=png&s=54159)

#### 2. 卡顿原因

  - cpu处理后GPU处理 若垂直同步信号早于GPU处理的速度那么会形成掉帧问题
    ![](https://user-gold-cdn.xitu.io/2019/5/13/16ab09457a52fe84?w=1572&h=428&f=png&s=43972)
- 在 iOS中有双缓存机制，有前帧缓存、后帧缓存

#### 2. 卡顿优化 - CPU

- 尽量使用轻量级的对象，比如用不到事件处理的地方，可以考虑使用CALayer 替代 UIView
  - UIView和CALayer的区别？
    - CALayer是UIView的一个成员 
    - CALayer是专门用来显示东西的
    - UIView是用来负责监听点击事件等
- 不要频繁的调用UIView的相关属性，比如frame、bounds、transform等属性，尽量减少不必要的修改。
- 尽量提前计算好布局，在有需要时一次性调整好对应的属性，不要多次修改属性。
- Autolayout会比直接设置frame消耗更多的CPU资源
- 图片的size最好跟UIImageView的size保持一致
  - 因为如果超出或者UIImageView会对图片进行伸缩的处理
- 控制线程的最大并发数量
- 尽量把一些耗时的操作放到子线程
  - 文本处理（储存计算和绘制）
  - 图片处理（解码、绘制）

#### 3. 卡顿优化 - GPU

- 尽量减少视图数量和层次
- 尽量避免短时间内大量图片的显示，尽可能将多张图片合成一张进行显示
- GPU能处理的最大纹理尺寸是4096x4096，一旦超过这个尺寸，机会占用CPU资源进行处理，所以纹理尽量不要超过这个尺寸。
- 减少透明视图，不透明的就设置opaque为YES
- 尽量避免离屏渲染

#### 4. 离屏渲染

- 在OpenGL中，GPU有2中渲染方式

  - On-Screen Rendering: 当前屏幕渲染，在当前用于显示的屏幕缓冲区进行渲染操作。
  - Off-Screen Rendering: 离屏渲染，在当前屏幕缓冲区以外新开辟一个缓冲区进行渲染操作

- 离屏渲染消耗性能原因：

  - 需要创建新的缓冲区
  - 离屏渲染的整个过程，需要多次切换上下文环境，先是从当前屏幕（On-Screen）切换到离屏（Off-Screen）；等到离屏渲染结束后，将离屏缓冲区的渲染结果显示到屏幕上，有需要将上下文环境从离屏切换到当前屏幕。

- 那些操作会出发离屏渲染？

  - 光栅化  layer.shouldRasterize = YES;

  - 遮罩，layer.mask = 

  - 圆角，同事设置layer.maskToBounds = YES、layer.cornerRadius大于0

    - 考虑通过CoreGraphics绘制圆角，或者美工直接提供。
    - 第一种方法:通过设置layer的属性 

    ```objc
    imageView.layer.masksToBounds = YES;
    [self.view addSubview:imageView];
    ```

    - 第二种方法:使用贝塞尔曲线UIBezierPath和Core Graphics框架画出一个圆角

    ```objc
    UIImageView *imageView = [[UIImageView alloc]initWithFrame:CGRectMake(100, 100, 100, 100)];
    imageView.image = [UIImage imageNamed:@"1"];
    //开始对imageView进行画图
    UIGraphicsBeginImageContextWithOptions(imageView.bounds.size, NO, 1.0);
    //使用贝塞尔曲线画出一个圆形图
    [[UIBezierPath bezierPathWithRoundedRect:imageView.bounds cornerRadius:imageView.frame.size.width] addClip];
    [imageView drawRect:imageView.bounds];
    
    imageView.image = UIGraphicsGetImageFromCurrentImageContext();
     //结束画图
    UIGraphicsEndImageContext();
    [self.view addSubview:imageView];
    ```

    - 第三种方法:使用CAShapeLayer和UIBezierPath设置圆角

     ```objc
     #import "ViewController.h"
    
    @interface ViewController ()
    
    @end
    
    @implementation ViewController
    
    - (void)viewDidLoad {
      [super viewDidLoad];
    UIImageView *imageView = [[UIImageView alloc]initWithFrame:CGRectMake(100, 100, 100, 100)];
    imageView.image = [UIImage imageNamed:@"1"];
    UIBezierPath *maskPath = [UIBezierPath bezierPathWithRoundedRect:imageView.bounds byRoundingCorners:UIRectCornerAllCorners cornerRadii:imageView.bounds.size];
    
    CAShapeLayer *maskLayer = [[CAShapeLayer alloc]init];
    //设置大小
    maskLayer.frame = imageView.bounds;
    //设置图形样子
    maskLayer.path = maskPath.CGPath;
    imageView.layer.mask = maskLayer;
    [self.view addSubview:imageView];
    }
    
     ```

    - 推荐三种方式，对内存的消耗最少啊，渲染速度快

  - 阴影， layer.shadowXXX

    - 如果设置了layer.shadowPath就不会出发离屏渲染

  

#### 4. 卡顿检测

- 平时所说的“卡顿”主要是因为在主线程执行了比较耗时的操作
- 可以添加Observer到主线程RunLoop中，通过监听RunLoop状态切换的耗时，以达到监控卡顿的目的
- 参考代码：[GitHub - UIControl/LXDAppFluecyMonitor](https://github.com/UIControl/LXDAppFluecyMonitor)
  - observer监听所有状态
  - while循环休眠之前  如果卡顿次数超过5次  打印主线程堆栈

#### 5. 耗电优化 

- 耗电来源 

  - CPU处理  Processing
  - 网络     Networking
  - 定位     Location
  - 图像     Graphice

- 尽可能降低CPU、GPU的功耗

- 少用定时器

- 优化I/O操作  文件读写

  - 尽量不要频繁的写入小数据，最好批量一次性写入
  - 读写大量重要的数据的时候，考虑使用dispatch_io，其提供了基于GCD的异步操作文件I/O的API。用dispatch_io系统会优化磁盘访问
  - 数据量比较大的，建议使用数据库（比如SQList、CoreData）

- 网络优化  

  - 减少，压缩网络数据
  - 多次请求结果相同，尽量使用缓存
  - 使用断点续传，否则网络不稳定时可能多次传输相同的内容
  - 网络不可用时尽量不要尝试执行网络请求
  - 让用户可以取消长时间运行或者网络速度很慢的网络操作，设置合理的超时时间
  - 批量传输，比如：下载视频流时，不要传输很小的数据包，直接下载整个文件或者一大块一大块的下载，如果下载广告，一次性多下载一些，然后慢慢展示。如果下载电子邮件，一次下载多封不要，一封一封的下载。 

- 定位优化

  - 如果只需要快速确定用户位置，最好用CLLocationManager的requestLocation方法。定位完成之后，会自动让定位硬件断电。
  - 如果不是导航应用，尽量不要实时更新位置，定位完毕之后就关掉定位服务
  - 尽量降低定位的精准度，如果没有需求的话尽量使用低精准度的定位。随软件自身要求
  - 如果需要后台定位，尽量设置pausesLocationUpdatasAutomatically为YES，如果用户不太可能移动的时候系统会自动暂停位置更新

  ### App启动

  - App启动分为两种
    - 冷启动（Cold Launch）:从零开始启动App
    - 热启动（Warm Launch）:App已经在内存中，在后台存活，再次点击图标启动App
  - App启动时间的优化，主要针对于冷启动
    - 通过添加环境变量可以打印app启动的时间分析（Edit scheme -> Run -> Arguments）
      - DYLD_PRINT_STATISTICS设置为1
      - 如果想要看更详细的内容，那就将DYLD_PRINTP_STATISTICS_DETAILS设置为1
  - 冷启动分为三大阶段
    - dyld（dynamic link editor）,Apple的动态连接器，可用来装在Mach-O文件（可执行文件，动态库等）
      - 启动时做的事情
        - 装载App的可执行文件，同时会递归加载所有依赖的动态库
        - 当dyld把所有的可执行文件和动态库都装载完毕之后，会通知runtime进行下一步处理
    - runtime
      - 启动时runtime所做的事情
        - 调用map_images进行可执行文件内容的解析和处理
        - 在load_images中调用call_load_methods,调用所有Class和Catrgory的+load方法
        - 进行各种Objc结构的初始化，（注册Objc类、初始化类对象等等）
        - 调用C++静态初始化器和__attribute__((constructor))修饰的函数
      - 到此为止可执行文件的动态库和所有的符号（Class、protocols、Selector、IMP...）都已经按格式成功加载到内存中，被runtime所管理
    - main
      - 总结一下
        - APP的启动有dyld主导，将可执行文件加载到内存、顺便加载所有依赖的动态库
        - 并有Runtime负责加载成objc定义的结构
        - 所有初始化工作结束后，dyld就会调用main函数
        - 接下来就是UIApplicationMain函数，AppDelegate的Application：didFinishLaunchingWithOptions:方法
          ![](https://user-gold-cdn.xitu.io/2019/5/13/16ab091b898deaed?w=960&h=280&f=png&s=68208)

- 如何优化启动时间

  - dyld
    - 减少动态库、合并一些动态库（定期清理不必要的动态库）
    - 减少Objc类、分类的数量、减少Selector数量（定期清理不必要的类、分类）
    - 减少C++虚函数数量
    - swift尽量使用struct
  - runtime
    - 用+initialize方法和dispatch_once取代所有的__attribute__((constructor))、C++静态构造器、Objc的+load
    - hook objc_msgSend 方式查看每个对象执行的时间
  - main
    - 在不影响用户体验的前提下，尽可能将一些操作延迟，不要全部都放在finishLaunching方法中
    - 按需求加载





1、main()函数之前总共使用了1.4s

2、加载动态库用了1.3s，指针重定位使用了36.75ms，ObjC类初始化使用了35.65ms，各种初始化使用了80.97ms。

3、在初始化耗费的80.97ms中



iOS 关于 热启动和冷启动，以及热启动和冷启动的时间的优化  https://www.jianshu.com/p/82c566a19075

浅谈 iOS 事件的传递和响应过程 https://www.jianshu.com/p/481465fc4f2d







QPLAPP  目前存在的问题 

1. 宏定义过多，有许多根本就没有使用的宏

   `#define`与`const`都可用来修饰常量。

   - 编译器处理方式不同
     define宏是在预处理阶段展开。
        　const常量是编译运行阶段使用。
   - 类型和安全检查不同
     define宏没有类型，不做任何类型检查，仅仅是展开。
        　const常量有具体的类型，在编译阶段会执行类型检查。
   - 存储方式不同
     define宏仅仅是展开，有多少地方使用，就展开多少次，不会分配内存。（宏定义不分配内存，变量定义分配内存。）
        　const常量会在内存中分配(可以是堆中也可以是栈中)。
   - const可以节省空间，避免不必要的内存分配。
   - 提高了效率；编译器通常不为普通const常量分配存储空间，而是将它们保存在符号表中，这使得它成为一个编译期间的常量，没有了存储与读内存的操作，使得它的效率也很高。
   - 宏替换只作替换，不做计算，不做表达式求解;

2. appleDelegate 瘦身

   流程：

   服务器设置

   bugly配置

   收银台配置

   引导页 （进入首页流程）

   - 服务器设置
   - 检测网络请求  无网络  去首页   结束当前代码
   - 获取 tabbar配置
   - 获取广告图片。保存本地下次使用
   - 去首页 
   - 检测当前版本
     - 新版本 显示引导页
       - 无网推出  去首页
     - 老版本 检测用户验证状态
     - 检测网络和token 是否执行退出
     - 请求UserInfoAsync 接口，弹出相应页面  

   ​	

   推送

   路由注册

   第一次发开app统计

   小能客服

   友盟推送

   友盟统计

   web UA获取

   微信支付

   科大讯飞语音

   高德地图

   神策

   网络监控

   图片加载监控

   

3. 

4. 







启动参考文章：

[抖音研发实践：基于二进制文件重排的解决方案 APP启动速度提升超15%](https://mp.weixin.qq.com/s?__biz=MzI1MzYzMjE0MQ==&mid=2247485101&idx=1&sn=abbbb6da1aba37a04047fc210363bcc9&scene=21&token=2051547505&lang=zh_CN#wechat_redirect)

[手淘架构组最新实践 | iOS基于静态库插桩的⼆进制重排启动优化](https://mp.weixin.qq.com/s/YDO0ALPQWujuLvuRWdX7dQ)

戴明老师app启动文章

[iOS App启动优化（一）—— 了解App的启动流程](https://www.jianshu.com/p/024b3d847fe0)
[iOS App启动优化（二）—— 使用“Time Profiler”工具监控App的启动耗时](https://www.jianshu.com/p/80f72cdb0aa9)
[iOS App启动优化（三）—— 自己做一个工具监控App的启动耗时](