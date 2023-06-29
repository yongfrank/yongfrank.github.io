---
title: "WWDC 23 and Swift Updates"
date: 2023-06-28T13:40:53+08:00
---

## Swift Macro

> [Applying Macros](https://developer.apple.com/documentation/Swift/applying-macros)
>
> [swift中的宏定义 - 2016](https://www.jianshu.com/p/5f5f7c4e1dc0)
>
> [开始写一个 Swift 宏吧](https://juejin.cn/post/7244720558771159098)

> OpenAI
>
> 在编程中，宏（Macro）是一种方式，允许你定义一段代码，然后在程序的其他地方多次使用。想象一下，你在编写一个程序，里面有一个任务需要你多次重复，而且每次重复的内容都是一样的。如果你手动写这些代码，不仅会浪费时间，而且如果要修改这段代码，你就需要在所有用到的地方都进行修改。
>
> 这就是宏的用武之地。你可以创建一个宏，定义那段需要重复的代码，然后在程序的其他地方只需要调用这个宏就可以了。这样，你就只需要写一次代码，然后在需要的地方重复使用。而且如果需要修改代码，只需修改宏的定义就可以了，而不需要在程序的每个地方都进行修改。
>
> 举个例子，如果你在编写一个游戏，并且在很多地方都需要计算玩家的分数，你可以创建一个计算分数的宏，然后在需要计算分数的地方都调用这个宏，这样就能避免重复编写相同的代码。
>
> 简单来说，宏就像是一个可以重复使用的代码片段，让你可以在多个地方使用同样的代码，而不需要重复编写。

> 宏（Macro）和函数是编程中两种常用的代码复用工具，它们都可以使程序员避免重复编写代码。然而，它们的工作方式和用途有所不同。以下是它们的主要区别：
>
> 1. 预处理和编译：
宏是在预处理阶段展开的。预处理器会在编译代码之前，将所有宏调用替换为相应的代码。而函数则是在编译阶段处理的，当函数被调用时，程序会跳转到函数所在的内存位置，执行函数代码，然后返回到调用处。
>
> 2. 性能：
由于宏只是简单地替换代码，所以它们不需要函数调用的开销（例如设置堆栈帧，跳转到函数代码等），因此它们可能会比函数快。然而，过度使用宏可能会导致代码膨胀，因为每次使用宏，编译器都会生成一份新的代码。另一方面，函数只在内存中存在一份，无论调用多少次。
>
> 3. 类型检查：
函数在编译时会进行类型检查，这可以帮助发现错误。而宏不会进行类型检查，因为它们只是文本替换，这可能会导致错误更难发现。
>
> 4. 范围：
函数有其自己的作用域，这意味着在函数内部定义的变量在函数外部不可见。而宏没有这样的作用域，它们只是简单地替换文本。
>
> 5. 表达能力：
函数比宏更强大，更灵活。函数可以有复杂的控制流程（如循环和条件语句），可以递归调用，可以返回值，等等。而宏的能力较为有限。
>
> 总的来说，宏和函数都有其用途。宏通常用于定义简单的代码片段，这些片段会在代码的多个地方完全一样地使用。函数则用于定义更复杂的代码行为，特别是当代码涉及到控制流程，或者需要被反复调用但不想造成代码膨胀时。

## Xcode Other Linker Flags

[Xcode 15 linking error](https://developer.apple.com/forums/thread/731089)

> Hi, I am getting a linking error when building my app to run against an iOS17 device, using Xcode15. Same project builds and runs fine with Xcode 14 and iOS16. The linking error just says:
>
> clang: error: unable to execute command: Segmentation fault: 11 clang: error: linker command failed due to signal (use -v to see invocation)
>
> Not sure what I should try to overcome this. I can't run my app on an iOS17 device. It builds, links and runs just fine on a simulator.
>
> Working now after I added "-ld64" to "Other linker Flags". Thank you @songme


[iOS 温习之路 ”Other Linker Flags“](https://zhuanlan.zhihu.com/p/34232905)

> 话又说回来了，Other Linker Flags 到底是干什么的呢？ 从文档上对这个参数的使用也大概能猜出一二，熟悉苹果的开发者应该都知道 XCode链接器man ld命令，通俗点讲，连接器的目的就是将目标文件连接为可执行程序，详细了解请阅读[源程序->可执行程序](https://zhuanlan.zhihu.com/p/31895059)

![源程序->可执行文件](https://pic1.zhimg.com/80/v2-f996490e3e745d2d91fcd01e28332074_1440w.webp)

[深入 iOS 静态链接器（一）— ld64](https://segmentfault.com/a/1190000040726418)

![静态链接（static linking）](https://segmentfault.com/img/remote/1460000040726420)