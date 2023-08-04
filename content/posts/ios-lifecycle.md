---
title: "iOS Lifecycle"
date: 2023-08-01T10:13:51+08:00
---

## Delegate

[What is a delegate in iOS?](https://www.hackingwithswift.com/example-code/language/what-is-a-delegate-in-ios)

> a delegate is any object that should be notified when something interesting has happened. What that "something interesting" means depends on the context: for example, a table view's delegate gets notified when the user taps on a row, whereas a navigation controller's delegate gets notified when the user moves between view controllers.

## Lifecycle

> * [iOS生命周期 - Main函数](https://tianziyao.github.io/2016/03/24/iOS生命周期%20-%20Main函数/)
> * [UIApplicationMain](https://developer.apple.com/documentation/uikit/1622933-uiapplicationmain)

```objc
UIApplicationMain(argc, argv, nil, NSStringFromClass([AppDelegate class]))
```

但是Swift项目中没有一个名为main.swift的文件，为什么app的入口没有了？官方文档的说法是这样的：

> In Xcode, Mac templates default to including a “main.swift” file, but for iOS apps the default for new iOS project templates is to add @UIApplicationMain to a regular Swift file. This causes the compiler to synthesize a mainentry point for your iOS app, and eliminates the need for a “main.swift” file.

> [在Xcode 12中"@main“是什么意思？](https://cloud.tencent.com/developer/ask/sof/107921052)
>
> [What does "@main" mean in Xcode 12?](https://stackoverflow.com/questions/64831598/what-does-main-mean-in-xcode-12)
> 
> Xcode 12支持在UIKit或基于AppKit的应用程序中使用@main代替@UIApplicationMain或@NSApplicationMain。

### AppDelegate SwiftUI

> [How to add an AppDelegate to a SwiftUI app](https://www.hackingwithswift.com/quick-start/swiftui/how-to-add-an-appdelegate-to-a-swiftui-app)

```swift
class AppDelegate: NSObject, UIApplicationDelegate {
    func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey : Any]? = nil) -> Bool {
        print("Your code here")
        return true
    }
}

@main
struct NewIn14App: App {
    @UIApplicationDelegateAdaptor(AppDelegate.self) var appDelegate

    var body: some Scene {
        WindowGroup {
            ContentView()
        }
    }
}
```