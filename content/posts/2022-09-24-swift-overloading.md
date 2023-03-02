---
categories: study
date: "2022-09-24T14:28:00Z"
title: Swift Overloading
---

<!--
 * @Author: Frank Chu
 * @Date: 2022-09-24 14:28:27
 * @LastEditors: Frank Chu
 * @LastEditTime: 2022-09-25 10:17:24
 * @FilePath: /blog/_posts/2022-09-24-swift-overloading.md
 * @Description: 
 * 
 * Copyright (c) 2022 by Frank Chu, All Rights Reserved. 
-->

## Operator Overloading

[how to use operator overloading - Hacking With Swift](https://www.hackingwithswift.com/example-code/language/how-to-use-operator-overloading)

Operator overloading is the practice of adding new operators and modifying existing ones to do different things.

To create your own operator you need to tell Swift whether it should be prefix (before its operand; the values used with it), postfix (after its operand), or infix. The most common is infix: +, -, *, and more are all infix.

```swift
import Foundation

//  Tell Swift that it's infix. The most common is infix: 
//  +, -, *, and more are all infix.
infix operator **

func **(lhs: Double, rhs: Double) -> Double {
    return pow(lhs, rhs)
}
print(2 ** 3)
```

### Extended Reading Materials

[Why does Swift need operator overloading?](https://www.hackingwithswift.com/quick-start/understanding-swift/why-does-swift-need-operator-overloading)

## Swift Fuction Overloading

[Swift Function Overloading - Programiz](https://www.programiz.com/swift-programming/function-overloading)

```swift
//  function with two parameters
func display(firstNumber: Int, secondNumber: Int) {
    print(firstNumber > secondNumber ? firstNumber : secondNumber)
}

//  function with a single parameter
func display(number: Int) {
    print(number)
}

//  function call with single parameter
display(number: 3)
//  function call with two parameters
display(firstNumber: 30, secondNumber: 40)
```

## What's function overloading?

[C++ Function Overloading](https://www.cnblogs.com/skynet/archive/2010/09/05/1818636.html)

> When two or more different declarations are specified for a single name in the same scope,  that name is said to overloaded.
>
> By extension, two declarations in the same scope that declare the same name but with different types are called overloaded declarations.
>
> Only function declarations can be overloaded; object and type declarations cannot be overloaded.
>
> ANSI C++ Standard. P290
