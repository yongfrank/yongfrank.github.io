---
title: "Runtime Explained"
date: 2023-01-03T16:13:29+08:00
---

## RUNTIME

### Generally

* Compile time error
* Link time
* Runtime error

|| Compile Time Error | Runtime Error |
|-|-|-|
| Time | Earlier | Later |
| Error | Grammar Error, Semantic Error | Memory Error, Math Error|
|  | Easy | Hard|

### Specifically

* Runtime Library (stdio.h)
* Runtime Environment
  * No Runtime: C/C++, Rust
  * Light Runtime: Golang
  * Heavy Runtime: Java(JVM), Python(CPython), C#(.NET Runtime)
* JRE = JVM + Library

|| No Runtime | Runtime |
|-|-|-|
| Memory Mgmt | Mannually | Automatically |
| Thread Model | Rely on OS | Rely on Runtime |
| System Interaction | Directly | Indirectly(rely on Runtime) |
| Efficiency | High | Low(High on JIT) |
|| Bare Metal | ------- |

运行时 = 运行时环境 + 运行时库

面向硬件环境 = 无运行时 + 无运行库

Runtime（运行时）是一种基于 C 语言的库，它为 Objective-C 和 Swift 提供了一组动态运行时库函数，允许开发人员在运行时操作对象、类和协议，以及动态地创建类、添加方法、获取属性、关联对象等。

在 Objective-C 中，Runtime 可以用于通过字符串名称创建类、添加方法、交换方法实现、获取类的实例变量、获取对象的属性列表等。Runtime 还允许开发人员使用消息传递机制来调用方法，这是 Objective-C 对象之间通信的基础。通过使用 Runtime，开发人员可以在运行时动态地修改对象和类的行为，从而实现更加灵活的编程。

> [OC史上最实用的 runtime 总结](https://cloud.tencent.com/developer/article/1447051)

1. 将某些OC代码转为运行时代码，探究底层，比如block的实现原理（上边已讲到）；
2. 拦截系统自带的方法调用（Swizzle 黑魔法），比如拦截imageNamed:、viewDidLoad、alloc；
3. 实现分类也可以增加属性；
4. 实现NSCoding的自动归档和自动解档；
5. 实现字典和模型的自动转换。

在 Swift 中，Runtime 的作用也很重要。Swift 在语言层面上提供了很多动态语言的特性，例如反射（Reflection）、动态派发（Dynamic Dispatch）等。这些功能都是通过 Swift Runtime 来实现的，因此可以说 Runtime 是支持 Swift 动态语言特性的基础。

Runtime 是 Apple 开发平台中许多重要框架的基础，例如 Cocoa Touch、UIKit、AppKit、Core Data、Foundation 等。除此之外，Runtime 还是许多第三方框架和库的基础，例如 AFNetworking、Mantle 等。

## Coding

### Objective

> [acedemy](https://academy.realm.io/cn/posts/mobilization-roy-marmelstein-objective-c-runtime-swift-dynamic/)

```objc
typedef struct objc_class *Class;

struct objc_object {
  Class isa;
};

struct objc_class {
  Class isa;
  Class super_class;
  const char *name;
  long version;
  long info;
  long instance_size;
  struct objc_ivar_list *ivars;
  struct objc_method_list **methodLists;
  struct objc_cache *cache;
  struct objc_protocol_list *protocols;
};
```

### Method Swizzling

> [Method swizzling in iOS swift](https://abhimuralidharan.medium.com/method-swizzling-in-ios-swift-1f38edaf984f)

```swift
import UIKit
import Foundation

extension UIColor {
    @objc func colorDescription() -> String {
        return "Print rainbow colours."
    }
    
    static let swizzleDescriptionImplementation: Void = {
        let instance: UIColor = .red
        let aClass: AnyClass! = object_getClass(instance)
        var originSelector: Selector = #selector(description)
        var swizzledSelector: Selector = #selector(colorDescription)
        let originalMethod: Method? = class_getInstanceMethod(aClass, originSelector)
        let swizzledMethod: Method? = class_getInstanceMethod(aClass, swizzledSelector)
        
        if let originalMethod = originalMethod, let swizzledMethod = swizzledMethod {
            method_exchangeImplementations(originalMethod, swizzledMethod)
        }
    }()
    
    static func swizzleDescription() {
        self.swizzleDescriptionImplementation
    }
}

print(UIColor.red)
print(UIColor.blue)
UIColor.swizzleDescription()
print(UIColor.red)
print(UIColor.blue)
UIColor.swizzleDescription()
print(UIColor.red)
print(UIColor.blue)
```

### KVC, KVO

> [Realm](https://academy.realm.io/cn/posts/mobilization-roy-marmelstein-objective-c-runtime-swift-dynamic/)

```objc
@property (nonatomic, strong) NSNumber *number;

[myClass valueForKey:@"number"];
[myClass setValue:@(4) forKey:@"number"];

[myClass addObserver:self
    forKeyPath:@"number"
    options:NSKeyValueObservingOptionInitial | NSKeyValueObservingOptionNew
    context:nil];

- (void)observeValueForKeyPath:(NSString *)keyPath
                      ofObject:(id)object
                        change:(NSDictionary<NSKeyValueChangeKey,id> *)change
                       context:(void *)context{
  // Respond to observation.
}
```

## Reference

[Discussion on RUNTIME - bilibili](https://www.bilibili.com/video/BV1Vr4y1G76c/)
