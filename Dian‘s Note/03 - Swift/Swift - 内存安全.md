# Swift - 内存安全、访问控制、自动引用计数



## 内存管理

- 跟OC一样，Swift也是才取基于引用计数的ARC内存管理方案 （针对堆空间）

- Swift 的ARC中有3种引用

  - 强引用(Strong)： 默认情况下，引用就是强引用

  - 弱引用(weak）： 通过weak定义弱引用

    - 必须是 `可选类型  var` ， 因为实例销毁后  ARC 会自动将弱引用设置为nil
    - ARC自动给弱引用设置为nil 时，是不会出发属性观察器的（willSet、didSet）

  - 无主引用（ unowned ）：通过unowned定义无主引用

    - 不会产生强引用，实例销毁后扔存储这实例的内存地址（类似于OC的unsafe_unretained）

    - 试图在实例销毁后访问无主引用，会产生运行时错误（野指针）

      ![image-20191023164534703](/Users/yuangonmg/Library/Application Support/typora-user-images/image-20191023164534703.png)

### weak和unowned使用限制

- weak和unowned只能使用在类实例上面

### Autoreleasepool



### 循环引用

- weak和unowned都能解决循环引用给的问题，unowned要比weak少一些性能消耗

  - 在生命周期中可能会变为nil的使用weak

  - 初始化赋值之后在也不会变为nil的使用 unowned

    

### @escaping    逃逸闭包

### 内存访问冲突

- 如果下面的条件可以满足，就说明重叠访问结构体的属性是安全的
  - 只访问实例存储属性，不是计算属性或者类属性
  - 结构体是 局部变量  而非全局变量
  - 结构体要么没有被播报捕获， 要么只被非逃逸闭包捕获

### 指针  

指针和引用是不同的额

这个是swift  专门提供的指针类型  unsafe的  不安全的

swift中也有专门的指针类型，这些都被定性为  不安全的， 常见的有4种

- UnsafePointer<Pointee>  类似于 const Pointee *
- UnsafeMutablePointer<Pointee>  类似于 Pointee *
- UnsafeRawPointer  类似于  const void *
- UnsafeMutableRawPointer  类似于  void *



















------

<p align="right" color="orange">	小李小李一路有你</p><p align="right" color="orange">	Dian。</p>	
