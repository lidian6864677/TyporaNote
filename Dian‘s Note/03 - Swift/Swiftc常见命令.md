## swiftc 常见命令



- swift语言编译成一个.out文件  可执行文件

  ```swift
  swiftc -o main.out main.swift
  ```

- 执行生成的.out文件

  ```
  ./main.out
  ```

- 生成抽象语法树( Swift Abstract Syntax Tree (AST) )

  ```swift
   swiftc main.swift -dump-ast
  ```

- 生成Swift中间语言( Swift Intermediate Language (SIL) )

  ```swift
   swiftc main.swift -emit-sil
  ```

- 生成LLVM中间便是层的命令( LLVM Intermediate Representation (LLVM IR) )

  ```swift
   swiftc main.swift -emit-ir
  ```

- 生成汇编语言 ( Assembly Language )

  ```swift
   swiftc main.swift -emit-assembly
  ```

  





















------

<p align="right" color="orange">	小李小李一路有你</p><p align="right" color="orange">	Dian。</p>	



