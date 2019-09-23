# swift学习笔记



1. ### 常量：let   变量： var 

   ```swift
   let implicitInteger = 70
   let implicitDouble = 70.0
   let explicitDouble: Double = 70
   ```

2. ### swift中没有隐式转换  需要的时候要显式转换

   ```swift
   let label = "The width is"
   let width = 94
   let widthLabel = label + String(width)
   ```

3. ### 反斜杠( \ )用法   值转换

   ```swift
   let apples = 3
   let oranges = 5
   let appleSummary = "I have \(apples) apples."
   let fruitSummary = "I have \(apples + oranges) pieces of fruit."
   ```

4. 三引号 """用法  可多行字符串

   ```swift
   let quotation = """
   I said "I have \(apples) apples."
   And then I said "I have \(apples + oranges) pieces of fruit."
   """
   ```

   

5. ```swift
    let x where x.hasSuffix("pepper"):    let where 条件判断关键字where
   ```

6. ### (0..<4) 范围不包含上届，   （0...4）包含上下界

7. ### 函数

   - **函数 多返回值**

     ```swift
     func calculateStatistics(scores: [Int]) -> (min: Int, max: Int, sum: Int) {
         var min = scores[0]
         var max = scores[0]
         var sum = 0
         for score in scores {
             if score > max {
                 max = score
             } else if score < min {
                 min = score
             }
             sum += score
         }
         return (min, max, sum)
     }
     let statistics = calculateStatistics(scores:[5, 3, 100, 3, 9])
     print(statistics.sum)
     print(statistics.2)	
     ```

   - **函数嵌套**

     ```swift
     func returnFifteen() -> Int {
         var y = 10
         func add() {
             y += 5
         }
         add()
         return y
     }
     returnFifteen()
     ```

     

   - **返回值为函数的函数**

     ```swift
     // 函数是第一等类型，这意味着函数可以作为另一个函数的返回值。
     
     func makeIncrementer() -> ((Int) -> Int) {
         func addOne(number: Int) -> Int {
             return 1 + number
         }
         return addOne
     }
     var increment = makeIncrementer()
     increment(7)
     ```

   - **函数当做参数**

     ```swift
     func hasAnyMatches(list: [Int], condition: (Int) -> Bool) -> Bool {
         for item in list {
             if condition(item) {
                 return true
             }
         }
         return false
     }
     func lessThanTen(number: Int) -> Bool {
         return number < 10
     }
     var numbers = [20, 19, 7, 12]
     hasAnyMatches(list: numbers, condition: lessThanTen)	
     ```

     

   - **多返回值函数**

     ```swift
     func minMax(array: [Int]) -> (min: Int, max: Int)? {
         if array.isEmpty { return nil }
         var currentMin = array[0]
         var currentMax = array[0]
         for value in array[1..<array.count] {
             if value < currentMin {
                 currentMin = value
             } else if value > currentMax {
                 currentMax = value
             }
         }
         return (currentMin, currentMax)
     }	
     
     if let bounds = minMax(array: [8, -6, 2, 109, 3, 71]) {
         print("min is \(bounds.min) and max is \(bounds.max)")
     }
     // 打印“min is -6 and max is 109”
     ```

   - **函数传递输入输出参数**

     ```swift
     func swapTwoInts(_ a: inout Int, _ b: inout Int) {
         let temporaryA = a
         a = b
         b = temporaryA
     }
     
     
     var someInt = 3
     var anotherInt = 107
     swapTwoInts(&someInt, &anotherInt)
     print("someInt is now \(someInt), and anotherInt is now \(anotherInt)")
     // 打印“someInt is now 107, and anotherInt is now 3”
     ```

     

   - **函数类型作为参数类型**

     ```swift
     func addTwoInts(_ a: Int, _ b: Int) -> Int {
         return a + b
     }
     
     func printMathResult(_ mathFunction: (Int, Int) -> Int, _ a: Int, _ b: Int) {
         print("Result: \(mathFunction(a, b))")
     }
     printMathResult(addTwoInts, 3, 5)
     // 打印“Result: 8”
     ```

     

   -  **函数嵌套**

     ```swift
     func chooseStepFunction(backward: Bool) -> (Int) -> Int {
         func stepForward(input: Int) -> Int { return input + 1 }
         func stepBackward(input: Int) -> Int { return input - 1 }
         return backward ? stepBackward : stepForward
     }
     var currentValue = -4
     let moveNearerToZero = chooseStepFunction(backward: currentValue > 0)
     // moveNearerToZero now refers to the nested stepForward() function
     while currentValue != 0 {
         print("\(currentValue)... ")
         currentValue = moveNearerToZero(currentValue)
     }
     print("zero!")
     // -4...
     // -3...
     // -2...
     // -1...
     // zero!
     ```

     

8. 函数相同

   相同（`===`）

   不相同（`!==`）

   “相同”（用三个等号表示，`===`）与“等于”（用两个等号表示，`==`）的不同。“相同”表示两个类类型（class type）的常量或者变量引用同一个类实例。“等于”表示两个实例的值“相等”或“等价”，判定时要遵照设计者定义的评判标准。

9. swift 修饰符：

   |       修饰符        |                             功能                             |
   | :-----------------: | :----------------------------------------------------------: |
   |      mutating       | 写在func关键字之前，表示可以在该方法中修改他所属的实例及实例的任意属性的值 |
   |      required       |   使用required修饰可以确保所有子类也必须提供此构造器的实现   |
   |      override       |                        重写父类的func                        |
   | required & override |          子类重写了父类的必要构造器需要用这两个修饰          |
   |        final        |        使用final修饰符方法属性或下标来防止它们被重写         |
   |      optional       |   在协议中使用optional关键字为前缀在协议中使用 为可选属性    |
   |   associatedtype    |                                                              |
   |                     |                                                              |
   |                     |                                                              |
   |                     |                                                              |
   |                     |                                                              |
   |                     |                                                              |
   |                     |                                                              |
   |                     |                                                              |

   

10. 

11. 

12. 



















------

<p align="right" color="orange">	小李小李一路有你</p><p align="right" color="orange">	Dian。</p>	

