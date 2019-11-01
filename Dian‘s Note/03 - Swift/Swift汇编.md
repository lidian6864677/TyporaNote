



## 小技巧

1. **查看汇编 :  Debung  -->  Debug Workflow -->  Always Show Disassembly**

2. **查看内存：Debug -->  Debug Workflow  -->  View Memory**

3. **查看对象是否在对空间 在汇编中查看 对象是否调用了  alloc  或者malloc 函数。 调用了即在对空间**

4. **ni: 单步运行，把子函数当做整体一步运行（汇编级别）**

5. **si: 单步运行， 遇到子函数会进图子函数（汇编级别）**

6. **查看对象占用的内存空间**

   ```swift
   // eg1
   enum Password{
     case number( Int, Int, Int, Int)
     case other
   }	
   var pwd = Password.number(1, 2, 4, 6)
   pwd = .other
   MemoryLayout<Password>.size      ///  实际使用的   33
   MemoryLayout<Password>.stride    ///  系统分配的  40
   MemoryLayout<Password>.alignment /// 内存对其基数  8
   ```

   

7. **汇编语言中   如果变量变量的地址值是 -0x10(%rbp) , 一般来说都是局部变量。 栈空间 **

8. **汇编语言中  如果变量是0x7124(%rip)，一般来说是全局变量**

9. **堆空间 地址   不是0x10008934    如果是0x10001923一般来说是全局变量 代码段**  

   

10. **123**



<img src="/Users/yuangonmg/Library/Application Support/typora-user-images/image-20191021154326259.png" alt="image-20191021154326259" style="zoom:33%;" />

lldb常用命令：

- thread step-over 、 next 、 n

  单步运行，把子函数当做整体一步运行（源码级别）

- thread strp-in、 step、 s

  单步运行， 遇到子函数会进入子函数（源码级别）

- Thread step-inst-over、 nexti、 ni

   单步运行，把子函数当做整体一步运行（汇编级别）

- Thread step-inst、 stepi、si

  单步运行， 遇到子函数会进图子函数（汇编级别）

- Thread strp-out、 finish

  直接执行完当前函数所有代码，返回到上一个函数（遇到断电会卡住）







|   第一列    |                AT&T                |             intel             |               说明               |
| :---------: | :--------------------------------: | :---------------------------: | :------------------------------: |
| 寄存器名称  |                %rax                |              rax              |                                  |
| 操作数顺序  |          movq %rax, %rdx           |         mov rax, rdx          |          将rax赋值给rdx          |
| 常数/立即数 | movq $3, %rax  \|  movq 0x10, %rax | mov rax, 3  \|  mov rax, 0x10 | 将3赋值给rax  \| 将0x10赋值给rax |
|             |                                    |                               |                                  |
|             |                                    |                               |                                  |
|             |                                    |                               |                                  |
|             |                                    |                               |                                  |
|             |                                    |                               |                                  |
|             |                                    |                               |                                  |

![image-20191009143710793](/Users/yuangonmg/Library/Application Support/typora-user-images/image-20191009143710793.png)