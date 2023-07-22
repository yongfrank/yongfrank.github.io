---
title: "iOS Memory Management"
date: 2023-04-08T23:29:01+08:00
---

## Stack Heap

[iOS内存分配-栈和堆](https://juejin.cn/post/6938755477165572132)

> 只有OC对象需要进行内存管理的本质原因？
> 
> 因为OC对象在内存中是以堆的方式分配内存的，堆内存是由我们自己释放的（release）
> 
> 而非OC对象一般存在栈里，栈内存会被系统自动回收

![进程内存区域](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/e92e9c2af23d4324b0eb2f388db12d5a~tplv-k3u1fbpfcp-zoom-in-crop-mark:4536:0:0:0.awebp)

1. 代码区：代码段是用来存放可执行文件的操作指令（存放函数的二进制代码），也就是说是它是可执行程序在内存种的镜像。代码段需要防止在运行时被非法修改，所以只准读取操作，而不允许写入（修改）操作——它是不可写的。
2. 常量区：常量存储区，这是一块比较特殊的存储区，他们里面存放的是常量，
3. 全局（静态）区：放全局变量和静态变量，包含下面两个分区：
   1. 数据区：数据段用来存放可执行文件中已初始化全局变量，换句话说就是存放程序静态分配的变量和全局变量
   2. BSS区：BSS段包含了程序中未初始化全局变量
4. 堆（heap）区：堆是由程序员分配和释放，用于存放进程运行中被动态分配的内存段，它大小并不固定，可动态扩张或缩减


## iOS Memory Mgmt / ARC

> [iOS-[内存管理]](https://juejin.cn/post/7078882545403854861)

![ARC 抽象原理图](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/8a17346d91a24355b207b7f5e4785bf8~tplv-k3u1fbpfcp-zoom-in-crop-mark:4536:0:0:0.awebp)

在 Objective-C 中，“对象” 相当于办公室里的照明设备。在现实世界中办公室里的照明设备只有一个，但在 Objective-C 的世界里，虽然计算机的资源有限，但一台计算机可以同时处理好几个对象。

此外，“对象的使用环境” 相当于上班进入办公室的人。虽然这里的 “环境” 有时也指在运行中的程序代码、变量、变量作用域、对象等，但在概念上就是使用对象的环境。上班进入办公室的人对办公室照明设备发出的动作，与 Objective-C 中的对应关系则如下表所示

![图表 objc memory mgmt](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/b953465c8b994c55b7f5e2be017b224a~tplv-k3u1fbpfcp-zoom-in-crop-mark:4536:0:0:0.awebp)

## 使用弱引用来避免 Retain Cycles

retain对象会创建对该对象的强引用（即引用计数 +1）。一个对象在release它的所有强引用之后（即引用计数 =0）才会dealloc。如果两个对象相互retain强引用，或者多个对象，每个对象都强引用下一个对象直到回到第一个，就会出现 “Retain Cycles（循环引用）” 问题。循环引用会导致它们中的任何对象都无法dealloc，就产生了内存泄漏。

举个例子，Document 对象中有一个属性 Page 对象，每个 Page 对象都有一个属性，用于存储它所在的 Document。如果 Document 对象具有对 Page 对象的强引用，并且 Page 对象具有对 Document 对象的强引用，则它们都不能被销毁。

“Retain Cycles” 问题的解决方案是使用弱引用。弱引用是非持有关系，对象do not retain它引用的对象。

## ARC新规则

ARC会分析对象的生存期需求，并在编译时自动插入适当的内存管理方法调用的代码，而不需要你记住何时使用retain、release、autorelease方法。编译器还会为你生成合适的dealloc方法。一般来说，如果你使用ARC，那么只有在需要与使用MRC的代码进行交互操作时，传统的 Cocoa 命名约定才显得重要。

ARC引入了一些在使用其他编译器模式时不存在的新规则。这些规则旨在提供完全可靠的内存管理模型。有时候，它们直接地带来了最好的实践体验，也有时候它们简化了代码，甚至在你丝毫没有关注内存管理问题的时候帮你解决了问题。在ARC下必须遵守以下规则，如果违反这些规则，就会编译错误。

* 不能使用 retain / release / retainCount / autorelease
* 不能使用 NSAllocateObject / NSDeallocateObject
* 须遵守内存管理的方法命名规则
* 不能显式调用 dealloc
* 使用 @autoreleasepool 块替代 NSAutoreleasePool
* 不能使用区域（NSZone）
* 对象型变量不能作为 C 语言结构体（struct / union）的成员
* 显式转换 “id” 和 “void *” —— 桥接

代码示例中，我们创建了一个 ViewController 类，并使用 ARC 管理内存。我们在 ViewController 中创建了一个 UIView 对象，并将其存储在强引用变量中，以便我们可以访问和使用它。我们还创建了一个方法 createSubview，在其中创建了另一个 UIView 对象，并将其存储在弱引用变量中。在方法结束时，该对象将被释放，因为没有强引用变量引用它。

在 ViewController 中，我们还声明了一个属性 message，它使用 strong 修饰符进行声明。这意味着我们在使用 setter 方法设置属性值时，该属性值将被自动存储在强引用变量中，并在需要时自动释放。在使用 getter 方法获取属性值时，我们将返回一个指向该属性值的强引用。

在使用 ARC 时，您不需要手动释放对象，因为编译器会自动在适当的时候生成相应的代码来管理内存。但是，您需要注意避免循环引用，以免出现内存泄漏的问题。在上面的代码示例中，我们使用 __weak 修饰符来创建弱引用，以避免循环引用。

```objc
// ViewController.h

#import <UIKit/UIKit.h>

@interface ViewController : UIViewController

@property (nonatomic, strong) NSString *message;

@end
// ViewController.m

#import "ViewController.h"

@implementation ViewController

- (void)viewDidLoad {
    [super viewDidLoad];
    
    self.message = @"Hello, world!"; // 使用 setter 方法设置属性值
    NSLog(@"%@", self.message);     // 使用 getter 方法获取属性值
    
    // 创建对象并将其存储在强引用变量中
    UIView *view = [[UIView alloc] initWithFrame:CGRectMake(0, 0, 100, 100)];
    self.view = view;
    
    // 在方法中创建对象，由于该对象未被存储在强引用变量中，因此会被释放
    [self createSubview];
}

- (void)createSubview {
    // 创建对象并将其存储在弱引用变量中
    UIView __weak *weakSubview = [[UIView alloc] initWithFrame:CGRectMake(0, 0, 50, 50)];
    
    // 添加子视图，此时子视图仍然存在
    [self.view addSubview:weakSubview];
    
    // 使用 __weak 修饰符创建弱引用，防止循环引用
    ViewController __weak *weakSelf = self;
    
    // 在 block 中使用 weakSelf，防止循环引用
    [UIView animateWithDuration:1.0 animations:^{
        weakSelf.view.alpha = 0.5;
    }];
    
    // 在方法结束时，weakSubview 对象将被释放，因为没有强引用变量引用它
}

@end
```

## Swift

> [strong、weak和unowned的区别](https://github.com/pro648/tips/blob/master/sources/strong%E3%80%81weak%E5%92%8Cunowned%E7%9A%84%E5%8C%BA%E5%88%AB.md)

### 1. ARC

自动引用计数（即 Automated Reference Count，简称 ARC）是 Xcode 4.2版本的新特性，其与手动管理内存使用了相同的计数系统。不同点在于：系统在编译时会帮助我们插入合适的内存管理方法，保留和释放都会自动进行，避免了手动管理引用计数的一些潜在问题。

Swift 使用自动引用计数跟踪、管理app的内存。通常情况下，这意味着ARC会自动管理内存，开发者无需关注内存管理。当类的实例不再使用时，ARC会自动释放其占用的内存。

为帮助管理内存，ARC 有时需了解类之间的关系。在 Swift 中使用 ARC 与在 Objective-C 中使用 ARC 类似。

引用计数只适用类的实例。结构体和枚举是值类型，不是引用类型，存储和传递的时候并非使用引用。

### 2. strong

`strong`指针通过增加指向对象的引用计数，保护被指向对象不被ARC释放。即，只要有一个强指针指向该对象，它就不会被释放。

Swift 中声明的属性默认是`strong`。当对象间引用关系是线性时，使用`strong`指针不会产生问题。

当两个实例使用强指针指向彼此时，两个实例引用计数都不会变为零，即产生循环引用（strong reference cycle）。

下面是一个循环引用的示例：

```swift
class Person {
    let name: String
    init(name: String) {
        self.name = name
    }
    
    var apartment: Apartment?
    deinit {
        print("\(name) is being deinitizlized")
    }
}

class Apartment {
    let unit: String
    init(unit: String) {
        self.unit = unit
    }
    
    var tenant: Person?
    deinit {
        print("Apartment \(unit) is being deinitialized")
    }
}
```

### 3. 解决循环引用

Swift 提供了两种解决循环引用的方案：`weak`和`unowned`。`weak`和`unowned`引用其它实例时不会产生强引用，引用计数不会加一。因此，不会产生循环引用。

当一个实例的生命周期短于另一个时（即一个实例可以先被销毁），使用`weak`引用。在上面公寓的示例中可能出现公寓没有住户的情况。因此，可以使用`weak`解决循环引用问题。当另一个实例生命周期与当前实例相同，或长于当前实例时，使用`unowned`引用。
