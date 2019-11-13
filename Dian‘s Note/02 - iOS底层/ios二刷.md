

- class_getInstanceSize    
  - 返回 iVar的大小   成员大小

- malloc_size((__bridge const void *) ins)  
  - 返回开辟内存的大小(需要桥接一下)




生成   .cpp 文件

```swift
xcrun  -sdk  iphoneos  clang  -arch  arm64  -rewrite-objc  OC源文件  -o  输出的CPP文件
eg:
xcrun -sdk iphoneos clang -arch arm64 -rewrite-objc main.m -o main-arm64.cpp 


xcrun -sdk iphoneos clang -arch64 -rewrite-objc main.m -o main-arm64.cpp
```





## isa

类对象和元类对象内部都是这个结构   只不过 元类对象中的属性信息，协议信息为空

![image-20191111152525825](/Users/yuangonmg/Library/Application Support/typora-user-images/image-20191111152525825.png)

属性是@property

成员变量是 _开头的变量

实例对象中的成员变量存储的是成员变量的值， 每一个实例对象可能对应不同的值  需要存储多份

类对象中的成员变量的信息， 如叫什么名字， 什么类型等 只需要存储一份



## KVO

key value observing

当一个实例添加的KVO之后他会利用Runtime生成一个新的类  继承一个原始类，实例的isa指向这个生成的类，NSSetXxxValueAndnotify_className、

（_NSSetDoubleValueAndnotify、_NSSetStringValueAndnotify根据类型不同会有变化）

这个类中  有自己的Set方法（superClass、class、delloc、_isKVOA）， Set方法中实现了 _NSSetStringValueAndnotify方法（此方法在foundation框架中可以找得到）

根据汇编可以看出 新类Set方法中实现了 WillChangeValueForKey  ->  super.set  --> didChangeValueForKey

三个方法  在didChange中实现了 observerValueForKeyPath: ofObject: change: context的方法 通知监听器 添加监听的方法 去实现的 



## KVC

Key Value Coding











