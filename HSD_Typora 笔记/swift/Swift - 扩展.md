# Swift - 扩展



- 扩展输出结构

  ```swift
  import UIKit
  class Person{
      var name: String
      var age: Int
      init(name: String, age: Int) {
          self.name = name
          self.age = age
      }
  }
  /// 扩展自 CustomStringConvertible
  extension Person: CustomStringConvertible{ 
      var description: String {
          get{
              return "\(name) - \(age)"
          }
      }
  }
  let li = Person(name: "Dian", age: 18)
  ```

  