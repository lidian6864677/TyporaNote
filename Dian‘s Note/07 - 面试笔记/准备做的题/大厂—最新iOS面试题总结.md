# 大厂—最新iOS面试题总结

**面试题**

1. 详细描述一下UIView与 CALAyer的关系draw Rect一定会影响性能吗? UI Dynamics与UIKit Animation的最本质区别是什么?

   - UIView和CALayer的关系

     UIView显示原理：创建UIView对象时，UIView内部会自动创建一个CALayer对象，当需要显示在屏幕上的时候，会调用drawRect方法进行图形渲染，并将所有的内容绘制到自己的层上，绘制完毕后，系统会将层拷贝到屏幕上，从而完成了UIView的显示。

     1. UIView继承于UIResponder,可以响应事件 . CALayer继承于NSObject不能,
     2. UIView侧重于显示内容的管理，CALayer侧重于对内容的绘制
     3. UIView是CALayer的delegate
     4. UIView内部都有一个CALayer在背后提供内容的绘制和显示，更像是一个CAlayer的管理器，访问和绘制、坐标相关的属性，如frame、bounds等，实际上内部都是访问它所在的Layer相关属性
     5. 

   - draw Rect一定会影响性能吗？

     drawRect影响在touchevent的时候会重新绘制，

     调用时机：

      - touchevent的时候会重新绘制，

      - 视图第一次显示。系统自动调用的在IUViewController->loadView和Viewdidload方法之后调用

      - 如果UIView没有设置Rect 那么将导致drawRect不会被调用

      - 方法在调用sizeThatFits后被调用，所以可以先调用sizeToFit计算出size，然后系统自动调用都drawRect方法

      - 通过设置contentMode属性值为UIViewContentModeRedraw，那么将在每次更高frame的时候都会自动调用drawRect

      - 直接调用setNeedDisplay，或者setNeedDisplayInRect出发drawRect，前提是rect不能为0

        **`如果初始化一个View的时候、没有使用initWithFrame：方法，drawRect将不会被调用`**

        

   - UIDynamics与UIKit Animation的本质区别

     UIDynamics 动力学动画

     coreAnimation动画

2. 如何用 UllmageView显示超大分辨率的图?如果要支持缩放呢?
   - 压缩，显示？ 图片切割后分块展示
3. 了解 fishhook吗?说说为什么 fishhook不能修改非动态连接库中的符号?

4. C++调用虚方法与 Objective-C发消息有什么区别?

5. 了解 placement new吗? Objective-C中如何实现这个功能

6. 如何在ARC环境下用C++标准库容器来管理Objective-C对象?

7. id、self、 super它们从语法上有什么区别?

8. isa是什么?是指向Cass对象本身的指针吗?

9. block修改捕获变量除了用 block还可以怎么做?有哪些局限性?

10. NSDictionary与 NSHashTable有什么区别,它们的使用场景是怎样的?
11. 用过Swit吗?如何评价 String index的设计？

12. 假设 iPhone上有一个与服务器的TCP连接,此时 iPhone忽然断网,服务器能在短时间内知会 iPhone的离线吗?

13. 为什么 Wireshark不能直接抓取SSL的原始数据？

14. backtrace是在用户态实现的吗?能否讲讲实现它的大致思路?

15. malloc的指针 double free产生的异常与访问freed指针有可能产生的异常有什么区别?为什么访问 freed指针不一定产生异常?

16. RunLoop是一个不停歇在运行的死循环吗?为什么?

17. 看过 runtime的源码吗?源码中常有的fastpath、 flowpath是什么?

18. runtime中 SideTable(不是 SideTable)存在的意义是什么

19. Objective-C是如何保证系统升级后的AB稳定性的?
20. iOS响应链