---
categories: study
date: "2022-09-21T11:31:00Z"
title: Swift throws and rethrows, function as parameter
---

<!--
 * @Author: Frank Chu
 * @Date: 2022-09-19 16:47:12
 * @LastEditors: Frank Chu
 * @LastEditTime: 2022-09-21 11:32:00
 * @FilePath: /blog/_posts/2022-09-21-swift-throws-rethrows.md
 * @Description: 
 * 
 * Copyright (c) 2022 by Frank Chu, All Rights Reserved. 
-->

## Keyword throws

[What is a throwing function?](https://www.hackingwithswift.com/example-code/language/what-is-a-throwing-function)

Throwing functions are those that will flag up errors if problems happen, and Swift requires you to handle those errors in your code.

[How to handle errors in functions](https://www.hackingwithswift.com/quick-start/beginners/how-to-handle-errors-in-functions)

``` swift
enum PasswordError: Error {
    case short, obvious
}

func checkPassword(_ password: String) throws -> Bool {
    if password.count < 6 {
        throw PasswordError.short
    }
    
    if password == "123456" {
        throw PasswordError.obvious
    }
    
    return true
}

let password = "123456"

do {
    let isPasswordPassed = try checkPassword(password)
    if isPasswordPassed {
        print("Password Passed!")
    }
} catch {
    // If function throw line works, the error will be printed.
    print("Error:", error)
}

```

### Extended Reading Materials

* [When should you write throwing functions?](https://www.hackingwithswift.com/quick-start/understanding-swift/when-should-you-write-throwing-functions)
* [Why does Swift make us use try before every throwing function?](https://www.hackingwithswift.com/quick-start/understanding-swift/why-does-swift-make-us-use-try-before-every-throwing-function)

## Keyword rethrows

[How to use the rethrows keyword - Hacking With Swift](https://www.hackingwithswift.com/example-code/language/how-to-use-the-rethrows-keyword)

```swift
// That little String extension allows us to throw strings as errors, which saves a little time.
extension String: Error { }

func authenticateByPassword(_ user: String) -> Bool {
    return true
}

// So, biometric authentication (Touch ID, Face ID) always throws an error, and password authentication always works.
func authenticateBiometrically(_ user: String) throws -> Bool {
    throw "Failed"
}

// Because one of the two possibilities can throw, we mark its parameter as throwing, like this: method: (String) throws -> Bool.
func authenticateUser(method: (String) throws -> Bool) throws {
    try method("twostraws")
    print("Success!")
}

do {
    try authenticateUser(method: authenticateBiometrically)
} catch {
    print("D'oh")
}
```

Now for the important part: we both know that authenticateByPassword() doesn’t throw errors, and Swift can see that too, so if we change the definition of authenticateUser from throws to rethrows Swift will no longer require us to use do/catch when passing it a non-throwing parameter.

```swift
func authenticateUser(method: (String) throws -> Bool) rethrows {
    try method("twostraws")
    print("Success!")
}

authenticateUser(method: authenticateByPassword)

```

Now Xcode will give you a warning: the catch block later on is unreachable because authenticateUser will never throw errors. But if you were to call it using authenticateBiometrically then you would need the do/catch blocks – Swift is able to evaluate the flow of our code much better, which means we need to write less code.

## Function As Parameter

[How to accept functions as parameters](https://www.hackingwithswift.com/quick-start/beginners/how-to-accept-functions-as-parameters)

``` swift
func greetUser() {
    print("Hi there!")
}

var greetCopy: () -> Void = greetUser
greetCopy()

/// I’ve added the type annotation in there intentionally, 
/// because that’s exactly what we use when specifying functions as parameters: 
/// we tell Swift what parameters the function accepts, as well its return type.
```

```swift
func makeArray(size: Int, using generator: () -> Int) -> [Int] {
    var numbers = [Int]()
    
    for _ in 0..<size {
        let newNumber = generator()
        numbers.append(newNumber)
    }
    
    return numbers
}

// Example No.1
let rolls = makeArray(size: 50) {
    Int.random(in: 1...20)
}
print("Rolls:", rolls)

// Example No.2
func generateNumber() -> Int {
    Int.random(in: 1...20)
}
let newRolls = makeArray(size: 20, using: generateNumber)
print("New rolls:", newRolls)
```

1. The function is called makeArray(). It takes two parameters, one of which is the number of integers we want, and also returns an array of integers.
2. The second parameter is a function. This accepts no parameters itself, but will return one integer every time it’s called.
3. Inside ```makeArray()``` we create a new empty array of integers, then loop as many times as requested.
4. Each time the loop goes around we call the generator function that was passed in as a parameter. This will return one new integer, so we put that into the numbers array.
Finally the finished array is returned.

### Accept multiple function parameter

``` swift
func doImportantWork(first: () -> Void, second: () -> Void, third: () -> Void) {
    print("About to start first work")
    first()
    print("About to start second work")
    second()
    print("About to start third work")
    third()
    print("Done!")
}

doImportantWork {
    print("1: This is the first work.\n")
} second: {
    print("2: This is the second work.\n")
} third: {
    print("3: This is the third work\n")
}
```
