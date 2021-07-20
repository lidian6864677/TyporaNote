目录

[TOC]

# MaterialApp  程序根root

概念：MaterialApp是一个方便的Widget，它封装了应用程序实现Material Design所需要的一些Widget

常用参数：

```dart
    this.navigatorKey, // 导航的主  跨 Widget 访问状态
    this.home,     // 程序打开的起始页
    this.routes = const <String, WidgetBuilder>{}, // 程序路由表 路由无法传递参数
    this.initialRoute, // 初始路由 当用户进入程序默认路由
    this.onGenerateRoute,  // routes查找不到时，会调用该方法
    this.onGenerateInitialRoutes,  //  初始路由 
    this.onUnknownRoute,  // 顺序 onGenerateRoute ==> onUnknownRoute
    this.navigatorObservers = const <NavigatorObserver>[],  // 导航观察期器
    this.builder, // 观察器 构建一个Widget前调用 一般做字体大小，方向，主题颜色等配置
    this.title = '', // 应用标题 Android：任务管理器的程序快照之上 IOS: 程序切换管理器中
    this.onGenerateTitle, // 同上 生成应用标题 于做本地化
    this.color, // 颜色为Android中程序切换中应用图标背景的颜色，当应用图标背景为透明时
    this.theme, // 应用程序的主题，各种的定制颜色都可以设置，用于程序主题切换
    this.darkTheme, 
    this.themeMode = ThemeMode.system,

		// 国际化
    this.locale, // 地点 当前区域，如果为null则使用系统区域
    this.localizationsDelegates, // 本地化的代理类 
		// 通过localeResolutionCallback或localeListResolutionCallback回调来监听locale改变的事件
		// 当传入的是不支持的语种，可以根据这个回调，返回相近,并且支持的语种
    this.localeListResolutionCallback, // 本地语言回调
    this.localeResolutionCallback,  // 本地语言回调 
    this.supportedLocales = const <Locale>[Locale('en', 'US')],  // 应用支持的语种数组
		// Debug
    this.debugShowMaterialGrid = false, // 调试模式是否显示 Material 网格
    this.showPerformanceOverlay = false, // 显示性能监控叠层
    this.checkerboardRasterCacheImages = false,  // 棋盘格光栅缓存图像
    this.checkerboardOffscreenLayers = false,  // 棋盘格层
    this.showSemanticsDebugger = false,  // 显示语义调试器
    this.debugShowCheckedModeBanner = true,  // 调试模式是否显示 DEBUG 横幅
    this.shortcuts, 
    this.actions,
```

## Scaffold 脚手架

概念：Scaffold就是一个提供 Material Design 设计中基本布局的 widget。此类提供了用于显示drawer、snackbar和底部sheet的API。

常用参数：

```dart
Key key,
    this.appBar, // 用来定义顶部导航栏
    this.body,  // 页面中显示主要内容的区域，可自定义Widget
    this.floatingActionButton, // 悬浮按钮 默认显示在右下角
    this.floatingActionButtonLocation, // 悬浮按钮  显示的位置
    this.floatingActionButtonAnimator, // 悬浮按钮  位置移动动画
    this.persistentFooterButtons,  // 固定在下方显示的按钮
    this.drawer, // 左侧的抽屉菜单
    this.endDrawer,// 右侧的抽屉菜单
    this.bottomNavigationBar,  // 显示在底部的导航栏按钮栏
    this.bottomSheet,  // 从底部弹出一个Widget
    this.backgroundColor, // body 页面的背景颜色 
    this.resizeToAvoidBottomPadding, // 已经弃用
    this.resizeToAvoidBottomInset, // 如果在支架上方显示了屏幕上的键盘，则可以调整主体的大小以避免键盘重叠，这可以防止键盘遮盖主体内部的小部件。
    this.primary = true,  // 是否填充顶部
    this.drawerDragStartBehavior = DragStartBehavior.start,// 确定处理拖动开始行为的方式
		// 如果为true，并且指定了bottomNavigationBar或persistentFooterButtons，则body将延伸到Scaffold的底部，而不是仅延伸到bottomNavigationBar或persistentFooterButtons的顶部
    this.extendBody = false,
    this.extendBodyBehindAppBar = false,
    this.drawerScrimColor, // 打开抽屉时遮罩背景颜色。
    this.drawerEdgeDragWidth, // 打开抽屉的区域的宽度
    this.drawerEnableOpenDragGesture = true,
    this.endDrawerEnableOpenDragGesture = true,
```

### AppBar

<img src="/Users/dian 1/Library/Application Support/typora-user-images/image-20200519195402496.png" alt="image-20200519195402496" style="zoom: 33%;" /> 

```dart

    this.leading, //  左侧显示的图标 通常首页显示的为应用logo 在其他页面为返回按钮
    this.automaticallyImplyLeading = true,  // 配合leading使用
    this.title,  // 标题
    this.actions,	// 右侧itme
    this.flexibleSpace, // 显示在 AppBar 下方的控件，高度和 AppBar 高度一样，可以实现一些特殊的效果，该属性通常在 SliverAppBar 中使用
    this.bottom, // 这个小部件出现在应用程序栏的底部。 通常是一个TarBar，即一个标签栏
    this.elevation, // 控制下方阴影栏的坐标
    this.shape,  // 设置底栏的形状，一般使用这个都是为了和floatingActionButton融合，所以使用的值都是CircularNotchedRectangle(),有缺口的圆形矩形。
    this.backgroundColor, // AppBar背景色
    this.brightness, // AppBar亮度 有黑白两种主题
    this.iconTheme, //  AppBar上图标的样式
    this.actionsIconTheme, // AppBar上actions图标的样式
    this.textTheme, // AppBar上文本样式
    this.primary = true, 
    this.centerTitle, //  标题是否居中
    this.excludeHeaderSemantics = false,
    this.titleSpacing = NavigationToolbar.kMiddleSpacing, // 标题与其他控件的空隙
    this.toolbarOpacity = 1.0, //  AppBar tool区域透明度
    this.bottomOpacity = 1.0, // bottom区域透明度
```

### Container

```dart

    this.alignment,  // 设置Container距离外边容器的边距
    this.padding, // 设置Container内部子空间和自己的边距
    this.color, // 设置背景颜色
    this.decoration,  // 装饰，设置边框 背景色，图片等
    this.foregroundDecoration,  // 绘制在child前面的装饰。
    double width, // 设置宽度大小
    double height,  // 高
    BoxConstraints constraints, // 添加到child上额外的约束条件。
    this.margin, // 围绕在decoration和child之外的空白区域，不属于内容区域。
    this.transform, // 设置container的变换矩阵，类型为Matrix4。
    this.child, // container中的内容widget。
    this.clipBehavior = Clip.none, // 裁剪的方式 4种
```

#### Text

```dart
this.data, {
    this.style, // 字体的样式设置  TextStyle
    this.strutStyle,
    this.textAlign, // 文本对齐方式（center 居中，left 左对齐，right 右对齐，justfy 两端对齐）
    this.textDirection, // 文本方向（ltr 从左至右，rtl 从右至左）
    this.locale,   // 国际化 本地
    this.softWrap, // 是否自动换行 false文字不考虑容器大小  单行显示   超出；屏幕部分将默认截断处理
    this.overflow, // 文字超出屏幕之后的处理方式（clip裁剪，fade 渐隐，ellipsis 省略号）
    this.textScaleFactor, // 字体显示倍率
    this.maxLines, // 文字显示最大行数
    this.semanticsLabel,// 图像的语义描述，用于向Andoid上的TalkBack和iOS上的VoiceOver提供图像描述
    this.textWidthBasis, // 一行或多行文本宽度的不同方式，类型是TextWidthBasis
    this.textHeightBehavior,
```

##### TextStyle

```dart
this.inherit = true,
    this.color, // 文字颜色
    this.backgroundColor, // 背景色
    this.fontSize, // 文字大小
    this.fontWeight, // 字体粗细（bold 粗体，normal 正常体）
    this.fontStyle, // 文字样式（italic 斜体，normal 正常体）
    this.letterSpacing, // 字母间隙（如果是负值，会让字母变得更紧凑）
    this.wordSpacing, // 单词间隙（如果是负值，会让单词变得更紧凑
    this.textBaseline,  // 基线，两个值，字面意思是一个用来排字母的，一人用来排表意字的（类似中文）
    this.height,  // 当用来Text控件上时，行高（会乘以fontSize,所以不以设置过大）
    this.locale, // 国际化  本地化语言 
    this.foreground, // 文本的前景色，不能与color共同设置
    this.background, // 文本的背景颜色
    this.shadows, // 文本的阴影可以利用列表叠加处理
    this.fontFeatures, // 字体特征
    this.decoration, // 文字装饰线（none 没有线，lineThrough 删除线，overline 上划线，underline 下划线）
    this.decorationColor, // 文字装饰线颜色
    this.decorationStyle, // 文字装饰线风格（[dashed,dotted]虚线，
    this.decorationThickness,  // 用来指定修饰线的粗细,
    this.debugLabel, // 这种文本样式的可读描述，此属性仅在调试构建中维护。
    String fontFamily, // 行风格
    List<String> fontFamilyFallback,
    String package,
```

##### BoxDecoration	

类提供了多种绘制盒子的方法。

```dart
this.color,
    this.image,
    this.border,  // 描边
    this.borderRadius, // 圆角大小
    this.boxShadow, // 阴影
    this.gradient, // 过度效果
    this.backgroundBlendMode, // 混合Mode
    this.shape = BoxShape.rectangle, //形状矩形,BoxShape.circle和borderRadius不能同时使用
```

#### Image

```dart
Key key,
    @required this.image,
    this.frameBuilder, // 可以使用这个构建器添加动效，比如图片边距设置，加载动画如淡入效果）或者占位符
    this.loadingBuilder, // 图片加载的进度构建器
    this.errorBuilder, // 错误处理
    this.semanticLabel, // 图像的语义描述 ，用于向Andoid上的TalkBack和iOS上的VoiceOver提供图像描述
    this.excludeFromSemantics = false,
    this.width, // 用来指定显示图片区域的宽高（并非图片的宽高）
    this.height, // 用来指定显示图片区域的宽高（并非图片的宽高）
    this.color, // 这两个属性需要配合使用，就是颜色和图片混合，就类似于Android中的Xfermode
    this.colorBlendMode, // 这两个属性需要配合使用，就是颜色和图片混合，就类似于Android中的Xfermode
    this.fit,
    this.alignment = Alignment.center,  // 用来控制图片摆放的位置
    this.repeat = ImageRepeat.noRepeat, // 	用来设置图片重复显示（repeat-x水平重复，repeat-y垂直重复，repeat两个方向都重复，no-repeat默认情况不重复）
    this.centerSlice, //  设置图片内部拉伸，相当于在图片内部设置了一个.9图，但是需要注意的是，要在显示图片的大小大于原图的情况下才可以使用这个属性，要不然会报错
    this.matchTextDirection = false, // 这个需要配合Directionality进行使用
    this.gaplessPlayback = false, // 当图片发生改变之后，重新加载图片过程中的样式（1、原图片保留）
    this.isAntiAlias = false,
    this.filterQuality = FilterQuality.low,
```

## 参考

[Flutter之MaterialApp使用详解](https://cloud.tencent.com/developer/article/1337184)





