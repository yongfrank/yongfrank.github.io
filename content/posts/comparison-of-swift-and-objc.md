---
title: "Comparison of Swift and Objc"
date: 2023-04-17T15:47:55+08:00
---

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