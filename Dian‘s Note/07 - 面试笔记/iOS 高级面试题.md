1. **分类和扩展有什么区别？可以分别用来做什么？分类有哪些局限性？分类的结构体里面有哪些成员？** 

   - 区别：

     - extension在编译期间决定 伴随着类的额产生而产生，亦随之一起消亡。     category在运行期决议的，

     - extension 可以添加实例变量。  category不能添加实例变量（因为在运行期，对象的内存布局已经确定，如果添加实例变量就会破坏类的内部布局），这对编译行语言来说是灾难性的。

     -  分类应用 可以把类的实现分开在几个不同的文件里面。这样做有几个显而易见的好吃

       - 可以减少文件的体积
    - 可以把不同的功能组织到不同的category里
       - 可以由多个开发者共同完场一个类
       - 可以按需加载想要的category等等。
       - 声名私有方法
   
     - 扩展应用extension一般用来隐藏类的私有信息
   
     - 分类的结构体包括：
   
       ```objc
       struct category_t {
         constchar *name; /// 类的名字(cls)
      calssref_t cls;  ///> l类
         struct method_list_t *instanceMethods;     ///> category中所有给类添加的实例方法列表
      structmethod_list_t *calssMethods;         ///> category中所有添加的类方法列表
         structprotocol_list_t *protocols;          ///> category实现所有协议的列表
      structproperty_list_t *instanceproperties; ///> category中添加的所有属性
       }
    
       
    ```
   
2. **被weak修饰后的对象在被释放的时候会发生什么？是如何是实现的? 知道sideTableme ?里面的结构可以画出来吗？ ** 

   - 我的回答：weak

3. **应用启动流程** 

   - iOS 应用的启动分为两种
     - main函数之前 （pre-main）
       - 加载应用的可执行文件
       - 加载动态链接库加载器dyld
       - dyld递归加载应用所有依赖的dylib
     - main函数之后 （main）
       - dyld调用main()
       - 调用UIApplicationMain()
       - 调用applicationWillFinishLuanching
       - 调用didFinishlaunchingWithOptions

   - 启动耗时测量

     - pre-main 阶段

       对于pre-main阶段。apple提供了一个测量方法，在xcode中Edit secheme -> Run -> Auguments 将环境变量DULD_PRINT_STATISTICS 设置为1、之后控制台会出现类似的内容

       ```objc
       	Total pre-main time: 228.41 milliseconds (100.0%)
                dylib loading time:  82.35 milliseconds (36.0%)
               rebase/binding time:   6.12 milliseconds (2.6%)
                   ObjC setup time:   7.82 milliseconds (3.4%)
                  initializer time: 132.02 milliseconds (57.8%)
                  slowest intializers :
                    libSystem.B.dylib : 122.07 milliseconds (53.4%)
                       CoreFoundation :   5.59 milliseconds (2.4%)
       ```

       这样我们可以清晰的看到每个耗时了。

     - main() 阶段

       main()阶段主要是测量main()函数开始执行到弟弟FinishLaunchingWithOptions执行结束的时间，我们直接插入代码就可以了

       ```objc
       CFAbsoluteTime StartTime;
       int main(int argc, char * argv[]){
       		StartTime = CGAbsoluteTimeGetCurrent();
       }
       ```

       再在AppDelegate.m 文件中用ectern声名全局变量StartTime

       ```
       extern CFAbsoluteTime StartTime;
       ```

       最后在didFinishLaunchingWithOptions里，在获取一下当前的时间，与StartTime的差值即是main()阶段运行消耗的时间

       ```
       double launchTime = (CFAbsoluteTimeGetCurrent() - StartTime);
       ```

   - 改善启动时间

     - Pre-main阶段

       在这一阶段，我们能做的只要是优化dylib

       - 加载Dylib

         之前提到过加载系统的dylib很快，因为有优化。但加载内嵌(embedded)的dylib文件很占用时间，所以尽可能吧多个内嵌dylib合并成一个来加载，或者使用static archive。

         使用dlopen()来在运行时懒加载是不建议的，这么做可能会带来一些问题，并且总的开销更大。

       -  Rebase/Binding

         之前提到过Rebaing消耗了大量时间在I/O上，而在之后的Binding就不怎么需要I/O了，而是将时间消耗在了计算上。所以这两个步骤的耗时是混在一起的

         **对于OBJC来说就是减少Class、selector、category的元数据数量。** 从编码原则和设计模式之类的理论都会鼓励大家多些精致短小的类方法，并将每个部分放大独立出一个类别，其实这样会增加启动时间。

       - Objc setup

         大部分Objc初始化工程已经在Rebase/Bind阶段做完了，这一步dyld会注册所有声名过的Objc类，将分类插入到类的方法列表里，再检查每一个selector的唯一性。(这一步么什么优化可做的)

       - Initializers

         到了这一阶段，dyld开始运行程序的初始化函数，调用每个Objc类和分类的+load方法，调用C/C++中的构造器函数（用arrtibutle()(constructor)修饰的函数），和创建非基本类型的C++静态全局变量。Initializers阶段执行完后，dyld开始调用main函数。

         在这一步我们要做的：

         - 少在类的+load方法里做事情，尽量把这些推迟到+initialize
         - 介绍构造器函数个数，在构造器函数里少做些事情
         - 较少C++静态全局变量的个数

     - main()阶段的优化

       这一阶段的优化主要减少didFinishLaunchingWithOptions方法里的工作，在didFinishLuanchingWithOptions方法里，我们会创建应用的windiw，指定其rootViewController，调用Window的makeKeyAndVisible方法让其可见。由于业务需要，我们会初始化各个二方、三方库，设置系统UI风格，检查时候需要显示引导页、是否需要登录、是否有新版本等。由于历史原因，这里的代码比较容易变得庞大，启动耗时难以控制。

       所以，满足业务需求的前提下，didFinishLaunchingWithIptions在主线程里做的事情越少越好。早这一步我们可以做的优化：	

       - 梳理各个二/三方库，找到可以延迟加载的库，做延迟加载处理，比如放到首页控制器的ViewDidAppear方法里面
       - 梳理业务逻辑，把可以延迟执行的逻辑，做延迟执行处理。比如检测新版本、注册推送通知等逻辑
       - 避免复杂、多余的计算
       - 避免在首页控制器的ViewDidLoad和ViewWillAppear做太多事情，这2个方法执行完，首页控制器才会显示，部分可以延迟创建的视图应做延迟创建/懒加载处理
       - 首页控制器用纯代码方式构建

   - 总结：

     让系统在启动期间少做一些事。当然我们得先清除工程里做的那些事情是在启动期间做的、对启动速度应将有多大，然后case by case地方分析工程代码。通过放到子线程、延迟加载、懒加载等方式让系统在启动期间更轻松些

     

4. **UITableView调用顺序**

   - https://www.jianshu.com/p/07fef73d9b4e

   - 第一轮 

     ```objc
     	numberOfSectionInTableView：(调用一次)
     	tableView:heightForHeaderInSection：(调用两次) 
       tableView:heightForFooterInSection: (调用两次)
       tableView:numberOfRowsInSection: (调用一次)
       tableView:heightForRowAtIndexPath: (调用次数与组的行数一致)
     	一组cell的调用结果，如果多组，会重复调用上诉内容	
        
     ```

   - 第二轮和第三轮的调用结果和第一轮一样

   - 

5. ViewController调用顺序

   - initWithNibName:bundle
   - initWithCoder
   - awakeFromNib

   - loadView（当访问UIViewController的View属性时调用）
   - viewDidLoad（加载视图时调用）
   - viewwillAppear（视图即将出现时调用）
   - viewWillLayoutSubViews（视图布局改变时调用）
   - viewDidLayoutSubViews （view已经布局其SubViews，这里可以放置调整完成之后需要做的工作）
   - viewDidAppear（视图即将加入窗口时调用）
   - viewWillDisappear（视图将从屏幕上移除之前调用）
   - viewDidDisappear（视图已经将屏幕上移除，用户看不到这个视图了）
   - dealloc（视图被销毁，对在init和viewDidload中创建的视图进行释放）
   - didReceiveMemoryWarning（）

6. 程序启动的执行顺序

   ![img](https://upload-images.jianshu.io/upload_images/2252551-4c489571cabf79b7.png?imageMogr2/auto-orient/)

   - 具体执行流程

   - 程序入口

     进入main函数，设置APPDelegate称为函数的代理

   - 程序完成加载

     [AppDelegate application:didFinishLaunchingWithOptions:]

   - 创建window窗口

   - 程序被激活

     [AppDelegate applicationDidBecomeActive:]

   - 点击home键

     程序取消激活状态（在当前屏幕窗口，但是被其他应用终端 eg：来电话，锁屏）

     [AppDelegate applicationWillResignActive:];

     程序进入后台

     [AppDelegate applicationDidEnterBackground:];

   - 点击进入程序

     程序进入前台

     [AppDelegate applicationWillEnterForeground:];

     程序被激活

     [AppDelegate applicationDidBecomeActive:];

7. **copy和mutableCopy**

   - copy： 浅拷贝 不拷贝对象本身，仅仅拷贝指向对象的指针

   - mutableCopy： 深拷贝 直接拷贝整个对象的内存到另一块内存

   - ![img](https://img-blog.csdn.net/20170517201022425?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTM2Mzk4MQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

   - 坑：对于string进行copy后 改变str1的值会同时改变str2的值吗？

     copy还有它的特点：

     - 修改源对象的属性和行为，不会影响副本对象

     - 修改副本对象的属性和行为，不会影响源对象

       ```
       NSString *str1 = @"str1";
       NSString *str2 = [str1 copy];
       str1 = @"str1copy"
       NSLog(@"str1 = %@  \n str2 = %@",str1,str2);
       /** 结果
        *  str1 =  str1copy
        *  str2 = str1
        */
       ```

       为什么copy后事同一个地址，但是复制了str1之后地址就变了呢？

       因为str2 = str1的时候，两个字符串都是不可变的，并且值是一样的系统为了优化性能使用同一块内存地址，但是改变之后为了不影响的原则下 开辟了一块新的内存空间

     

8. 