# Unknown type name 'NSString'解决方法

[![img](https://upload.jianshu.io/users/upload_avatars/3689537/26d01b6013d6.png?imageMogr2/auto-orient/strip|imageView2/1/w/96/h/96/format/webp)](https://www.jianshu.com/u/ab0f608f63f2)

[shyizne](https://www.jianshu.com/u/ab0f608f63f2)关注

0.1192017.04.07 13:12:41字数 254阅读 2,083

今天看到个问题，编辑工程提示Unknown type name 'NSString'，如下图

![img](https://upload-images.jianshu.io/upload_images/3689537-57f6044e5bee1b18.png?imageMogr2/auto-orient/strip|imageView2/2/w/568/format/webp)

导致出现异常的原因是是因为工程中添加了一些.c文件(第三方开源解压缩库)



![img](https://upload-images.jianshu.io/upload_images/3689537-471db1e98e089f27.png?imageMogr2/auto-orient/strip|imageView2/2/w/506/format/webp)

一般情况下出现“Unknown type name”是头文件互相引用出现的，这里可以排除，由于源码使用是c\c++与oc混编，

考虑新的XCode编译文件类型导致的，尝试了几种方案，下面三种可以解决问题。

解决方案一：

选择所有.c文件，将属性的 identity and type 改为Objective-C Source。



![img](https://upload-images.jianshu.io/upload_images/3689537-a028db308e61084c.png?imageMogr2/auto-orient/strip|imageView2/2/w/524/format/webp)

解决方案二：

选择所有.c文件，将.c修改为.m

解决方案三：

将Compile Sources As 改为 Objective-C++

方案三由于修改所有文件的编译类型，所有可能会导致其他包括c、c++代码的提示错误，不过都是些的提示异常，按提示修改即可。