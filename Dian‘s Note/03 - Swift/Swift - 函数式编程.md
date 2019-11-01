# 函数式编程



Array

- map：  遍历数组的每一个元素， 每次调用闭包 处理 

- filter：返回值是bool  过滤       每次调用闭包处理    返回true 放入接收的数组中，返回false 不处理

- reduce： 遍历数组元素，两个参数第一个是初始值，第二个是闭包表达式，闭包中有两个参数  第一个 返回类型为第一个参数设定的值的类型

  <img src="/Users/yuangonmg/Library/Application Support/typora-user-images/image-20191031113202868.png" alt="image-20191031113202868" style="zoom:67%;" />

  

- 函数式编程： 是一种编程范式  就是编程规范的一种方式
  - 主要思想：把计算过程尽量分解成一系列的可复用函数
  - 主要特征：函数式 “一等公民”  类似于 Int Double等。。
    - 函数与其他数据类型的地位是一样的，可以赋值给其他变量，也可以当做函数参数，函数返回值
- 函数式编程中 几个常见的概念
  - Higher-Order Function、Function Curring
  - Functor、Applicative Functor、Monad

<img src="/Users/yuangonmg/Library/Application Support/typora-user-images/image-20191031162310574.png" alt="image-20191031162310574" style="zoom:67%;" />



### 高阶函数

- 高阶函数 至少满足下列一个条件的函数
  - 接收一个或者多个函数作为输入
  - 返回一个函数
- FP中都是高阶函数

### 柯里化 （Currying）

- 什么是柯里化？
  - 将一个接收多参数的函数转换为一系列只接受单个参数的函数  叫做柯里化

<img src="/Users/yuangonmg/Library/Application Support/typora-user-images/image-20191031162639409.png" alt="image-20191031162639409" style="zoom:67%;" />





可以写一篇文章了



### 函子

- 

## 面向协议编程

	- swift在2015WWDC大会中提出的
	- 在Swift的标准库中  有大量的POP的影子
	- 同时， Swift也是一门面向对象的编程语言 OOP

## 面向对象

- OOP 三大特性： 封装、继承、多态
- 继承使用经典场合
- 公共的东西进行抽取







<img src="/Users/yuangonmg/Library/Application Support/typora-user-images/image-20191031184904144.png" alt="image-20191031184904144" style="zoom:67%;" />

<img src="/Users/yuangonmg/Library/Application Support/typora-user-images/image-20191031184929666.png" alt="image-20191031184929666" style="zoom:67%;" />



​	<img src="/Users/yuangonmg/Library/Application Support/typora-user-images/image-20191031185709205.png" alt="image-20191031185709205" style="zoom:67%;" />

<img src="/Users/yuangonmg/Library/Application Support/typora-user-images/image-20191031185712102.png" alt="image-20191031185712102" style="zoom:67%;" />

![image-20191031190110696](/Users/yuangonmg/Library/Application Support/typora-user-images/image-20191031190110696.png)

为了 可以使用  mutating

实现 NSString String MSMUtableString  三种类型的 功能扩展 	

![image-20191031190524397](/Users/yuangonmg/Library/Application Support/typora-user-images/image-20191031190524397.png)