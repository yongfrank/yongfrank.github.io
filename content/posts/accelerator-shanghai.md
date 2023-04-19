---
title: "Accelerator April Shanghai"
date: 2023-04-18T15:40:25+08:00
---

## SwiftUI Layout and rendering Luo Shuang

## Layout by Harry Ng, Sorted

[Sorted 3 by Harry Ng](https://apps.apple.com/us/app/sorted-calendar-notes-tasks/id1306893526?itsct=apps_box_link&itscg=30200)

* [ViewThatFits](https://developer.apple.com/documentation/swiftui/viewthatfits)
* [AnyLayout](https://developer.apple.com/documentation/swiftui/anylayout)

## SwiftUI and UIKit Interaction by Harry Ng, Sorted

* [UIViewRepresentable](https://developer.apple.com/documentation/swiftui/uiviewrepresentable)

Performance Hitches

## SwiftUI Fast Design by @hzlzh

[@hzlzh at twitter](https://twitter.com/hzlzh)

* [MVVM](https://www.hackingwithswift.com/books/ios-swiftui/introducing-mvvm-into-your-swiftui-project)
* [Details polishing - Always on Display](https://developer.apple.com/documentation/activitykit)
* [LazyVGrid](https://developer.apple.com/documentation/swiftui/lazyvgrid)
* [ScrollViewReader](https://developer.apple.com/documentation/swiftui/scrollviewreader)
* [import WeatherKit - SF Symbols](https://developer.apple.com/documentation/weatherkit)
* [import MusicKit & HealthKit](https://developer.apple.com/documentation/musickit)
* [WKWebView - Notion](https://developer.apple.com/documentation/webkit/wkwebview)
* Advertisement SDK
* [AVKit](https://developer.apple.com/documentation/avkit)
* Multi-language: twitter = weibo
* AIGC - ChatGPT, [Copilot for Xcode](https://github.com/intitni/CopilotForXcode)
* Marketing Skills
  * TestFlight, Media, App Store, PressKit, contact in App KOL
  * [Crisp](https://crisp.chat/), [WeCom](https://www.tencent.com/en-us/responsibility/combat-covid-19-wecom.html)
  * Iteration: meet customers' requirements
  * daily operations: ChatGPT bot webpage footer - traffic acquisition

## SwiftUI Mac App by Justin Yan

Elepic

[Mach-O otool](https://www.jianshu.com/p/fc67f95eee41)

* TestFlight: Pure SwiftUI
* Weather: Catalyst
* Music: Pure AppKit

### SwiftUI Advantages

1. Navigation: [NavigationSplitView](https://developer.apple.com/documentation/swiftui/navigationsplitview)
2. Drag and Drop: onDrag, onDrop
3. Preferences: enum
4. Multiple Windows: openWindowWithId

### Drawbacks

* [NSViewRepresentable](https://developer.apple.com/documentation/swiftui/nsviewrepresentable) is powerful
* [GeometryReader](https://developer.apple.com/documentation/swiftui/geometryreader) -> Customized WindowReader
* viewModifier -> .getWindow

### Debug SwiftUI

View Redrawing

* [Instruments](https://developer.apple.com/videos/play/wwdc2019/411/)
* CompressionView
* `Self._printChanges()` debug only, [no doc](https://stackoverflow.com/questions/69859370/where-is-self-printchanges-defined-and-or-documented-for-swiftui)
* [CoreAnimation](https://twitter.com/jsh8080/status/1206617106160246784)

### What's next?

* Transform from ObservableObject to Combine
* Single Source of Truth

## Swift Charts by Eric Woo

[Pixel Weather](https://apps.apple.com/us/app/pixel-weather-forecast/id1278650505?itsct=apps_box_link&itscg=30200)

[W2Solo](https://www.w2solo.com/topics/3750)

* Chalkduster
* `interpolationMethod(.catmullRom)`
* Hello Swift Charts
* Swift Charts: Raise the bar
* Creating a a chart using Swift Charts

```
活动安排 ｜2023 年 4 月 18 日：
13:30 - 14:00 签到
14:00 - 14:05 欢迎致辞 Jason Wang
14:05 - 14:40 SwiftUI 布局与渲染 罗爽
14:50 - 16:30 SwiftUI 实践
                    - 开始使用 Layout 协议；Harry Ng, Sorted
                    - SwiftUI 快速响应产品设计； ZiLi, 
                    - SwiftUI 高效开发 Mac App；
                    - Swift Charts 实践分享。
16:30 - 17:00 茶歇 / 自由讨论
```