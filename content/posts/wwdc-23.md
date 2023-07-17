---
title: "WWDC 23 and Swift Updates"
date: 2023-06-28T13:40:53+08:00
---

## Vision Pro Simulator

* [Download Link](https://developer.apple.com/download/)

> [install with command line](https://developer.apple.com/documentation/xcode/installing-additional-simulator-runtimes)

```
xcode-select -s /Applications/Xcode-beta.app
xcodebuild -runFirstLaunch
xcrun simctl runtime add "~/Downloads/watchOS 9 beta Simulator Runtime.dmg"
```

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

[源程序->可执行程序 - 小萝卜的文章 - 知乎](https://zhuanlan.zhihu.com/p/31895059)

> .c（源程序）->.obj（目标文件）的过程我们称之为编译，.obj（目标文件）->.exe（可执行文件）的过程我们称之为链接。

![源程序->可执行文件](https://pic1.zhimg.com/80/v2-f996490e3e745d2d91fcd01e28332074_1440w.webp)

[深入 iOS 静态链接器（一）— ld64](https://segmentfault.com/a/1190000040726418)

![静态链接（static linking）](https://segmentfault.com/img/remote/1460000040726420)

## Safari Updates



| WWDC Safari Extension | | |
|-|-|-|
| WWDC23 | | |
| [What's new in Safari extensions](https://developer.apple.com/wwdc23/10119) | [WWDC23 [中字] 137:Safari扩展有什么新变化](https://www.bilibili.com/video/BV16m4y1v7H4/?spm_id_from=333.999.0.0&vd_source=4c66f8c5cfa0134c7d37b554951f6920) | 了解Safari扩展的最新改进。我们将介绍新的API，探索Safari应用扩展的每个站点权限，并分享如何确保您的扩展在私人浏览和配置文件中都能正常工作。|
|[What’s new in Web Inspector](https://developer.apple.com/videos/play/wwdc2023/10118/)|[WWDC23 [中字] 102:Web Inspector最新更新介绍](https://www.bilibili.com/video/BV15o4y1N7rN/?vd_source=4c66f8c5cfa0134c7d37b554951f6920)|Web Inspector 提供了一组强大的工具来调试和检查 macOS、iOS 和 iPadOS 上的网页、Web 扩展和 WKWebViews。我们将分享最新的更新，包括改进的排版检查、可变字体的编辑工具、模拟人们偏好的控件、DOM 节点树中的元素徽章以及符号断点。|
| [Explore media formats for the web](https://developer.apple.com/videos/play/wwdc2023/10122/) | [WWDC23 [中字] 135:探索网络媒体格式](https://www.bilibili.com/video/BV1RM4y1J7U1/?spm_id_from=333.337.search-card.all.click&vd_source=4c66f8c5cfa0134c7d37b554951f6920) | Explore media formats for the web 了解Safari 17支持的最新图像格式和视频技术。发现如何在您的网站和体验中使用JPEG XL、AVIF和HEIC，并了解它们与以前的格式的区别。我们还将向您展示如何使用Managed Media Source API比Media更省电... |
| [Rediscover Safari developer features](https://developer.apple.com/videos/play/wwdc2023/10262/) | [WWDC23 [中字] 100:重新发现Safari开发者功能](https://www.bilibili.com/video/BV1s14y1S7uL/?spm_id_from=333.999.0.0&vd_source=4c66f8c5cfa0134c7d37b554951f6920) | 准备好探索Safari为Web开发人员和设计师提供的丰富工具集。了解如何检查Web内容，了解响应式设计模式和WebDriver，并开始使用模拟器和设备。我们还将向您展示如何与Vision Pro配对，在其中使内容可检查... |
|[What’s new in CSS](https://developer.apple.com/wwdc23/10121)|[WWDC23 [中字] 136:CSS的最新进展](https://www.bilibili.com/video/BV1vu411h74P/?spm_id_from=333.999.0.0&vd_source=4c66f8c5cfa0134c7d37b554951f6920)|探索CSS的最新进展。学习使用广色域颜色、创建华丽的排版和编写简单而健壮的代码的技巧和最佳实践。我们还将展望未来，预览即将推出的布局和排版功能。|
|[Meet Safari for spatial computing](https://developer.apple.com/videos/play/wwdc2023/10279/)|[WWDC23 [中字] 94:探索Safari在空间计算中的应用（适用于visionOS）](https://www.bilibili.com/video/BV1Gj411Q7ZG/?spm_id_from=333.999.0.0&vd_source=4c66f8c5cfa0134c7d37b554951f6920)|发现适用于visionOS的网络，并学习人们如何以全新的方式体验您的网络内容。探索支持该平台的独特输入模型，并了解如何优化您的网站以适应空间计算。我们还将分享新兴标准如何帮助塑造3D体验...|
|[What’s new in web apps](https://developer.apple.com/videos/play/wwdc2023/10120/)|[WWDC23 [中字] 138:网页应用的新功能介绍](https://www.bilibili.com/video/BV1Ak4y1H76B/?spm_id_from=333.999.0.0&vd_source=4c66f8c5cfa0134c7d37b554951f6920)|发现Mac上的网页应用——一种从Dock体验您的网站的强大方式。了解如何自定义您的网页应用程序，以便在人们添加您的网站时为他们提供最佳体验。我们还将分享如何利用推送通知和徽章来为Mac和主屏幕的网页应用程序提供更好的体验...|
|WWDC22|
|[What's new in Safari Web Extensions](https://developer.apple.com/wwdc22/10099)||Learn how you can use the latest improvements to Safari Web Extensions to create even better experiences for people browsing the web. We'll show you how to upgrade to manifest version 3, adopt the latest APIs for Web Extensions, and sync extensions across devices.|
|[What's new in WKWebView](https://developer.apple.com/videos/play/wwdc2022/10049/)|[wwdc2022-10049-What's new in WKWebView](https://www.bilibili.com/video/BV1ma411L7bS/?spm_id_from=333.337.search-card.all.click&vd_source=4c66f8c5cfa0134c7d37b554951f6920)|Explore the latest updates to WKWebView, our framework for incorporating web content into your app's interface. We'll show you how to use the JavaScript fullscreen API, explore CSS viewport units, and learn more about find interactions. We'll also take you through refinements to content blocking controls, embedding encrypted media, and using the Web Inspector.|
|[What's new in web accessibility](https://developer.apple.com/wwdc22/10153)||Discover techniques for building rich, accessible web apps with custom controls, SSML, and the dialog element. We'll discuss different assistive technologies and help you learn how to use them when testing the accessibility of your web apps.|
|[Replace CAPTCHAs with Private Access Tokens](https://developer.apple.com/videos/play/wwdc2022/10077/)||Don't be captured by CAPTCHAs! Private Access Tokens are a powerful alternative that help you identify HTTP requests from legitimate devices and people without compromising their identity or personal information. We'll show you how your app and server can take advantage of this tool to add confidence to your online transactions and preserve privacy.|
|WWDC21|
|[Meet Safari Web Extensions on iOS](https://developer.apple.com/videos/play/wwdc2021/10104/)|[[WWDC][2021] Meet Safari Web Extensions on iOS](https://www.bilibili.com/video/BV1qb4y1k7wX/?spm_id_from=333.337.search-card.all.click&vd_source=4c66f8c5cfa0134c7d37b554951f6920)|Safari Web Extensions use HTML, CSS, and JavaScript to offer people powerful browser customizations — and you can now create them for every device that supports Safari. Learn how to build a Safari Web Extension that works for all devices, and discover how you can convert an existing extension to Safari through Xcode and the Safari Web Extension Converter.|
|[Explore Safari Web Extension improvements](https://developer.apple.com/videos/play/wwdc2021/10027)||Learn how you can extend Safari's functionality with Safari Web Extensions. We'll introduce you to the latest WebExtension APIs, explore non-persistent background page support — a particularly relevant topic if you're developing for iOS — and discover how you can use the Declarative Net Request WebExtensions API to block content on the web. Finally, we'll show you how to customize tabs in Safari 15.|
|[Design for Safari 15](https://developer.apple.com/videos/play/wwdc2021/10029)||Meet Safari 15: redesigned and ready to help people explore the web. Discover how you can approach designing websites and apps for Safari, and learn how to incorporate the tab bar in your designs. We'll also take you through features like Live Text and accessibility best practices, explore the latest updates to CSS and Form Controls, and learn how to use the aspect-ratio property in CSS to create incredible websites.|
|[Develop advanced web content](https://developer.apple.com/wwdc21/10030l)||Develop in JavaScript, WebGL, or WebAssembly? Learn how the latest updates to Safari and WebKit — including language changes to class syntax — can help simplify your development process, enhance performance, and improve security. We'll explore several web APIs that can help provide better interoperability and bring new capabilities to your web content.|
|[Meet privacy-preserving ad attribution](https://developer.apple.com/videos/play/wwdc2021/10033)||Discover how you can measure your ad campaigns in apps and on the web without compromising privacy. We'll introduce you to Private Click Measurement and explore SKAdNetwork, which provides you with a more secure, private, and useful way to measure your app installs.|
|WWDC20|||
|[Meet Safari Web Extensions](https://developer.apple.com/videos/play/wwdc2020/10665/)|| When you create a Safari Web Extension, you can help people get common online tasks done more quickly and efficiently. We'll show you how to build a new Safari Web Extension and host it on the App Store, as well as how to use the safari-web-extension-converter tool to migrate existing extensions from other web browsers like Chrome, Firefox, or Edge with very little effort. |