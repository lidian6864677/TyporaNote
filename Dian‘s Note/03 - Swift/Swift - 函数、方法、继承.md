# Swift - 函数、方法、继承



## 方法

- 枚举、结构体、类都可以定义为实例方法、类型方法

  - 实例方法：通过实例调用
  - 类型方法：通过类型调用。用static或者class关键字定义

  ```swift
  class Car() {
    
    static var count = 0
    init(){
      Car.count += 1
    }
    static func getCount() -> Int {
  	    count    // 等价于直接写  self.count 
    }
    let c0 = Car()
    let c1 = Car()
    let c1 = Car()
    print(Car.getCount) /// 3
  }
  ```

### 实例方法

### 类型方法

### 关键字 mutating

结构体和枚举的值类型，默认情况下值类型的属性不能被自身的实例方法修改

如果在方法中修改值类型的属性的话  在 func前面加上`mutating`就可以允许这种修改行为了

```swift
// 结构体和枚举的值类型
struct Point{
  var x = 0.0, y = 0.0
  func moveBy(deltaX: Double, deltaY: Double)	{
    x += deltaX     // ❌
    y += deltaY     // ❌
  }
  /// 添加 mutating 就可以了
  mutating func moveBy(deltaX: Double, deltaY: Double)	{
    x += deltaX     // ✅
    y += deltaY     // ✅
  }
}

// 类 的话 就可以了
class Point{
  var x = 0.0, y = 0.0
  func moveBy(deltaX: Double, deltaY: Double)	{
    x += deltaX     // ✅
    y += deltaY     // ✅
  }
}
```

### 关键字 @discardableResult

在func 前面加上`@discardableResult`当函数有返回值但是未使用到，不想产生警告的时候使用

```swift
struct Point{
  var x = 0.0, y = 0.0
  mutating func moveX(detlaX: Double) -> Double{
    x += detlaX
    return x
  }
  @discardableResult mutating func moveXXX(detlaX: Double) -> Double{
    x += detlaX
    return x
  }
  var p = Point()
  p.moveX(detlaX:10)  // ⚠️ 有返回值但是未使用 
  p.moveXXX(detlaX:10)  // 不会产生警告
}
```



## 函数

啊

## 继承

- 值类型（枚举、结构体） 不支持继承、只有类支持继承
- 没有父类的类 成为基类
  - Swift 并没有像OC JAVA 那样规定，任何一个类都要继承自某个类
  - 子类可以重写父类的 下标、方法、属性。重写必须加上override关键字

### 内存结构

![image-20191017110136945](/Users/yuangonmg/Library/Application Support/typora-user-images/image-20191017110136945.png)

### 重写属性

- 子类可以将父类的属性（存储属性、计算属性）重写为计算属性
- 子类不可以将父类属性重写为存储属性
- 只能重写var属性，不能重写let属性
- 重写时，属性名、类型要一致
- 子类重写后的属性权限不能小于父类属性的权限
  - 如果父类属性是只读的，那么子类重写后的属性可以是只读的、也可以是可读可写的
  - 如果父类属性是可读写的，那么子类重写后的属性必须是可读写的

#### 重写实例属性

![image-20191017110121181](/Users/yuangonmg/Library/Application Support/typora-user-images/image-20191017110121181.png)

#### 重写类型属性

![image-20191017111507310](/Users/yuangonmg/Library/Application Support/typora-user-images/image-20191017111507310.png)





属性观察器 

之前说过 一个类中不能同时存在 计算属性和属性观察器， 但是 可以通过继承实现

let属性和只读属性  不能添加属性观察器

![image-20191017113746370](/Users/yuangonmg/Library/Application Support/typora-user-images/image-20191017113746370.png)



#### 关键字 final

如果这个类不希望被别人继承 就加上`final`关键字



#### 多态的实现原理

- 父类类型的指针直指向子类对象

  ![image-20191017114408160](/Users/yuangonmg/Library/Application Support/typora-user-images/image-20191017114408160.png)

- OC ： runtime  将方法放到方法列表里main 
- C++： 虚函数表
- swift： 更接虚表、







指针变量 ->  指向了堆空间

- 前8个字节存放的 内存地址  内存地址 指向另一个地方  就是 方的类型信息 元类数据
  - 类型信息  类相关的东西
- 后8个字节存放的是 引用计数
- 成员变量....

























------

<p align="right" color="orange">	小李小李一路有你</p><p align="right" color="orange">	Dian。</p>	
