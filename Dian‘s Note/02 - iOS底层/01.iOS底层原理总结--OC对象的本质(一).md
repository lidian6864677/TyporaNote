[窥探iOS底层实现--OC对象的本质(一)](http://www.hsdian.cn/?p=200)

[窥探iOS底层实现--OC对象的本质(二)](http://www.hsdian.cn/?p=138)

[窥探iOS底层实现--OC对象的分类：instance、class、meta-calss对象的isa和superclass](http://www.hsdian.cn/?p=146)

[窥探iOS底层实现-- KVO/KVC的本质](http://www.hsdian.cn/?p=133)

...

### `问题： 一个NSObject对象占用多少个内存?`

```objc
int main(int argc, char * argv[]) {
    @autoreleasepool {
        NSObject *obj = [[NSObject alloc]init];
        /// obj对象占用了多少内存？
    }
}
return 0;

```


### Objective-c的本质：
平时我们编写的Objective-c的代码，底层的实现其实都是C/C++的代码。

![OC_%E5%B1%82%E6%AC%A1%E7%BB%93%E6%9E%84%E5%9B%BE.png](https://user-gold-cdn.xitu.io/2018/12/19/167c64351f692343?w=1752&h=162&f=png&s=39831)

所以Objective-c 的面向对象都是基于C/C++的数据结构实现的。

`思考问题： Objective-C的对象、类主要是基于C\C++的什么数据结构实现的？`


```objc
///> Student类
@interface Student: NSObject{
 @public
    int _no;
    int _age;
}
@end
@implementation Student

@end

int main(int argc, char * argv[]) {
    @autoreleasepool {
        NSObject *obj = [[NSObject alloc]init];
        /// obj对象占用了多少内存？
    }
}
return 0;
```
如果Objective-c的对象转成C/C++的代码实际上最重转成了C
/C++的机构体。

`那么我们怎么把OC的代码转换成C/C++的代码呢？`
1. 终端进入到程序的目录下：
![OC_%E7%BB%88%E7%AB%AF%E7%9B%AE%E5%BD%95.png](https://user-gold-cdn.xitu.io/2018/12/19/167c64351f448b1f?w=1218&h=64&f=png&s=53673)

 2. 输入命令行

        xcrun  -sdk  iphoneos  clang  -arch  arm64  -rewrite-objc  OC源文件  -o  输出的CPP文件
        eg:
        xcrun -sdk iphoneos clang -arch arm64 -rewrite-objc main.m -o main-arm64.cpp 
    
 3. 然后机会生成一个main-arm64.cpp的文件 这里面就是我们的C/C++的实现。
 4. 如果需要链接其他框架，使用-framework参数。比如-framework UIKit

5. 在生成main-arm64.cpp 内搜索 **NSObject_IMPL** 
    - NSObject_IMPL: NSObject Implementation
    - NSObject在内存中其实就是
```objc
///> NSObject_IMPL
struct NSObject_IMPL {
    Class isa; /// isa
};
```
`NSObject的底层实现结构图`
![OC_NSObject_%E5%BA%95%E5%B1%82%E5%AE%9E%E7%8E%B0%E7%BB%93%E6%9E%84%E5%9B%BE.png](https://user-gold-cdn.xitu.io/2018/12/19/167c64351f4e5fc8?w=1770&h=704&f=png&s=155180)

上图实际上NSObject对象中存在一个isa指针，isa指针在64位系统中占用8个字节，在32位的系统中占用4个字节，目前用的是64位系统，所以在我们NSObject中isa指针会占用8个字节。CLass isa的内部实现为结构体。
```objc
  /// class 其实就是一个指针  指向一个结构体的指针
  typedef struct objc_class *Class 
  
```


```objc
///   创建并分配存储空间
NSObject *obj = [[NSObject alloc]init];
```
假设我们NSObject对象分配了一块存储空间，假设之后8个字节，在这8个字节中我们只放了isa指针，假设我们的isa的地址为0x100400110，这个isa的地址就是结构体的地址。所以说obj的地址就是0x100400110。

### NSObject占用的内存
```objc
#import <malloc/malloc.h>
#import <OBJC/runtime.h>
///> main
int main(int argc, char * argv[]) {
    @autoreleasepool {
        NSObject *obj = [[NSObject alloc]init];
        ///> 获得NSObject类的实例对象的大小
        NSLog(@"%zd", class_getInstanceSize([NSObject class]));
        ///> 获取obj指针指向内存的大小
        NSLog(@"%zd", malloc_size((__bridge const void *)obj));        
        
        
        /**输出结果
        8    
        16   
        */ 
    }
    return 0;
}
```

- 首先我们用的Runtime的 class_getInstanceSize()方法去查看 NSObject类的实例对象的大小
    - 传入 类 class
    - 注意： **Instance** 实例，返回一个类的实例大小占用了内存空间的大小为8

-  然后我们用malloc_size的方法去查看obj指针指向内存的大小 为16；
    -  传入obj的指针（会有错误提示 然后写上桥接就好了(__bridge const void *) ）

malloc_size 为什么是16接下来我们可以去查看源码去解决问题：
源码地址：[Source Browser:OBJective-c源码](https://opensource.apple.com/tarballs/)
找到objc4，下载版本号最大的就是最新的源码去查看 
![OC_%E6%BA%90%E7%A0%81_objc4.png](https://user-gold-cdn.xitu.io/2018/12/19/167c64351f5be463?w=1392&h=842&f=png&s=120802)
alloc本质调用的是allocWithZone。 在源码中搜索allocWithZone

![OC_malloc_size_01.png](https://user-gold-cdn.xitu.io/2018/12/19/167c64351f790e02?w=844&h=278&f=png&s=153273)
在底层代码中间我们找到allocWithZone的底层方法。发现obj是由class_cerateInstance(cls,0)创建出来的。

![OC_malloc_size_02.png](https://user-gold-cdn.xitu.io/2018/12/19/167c64351f866c66?w=1012&h=204&f=png&s=156358)
然后我们在进入_class_createInstanceFromZone(cls, extraBytes, nil);

![OC_malloc_size_03.png](https://user-gold-cdn.xitu.io/2018/12/19/167c64360fb36a0b?w=1148&h=892&f=png&s=468192)
进入后会看到calloc(1,size)的alloc的实现代码，传入了一个size，size是instanceSize(extraBytes)得到的，我们再次进入

![OC_malloc_size_04.png](https://user-gold-cdn.xitu.io/2018/12/19/167c6436418ad137?w=1078&h=364&f=png&s=203705)
规定对象的字节至少是16个字节，
当我们的分配的size值小于16是 会把size设置为16
我们size传进来的就是alignedInstanceSize() 就是我们传进来的实例变量的大小 为8  所以当小于16的时候底层代码中返回的就是16 ， 所以分配的内存大小至少是16。

- `一个NSObject对象占用多少内存？`
  - `系统分配了16个字节给NSObject对象（通过malloc_size函数获得）`
  - `但NSObject对象内部只使用了8个字节的空间（64bit环境下，可以通过class_getInstanceSize函数获得`





### 自定义NSObject 占用的内存

```objc
#import <malloc/malloc.h>
#import <OBJC/runtime.h>
///> Student类
@interface Student: NSObject{
    @public
    int _no;
    int _age;
}

///> 实际底层的结构体 结构
//struct Student_IMPL{
//    Class isa,
//    int _no,
//    int _age;
//}
@end

@implementation Student
@end

///> main
int main(int argc, char * argv[]) {
    @autoreleasepool {
        NSObject *obj = [[NSObject alloc]init];
        Student *stu = [[Student alloc]init];
        stu->_no = 4;
        stu->_age = 5;
        NSLog(@"no:%d, age:%d",stu->_no, stu->_age);
        NSLog(@"%zd", class_getInstanceSize([Student class]));
        NSLog(@"%zd", malloc_size((__bridge const void *)stu));        
        
        /**输出结果
        no:4, age:5
        16
        16
        */ 
    }
    return 0;
}
```
- 当我们自定义一个NSObject的时候实际底层会有三个成员变量，isa指针占用8个字节，_no占用4个字节 _age真用4个字节，所以我们最后的结果为 **16，16**



**`思考： 如果我的Student有三个成员变量 那么会占用对少个字节？`**

**`class_getInstanceSize([Student class]) 的输出是多少？`**

**`malloc_size((__bridge const void *)stu的输出是多少？`**

```objc
#import <malloc/malloc.h>
#import <OBJC/runtime.h>
///> Student类
@interface Student: NSObject{
    @public
    int _no;
    int _age;
    int _gender;
}

///> 实际底层的结构体 结构
//struct Student_IMPL{
//    Class isa,
//    int _no,
//    int _age;
//    int _gender;

//}
@end

@implementation Student
@end

///> main
int main(int argc, char * argv[]) {
    @autoreleasepool {
        NSObject *obj = [[NSObject alloc]init];
        Student *stu = [[Student alloc]init];
        stu->_no = 4;
        stu->_age = 5;
        stu->_gender = 0;
        NSLog(@"%zd", class_getInstanceSize([Student class]));
        NSLog(@"%zd", malloc_size((__bridge const void *)stu));        
        
        /**输出结果
        24
        32
        */ 
    }
    return 0;
}
```
- 最重的输出结果为：  
  - class_getInstanceSize： 24
  - malloc_size： 32

isa:占用8个字节，_no:占用4个字节，_age:占用4个字节， _gender:占用4个字节, 不应该是一共占用了20个自己吗？为什么是24个呢？

**为什么会是24和32呢？？？？**
[窥探iOS底层实现--OC对象的本质(二) - 掘金](https://juejin.im/post/5c1b5c56e51d451b1c6db274)

---
- 文章总结自MJ老师底层视频。