# 属性

### 存储实例属性

### 存储类型属性

- 属性观察器，  相当有 监听更改

  ```swift
  struct Circle {
    var radius: Double {
      willSet{
        print("willSet", newValue)
      }
      didSet{
        print("didSet", oldValue, newValue)
      }
    }
    init() {
      self.radius = 1.0
      print("Circle init!")
    }
  }
  
  // Circle init!! 
  var circle = Circle()
  
  // willSet 10.5
  // didSet 1.0 10.5
  circle.radius = 10.5
  
  //10.5
  print(circle.radius)
  
  
  ```

- Set、Get 方法  存储属性和计算属性

  -   结构体只有 radius 存在了内存中， 栈空间
  - diameter 是计算属性  因为不确定 不需要存储 只是调用了set方法

  ```swift
  struct Circle {
    /// 存储属性
    var radius: Double
    /// 计算属性
    var diameter: Double {
      set{
        radius = newValue/2
      }
      get{
        radius * 2
      }
    }
  }
  
  ```

  

- 输入输出参数 inout

  - 如果输入输出参数传递的是一个 计算属性的参数 
  - 那么汇编中 
    - 是先调用了 属性的get方法，创建一块临时栈空间，将临时的栈空间地址值传递给test(包含inout的函数) ,
    - 执行函数更改 属性的值
    - 调用计算属性的set方法  给属性赋值   结束

  ```swift
  
  ```

- inout本质总结：

  - 如果实参有物理内存地址，并且没有设置属性观察值
    - 就会直接将实参地址传入函数  实参进行引用传递
  - 如果实参有物理内存地址， 设置了属性观察器
    - 采取了  copy In Copy Out做法
      - 调用函数时，先复制实参的值，产生副本（局部变量）【get】
      - 将副本的地址传入函数（副本进行引用传递），在函数内部修改副本的值
      - 函数返回后，再将副本的值覆盖实参的值【set】
  - 总结inout 本质就是 引用传递  地址传递。

  

  

### 类型属性



```swift
// 结构体重 只能 使用  static
struct Shape: Int{
  var width: Int = 10
	static var height = 10
}

Shape.Count = 10
print(Shape.Count)  /// 10


// 类中 使用  static和 class都可以
class Shape: Int{
  var width: Int = 10
	static var height = 10
  class var height1 = 10
}

Shape.Count = 10
print(Shape.Count)  /// 10
```





```swift
struct Shape: Int{
  var width: Int = 10
	static var height {
    return 10  // 只读属性
  }
}
print(Shape.Count)  /// 10
```





```swift
struct Shape: Int{
	static var height = 10
  init(){
    height += 1
  }
}
print(Shape.Count)  /// 10

var s1 = Shape()
var s2 = Shape()
var s2 = Shape()
print(Shape.height) /// 13
Shape.height ///是一个值 唯一的 




```



### 单利

```swift
public class FileManager {
  public static let share = FileManager()
  // or
  public static let share = FileManager{
    // balabala
    return FileManager()
  }()
  private init(){}
  func open(){
    
  }
  func close(){
    	
  }
}

FileManager.share.open()
FileManager.share.close()

```

### 存储实例属性

### 存储类型属性



### 类型属性细节

- 不同于存储实例属性，你必须给他一个存储类型属性设定一个初始值
  - 因为类型没有想 实例的init初始化器来初始化存储属性
- 存储类型属性默认是有lazy的  会在第一次使用的时候初始化
  - 就算是被多个线程访问 也只会初始化一次 
- 类型存储属性 可以是let的 ，    在lazy是不可以使用let的  但是类型属性是可以的 不存在实力初始化了





枚举是不可以定义存储实例属性    但是可以定义  `存储类型属性`

```swift
enum Shape{
  static var width: Int = 10  
	case s,w,e,d
}
print(Shape.width)  /// 10
/// 因为 存储类型属性不存在 枚举实例中   它类似于全局变量
```





0x100002158

0x100002160

0x100002168

0x100002170











