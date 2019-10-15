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



























------

<p align="right" color="orange">	小李小李一路有你</p><p align="right" color="orange">	Dian。</p>	