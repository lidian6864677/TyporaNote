# 从OC到Swift

- **编程范式**
  - Swift 可以面向协议、函数式、面向对象编程。
  - OC 只能面向对象编程、不过引入 RAC类库也可以实现函数式编程。
- **类型安全**
  - Swift 是类型安全的语言。鼓励程序员在代码中清晰明确值的类型。如果代码中使用String定义了一个字符串，那么你不能传递一个Int类型给他，会在编译的时候就做类型检查，标记错误
  - OC 声名一个NSString类型的变量，仍然可以传递一个NSNumber给它，会Warning但是可以使用
- **值类型增强**
  - Swift  典型的struct、enum、tuple都是值类型。平时使用的Int、Double、Float、String、Array、Dictionary、Set其实都是结构体实现的，也是值类型
  - OC 中NSNumber、NSString以及集合类对象都是指针类型的
- **枚举增强**
  - Swift 的枚举可以使用整形、浮点型、字符串等。还能拥有属性和方法，甚至支持泛型、协议、扩展等
  - OC中的枚举就很鸡肋，仅用于整形。
- **泛型**
  - Swift 中支持泛型，也支持泛型的类型约束等特性
  - OC 在Swift2.0的时候也为OC增加了泛型， 但是仅仅停留在编译器会警告的阶段。
- **协议和扩展**
  - Swift 对协议的支持更加丰富，配合扩展(extension)、泛型、关联类型等可以实现面向协议编程、从而打打提高代码的灵活性。同时，Swift中的protocol还可以用于值类型，如结构体和枚举。
  - OC 的协议缺乏强约束，提供的optional特性会成为很多问题的来源，但是舍弃optional就会让代码的实现代价变大。
- **函数和闭包**
  - Swift 可以直接定义函数类型变量，可以作为其他函数参数传递，可以作为函数返回值
  - OC 需要用selector封装或者block才能实现Swift中的效果



## 从OC带Swift

### 注释 标记

```swift
// MARK:    类似于OC中的  #pragma mark 
// MARK:-   类似于OC中的  #pragma mark -
// TODO:  	用来标记未完成的任务	
// FIXME:   待修复的问题
	#warning： 警告⚠️  比较明显

```

​	![image-20191025164439963](/Users/yuangonmg/Library/Application Support/typora-user-images/image-20191025164439963.png)

### 条件编译	

```swift
// 操作系统：macOS\iOS\tvOS\watchOS\linux\Android\Windows\FreeBSD
#if os(macOS) || os(iOS)
// CPU构架：i386\x86_64\arm\arm64
#elseif arch(x86_64) || arch(arm64)
// swift版本
#elseif swift(<5) && swift(>=3)
// 模拟器
#elseif targetEnvironment(simulator)
// 可以导入某模块
#elseif canImport(Foundation)
#else
#endif
```

​	![image-20191025165514814](/Users/yuangonmg/Library/Application Support/typora-user-images/image-20191025165514814.png)

### 系统检测版本

```swift
if #available(iOS 10, macOS 10.12, *){
  // 对于iOS10平台，10及以上版本
  // macOS 10.12及以上版本
  // * 其他所有平台
}
```





### Swift调用OC



```
{targetname}-Bridging-Header.h
```



<img src="/Users/yuangonmg/Library/Application Support/typora-user-images/image-20191025173637044.png" alt="image-20191025173637044" style="zoom:50%;" />

<img src="/Users/yuangonmg/Library/Application Support/typora-user-images/image-20191025173832152.png" alt="image-20191025173832152" style="zoom: 50%;" />

 - swift 和OC中有一样的函数的时候怎么办

    - 函数会优先调用swift中的函数

    - 如果不能改两边的函数名  就使用

      ```swift
      @_silgen_name("sum")
      func swift_sum(_ a: Int32, _ b: Int32) -> Int32
      ```



### OC调用Swift

![image-20191030140237966](/Users/yuangonmg/Library/Application Support/typora-user-images/image-20191030140237966.png)

重点： Swift的类要想暴露给OC使用

1.  必须继承自NSObject
2. 选择性暴露方法  方法前加上@objc
3. 全部暴露的话      类前加上 @objcMenbers

问题： 

- 为什么swift的类 暴露给OC的使用要继承自NSObject  ？
  - 因为在OC中实现这个类的时候调用的是  alloc init， 需要使用isa指针这些都在NSObject中，走的是  msg_send的流程 所以需要继承自NSObject
- swift中调用纯swift的实例方法实现原理
  - 以类似于C++中续表的形式调用的，swift实例的指针变量指向的是对空间地址，前8个字节指向的存放类信息的地址 - 接下来的8个字节存储的是引用计数等信息，之后才是实例的成员变量等信息
- swift中调用 OC实例方法实现原理
  - 通过汇编查看  看到了汇编中有调用msg_send的方法信息， 所以和OC的实现原理相同
- OC中调用 Swift的实例方法实现原理
  - 同问题(1) 由于创建的时候使用了alloc init， 走的也是OC 中的 msg_send 原理 isa指针
- OC中调用OC的实例方法实现原理
  - msg_send

- 如果swift实例对象继承自NSObject 内存结构有什么变化？ 例如有两个属性
  - 没有继承的实例对象：
    -  共占用32个字节 
      - 8：指向元类信息数据的内存地址，
      - 8：引用计数
      - 8：实例对象的属性
      - 8：实例对象的属性
  - 继承自NSObject的实例对象
    - 占用32个字节
      - 8： isa
      - 实例对象的属性
      - 实例对象的属性



------

<p align="right" color="orange">	小李小李一路有你</p><p align="right" color="orange">	Dian。</p>	
