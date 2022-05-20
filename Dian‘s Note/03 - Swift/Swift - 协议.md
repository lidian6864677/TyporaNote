Swift - 协议 (protocol )

## 前序

1. 协议可以用来定义方法、属性、下标的声名，协议可以被枚举、结构体、类遵守（多个协议用 逗号 分割）
2. 协议中定义方法是不能有默认参数值
3. 默认情况下， 协议中定义的内容必须全部实现
   1. 也有办法办到只实现部分内容，

## 协议中的属性

- 默认情况下， 协议中定义的内容必须全部实现

- 协议中定义属性必须用var关键字

- 实现协议时的属性权限要不小于协议中定义的属性权限
  - 协议中定义 set get  实现必须有set get   使用var 关键字 
  - 协议使用了  get    就最低支持 get

```swift
protocol Drawable{
  func draw() 
  var x: Int { get set }
  var y: Int { get}
  subscript(index: Int) -> Int { get set }
}

class Person: Drawable{
  var x: Int = 0
//  或者
  var x :Int {
    set{}
    get{0}
  }
  let y: Int = 10
  func draw(){
    print("11")
  }
  subscript(index: Int) -> Int{
    set{}
    get{0}
  }
  
  
}
```

#### static class

- 为了保证通用，协议中必须用  static 定义类型方法、类型属性、类型下标
  - 因为 使用class的话， 在枚举和结构体中 不能使用了就   所以使用static 

#### mutating

- 只有将协议中的实例方法标记为 mutating（在结构体、枚举中 定义的函数添加mutating 可以允许内部修改外部定义的属性） 
  - 才能允许结构体。枚举。具体的实例修改自身内存
  - 类在实现方法是不用家mutating，枚举、结构体才需要接mutating

#### init

- 协议中还可以定义初始化器 init

  - 非final类实现时必须加上requied

    ```swift
    protocol Drawable{
      init(x: Int, y:Int)
    }
    
    class Point: Drawable{
      required init(x: Int, y: Int){}   
    }  
    final class Size: Drawable{
       init(x: Int, y: Int){}
     }
    }
    
    /// Point 遵守了 Drawable协议， 就要实现init初始化器， 为了保证继承于Point的子类都有init 所以加上required关键字， 以后继承了Point的子类都要实现init初始化器
    ```

- 如果一个类，继承的父类和遵守的协议， 的初始化器是相同的，还都必须要实现

  - 那么这个初始化必须加上 required override

    ```swift
    protocol Livable{
      init(age: Int)
    }
    class Person{
      init(age: Int){}
    }
    
    class Student: Person, Livable{
      required override init(age: Int){
        super.init(age: age)
      }
    }
    
    ```

    

    

- 协议可以遵守其他协议

  <img src="https://tva1.sinaimg.cn/large/006tNbRwgy1g9i6jibxqfj30g20h6juv.jpg" alt="image-20191018161234349" style="zoom:33%;" />

  

- 协议组合
  - 协议组合只能包含一个类类型，  
  
    <img src="https://tva1.sinaimg.cn/large/006tNbRwgy1g9i6jpu8ryj30xg0lqdut.jpg" alt="image-20191018161538655" style="zoom:50%;" />

## 常用协议

- CaseInterable  枚举遍历协议

  - 让枚举遵守CaseInterable协议，可以实现遍历枚举值

    ​	<img src="https://tva1.sinaimg.cn/large/006tNbRwgy1g9i6jk6tdoj30iu0a4tck.jpg" alt="image-20191018161638774" style="zoom:50%;" />

- CustomStringConvertible  自定义实例打印字符串

  
  
  <img src="https://tva1.sinaimg.cn/large/006tNbRwgy1g9i6jwax5qj30hy0emwjg.jpg" alt="image-20191018163428539" style="zoom:50%;" />

### 关键字

- Any、AnyObject

  - Any：可以代表任意类型（枚举、结构体、类、也包括函数类型）

  - AnyObject：可以代表任意`类`类型（在协议后面写上：AnyObject 代表只能 类遵守这个协议）

    
    
    <img src="https://tva1.sinaimg.cn/large/006tNbRwgy1g9i6jzkrbij30ys05k76t.jpg" alt="image-20191018163638377" style="zoom:50%;" />

- is 、as? 、as！、as

  - is：is是来判断是否为某种类型

  - as：用来做强制转换

    

    <img src="https://tva1.sinaimg.cn/large/006tNbRwgy1g9i6k39mkhj319q0l0qgo.jpg" alt="image-20191018164610921" style="zoom:33%;" />
    
    

- X.self、X.Type、AnyClass
  - X.self:是一个元类型（metadata）的指针，metadata存放着类型相关信息
  - X.self属于X.Type类型

### 元类型应用

<img src="https://tva1.sinaimg.cn/large/006tNbRwgy1g9i6kc5cbtj313s0qidrg.jpg" alt="image-20191202104254298" style="zoom: 33%;" />























------

<p align="right" color="orange">	小李小李一路有你</p><p align="right" color="orange">	Dian。</p>	
