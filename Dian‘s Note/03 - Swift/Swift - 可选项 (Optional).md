# Swift - 可选项（Optional）





```swift
let a: Int? = nil
let b: Int? = 2
if let c = a ?? b	{
  print(c)
}
// 等价于：  if a!= nil || b != nil
```



```swift  print(c)
if let c = a, let d = b{
  print(c)
  print(d)
}
// 等价于:    if a!= nil && b != nil
```

可选值 的本质是枚举 和泛型的结合 

```swift
public enum Optional<Wrapped> : ExpressibleByNilLiteral {
  
    /// In code, the absence of a value is typically written using the `nil`
    case none
  
    /// The presence of a value, stored as `Wrapped`.
    case some(Wrapped)
  
    /// Creates an instance that stores the given value.
    public init(_ some: Wrapped)

    @inlinable public func map<U>(_ transform: (Wrapped) throws -> U) rethrows -> U?

    @inlinable public func flatMap<U>(_ transform: (Wrapped) throws -> U?) rethrows -> U?

    public init(nilLiteral: ())

    @inlinable public var unsafelyUnwrapped: Wrapped { get }

    public static func ~= (lhs: _OptionalNilComparisonType, rhs: Wrapped?) -> Bool
  
    public static func == (lhs: Wrapped?, rhs: _OptionalNilComparisonType) -> Bool

    public static func != (lhs: Wrapped?, rhs: _OptionalNilComparisonType) -> Bool
  
    public static func == (lhs: _OptionalNilComparisonType, rhs: Wrapped?) -> Bool
    
    public static func != (lhs: _OptionalNilComparisonType, rhs: Wrapped?) -> Bool
}
```



























------

<p align="right" color="orange">	小李小李一路有你</p><p align="right" color="orange">	Dian。</p>	