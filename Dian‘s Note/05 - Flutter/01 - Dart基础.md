- const && final区别

  ```dart
  	final data = new DateTime.now();
  	print(data);
    const data1 = new DateTime.now();
  ```

  

  - const：
  - final: 可以开始不赋值，只能赋值一次，
    - final不仅有const的在编译时期的常量
    - 而且也是运行时的常量，切为懒加载初始化，在运行时第一次使用的时候才会初始化

- ```
  var 
  string
  int
  double
  
  list
  ["",""]    
  取:list[0]
  
  map
  {"":"","":""}
  取:map[""]
  
  
  // String -> int
  var one = int.parse('1');
  
  // String -> double
  var onePointOne = double.parse('1.1');
  
  // int -> String
  String oneAsString = 1.toString();
  
  // double -> String
  String piAsString = 3.14159.toStringAsFixed(2);
  
  
  ```

- 自增运算赋值    b=a++    先赋值在++     b=++a 先++在赋值

- List && Map && Set

  ```dart
  	List
      .add  // 添加一个
      .addAll // 添加多个
      .indexof // 获取索引 查找到返回索引 查找不到返回-1
      .remove // obj
      .removeAt // 索引
      .fillRange(start,end) //  修改位置    .fillRange(1,2)  左开右闭  修改List中index=1 的值
      											//   .fillRange(1,3)  左开右闭  修改List中index=1&&index=2 的值
      .join(",") // 讲列表转换为字符串  以','分割
      				   //str.split(",")   字符串以','分割转换为数组
      
    Set  ->  "主要有一个去重的操作"
       // 与List的用法大致一致，  小技巧：List去重转为List之后再转回List就可以实现去重了
    Map
      .keys // 获取所有的key
      .values
      .isEmpty
      
      
  ```

  <img src="https://tva1.sinaimg.cn/large/007S8ZIlly1getckmgrv0j30do052761.jpg" alt="image-20200515191332194" style="zoom:50%;" /> 

<img src="/Users/dian 1/Library/Application Support/typora-user-images/image-20200515191442444.png" alt="image-20200515191442444" style="zoom:50%;" /> 

<img src="/Users/dian 1/Library/Application Support/typora-user-images/image-20200515191502426.png" alt="image-20200515191502426" style="zoom:50%;" /> 

<img src="/Users/dian 1/Library/Application Support/typora-user-images/image-20200515191533808.png" alt="image-20200515191533808" style="zoom:50%;" /> 

- 函数

  ```dart
  返回值 函数名 (参数类型 参数名){
  }
  返回值 函数名 (参数类型 参数名, [参数类型 参数名]){ // 可选参数 "[]"包住
  }
  返回值 函数名 (参数类型 参数名, [参数类型 参数名=xx，参数类型 参数名]){ // 可选参数 默认值
  }
  // 命名参数"{}"包住   调用的时需要(参数名：参数)
  返回值 函数名 (参数类型 参数名, {参数类型 参数名=xx，参数类型 参数名}){ 
  }
  
  
  
  
  ```

- 抽象类：

  关键字：'abstract' 可被继承， 不可以被实例化，  

  内部方法只要不写实现就是抽象类的抽象方法， 子类必须实现当前抽象方法。

- 多态:

  父类定义方法不去实现， 让每一个子类去实现  每一个子类有不同的表现   or （父类调用子类的方法）

- 接口

  接口就是约定， 