# Swift - 泛型、不透明类型

- 泛型可以将类型参数化，提高代码复用率，减少代码量

  ```swift
  var n1 = 10
  var n2 = 20
  func swapValues(_ a:inout Int, _ b: inout Int ){
    (a, b) = (b, a)
  }
  // 泛型
  func swapValues<T>(_ a:inout T, _ b: inout T ){
    (a, b) = (b, a)
  }
  swapValues(&n1, &n2)
  ```

### 泛型函数赋值给变量

![image-20191021140157311](/Users/yuangonmg/Library/Application Support/typora-user-images/image-20191021140157311.png)



### 关联类型

- 关联类型的作用： 给协议中用到的类型定义一个占位名称
  - 协议想实现泛型的话使用关联类型（协议中的泛型）
- 协议中可以拥有多个关联对象

![image-20191021141901561](/Users/yuangonmg/Library/Application Support/typora-user-images/image-20191021141901561.png)



### 类型约束

![image-20191021142659923](/Users/yuangonmg/Library/Application Support/typora-user-images/image-20191021142659923.png)

###  

协议类型的注意点

![image-20191021145640301](/Users/yuangonmg/Library/Application Support/typora-user-images/image-20191021145640301.png)

解决方案：

1. ![image-20191021145716608](/Users/yuangonmg/Library/Application Support/typora-user-images/image-20191021145716608.png)

2. **不透明类型**

   一个函数不想暴露内部实现细节， 使用协议开放部分功能， 但是内部有（关联类型），使用不透明类型，some关键字   但是some关键字 只能返回一种类型  多个类型就不可以了

   1. ​	使用some关键字声名一个不透明类型

      ​	![image-20191021145930347](/Users/yuangonmg/Library/Application Support/typora-user-images/image-20191021145930347.png)

   2. some 限制只能返回一种类型

   3. some除了用在返回类型上，，还可以用在属性类型上

      ![image-20191021152112930](/Users/yuangonmg/Library/Application Support/typora-user-images/image-20191021152112930.png)

3. 







------

<p align="right" color="orange">	小李小李一路有你</p><p align="right" color="orange">	Dian。</p>	
