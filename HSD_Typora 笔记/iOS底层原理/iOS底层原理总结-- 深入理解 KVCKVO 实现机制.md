[窥探iOS底层实现--OC对象的本质(一)](http://www.hsdian.cn/?p=200)

[窥探iOS底层实现--OC对象的本质(二)](http://www.hsdian.cn/?p=138)

[窥探iOS底层实现--OC对象的分类：instance、class、meta-calss对象的isa和superclass](http://www.hsdian.cn/?p=146)

[窥探iOS底层实现-- KVO/KVC的本质](https://juejin.im/post/5c2189dee51d454517589c8b)

...

## 一. KVO的实现原理
**面试题：**

    KVO相关：
    1. iOS用什么方式来实现对一个对象的KVO？（KVO的本质是什么？）
    2. 如何手动出发KVO？
    3. 直接修改成员变量会触发KVO么？
    
    KVC相关： 
    1. 通过KVC修改属性会触发KVO么？
    2. KVC的赋值和取值过程是怎样的？原理是什么？


### 1. 什么是KVO？
    KVO的全称是Key-Value Observing，俗称"键值监听"，可以用于监听摸个对象属性值得改变。

 


![](https://user-gold-cdn.xitu.io/2018/12/26/167e910c7b4d94ae?w=1524&h=376&f=png&s=36366)
要监听Person中的age属性，我们就创建一个observer用来监听age的变化，当我们age一旦发生改变，就会通知observer。

### 2. KVO简单的实现
我们先简单的回顾一下 KVO的代码实现。
```objc
///> DLPerson.h 文件

#import <Foundation/Foundation.h>

NS_ASSUME_NONNULL_BEGIN

@interface DLPerson : NSObject

@property (nonatomic, assign) int age;

@end

NS_ASSUME_NONNULL_END
```
```objc
///> ViewController.m 文件

#import "ViewController.h"
#import "DLPerson.h"
@interface ViewController ()
@property (nonatomic, strong) DLPerson *person1;
@property (nonatomic, strong) DLPerson *person2;
@end

@implementation ViewController

- (void)viewDidLoad {
    [super viewDidLoad];
    self.person1 = [[DLPerson alloc]init];
    self.person1.age = 1;
    
    self.person2 = [[DLPerson alloc]init];
    self.person2.age = 2;
    
    ///> person1添加kvo监听
    NSKeyValueObservingOptions options = NSKeyValueObservingOptionNew | NSKeyValueObservingOptionOld;
    [self.person1 addObserver:self forKeyPath:@"age" options:options context:nil];
}

- (void)touchesBegan:(NSSet<UITouch *> *)touches withEvent:(UIEvent *)event{
    self.person1.age = 20;
    self.person2.age = 30;
}

- (void)observeValueForKeyPath:(NSString *)keyPath ofObject:(id)object change:(NSDictionary<NSKeyValueChangeKey,id> *)change context:(void *)context{
    NSLog(@"监听到了%@的%@属性发生了改变%@",object,keyPath,change);
}

- (void)dealloc{
    ///> 使用结束后记得移除
    [self.person1 removeObserver:self forKeyPath:@"age"];
}

@end

@end
```

    输出结果：
    监听到了<DLPerson: 0x6000033d4e40>的age属性发生了改变- {
    kind = 1;
    new = 20;
    old = 1;
    }
    
    ///>  因为我们只监听了person1  所以只会输出person1的改变。

### 3. KVO存在的疑问
```objc
- (void)touchesBegan:(NSSet<UITouch *> *)touches withEvent:(UIEvent *)event{
    // self.person1.age = 20;
    [self.person1 setAge:20]; ///> 等同于self.person1.age = 20;
    
    self.person2.age = 30;
    [self.person2 setAge:20];///> 等同于self.person2.age = 20;
}
```
因为当我们在DLPerson中使用@property声名一个属性的时候会自动生成声名属性的set和get方法。如下代码：
``` objc
///> DLPerson.m文件

#import "DLPerson.h"

@implementation DLPerson

- (void)setAge:(int)age{
    _age = age;
}

- (int)age{
    return _age;
}

@end
```
既然person1和person2的本质都是在调用set方法，就一定都会走在DLPerson类中的setAge这个方法。

那么问题来了，同样走的是DLPerson类中的setAge方法，为什么person1就会走到
```objc
- (void)observeValueForKeyPath:(nullable NSString *)keyPath ofObject:(nullable id)object change:(nullable NSDictionary<NSKeyValueChangeKey, id> *)change context:(nullable void *)context;
```
方法中而person2就不会呢？

### 4. KVO的本质分析


如果不是很了解OC对象的isa指针相关知识的同学，建议先请前往[窥探iOS底层实现--OC对象的分类：instance、class、meta-calss对象的isa和superclass - 掘金](https://juejin.im/post/5c1b5db55188250850605c44) 文章了解一下先。

接下来我们探究一下两个对象的本质，首先我们person1和person2的isa打印出来查看一下他们的实例对象isa指向的类对象是什么？

![](https://user-gold-cdn.xitu.io/2018/12/26/167e980cbe1dcfda?w=1650&h=370&f=png&s=165840)
我们会发现person1的isa指针打印出的是: **NSKVONotifying_DLPerson**

而person2的isa指针打印出来的是: **DLPerson**

person1和person2都是实例对象 所以person1和person2的isa指针指向的都是类对象，

 **`所以说，如果对象没有添加KVO监听那么它的isa指向的就是自己原来的类对象，如下图`**

        person2.isa == DLPerson

![未使用KVO监听的对象](https://user-gold-cdn.xitu.io/2018/12/26/167e98a78ba2002b?w=1774&h=714&f=png&s=123309)

 

**`当一个对象添加了KVO的监听时，当前对象的isa指针指向的就不是你原来的类，指向的是另外一个类对象，如下图`**

        person1.isa == NSKVONotifying_DLPerson


![使用了KVO监听的对象](https://user-gold-cdn.xitu.io/2018/12/26/167e98d0c927eba4?w=1812&h=868&f=png&s=230393)
- NSKVONotifying_DLPerson类是 Runtime动态创建的一个类，在程序运行的过程中产生的一个新的类。
- NSKVONotifying_DLPerson类是DLPerson的一个子类。
- NSKVONotifying_DLPerson类存在自己的 setAge:、class、dealloc、isKVOA...方法。

    **`当我们DLperson的实例对象调用setAge方法时，`**
    
    **`实例对象的isa指针找到类对象，然后在类类对象中寻找相应的对象方法，如果有则调用，`**

    **`如果没有则去superclass指向的父类对象中寻找相应的对象方法，如果有则调用，`**

    **`如果未找到相应的对象方法则会报：unrecognized selector sent to instance 错误`**
- 由于上图可分析出我们的person1的isa指针指向的类对象是NSKVONotifying_DLPerson，并且NSKVONotifying_DLPerson中还带有setAge: 方法，所以：
    ```objc
    ///> person1的setAge方法走的是NSKVONotifying_DLPerson类中的setAge方法，
    ///> 并且在KVO监听中实现了[superclass setAge:age];，
    [self.person1 setAge:20]; 
    
    ///> person2的setAge方法走的是DLPerson中的setAge:方法，
    [self.person2 setAge:30]; 
    ```
    上次代码所示，两个实例对象person1和person2都走了DLPerson的setAge:方法，只是添加了KVO的person1在自己的setAge方法中添加了 **`其他操作`**。
- 其他操作猜想  **伪代码！**：

    ```objc
    ///> NSKVONotifying_DLPerson.m 文件

    #import "NSKVONotifying_DLPerson.h"

    @implementation NSKVONotifying_DLPerson

    - (void)setAge:(int)age{
      _NSSetIntValueAndNotify();  ///> 文章末尾 知识点补充小结有此方法来源
    }

    void _NSSetIntValueAndNotify(){
      [self willChangeValueForKey:@"age"];
      [super setAge:age];
      [self didChangeValueForKey:@"age"];
    }

    - (void)didChangeValueForKey:(NSString *)key{
      ///> 通知监听器 key发生了改变
      [observe observeValueForKeyPath:key ofObject:self change:nil context:nil];
    }

    @end
    ```
    **`_NSSetIntValueAndNotify();  ///> 文章末尾 知识点补充小结有此方法来源`**
    
### 5. KVO的调用顺序

由于我们的NSKVONotifying_DLPerson类不能参与编译所以可以在 DLPerson.m中重写它父类的方法代码如下：
```objc
///> ViewController.m文件

#import "ViewController.h"
#import "DLPerson.h"
@interface ViewController ()
@property (nonatomic, strong) DLPerson *person1;
@end

@implementation ViewController

- (void)viewDidLoad {
    [super viewDidLoad];
    self.person1 = [[DLPerson alloc]init];
    self.person1.age = 10;
    
    ///> person1添加kvo监听
    NSKeyValueObservingOptions options = NSKeyValueObservingOptionNew | NSKeyValueObservingOptionOld;
    [self.person1 addObserver:self forKeyPath:@"age" options:options context:nil];
}

- (void)touchesBegan:(NSSet<UITouch *> *)touches withEvent:(UIEvent *)event{
    [self.person1 setAge:20];
}

- (void)observeValueForKeyPath:(NSString *)keyPath ofObject:(id)object change:(NSDictionary<NSKeyValueChangeKey,id> *)change context:(void *)context{
    NSLog(@"监听到了%@的%@属性发生了改变%@",object,keyPath,change);
}

- (void)dealloc{
    ///> 使用结束后记得移除
    [self.person1 removeObserver:self forKeyPath:@"age"];
}

@end
```

```objc
///> DLPerson.m文件

#import "DLPerson.h"

@implementation DLPerson

- (void)setAge:(int)age{
    _age = age;
}

- (void)willChangeValueForKey:(NSString *)key{
    [super willChangeValueForKey:key];
    NSLog(@"willChangeValueForKey");
}

- (void)didChangeValueForKey:(NSString *)key{
    NSLog(@"didChangeValueForKey - begin");
    [super didChangeValueForKey:key];
    NSLog(@"didChangeValueForKey - end");
}
@end
```

输出结果：
```
willChangeValueForKey
didChangeValueForKey - begin
监听到了<DLPerson: 0x60000041afe0>的age属性发生了改变{
    kind = 1;
    new = 20;
    old = 10;
}
didChangeValueForKey - end
```
- 调用willChangeValueForKey:
- 调用原来的setter实现
- 调用didChangeValueForKey:


**`总结：didChangeValueForKey:内部会调用observer的observeValueForKeyPath:ofObject:change:context:方法`**


## 二. KVC的实现原理
### 1. 什么是KVC？
    KVC的全称key - value - coding，俗称"键值编码",可以通过key来访问某个属性


​    
常见的API有：
```objc
- (void)setValue:(id)value forKey:(NSString *)key;
- (void)setValue:(id)value forKeyPath:(NSString *)keyPath;

- (id)valueForKey:(NSString *)key; 
- (id)valueForKeyPath:(NSString *)keyPath;

```
简单的代码实现:
DLPerson 和 DLCat
```objc
///> DLPersin.h 文件

#import <Foundation/Foundation.h>

NS_ASSUME_NONNULL_BEGIN

/**
 DLCat
 */
@interface DLCat : NSObject
@property (nonatomic, assign) int weight;
@end


/**
 DLPerson
 */
@interface DLPerson : NSObject
@property (nonatomic, assign) int age;
@property (nonatomic, strong) DLCat *cat;
@end

NS_ASSUME_NONNULL_END
```
#### 1.1 - (void)setValue:(id)value forKey:(NSString *)key;

```objc
///> ViewController.m 文件

- (void)viewDidLoad {
    [super viewDidLoad];
    DLPerson *person = [[DLPerson alloc]init];
    [person setValue:@20 forKey:@"age"];
    NSLog(@"%d",person.age);
}
```

#### 1.1 - (void)setValue:(id)value forKeyPath:(NSString *)keyPath;
```objc
///> ViewController.m 文件

- (void)viewDidLoad {
    [super viewDidLoad];
    DLPerson *person = [[DLPerson alloc]init];
    person.cat = [[DLCat alloc]init];
    [person setValue:@20 forKeyPath:@"cat.weight"];
    NSLog(@"%d",person.age);
    NSLog(@"%d",person.cat.weight);
}
```
**`setValue:(id)value forKeyPath:(NSString *)keyPath`** 和 **`setValue:(id)value forKey:(NSString *)key`** 的区别：
- keyPath 相当于根据路径去寻找属性，一层一层往下找，
- key  是直接哪去属性的名字设置，如果按路径找会报错



#### 1.3 - (id)valueForKey:(NSString *)key; 
```objc
///> ViewController.m 文件

- (void)viewDidLoad {
    [super viewDidLoad];
    DLPerson *person = [[DLPerson alloc]init];
    person.age = 10;
    NSLog(@"%@",[person valueForKey:@"age"]);
}
```

#### 1.4 - (id)valueForKeyPath:(NSString *)keyPath;
```objc
///> ViewController.m 文件

- (void)viewDidLoad {
    [super viewDidLoad];
    DLPerson *person = [[DLPerson alloc]init];
    person.age = 10;
    NSLog(@"%@",[person valueForKey:@"cat.weight"]);
}
```
**`(id)valueForKey:(NSString *)key;`** 和 **`(id)valueForKeyPath:(NSString *)keyPath`** 的区别：
- 同理1.2-1.3

### 2. setValue:forKey:的原理

![ setValue:forKey:的原理](https://user-gold-cdn.xitu.io/2018/12/26/167ea605f5f2c483?w=1918&h=704&f=png&s=219919)
- 当我们设置setValue:forKey:时
- 首先会查找setKey:、_setKey: (按顺序查找)  
- 如果有直接调用
- 如果没有，先查看accessInstanceVariablesDirectly方法
  ```objc
  + (BOOL)accessInstanceVariablesDirectly{
        return YES;   ///> 可以直接访问成员变量
    //    return NO;  ///>  不可以直接访问成员变量,  
    ///> 直接访问会报NSUnkonwKeyException错误  
    }
  ```
 - 如果可以访问会按照 _key、_isKey、key、iskey的顺序查找成员变量
 - 找到直接复制
 - 未找到报错NSUnkonwKeyException错误

 _key、_isKey、key、iskey复制顺序
```objc
///> DLPerson.h 文件

@interface DLPerson : NSObject{
    @public
    int _age;
    int _isAge;
    int age;
    int isAge;
}

@end
```
```objc
- (void)viewDidLoad {
    [super viewDidLoad];
    self.person1 = [[DLPerson alloc]init];
    ///> person1添加kvo监听
    NSKeyValueObservingOptions options = NSKeyValueObservingOptionNew | NSKeyValueObservingOptionOld;
    [self.person1 addObserver:self forKeyPath:@"age" options:options context:nil];
    
    ///> 通过KVC修改person.age的值
    [self.person1 setValue:@20 forKey:@"age"];
    
     NSLog(@"------");
}

- (void)observeValueForKeyPath:(NSString *)keyPath ofObject:(id)object change:(NSDictionary<NSKeyValueChangeKey,id> *)change context:(void *)context{
    NSLog(@"监听到了%@的%@属性发生了改变%@",object,keyPath,change);
}

- (void)dealloc{
    ///> 使用结束后记得移除
    [self.person1 removeObserver:self forKeyPath:@"age"];
}
```
- 在NSLog(@"------");位置打下短点注释方式去查看各个值得复制顺序 
- 然后在控制台中查看复制查找的顺序就OK了，
- DLPerson文件中的顺序即使是调换了也无所谓。




### 3. valueForKey:的原理

![valueForKey:的原理](https://user-gold-cdn.xitu.io/2018/12/26/167ea61203a823f5)

- kvc取值按照 getKey、key、iskey、_key 顺序查找方法
- 存在直接调用
- 没找到同样，先查看accessInstanceVariablesDirectly方法
  ```objc
  + (BOOL)accessInstanceVariablesDirectly{
        return YES;   ///> 可以直接访问成员变量
    //    return NO;  ///>  不可以直接访问成员变量,  
    ///> 直接访问会报NSUnkonwKeyException错误  
    }
  ```
- 如果可以访问会按照 _key、_isKey、key、iskey的顺序查找成员变量
- 找到直接复制
- 未找到报错NSUnkonwKeyException错误

## 三. 知识点补充
### 1. _NSSetIntValueAndNotify();方法来源
```objc
- (void)viewDidLoad {
    [super viewDidLoad];
    self.person1 = [[DLPerson alloc]init];
    self.person1.age = 10;
    
    self.person2 = [[DLPerson alloc]init];
    self.person2.age = 20;
    
    ///> person1添加kvo监听
    NSLog(@"person添加KVO之前 - person1：%@， person2：%@",object_getClass(self.person1), object_getClass(self.person2));
    
    NSKeyValueObservingOptions options = NSKeyValueObservingOptionNew | NSKeyValueObservingOptionOld;
    [self.person1 addObserver:self forKeyPath:@"age" options:options context:nil];
    
    NSLog(@"person添加KVO之前 - person1：%@， person2：%@",object_getClass(self.person1), object_getClass(self.person2));
}

```
输出结果：
```
person添加KVO之前 - person1：DLPerson， person2：DLPerson

person添加KVO之后 - person1：NSKVONotifying_DLPerson， person2：DLPerson
```
由此可见在没有 为person1添加KVO之前 person1.isa指针仍然是DLPerson


**`那么我们就可以使用- (IMP)methodForSelector:(SEL)aSelector;去查看实现方法的地址，的具体方法代码如下：`**
```objc
///> person1添加kvo监听
    NSLog(@"person添加KVO之前 - person1：%p， person2：%p \n",[self.person1 methodForSelector:@selector(setAge:)], [self.person2 methodForSelector:@selector(setAge:)]);
    
    NSKeyValueObservingOptions options = NSKeyValueObservingOptionNew | NSKeyValueObservingOptionOld;
    [self.person1 addObserver:self forKeyPath:@"age" options:options context:nil];
    
    NSLog(@"person添加KVO之后 - person1：%p， person2：%p \n",[self.person1 methodForSelector:@selector(setAge:)], [self.person2 methodForSelector:@selector(setAge:)]);
}
```
输出结果：
```
person添加KVO之前 - person1：0x10852a560， person2：0x10852a560 
person添加KVO之后 - person1：0x108883fc2， person2：0x10852a560 
```
由此可见，在添加之前person1和person2实现的setAge方法是一个，添加之后person1的setAge方法就有了变化。

**然后我们打入短点去查看实现的方法：**

![_NSSetIntValueAndNotify();方法来源](https://user-gold-cdn.xitu.io/2018/12/26/167e9d579dd0e059?w=1998&h=362&f=png&s=166045)
在控制台中使用  **`p (IMP)方法地址`**  来打印得到方法的名称。
所以我们在添加KVO之后的 **`setAge:`** 方法调用了 **`_NSSetIntValueAndNotify()`** 。

    如果定义的属性是类型是double则调用的是_NSSetDoubleValueAndNotify()
    大家可以自己测试一下。
    此方法在Foundtion框架中有对应的
    NSSetDoubleValueAndNotify()
    NSSetIntValueAndNotify()
    NSSetCharValueAndNotify()
    ...
**`目前还未深入接触到逆向工程。等以后学到了在给大家详解解释吧。`**


### 2. NSKVONotifying_DLPerson的isa指针指向哪里？

在 **KVO的本质分析** 中我们得知，添加了KVO监听的实例对象isa指针指向了NSKVONotifying_DLPerson类， 那么NSKVONotifying_DLPerson的isa指针的指向？
```objc
- (void)viewDidLoad {
    [super viewDidLoad];
    self.person1 = [[DLPerson alloc]init];
    self.person1.age = 10;

    self.person2 = [[DLPerson alloc]init];
    self.person2.age = 20;

    ///> person1添加kvo监听
    NSKeyValueObservingOptions options = NSKeyValueObservingOptionNew | NSKeyValueObservingOptionOld;
    [self.person1 addObserver:self forKeyPath:@"age" options:options context:nil];

    NSLog(@"类对象 - person1: %@<%p>   person2: %@<%p>",
          object_getClass(self.person1),  ///> self.person1.isa  类名
          object_getClass(self.person1),  ///> self.person1.isa
          object_getClass(self.person2),  ///> self.person1.isa  类名
          object_getClass(self.person2)  ///> self.person1.isa
          );
    
    NSLog(@"元类对象 - person1: %@<%p>   person2: %@<%p>",
          object_getClass(object_getClass(self.person1)),  ///> self.person1.isa.isa 类名
          object_getClass(object_getClass(self.person1)),  ///> self.person1.isa.isa
          object_getClass(object_getClass(self.person2)),  ///> self.person2.isa.isa 类名
          object_getClass(object_getClass(self.person2))  ///> self.person2.isa.isa
          );
}
```
输出结果：
```
类对象 - person1: NSKVONotifying_DLPerson<0x6000002cef40>   person2: DLPerson<0x1002c9048>

元类对象 - person1: NSKVONotifying_DLPerson<0x6000002cf210>   person2: DLPerson<0x1002c9020>
```
结果发现：每一个类对象的地址是不一样的，而且元类对象的地址也不一样的，所以我们可以认为 NSKVONotifying_DLPerson类有自己的元类对象， NSKVONotifying_DLPerson.isa指向着自己的元类对象。


## 四. 面试题答案
1. iOS用什么方式实现对一个对象的KVO？(KVO的本质是什么？)

        利用RuntimeAPI动态生成一个子类，并且让instance对象的isa指向这个全新的子类
        当修改instance对象的属性时，会调用Foundation的_NSSetXXXValueAndNotify函数
        willChangeValueForKey:
        父类原来的setter
        didChangeValueForKey:
        内部会触发监听器（Oberser）的监听方法(observeValueForKeyPath:ofObject:change:context:）

2. 如何手动触发KVO？

        手动调用willChangeValueForKey:和didChangeValueForKey:

3. 直接修改成员变量会触发KVO么？

        不会触发KVO，因为直接修改成员变量并没有走set方法。


KVC相关：

1. 通过KVC修改属性会触发KVO么？

        会触发KVO，如上流程图

2. KVC的赋值和取值过程是怎样的？原理是什么？

        如上流程图
   
   
   ​     

---
- 文章总结自MJ老师底层视频。