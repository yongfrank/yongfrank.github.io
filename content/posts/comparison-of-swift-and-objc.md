---
title: "Comparison of Swift and Objc"
date: 2023-04-17T15:47:55+08:00
---

## struct and class

[Why does Swift have both classes and structs?](https://www.hackingwithswift.com/quick-start/understanding-swift/why-does-swift-have-both-classes-and-structs)

* Variable properties in constant classes can be modified freely, but variable properties in constant structs cannot. 常量类中的变量属性可以自由修改，但常量结构中的变量属性不能。
* Classes have deinitializers, which are methods that are called when an instance of the class is destroyed, but structs do not.
* Copies of structs are always unique, whereas copies of classes actually point to the same shared data.
* One class can be built upon (“inherit from”) another class, gaining its properties and methods.
* Classes do not come with synthesized memberwise initializers.

[Why @State only works with structs](https://www.hackingwithswift.com/books/ios-swiftui/why-state-only-works-with-structs)

Previously we looked at the differences between classes and structs, and there were two important differences I mentioned. First, that structs always have unique owners, whereas with classes multiple things can point to the same value. And second, that classes don’t need the mutating keyword before methods that change their properties, because you can change properties of constant classes.

When User was a struct, every time we modified a property of that struct Swift was actually creating a new instance of the struct. @State was able to spot that change, and automatically reloaded our view. Now that we have a class, that behavior no longer happens: Swift can just modify the value directly.

For SwiftUI developers, what this means is that if we want to share data between multiple views – if we want two or more views to point to the same data so that when one changes they all get those changes – we need to use classes rather than structs. 对于 SwiftUI 开发人员来说，这意味着，如果我们想在多个视图之间共享数据——如果我们希望两个或多个视图指向相同的数据，以便当一个视图更改时，它们都会得到这些更改——我们需要使用类而不是结构。

When User was a struct, every time we modified a property of that struct Swift was actually creating a new instance of the struct. @State was able to spot that change, and automatically reloaded our view. Now that we have a class, that behavior no longer happens: Swift can just modify the value directly. 
当 User 是一个结构体时，每次我们修改该结构的属性时，Swift 实际上都在创建该结构体的新实例。 @State 能够发现该更改，并自动重新加载我们的视图。现在我们有一个类，这种行为不再发生：Swift 可以直接修改值。

Remember how we had to use the mutating keyword for struct methods that modify properties? This is because if we create the struct’s properties as variable but the struct itself is constant, we can’t change the properties – Swift needs to be able to destroy and recreate the whole struct when a property changes, and that isn’t possible for constant structs. Classes don’t need the mutating keyword, because even if the class instance is marked as constant Swift can still modify variable properties. 
还记得我们如何将 mutating 关键字用于修改属性的结构方法吗？这是因为如果我们创建结构的属性为变量，但结构本身是常量的，我们就无法更改属性——Swift 需要能够在属性更改时销毁并重新创建整个结构，而这对于常量结构来说是不可能的。类不需要 mutating 关键字，因为即使类实例被标记为常量，Swift 仍然可以修改变量属性。

I know that all sounds terribly theoretical, but here’s the twist: now that User is a class the property itself isn’t changing, so @State doesn’t notice anything and can’t reload the view. Yes, the values inside the class are changing, but @State doesn’t monitor those, so effectively what’s happening is that the values inside our class are being changed but the view isn’t being reloaded to reflect that change.
我知道这一切听起来都很理论化，但这里有一个转折点：现在 User 这是一个类，属性本身没有改变，所以 @State 没有注意到任何东西，也无法重新加载视图。是的，类中的值正在更改，但不 @State 监视这些值，因此实际上正在发生的事情是我们类中的值正在更改，但视图未重新加载以反映该更改。

To fix this, it’s time to leave @State behind. Instead we need a more powerful property wrapper called @StateObject – let’s look at that now…
为了解决这个问题，是时候抛弃 @State 了。相反，我们需要一个更强大的属性包装器，称为 @StateObject – 现在让我们看看......

## block and closure

Objective-C中的Block和Swift中的闭包（Closure）都是一种匿名函数，可以作为参数传递给其他函数或者在函数内部定义。但它们在语法和使用上存在一些区别。

### 语法

Objective-C的Block使用^运算符来定义，后面跟着参数列表和函数体，例如：

```objectivec
void (^myBlock)(void) = ^{
    NSLog(@"Hello, world!");
};
```

Swift的闭包使用{}来定义，参数列表和函数体在{}中定义，例如：

```swift
let myClosure: () -> Void = {
    print("Hello, world!")
}
```

### 捕获变量

Objective-C的Block可以捕获外部变量并在函数内部使用，例如：

```objc
NSString *message = @"Hello, world!";
void (^myBlock)(void) = ^{
    NSLog(@"%@", message);
};
```

Swift的闭包也可以捕获外部变量，但是需要显式地声明捕获变量，并且可以指定捕获方式，例如：

```swift
let message = "Hello, world!"
let myClosure: () -> Void = { [message] in
    print(message)
}
```

### 内存管理

Objective-C的Block需要手动管理内存，因为Block在创建时会捕获外部变量并将其包含在Block中。如果Block被复制到堆上，则需要使用__block修饰符来解决循环引用的问题。

Swift 的闭包使用**捕获列表**来捕获外部变量，并且自动管理内存。如果闭包被捕获并持有了一个对象，只要闭包存在，这个对象就会被保持在内存中。

总之，Objective-C的Block和Swift的闭包都是一种函数式编程的重要工具，可以用于编写更加简洁、灵活的代码。但是它们在语法和使用上存在一些区别，需要开发者根据实际情况进行选择和使用。

## NS and Non-NS

### TimeInterval & NSTimeInterval

```swift
typealias TimeInterval = Double
```

```objc
typedef double NSTimeInterval;
```

## Property

在 Swift 中，@property 被替换为普通的属性定义。nonatomic 属性特性在 Swift 中默认为使用。assign 属性特性用于简单类型的属性，表示直接赋值。NSTimeInterval 被替换为 TimeInterval，BOOL 被替换为 Bool，double 保持不变。

### nonatomic

* 非默认属性。
* 两个线程同时访问同一个属性将会导致无法预计的结果。
* 优点是程序运行速度快。

### assign

* 与copy相反，只是reference，不是owner。只返回指针。
* 用于float、int、BOOL等类型。
* 释放后再发送消息会导致程序崩溃。

## Class Init

在 Objective-C 中，我们使用 alloc 和 init 方法来创建对象实例。alloc 方法分配内存空间，init 方法对对象进行初始化，并返回初始化后的对象实例。

而在 Swift 中，我们可以使用简化的语法来创建对象实例。通过使用类名后跟一对空括号 ()，可以直接创建一个类的实例。

在给定的代码示例中，ViewController() 使用的是 Swift 中的对象创建语法，它等同于 Objective-C 中的 alloc init。因此，它们的作用是相同的，都是用来创建 ViewController 类的实例。

如果你的项目是使用 Objective-C 编写的，你应该使用 alloc init 的方式来创建对象实例。而如果你的项目是使用 Swift 编写的，你可以使用简化的语法 ViewController() 来创建对象实例。

## Class Init

在 Objective-C 中，我们使用 alloc 和 init 方法来创建对象实例。alloc 方法分配内存空间，init 方法对对象进行初始化，并返回初始化后的对象实例。

而在 Swift 中，我们可以使用简化的语法来创建对象实例。通过使用类名后跟一对空括号 ()，可以直接创建一个类的实例。

在给定的代码示例中，ViewController() 使用的是 Swift 中的对象创建语法，它等同于 Objective-C 中的 alloc init。因此，它们的作用是相同的，都是用来创建 ViewController 类的实例。

如果你的项目是使用 Objective-C 编写的，你应该使用 alloc init 的方式来创建对象实例。而如果你的项目是使用 Swift 编写的，你可以使用简化的语法 ViewController() 来创建对象实例。
