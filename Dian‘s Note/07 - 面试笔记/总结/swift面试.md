# Swift系列面试题总结

## 基础题：

### 1.类(class) 和 结构体(struct) 有什么区别? 类(class) 和 结构体(struct)  比较,优缺点?

```
答：
struct是值类型，class是引用类型。

值类型的变量直接包含它们的数据，对于值类型都有它们自己的数据副本，因此对一个变量操作不可能影响另一个变量。
引用类型的变量存储对他们的数据引用，因此后者称为对象，因此对一个变量操作可能影响另一个变量所引用的对象。

二者的本质区别：struct是深拷贝，拷贝的是内容；class是浅拷贝，拷贝的是指针。

property的初始化不同：class 在初始化时不能直接把 property 放在 默认的constructor 的参数里，而是需要自己创建一个带参数的constructor；而struct可以，把属性放在默认的constructor 的参数里。
变量赋值方式不同：struct是值拷贝；class是引用拷贝。
immutable变量：swift的可变内容和不可变内容用var和let来甄别，如果初始为let的变量再去修改会发生编译错误。struct遵循这一特性；class不存在这样的问题。
mutating function： struct 和 class 的差別是 struct 的 function 要去改变 property 的值的时候要加上 mutating，而 class 不用。
继承： struct不可以继承，class可以继承。
struct比class更轻量：struct分配在栈中，class分配在堆中。
复制代码
答：
class 有以下功能,struct 是没有的:

class可以继承,子类可以使用父类的特性和方法
类型转换可以在运行时检查和解释一个实例对象
class可以用 deinit来释放资源
一个类可以被多次引用
struct 优势:

结构较小,适用于复制操作,相比较一个class 实例被多次引用,struct 更安全
无需担心内存泄露问题
复制代码
```





### 2.Swift 是面向对象还是函数式的编程语言?

```
答：
Swift 既是面向对象的，又是函数式的编程语言。
说 Swift 是面向对象的语言，是因为 Swift 支持类的封装、继承、和多态，从这点上来看与 Java 这类纯面向对象的语言几乎毫无差别。
说 Swift 是函数式编程语言，是因为 Swift 支持 map, reduce, filter, flatmap 这类去除中间状态、数学函数式的方法，更加强调运算结果而不是中间过程。
复制代码
```





### 3.什么是泛型，swift哪些地方使用了泛型？

```
答：
泛型（generic）可以使我们在程序代码中定义一些可变的部分，在运行的时候指定。使用泛型可以最大限度地重用代码、保护类型的安全以及提高性能。
泛型可以将类型参数化，提高代码复用率，减少代码量。

例如 optional 中的 map、flatMap 、?? (泛型加逃逸闭包的方式，做三目运算)
复制代码
```

[Swift 泛型](https://www.runoob.com/swift/swift-generics.html) 
 
 

### 4.swift 语法糖 ？ ！的本质（实现原理）

```
答：
？为optional的语法糖
optional<T> 是一个包含了nil 和普通类型的枚举，确保使用者在变量为nil的情况下处理

！为optional 强制解包的语法糖
复制代码
```





### 5.什么是可选型(Optional)，Optional（可选型） 是用什么实现的

```
答：
1.在 Swift 中,可选型是为了表达一个变量为空的情况,当一个变量为空,他的值就是 nil
在类型名称后面加个问号? 来定义一个可选型
值类型或者引用类型都可以是可选型变量

2.Optional 是一个泛型枚举
大致定义如下:

enum Optional<Wrapped> {
  case none
  case some(Wrapped)
}

除了使用 let someValue: Int? = nil 之外, 还可以使用let optional1: Optional<Int> = nil 来定义
复制代码
```





### 6.什么是高阶函数

```
答：
一个函数如果可以以某一个函数作为参数, 或者是返回值, 那么这个函数就称之为高阶函数, 如 map, reduce, filter
复制代码
```





### 7.如何解决引用循环

```
答：
转换为值类型, 只有类会存在引用循环, 所以如果能不用类, 是可以解引用循环的,
delegate 使用 weak 属性.
闭包中, 对有可能发生循环引用的对象, 使用 weak 或者 unowned, 修饰
复制代码
```





### 8.定义静态方法时关键字 static 和 class 有什么区别

```
答：
static 定义的方法不可以被子类继承, class 则可以

class AnotherClass {
    static func staticMethod(){}
    class func classMethod(){}
}
class ChildOfAnotherClass: AnotherClass {
    override class func classMethod(){}
    //override static func staticMethod(){}// error
}
复制代码
```





### 9.请说明并比较以下关键词：Open, Public, Internal, File-private, Private

```
答：
Swift 有五个级别的访问控制权限，从高到底依次为比如 Open, Public, Internal, File-private, Private。

他们遵循的基本原则是：高级别的变量不允许被定义为低级别变量的成员变量。比如一个 private 的 class 中不能含有 public 的 String。反之，低级别的变量却可以定义在高级别的变量中。比如 public 的 class 中可以含有 private 的 Int。

Open 具备最高的访问权限。其修饰的类和方法可以在任意 Module 中被访问和重写；它是 Swift 3 中新添加的访问权限。
Public 的权限仅次于 Open。与 Open 唯一的区别在于它修饰的对象可以在任意 Module 中被访问，但不能重写。
Internal 是默认的权限。它表示只能在当前定义的 Module 中访问和重写，它可以被一个 Module 中的多个文件访问，但不可以被其他的 Module 中被访问。
File-private 也是 Swift 3 新添加的权限。其被修饰的对象只能在当前文件中被使用。例如它可以被一个文件中的 class，extension，struct 共同使用。
Private 是最低的访问权限。它的对象只能在定义的作用域内使用。离开了这个作用域，即使是同一个文件中的其他作用域，也无法访问。
复制代码
```





### 10.swift中,关键字 guard 和 defer 的用法 guard也是基于一个表达式的布尔值去判断一段代码是否该被执行。与if语句不同的是，guard只有在条件不满足的时候才会执行这段代码。

```
答：
guard let name = self.text else {  return }
复制代码
defer的用法是，这条语句并不会马上执行，而是被推入栈中，直到函数结束时才再次被调用。

defer {
   //函数结束才调用
}
复制代码
```





### 11.关键字:Strong,Weak,Unowned 区别?

```
答：
Swift 的内存管理机制同OC一致,都是ARC管理机制; Strong,和 Weak用法同OC一样

Unowned(无主引用), 不会产生强引用，实例销毁后仍然存储着实例的内存地址(类似于OC中的unsafe_unretained), 试图在实例销毁后访问无主引用，会产生运行时错误(野指针)
复制代码
```





### 12.如何理解copy-on-write?

```
答：
值类型(比如:struct),在复制时,复制对象与原对象实际上在内存中指向同一个对象,当且仅当修改复制的对象时,才会在内存中创建一个新的对象,

为了提升性能，Struct, String、Array、Dictionary、Set采取了Copy On Write的技术
比如仅当有“写”操作时，才会真正执行拷贝操作
对于标准库值类型的赋值操作，Swift 能确保最佳性能，所有没必要为了保证最佳性能来避免赋值
复制代码
```





### 13.什么是属性观察?

```
答：
属性观察是指在当前类型内对特性属性进行监测,并作出响应,属性观察是 swift 中的特性,具有2种, willset 和 didset

var title: String {
    willSet {
        print("willSet", newValue)

    }
    didSet {
        print("didSet", oldValue, title)
    }
}

willSet会传递新值，默认叫newValue
didSet会传递旧值，默认叫oldValue
在初始化器中设置属性值不会触发willSet和didSet
复制代码
```





### 14.swift 为什么将 String,Array,Dictionary设计为值类型?

```
答：
值类型和引用类型相比,最大优势可以高效的使用内存,值类型在栈上操作,引用类型在堆上操作,栈上操作仅仅是单个指针的移动,而堆上操作牵涉到合并,位移,重链接,Swift 这样设计减少了堆上内存分配和回收次数,使用 copy-on-write将值传递与复制开销降到最低
复制代码
```





### 15..如何将Swift 中的协议(protocol)中的部分方法设计为可选(optional)?

```
答：
1.在协议和方法前面添加 @objc,然后在方法前面添加 optional关键字,改方式实际上是将协议转为了OC的方式
@objc protocol someProtocol {
  @objc  optional func test()
}

2.使用扩展(extension),来规定可选方法,在 swift 中,协议扩展可以定义部分方法的默认实现

protocol someProtocol {
    func test()
}

extension someProtocol{
    func test() {
        print("test")
    }
}
复制代码
```





### 16.比较Swift 和OC中的初始化方法 (init) 有什么不同?

```
答：
swift 的初始化方法,更加严格和准确, swift初始化方法需要保证所有的非optional的成员变量都完成初始化, 同时 swfit 新增了convenience和 required两个修饰初始化器的关键字

convenience只提供一种方便的初始化器,必须通过一个指定初始化器来完成初始化
required是强制子类重写父类中所修饰的初始化方法

复制代码
```





### 17.比较 Swift和OC中的 protocol 有什么不同?

```
答：
Swift 和OC中的 protocol相同点在于: 两者都可以被用作代理;
不同点: Swift中的 protocol还可以对接口进行抽象,可以实现面向协议,从而大大提高编程效率,Swift中的protocol可以用于值类型,结构体,枚举;

复制代码
```





### 18.swift 和OC 中的自省 有什么区别?

```
答：
自省在OC中就是判断某一对象是否属于某一个类的操作,有以下2中方式

[obj iskinOfClass:[SomeClass class]]
[obj isMemberOfClass:[SomeClass class]]

在 Swift 中由于很多 class 并非继承自 NSObject, 故而 Swift 使用 is 来判断是否属于某一类型, is 不仅可以作用于class, 还是作用于enum和struct

复制代码
```





### 19.什么是函数重载? swift 支不支持函数重载?

```
答：
函数重载是指: 函数名称相同,函数的参数个数不同, 或者参数类型不同,或参数标签不同, 返回值类型与函数重载无关

swift 支持函数重载

复制代码
```





### 20.swift 中的枚举,关联值 和 原始值的区分?

```
答：
1.关联值--有时会将枚举的成员值跟其他类型的变量关联存储在一起，会非常有用
// 关联值
enum Date {
  case digit(year: Int, month: Int, day: Int)
  case string(String)
}

2.原始值--枚举成员可以使用相同类型的默认值预先关联，这个默认值叫做:原始值
// 原始值
enum Grade: String {
  case perfect = "A"
  case great = "B"
  case good = "C"
  case bad = "D"
}

复制代码
```





### 21.swift 中的闭包结构是什么样子的?什么是尾随闭包?什么是逃逸闭包?什么是自动闭包?

```
答：
1.
{
    (参数列表) -> 返回值类型 in 函数体代码
}

复制代码
答：
2.将一个很长的闭包表达式作为函数的最后一个实参
  使用尾随闭包可以增强函数的可读性
  尾随闭包是一个被书写在函数调用括号外面(后面)的闭包表达式
 
// fn 就是一个尾随闭包参数
func exec(v1: Int, v2: Int, fn: (Int, Int) -> Int) {
    print(fn(v1, v2))
}

// 调用
exec(v1: 10, v2: 20) {
    $0 + $1
}
复制代码
答：
3.当闭包作为一个实际参数传递给一个函数或者变量的时候，我们就说这个闭包逃逸了，可以在形式参数前写 @escaping 来明确闭包是允许逃逸的。

非逃逸闭包、逃逸闭包，一般都是当做参数传递给函数
非逃逸闭包:闭包调用发生在函数结束前，闭包调用在函数作用域内
逃逸闭包:闭包有可能在函数结束后调用，闭包调用逃离了函数的作用域，需要通过@escaping声明

// 定义一个数组用于存储闭包类型
var completionHandlers: [() -> Void] = []

//  在方法中将闭包当做实际参数,存储到外部变量中
func someFunctionWithEscapingClosure(completionHandler: @escaping () -> Void) {
    completionHandlers.append(completionHandler)
}
复制代码
答：
4.自动闭包是一种自动创建的用来把作为实际参数传递给函数的表达式打包的闭包。它不接受任何实际参数，并且当它被调用时，它会返回内部打包的表达式的值。这个语法的好处在于通过写普通表达式代替显式闭包而使你省略包围函数形式参数的括号。

func getFirstPositive(_ v1: Int, _ v2: @autoclosure () -> Int) -> Int? {
    return v1 > 0 ? v1 : v2()
}
getFirstPositive(10, 20)


为了避免与期望冲突，使用了@autoclosure的地方最好明确注释清楚:这个值会被推迟执行
@autoclosure 会自动将 20 封装成闭包 { 20 }
@autoclosure 只支持 () -> T 格式的参数
@autoclosure 并非只支持最后1个参数
有@autoclosure、无@autoclosure，构成了函数重载
如果你想要自动闭包允许逃逸，就同时使用 @autoclosure 和 @escaping 标志。
复制代码
```





### 22. swift中, 存储属性和计算属性的区别?

```
答：
Swift中跟实例对象相关的属性可以分为2大类

存储属性(Stored Property)

类似于成员变量这个概念
存储在实例对象的内存中
结构体、类可以定义存储属性
枚举不可以定义存储属性
计算属性(Computed Property)

本质就是方法(函数)
不占用实例对象的内存
枚举、结构体、类都可以定义计算属性

struct Circle {
    // 存储属性
    var radius: Double
    // 计算属性
    var diameter: Double {
        set {
            radius = newValue / 2
        }
        get {
            return radius * 2
        }
    }
}
复制代码
```





### 23.什么是延迟存储属性(Lazy Stored Property)?

```
答：
使用lazy可以定义一个延迟存储属性，在第一次用到属性的时候才会进行初始化(类似OC中的懒加载)

lazy属性必须是var，不能是let
 let必须在实例对象的初始化方法完成之前就拥有值
如果多条线程同时第一次访问lazy属性
 无法保证属性只被初始化1次
 
 class PhotoView {
    // 延迟存储属性
    lazy var image: Image = {
        let url = "https://...x.png"        
        let data = Data(url: url)
        return Image(data: data)
    }() 
} 

复制代码
```





### 24.swift 中如何使用单例模式?

```
答：
可以通过类型属性+let+private 来写单例; 代码如下如下:

 public class FileManager {
    public static let shared = {
        // ....
        // ....
        return FileManager()
}()
    private init() { }
}

复制代码
```





### 25.swift 中的下标是什么?

```
答：
使用subscript可以给任意类型(枚举、结构体、类)增加下标功能，有些地方也翻译为:下标脚本
subscript的语法类似于实例方法、计算属性，本质就是方法(函数)

使用如下:
class Point {
    var x = 0.0, y = 0.0
    subscript(index: Int) -> Double {
        set {
            if index == 0 {
                x = newValue
            } else if index == 1 {
                y = newValue }
        }
        get {
            if index == 0 {
                return x
            } else if index == 1 {
                return y
            }
            return 0
        }
    }
}

var p = Point()
// 下标赋值
p[0] = 11.1
p[1] = 22.2
// 下标访问
print(p.x) // 11.1
print(p.y) // 22.2

复制代码
```





### 26.简要说明Swift中的初始化器?

```
答：
类、结构体、枚举都可以定义初始化器
类有2种初始化器: 指定初始化器(designated initializer)、便捷初始化器(convenience initializer)

// 指定初始化器 
init(parameters) {
    statements 
}
// 便捷初始化器
convenience init(parameters) {
    statements 
} 


规则:

每个类至少有一个指定初始化器，指定初始化器是类的主要初始化器
默认初始化器总是类的指定初始化器
类偏向于少量指定初始化器，一个类通常只有一个指定初始化器
初始化器的相互调用规则

指定初始化器必须从它的直系父类调用指定初始化器
便捷初始化器必须从相同的类里调用另一个初始化器
便捷初始化器最终必须调用一个指定初始化器


复制代码
```





### 27.什么可选链?

```
答：
可选链是一个调用和查询可选属性、方法和下标的过程，它可能为 nil 。如果可选项包含值，属性、方法或者下标的调用成功；如果可选项是 nil ，属性、方法或者下标的调用会返回 nil 。多个查询可以链接在一起，如果链中任何一个节点是 nil ，那么整个链就会得体地失败。

多个?可以链接在一起
如果链中任何一个节点是nil，那么整个链就会调用失败
复制代码
```





### 28.什么是运算符重载(Operator Overload)?

```
答：
类、结构体、枚举可以为现有的运算符提供自定义的实现，这个操作叫做:运算符重载

struct Point {
    var x: Int
    var y: Int
    
    // 重载运算符
    static func + (p1: Point, p2: Point) -> Point   {
        return Point(x: p1.x + p2.x, y: p1.y + p2.y)
    }
}

var p1 = Point(x: 10, y: 10)
var p2 = Point(x: 20, y: 20)
var p3 = p1 + p2
复制代码
```




作者：iOShuyang
链接：https://juejin.im/post/5d7f35976fb9a06b20059680
来源：掘金
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。