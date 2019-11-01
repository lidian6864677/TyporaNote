# Swift - 枚举、类、结构体

## 枚举

OC语言的枚举 本质就是整形

先来看看怎么写的；

```swift
enum Num{
	case:	one
  case: two
	case: three
	case: four
}	
enum Num{
  case: one, two, three, four
}


var nu = Num.one
nu = Num.two
nu = .three
print(nu)

```



也可以用在swift中

- 关联值

  枚举的成员之和其他类型的值关联存储在一起的时候

- 原始值

  啊

- Memorylayout

  ```swift
  var age = 10
  
  MemoryLayout<Int>.size      ///  实际使用的   8
  MemoryLayout<Int>.stride    ///  系统分配的   8
  MemoryLayout<Int>.alignment /// 内存对其基数  8
  
  MemoryLayout.size(ofValue:age)	
  MemoryLayout.stride(ofValue:age)
  MemoryLayout.alignment(ofValue:age)
  ```

  下面的输出是多少

  关联值

  原始值

  

  ```swift
  // eg1
  enum Password{
    case number( Int, Int, Int, Int)
    case other
  }	
  var pwd = Password.number(1, 2, 4, 6)
  pwd = .other
  MemoryLayout<Password>.size      ///  实际使用的   33
  MemoryLayout<Password>.stride    ///  系统分配的  40
  MemoryLayout<Password>.alignment /// 内存对其基数  8
  
  
  //  eg:2
  enum Season{
    case spring, summer, autumn, winter
  }
  var sea = Season.spring
  MemoryLayout<Season>.size      ///  实际使用的   1
  MemoryLayout<Season>.stride    ///  系统分配的   1
  MemoryLayout<Season>.alignment /// 内存对其基数   1
  ```


枚举的值  值占用1个字节 

原始值占用的是对应的 类型内存   eg： Int: 8个

### rawValue

- 枚举的rawValue本质实际是一个计算属性

  ```swift
  enum Season{
    case spring = 1, summer, autumn, winter
  }
  var s = Season.spring
  print(s.rawValue)  // 1
  
  enum Season{
    case spring = 1, summer, autumn, winter
    var rawValue: Int{
      switch self{
        case: .spring
  	      return 11
        case: .summer
  	      return 22
  			case: .autumn
  	      return 33
        case: .winter
  	      return 44
      }
    }
  }
  var s = Season.spring
  print(s.rawValue)  // 11
  
  
  ```

  所以  枚举的原始值rawValue 是一个计算属性取的值， 并没有占用内存

  ![image-20191015195251353](/Users/yuangonmg/Library/Application Support/typora-user-images/image-20191015195251353.png)

  并且不能更改   只有get方法





## 结构体

在swift 标准库中，绝大多数的公开类型都是结构体，而枚举和类只占很小一部分

比如 Bool、Int、Double、String、Array、 Dictionary等常见数据类型都是结构体



```swift
struct Date{
	var year: Int
	var month: Int
	var day: Int
}
var date = Date(year:2019, month:10, day:8)	
```

所有的机构体都是一个编译器自动生成的初始化器（Initializers，初始化方法，构造器，构造方法）

在第6行调用时，可以传入所有成员的值，用来初始化所有成员（存储属性、Stored Property）

#### 结构体的初始化器

编译器会根据情况，可能会为结构体生成朵哥初始化器，宗旨是：保证所有成员都有初始化的值

```swift
struct Point{
  var x: Int
  var y: Int
}

var p1 = Point(x: 10, y: 10)
var p2 = Point(y: 10)  	 /// ❌ 报错
var p3 = Point(x: 10) 		 /// ❌ 报错
var p4 = Point()			   /// ❌ 报错
```

```swift
struct Point{
  var x: Int = 0
  var y: Int
}

var p1 = Point(x: 10, y: 10)
var p2 = Point(y: 10)  	 
var p3 = Point(x: 10) 		 /// ❌ 报错
var p4 = Point()			   /// ❌ 报错
```

```swift
struct Point{
  var x: Int
  var y: Int = 0
}

var p1 = Point(x: 10, y: 10)
var p2 = Point(y: 10)  	 /// ❌ 报错
var p3 = Point(x: 10) 		 
var p4 = Point()			   /// ❌ 报错
```

```swift
struct Point{
  var x: Int = 0
  var y: Int = 0
}

var p1 = Point(x: 10, y: 10)
var p2 = Point(y: 10)  	 
var p3 = Point(x: 10) 		 
var p4 = Point()			   
```



#### 自定义初始化器

一旦自定义了初始化器， 编译器将不会自动生成其它初始化器

```swift
struct Point{
  var x: Int = 0
  var y: Int = 0
  init(x: Int, y:Int){
    self.x = x
    self.y = y 
  }
}

var p1 = Point(x: 10, y: 10)
var p2 = Point(y: 10)    /// ❌ 报错
var p3 = Point(x: 10)   	/// ❌ 报错
var p4 = Point()	      /// ❌ 报错
```



## 类

类的定义和结构体类似，但是编译器并没有为类生成可以传入的成员值的初始化器

```swift
class Point{
  var x: Int = 0
  var y: Int = 0
}
var p4 = Point()	      
var p1 = Point(x: 10, y: 10) /// ❌ 报错
var p2 = Point(y: 10)       /// ❌ 报错
var p3 = Point(x: 10)   	  /// ❌ 报错   结构体中可以使用但是类中不可以
```

```swift
class Point{
  var x: Int
  var y: Int
}
var p4 = Point()	  /// ❌ 报错   如果是无参数初始值的  就什么初始化器都不生成，
```



### 结构体和类的本质区别



- 类是支持继承的
- 结构体不支持

在swift中结构体和类中都可以定义方法的

值类型： 结构体和枚举，字典，数组。

引用类型： 类，指针类型

```swift
class Size{
	var width = 1
  var height = 2
}

struct Point{
  var x = 3
  var y = 4
}

func test() {
  var size = Size()  // 指针变量
  var point = Point() 
}
```

![image-20191010162250569](/Users/yuangonmg/Library/Application Support/typora-user-images/image-20191010162250569.png)

- point 内存 和 size的指针变量内存存储在栈空间
  - 堆空间size 存储的是 size对象的在堆空间存储的内存地址	

## 值类型

### 值类型的赋值操作

- 在swift标准库中，为了提升性能， String、Array、Set 才去了Copy On Write 
  - 比如当仅有“写”操作时， 才会真正的进行拷贝操作
  - 对于标准库值类型的操作  swift能确保最佳性能，所以没有必要考虑保证性能而避免赋值。
  - 只有标准库有这个操作  自己定义的结构体不会的！！！！
- 不需要修改的  尽量定义为   let

```swift
var s1 = "asd"
var s2 = s1
 /// 如果s1 和s2 没有做修改的操作是不会copy的  可以认为 s1和s2是同一块内存 

s2 = "123123"
/// 如果s1 和s2 有Wirte的操作就会进行深拷贝，  可以认为 s1和s2  不是同一块内存
```



### 结构体和枚举存储位置

- 在外部声名是全局变量的话  ：  **存储在 全局区 数据段**

- 在函数中声名的话    ：**存储在 栈空间**

- 在对象中声名了一个 结构体 ： **会跟随对象在 对空间存储**

- 枚举同理

- 类的 存储空间  

  无论在哪里用类创建对象  ，   对象的内存在堆空间

  但是 类的  指针变量的内存 和结构体和枚举相同

```swift


struct Point{
  var x = 3
  var y = 4
}
var point = Point()    //// 这个在全局区
//  -------
func test() {
  class Size{
		var width = 1
	  var height = 2
    var point = Point()   /// 跟随 对象在堆空间
	}
  //  -------
  var size = Size()  
  struct Point{
  	var x = 3
  	var y = 4
	}
	var point = Point()    //// 这个在 栈空间
}
```



## 引用类型



Point: 值类型 是在函数里面创建的 在栈空间里面的

### 对象的对空间申请过程

- 在swift中创建类的实例对象，要想堆空间申请内存，大概流程

  - Class.__allocation_init()
  - Libswiftcore.dylib: _swift_allobject_
  - Libswiftcore.dylib: swift_slowAlloc
  - Libsystem_malloc.dylib: malloc

- 在Mac、iOS中的malloc函数分配的内存大小的总数是16的倍数

- 通过 class_getInstanceSize可以得知类对象真正使用的内存大小

  ```swift
  class Point {
    var x = 11
    var test = true 
    var y = 22
  }
  var p = Point()
  class_getInstanceSize(type(of: p)) // 40
  class_getInstanceSize(Point.self) // 40
  ```

  



### 全局变量：

​	程序已启动  内存地址就固定的不变的

局部变量  每次调用都有新的分配内存



方法也好 函数也好都是写死在代码段的 

创建的时候分配内存存放他自己的成员变量 

对象中不会有内存拿来放函数和方法的代码





## 初始化器

- 指定初始化器
- 便捷初始化器
- 每一个类至少有一个指定初始化器、为主要初始化器
- 默认初始化器 是类的指定初始化器
- 类偏向于少量的指定初始化器，多使用便捷初始化器 。 一个类通常只有一个指定初始化器

- 初始化器相互调用原则

  - 指定初始化器必须从它的直系父类调用指定初始化器

    - 继承下来类
      - 如果是指定初始化器 一定要调用父类的指定初始化器
      - 如果是便捷初始化器 一定要调用自己的指定初始化器

  - 便捷初始化器 必须从相同的类中调用另外一个初始化器

    - 继承下来类

    - 如果是指定初始化器 一定要调用父类的指定初始化器
    - 如果是便捷初始化器 一定要调用自己的指定初始化器

  - 便捷初始化器最终 必须调用一个指定初始化器 

    

#### 关键字 convenience

为了安全  保证任何一套初始化器都是完整 安全的

![image-20191017164736100](file:///Users/yuangonmg/Library/Application%20Support/typora-user-images/image-20191017164736100.png?lastModify=1571302103)

### 两段式初始化

swift 在安全方面很下功夫、为了保证初始化过程安全，设定了两端初始化器、安全检查

- 两段初始化器

  - 第一阶段，初始化所有存储属性

    - 外层调用指定/便捷初始化器

    - 分配内存给实例，但未初始化，（内部可能是一些垃圾数据 没有具体的值）

    - 指定初始化器确保当前类定义的存储属性都初始化（最重都会调用父类的指定初始化器）

    - 指定初始化器调用父类的指定初始化器，不断向上调用，行成初始化器链条

      (继承过来的 要先初始化好自己的存储属性然后在调用父类的初始化器，父类去初始化自己的存储属性等...)

  - 第二阶段，设置新的存储属性的值

    - 从顶部初始化器往下，链中的每一个指定初始化器都有机会进一步定制这个实例
    - 初始化器现在能够使用self（访问、修改它的属性，调用它的实例方法等）
    - 最终，链中任何便捷初始化器都有机会定制实例以及使用self 

安全检查

#### 重写

- 当重写父类的指定初始化器的，必须加上override（即使子类实现的是便捷初始化器）
- 如果子类写了一个“匹配”父类便捷初始化器的初始化器，不用加上override
  - 因为父类的便捷初始化器永远不能被子类直接调用，因此严格来说，子类无法重写父类的便捷初始化器

#### 自动继承

1. 如果子类没有主动定义任何 指定初始化器， 那么子类会自动继承父类的初始化器
2. 如果子类提供了父类所有指定初始化器的实现（要么通过1.继承 要么重写）
   - 子类自动继承所有父类便捷初始化器



#### 关键字 required

用required 修饰指定初始化器，表明其所有子类都必须实现该初始化器（通过继承或者重写实现）

如果子类重写了 required初始化器，时也必须加上required，不能用override



### 类和结构体的选择

1. 类和结构体都可以用来定义自定义的数据类型，作为你的程序代码构建块。
2. 总之，结构体实例总是通过值来传递，而类实例总是通过引用来传递。
3. 按照通用准则，当符合以下一条或多条情形时应考虑创建一个结构体：

```swift
1. 结构体的主要目的是为了封装一些相关的简单数据值。
2. 当你在赋予或者传递结构体时，有理由需要封装的数据值被拷贝而不是被引用。
3. 任何存储在结构体中的属性是值属性，也将被拷贝而不是被引用。
4. 结构体不需要从一个已存在类型继承属性或者行为。
```

1. 合适的结构体候选者包括：

```swift
1. 几何形状的大小，可能封装了一个 width属性和 height属性，两者都为 double类型。
2. 一定范围的路径，可能封装了一个 start属性和 length属性，两者为 Int类型。
3. 三维坐标系的一个点，可能封装了 x , y 和 z属性，他们都是 double类型。
```

- **在其他情况下，定义一个类，并创建这个类的实例通过引用来管理和传递。事实上，大部分的自定义数据结构应该是类，而不是结构体。**







[Learning Swift](https://www.jianshu.com/nb/15792106)





------

<p align="right" color="orange">	小李小李一路有你</p><p align="right" color="orange">	Dian。</p>	
