

- **编程范式**
  - Swift 可以面向协议、函数式、面向对象编程。
  - OC 只能面向对象编程、不过引入 RAC类库也可以实现函数式编程。
- **类型安全**
  - Swift 是类型安全的语言。鼓励程序员在代码中清晰明确值的类型。如果代码中使用String定义了一个字符串，那么你不能传递一个Int类型给他，会在编译的时候就做类型检查，标记错误
  - OC 声名一个NSString类型的变量，仍然可以传递一个NSNumber给它，会Warning但是可以使用
- **值类型增强**
  - Swift  典型的struct、enum、tuple都是值类型。平时使用的Int、Double、Float、String、Array、Dictionary、Set其实都是结构体实现的，也是值类型
  - OC 中NSNumber、NSString以及集合类对象都是指针类型的
- **枚举增强**
  - Swift 的枚举可以使用整形、浮点型、字符串等。还能拥有属性和方法，甚至支持泛型、协议、扩展等
  - OC中的枚举就很鸡肋，仅用于整形。
- **泛型**
  - Swift 中支持泛型，也支持泛型的类型约束等特性
  - OC 在Swift2.0的时候也为OC增加了泛型， 但是仅仅停留在编译器会警告的阶段。
- **协议和扩展**
  - Swift 对协议的支持更加丰富，配合扩展(extension)、泛型、关联类型等可以实现面向协议编程、从而打打提高代码的灵活性。同时，Swift中的protocol还可以用于值类型，如结构体和枚举。
  - OC 的协议缺乏强约束，提供的optional特性会成为很多问题的来源，但是舍弃optional就会让代码的实现代价变大。
- **函数和闭包**
  - Swift 可以直接定义函数类型变量，可以作为其他函数参数传递，可以作为函数返回值
  - OC 需要用selector封装或者block才能实现Swift中的效果

















------

<p align="right" color="orange">	小李小李一路有你</p><p align="right" color="orange">	Dian。</p>	

