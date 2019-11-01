# Swift：String、Array底层

## 前言：

​	问题：

```swift
 var str1 = "0123456789"
 var str2 = "0123456789ABCDEF"
/** 
	1. 上面两个String变量分别占多少内存？
	2. 存储在什么位置?
	3. 底层存储有什么不同
*/
str1.append("ABCDE")
str1.append("F")
str1.append("G")
/**
	1. 如果对字符串进行了拼接操作，String变量的存储会发生什么变化？
*/


```

1. 通过MenoryLayout.stride(ofValue: str1) 打印获得

   ```swift
   // 打印str1占用内存
   pring(MemoryLayout.stride(ofValue: str1)) // 16
   
   // 内存存储样式
   //  0x3736353433323130 0xea00000000003938   内存中存储的是 字符串的  ASCII码的值
   var str1 = "0123456789"
   /**
   	0x3736353433323130 0xea00000000003938   内存中存储的是 字符串的  ASCII码的值
   	0xea00000000003938 中    
   	0xea：
   		a: 长度（最长的长度为f）
   		e：类型
   */
   //  0x3736353433323130 0xef4544434241403938 
   var str1 = "0123456789ABCDE"
   /**
   	0x3736353433323130 0xef4544434241403938 
   	0xef：
   		f: 长度（最长的长度为f）
   		e：类型 
   */
   "所以一个字符串有16个字节，  其中一个字符串存储类型和长度， 其余15个字节存储内容"
   
    //问题：：  如果存储的内容超过15个字符怎么办？
   
   
    
   
   ```

   

## 总结

```swift
// 字符串长度 <= 0xF（15），字符串内容直接存放在str1变量的内存中
var str1 = "0123456789"

// 字符串长度 > 0xF 字符串内存存放在 __TEXT_cstring中（常量区）
// 字符串的地址信息存放在str2的 后8个字节中
var str2 = "0123456789ABCDEF"

// 由于字符串长度 <= 0xF 所以字符串内容依旧直接存放在str1变量的内存中
str1.append("ABCDE")

// 开辟堆空间   因为这个时候在append 空间就不够了	
str1.append("F")

// 开辟堆空间
str2.append("G")


```



## dyld_stub_binder

- 符号的延迟绑定是通过  dyld_stub_bunder 完成的
  - 汇编中string.init 跳转一个假的地址  然后在通过静态库 执行真正的地址 在将真正的地址 放到之前的假地址
  - 此绑定操作只执行一次， 第二次直接跳转到静态库中
  - 



## 关于Array的思考

1. Array 占用多少内存 

2. 数据放在了 哪里 ？

   ```swift
   var arr = [1,2,3,4]
   
   MemoryLayout.stride(ofValue:arr)   // 8
   
   //arr 存放的是地址值   对空间的地址值   地址值指向
   
   ```

   







 addq  %rdx, %rdi



0xd000000000000013 0x8000000100000f70

0x7fffffffffffffe0

0x100000F90



"123456" 0x0000000100000f8e



"ABC"0x0000000100000f99





------

<p align="right" color="orange">	小李小李一路有你</p><p align="right" color="orange">	Dian。</p>	
