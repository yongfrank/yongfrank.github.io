---
title: "Swift Snippet"
date: 2023-04-08T11:44:42+08:00
---

## Swift Intro

> [Swift---一门智能型的编程语言](https://developer.aliyun.com/article/254320?spm=a2c6h.13262185.profile.358.699e167e7REVuk)

## Common Problem

### safe subsript

> [5 个让 Swift 更优雅的扩展——Pt.1](https://juejin.cn/post/7026271045652840461)
> [Swift 经常数组越界？教你一招，一劳永逸的解决数组越界的问题](https://www.bilibili.com/video/BV1ZT4y1V7Rs/?share_source=copy_web&vd_source=bf4952280cde801b178268abc99a7047)

```swift
extension Array {
    subscript (safe index: Int) -> Element? {
        guard index >= 0 && index < self.count else {
            return nil
        }
        return self[index]
    }
}

values[safe: 2] // "C"
values[safe: 3] // nil
```

## UIKit Preview

> [UIKit Preview](https://ruipfcosta.github.io/UIKitPreviewsGenerator-Website/index.html)

<!-- markdownlint-disable MD033 -->
<video style="max-width: 100%;box-shadow: rgba(50, 50, 93, 0.25) 0px 50px 100px -20px, rgba(0, 0, 0, 1) 0px 30px 60px -30px;" loop muted autoplay playsinline>
    <source src="https://ruipfcosta.github.io/UIKitPreviewsGenerator-Website/assets/video/demo.mov">
</video>

```swift
import UIKit

class ViewController: UINavigationController {

    override func viewDidLoad() {
        super.viewDidLoad()
        // Do any additional setup after loading the view.
        var text = UILabel(frame: CGRect(x: 20, y: 20, width: 300, height: 300))
        text.text = "UIKit Preview Testing"
        self.view.addSubview(text)
    }
}

#if DEBUG

import SwiftUI

struct ViewControllerRepresentable: UIViewControllerRepresentable {
    typealias UIViewControllerType = ViewController

    func makeUIViewController(context: Context) -> UIViewControllerType {
        ViewController()
    }
    
    func updateUIViewController(_ uiViewController: UIViewControllerType, context: Context) {
        
    }
}

struct ViewController_Previews: PreviewProvider {
    static var previews: some View {
        ViewControllerRepresentable()
    }
}
```

### Swift Guard

> [关于swift的guard使用一定要慎用](https://www.cnblogs.com/xuruofan/p/9967754.html)
>
> 使用if判断保护的变量，使用返回仅限于if的括号范围内，这就导致使用这个变量的代码，只能写在if括号范围之内，十分不方便，也不够优美。对，不够优美，但这并不表示可以随便使用guard这个关键字，因为，因为....上代码。
> 
> 在for，while，do-while等循环语句中使用guard，一旦变量不存在，就会直接跳出函数方法，导致剩下循环没办法进行，当程序使用数组的时候，容易造成数组越界，从而发生crash，这个问题很低级，但是我确实脑抽的发生了，造成很严重的后果，所以，写这边博客，一是记录一下自己的错误，二是提醒其他iOS开发者，尽量避免与我同样的错误。


## Swift REPL

> * [Introduction to the Swift REPL](https://developer.apple.com/swift/blog/?id=18)
> * [Swift REPL简介 - Translation](https://developer.aliyun.com/article/254323?spm=a2c6h.13262185.profile.355.699e167e7REVuk)

<!-- markdownlint-disable MD010 -->
```txt
Arrow Keys		Move cursor left/right/up/down
Control+F		Move cursor right one character, same as right arrow
Control+B		Move cursor left one character, same as left arrow
Control+N		Move cursor to end of next line, same as down arrow
Control+P		Move cursor to end of prior line, same as up arrow
Control+D		Delete the character under the cursor
Option+Left		Move cursor to start of prior word
Option+Right	Move cursor to start of next word
Control+A		Move cursor to start of current line
Control+E		Move cursor to end of current line
Delete			Delete the character to the left of the cursor
Esc <			Move cursor to start of first line
Esc >			Move cursor to end of last line

箭头键                   将光标向左/右/上/下移动
Control+F               将光标向右移动一个字符
Control+B               将光标向左移动一个字符
Control+N               将光标移动到下一行
Control+P               将光标移动到上一行
Control+D               删除被光标选中的字符
Option+Left             将光标移动到前一个单词的开始处
Option+Right            将光标移动到下一个单词的开始处
Control+A               将光标移动到当前行的开始处
Control+E               将光标移动到当前行的结束处
Delete                  删除光标左边的字符
Esc <                   将光标移动到第一行的开始处
Esc >                   将光标移动到最后一行的结束处
```

## Enum

> [Swift Enum Usage](https://www.jianshu.com/p/9c7a07163e5b)
>
> 与OC不一样，Swift的枚举牛逼得多了，OC只能玩Int，他能玩：
>
>> 整型(Integer)
浮点数(Float Point)
字符串(String)
布尔类型(Boolean)

### Enumeration and Structures

> [Docs.swift.org - A Swift Tour](https://docs.swift.org/swift-book/documentation/the-swift-programming-language/guidedtour/#Enumerations-and-Structures)

```swift
enum Rank: Int {
    case ace = 1
    case two, three, four, five, six, seven, eight, nine, ten
    case jack, queen, king

    func simpleDescription() -> String {
        switch self {
        case .ace:
            return "ace"
        case .jack:
            return "jack"
        case .queen:
            return "queen"
        case .king:
            return "king"
        default:
            return String(self.rawValue)
        }
    }
}
let ace = Rank.ace
let aceRawValue = ace.rawValue

if let convertedRank = Rank(rawValue: 3) {
    let threeDescription = convertedRank.simpleDescription()
}

enum Suit {
    case spades, hearts, diamonds, clubs

    func simpleDescription() -> String {
        switch self {
        case .spades:
            return "spades"
        case .hearts:
            return "hearts"
        case .diamonds:
            return "diamonds"
        case .clubs:
            return "clubs"
        }
    }
}
let hearts = Suit.hearts
let heartsDescription = hearts.simpleDescription()

enum ServerResponse {
    case result(String, String)
    case failure(String)
}

let success = ServerResponse.result("6:00 am", "8:09 pm")
let failure = ServerResponse.failure("Out of cheese.")

switch success {
case let .result(sunrise, sunset):
    print("Sunrise is at \(sunrise) and sunset is at \(sunset).")
case let .failure(message):
    print("Failure...  \(message)")
}
// Prints "Sunrise is at 6:00 am and sunset is at 8:09 pm."
```

### Method and Property

```swift
enum Device {
    case iPad, iPhone, AppleTV, AppleWatch
    
}
extension Device: CustomStringConvertible{
    
    func introduced() -> String {
        
        switch self {
        case .iPad: return "iPad"
        case .iPhone: return "iPhone"
        case .AppleWatch: return "AppleWatch"
        case .AppleTV: return "AppleTV"
        }
    }
 
    var description: String {
        
        switch self {
        case .iPad: return "iPad"
        case .iPhone: return "iPhone"
        case .AppleWatch: return "AppleWatch"
        case .AppleTV: return "AppleTV"
        }
    }
}

print(Device.AppleTV.description)

print(Device.iPhone.introduced())
```

### Associated Value

```swift
enum Trade {
    case Buy(stock:String,amount:Int)
    case Sell(stock:String,amount:Int)
}

let trade = Trade.Buy(stock: "003100", amount: 100)

switch trade {
case .Buy(let stock,let amount):
    print("stock:\(stock),amount:\(amount)")
case .Sell(let stock,let amount):
    print("stock:\(stock),amount:\(amount)")
default:
    ()
}
```

case .Buy(let stock, let amount) 和 case let .Buy(stock, amount) 的区别在于使用位置限定符（Positional Qualifiers）的位置不同。

在 case .Buy(let stock, let amount) 中，位置限定符 let 出现在变量名称之前，因此它们只能应用于该 case 语句中的变量。这种方式将限定符用于单个变量，可以使代码更加易读和明确。

在 case let .Buy(stock, amount) 中，位置限定符 let 出现在枚举 case 名称之前，因此它们将应用于该枚举 case 中的所有变量。这种方式将限定符用于整个 case，可以使代码更加简洁和紧凑。

在实际使用中，两种方式都是有效的，并且它们的区别并不影响代码的行为。选择使用哪种方式主要取决于您的个人偏好和代码风格。

### Protocol

> [CustomstringConvertible](https://developer.apple.com/documentation/swift/customstringconvertible)

```swift
enum Trade :CustomStringConvertible{
    case Buy(stock:String,amount:Int)
    case Sell(stock:String,amount:Int)
    
    var description: String {
        
        switch self {
        case .Buy(_, _):
            return "Buy"
            
        case .Sell(_, _):
            return "Sell"
        }
    }
}

print(Trade.Buy(stock: "003100", amount: 100).description)
// Buy
print(Trade.Buy(stock: "003100", amount: 100))
// Buy
```

> [How to get the name of enumeration value in Swift?](https://stackoverflow.com/questions/24113126/how-to-get-the-name-of-enumeration-value-in-swift)

Note that you can customise what is printed in each of these scenarios:

```swift
extension City: CustomStringConvertible {
    var description: String {
        return "City \(rawValue)"
    }
}

print(city)
// prints "City 1"

extension City: CustomDebugStringConvertible {
    var debugDescription: String {
        return "City (rawValue: \(rawValue))"
    }
}

debugPrint(city)
// prints "City (rawValue: 1)"
```

### Extesnsion

枚举也可以进行扩展。最明显的用例就是将枚举的case和method分离，这样阅读你的代码能够简单快速地消化掉enum内容，紧接着转移到方法定义:

Enum can also be extended. The most obvious use case is to separate the enum's case and method, so that you can quickly digest the contents of the enum, and then move on to the method definition:

```swift
enum Assets {
    case ballRed, bouncer
}

extension Assets {
    var stringValue: String {
        switch self {
        case .ballRed:
            return "ballRed"
        case .bouncer:
            return "bouncer"
        }
    }
}
```

### Generics

```swift
enum Rubbish<T> {
    
    case price(T)
    
    func getPrice() -> T {
        
        switch self {
        case .price(let value):
            return value
        }
        
    }
}

print(Rubbish<Int>.price(100).getPrice())
// 100
```

### CaseIterable

```swift
// https://stackoverflow.com/questions/26261011/how-to-choose-a-random-enumeration-value
enum Ball: String, CustomStringConvertible, CaseIterable {
    case ballRed, ballBlue, ballCyan, ballGreen, ballGrey, ballPurple, ballYellow
    
    var description: String {
        return self.rawValue
    }
    
    static func randomBall() -> Ball {
        return Self.allCases.randomElement() ?? ballRed
    }
}

enum Weekday: CaseIterable {
    case sunday, monday, tuesday, wednesday, thursday, friday, saturday

    static func random<G: RandomNumberGenerator>(using generator: inout G) -> Weekday {
        return Weekday.allCases.randomElement(using: &generator)!
    }

    static func random() -> Weekday {
        var g = SystemRandomNumberGenerator()
        return Weekday.random(using: &g)
    }
}
```

## Stride

> * [讓 for 迴圈不只加 1 的 stride](https://medium.com/彼得潘的-swift-ios-app-開發問題解答集/swift-3的for迴圈不只會每次加1-5d4d6e14197)
> * [`stride(from:to:by:)`](https://developer.apple.com/documentation/swift/stride(from:to:by:))

例1: 從 0, 100, 200，一直加到 900。(每次加 100，不包含 1000)

```swift
var sum = 0
for i in stride(from: 0, to: 1000, by: 100) {
    sum = sum + i
}
```

例2: 從 0, 100, 200，一直加到 1000。(每次加 100，包含 1000)

```swift
var sum = 0
for i in stride(from: 0, through: 1000, by: 100) {
    sum = sum + i
}
```

## Lazy

> See more in [objc-snippet](../objc-snippet#swift--objc-lazy)

## functionBuilder

> [Trees in Swift](https://www.hackingwithswift.com/plus/data-structures/trees)

## Codable & NSCoding

> [Swift Codable 和 NSCoding协议，以及归档，JSON编码](https://www.jianshu.com/p/4876e94862e2)

前面文章中 UserDefaults 的基本用法 中对UserDefaults 进行了简单的介绍，它可以将一些简单的数据类型存储在本地，需要使用的时候再去读取。

如果对于复杂对象的存储则需要将其进行序列化，将对象转化为 NSData(Swift Data)类型之后再进行操作，比如，将其存在本地的某个文件（eg.people.plist, people.txt等）中。

有2种序列化的方式：

1. NSCoding： 老的Cocoa方式，OC的方式
2. Codable： 新的swift方式

## Value type, Reference type

[Swift值类型&引用类型](https://www.jianshu.com/p/d02ef86bbf79)

Swift 中，值类型的赋值为深拷贝（Deep Copy），值语义（Value Semantics）即新对象和源对象是独立的，当改变新对象的属性，源对象不会受到影响，反之同理。

## Difference Between Struct and Class

[Understanding Swift](https://www.hackingwithswift.com/quick-start/understanding-swift/)

### Q1: [Why does Swift have both classes and structs?](https://www.hackingwithswift.com/quick-start/understanding-swift/why-does-swift-have-both-classes-and-structs)

* Classes have deinitializers, which are methods that are called when an instance of the class is destroyed, but structs do not.
* 常量类中的变量属性可以自由修改，但常量结构中的变量属性不能。
* Variable properties in constant classes can be modified freely, but variable properties in constant structs cannot.

### Q2: [Why do classes have deinitializers and structs don’t?](https://www.hackingwithswift.com/quick-start/understanding-swift/why-do-classes-have-deinitializers-and-structs-dont)

The job of deinitializers is to tell us when a class instance was destroyed. For structs this is fairly simple: the struct is destroyed when whatever owns it no longer exists. So, if we create a struct inside a method and the methods ends, the struct is destroyed.

So, the simple reason for why structs don’t have deinitializers is because they don’t need them: each struct has its own copy of its data, so nothing special needs to happen when it is destroyed.

You can put your deinitializer anywhere you want in your code, but I love this quote from Anne Cahalan: “Code should read like sentences, which makes me think my classes should read like chapters. So the deinitializer goes at the end, it's the ~fin~ of the class!”

### override

[Swift中final、extension、override、@objc等关键字的使用](https://blog.csdn.net/qq_28091923/article/details/86529569)

override 关键字在 Swift 中用来明确表示子类想要覆盖（或重写）父类的方法、属性或下标脚本。这个关键字提供了一种安全机制，如果你尝试覆盖的成员在父类中不存在，编译器将给出错误提示，避免了可能的误操作。

在 Objective-C 中，没有对应的 override 关键字。当你在子类中使用和父类相同的方法名定义方法时，Objective-C 会默认进行覆盖。这也意味着，在 Objective-C 中，如果你错误地拼写了想要覆盖的方法名，编译器将不会发出任何警告，这可能会导致意料之外的运行时行为。

所以，Swift 中的 override 关键字提供了额外的类型检查和安全性，可以在编译时就捕获可能的错误，这是它与 Objective-C 行为的一个主要区别。

### class var, static var

class var 只能在类（class）中定义，不能在枚举（enum）和结构体（struct）中定义。另外，class var 只能定义计算属性，不能定义存储属性，但是它们可以被子类重写。

所以简单总结就是，如果你希望定义一个类型级别的存储属性，只能用 static var；如果你希望定义一个可被子类重写的类型级别的计算属性，可以用 class var。

```swift
class MyClass {
    class var myVar: Int {
        return 42
    }
}

class MySubClass: MyClass {
    override class var myVar: Int {
        return 84
    }
}

print(MyClass.myVar) // 输出 42
print(MySubClass.myVar) // 输出 84
```

[`@property(class, nonatomic, readonly) BOOL isSupported;`](https://developer.apple.com/documentation/arkit/arconfiguration/2923553-issupported?language=objc)

## C, C++ Integration

[在 Swift 中去调用 C/C++ 代码](https://glumes.com/post/ios/swift-call-c-function/)

### Cpp integration

1. Cpp File
2. Objective-C Wapper, .h .mm
3. Bridging-Header

```cpp
// .hpp file
#ifndef CppTest_hpp
#define CppTest_hpp

#include <stdio.h>
#include <iostream>

class CppTest {
    
public:
    void test();
};

#endif /* CppTest_hpp */

// .cpp file
#include "CppTest.hpp"

void CppTest::test() {
    printf("swift can call from Cpp");
}
```

```objc
// .h file
#import <Foundation/Foundation.h>

NS_ASSUME_NONNULL_BEGIN

@interface ObjCpp : NSObject
- (void)run;
@end

NS_ASSUME_NONNULL_END

// .mm file
#import "ObjCpp.h"
#import "CppTest.hpp"

@implementation ObjCpp

- (void)run {
    auto sp = std::make_shared<CppTest>();
    sp->test();
}

@end
```

```objc
#import "ObjCpp.h"
```

### dylib for Xcode project

[Linking Homebrew Libraries in Xcode](https://youtu.be/_BhDsxX7jmQ)

```sh
cd opt/homebrew/include
# Header Search Paths
```
