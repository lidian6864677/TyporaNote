# Swift - 闭包



## 闭包表达式

在swift中，可以通过func定义一个函数，也
数

$0,  是代表第一个参数

- 基础定义

  ```swift
  
  ```

- 啊啊

  ```swift
  asd
  ```

  

## 闭包

- 一个函数和它所捕获的变量、常量环境组合而，称为闭包

  - 一般指定义在函数内部的函数

  - 一般它捕获的是外层函数的局部变量、常量


​	

## 尾随闭包

如果将一个很长的闭包表达式作为函数的最后一个参数，使用尾随闭包可以增强函数的可读性

- 尾随闭包是一个被书写在函数调用的外面的闭包表达式

  ```swift
  
  ```








## 自动闭包

- @autuclosure 会自动将20封装风闭包表达式{20}

- @autuclosure 只支持 ( ) -> T 格式的参数

- @autuclosure 并非只支持最后一个参数  在中间也可以

- 空合并运算符 ?? 就是使用了@autuclosure 技术

  ```swift
  var num1: Int? = 10
  var num2: Int? = nil
  num1 ?? num2
  public func ?? <T> (optional: T?, defautValue: @autoclosure () Throws -> T?) -> T?
  ```

- 有@autoclosure、无@autoclosure，构成了函数重载

  ```swift
  func getFirst(_ v1: Int, _ v2: () -> Int) -> Int{
  }
  func getFirst(_ v1: Int, _ v2: @autoclosure () -> Int) -> Int{
  }
  两个函数可以同时存在  构成了  函数重载	 
  
  ```

  

### 关键字 @escaping 逃逸闭包

- 非逃逸闭包、逃逸闭包，一般都是当做参数传递给函数

- 非逃逸闭包：闭包调用发生在函数结束前，闭包调用在函数作用域内

- 逃逸闭包：闭包有可能在函数结束之后调用，闭包调用逃离了函数作用域，需要通过@escaping来声名

  ```swift
  // 非逃逸闭包
  func test(_ fn: () -> ()){
  }
  test{
    print(1)
  }
  
  // 逃逸闭包
  typealias Fn = () -> ()
  var gFn:Fn
  func test2(_ fn: @escaping Fn){
    gFn = fn  
  }
  // 2. 
  import Dispatch
  func test3(_fn: @escaping Fn){
    // 异步调用  出去调用 所以加上逃逸闭包的内容
    DispatchQueue.gloabl().async{
    	fn()  
    }
  }
  
  
  ```

  























------

<p align="right" color="orange">	小李小李一路有你</p><p align="right" color="orange">	Dian。</p>	
​		 																																											