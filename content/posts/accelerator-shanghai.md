---
title: "Accelerator April Shanghai"
date: 2023-04-18T15:40:25+08:00
---

This page is open source. [Improve this page][opensource].

> [Steve Jobs Archive - Make Something Wonderful - Steve Jobs in his own words](https://stevejobsarchive.com/)

## SwiftUI Layout and rendering Luo Shuang

## Layout by Harry Ng, Sorted

[About Sorted3](https://www.sortedapp.com/about-us)

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
* Advertisement SDK - Size matters
* [AVKit](https://developer.apple.com/documentation/avkit)
* [Localization](https://developer.apple.com/documentation/xcode/localization): twitter = weibo
* AIGC - ChatGPT, [Copilot for Xcode](https://github.com/intitni/CopilotForXcode)
* Marketing Skills
  * TestFlight, Media, App Store, PressKit, contact in App KOL
  * [Crisp](https://crisp.chat/), [WeCom](https://www.tencent.com/en-us/responsibility/combat-covid-19-wecom.html)
  * Iteration: meet customers' requirements aka 「用户许愿，我们实现」
  * daily operations: ChatGPT bot webpage footer - traffic acquisition

## SwiftUI Mac App by Justin Yan

Elepic

[justinyan.app](https://justinyan.app/)

* [Keynote: gamma.app](https://gamma.app)
* [Justin Yan - Access own window in SwiftUI](https://justinyan.me/post/5656)
* [Mach-O otool](https://www.jianshu.com/p/fc67f95eee41)

### Mac App

* [Justin Yan - SwiftUI Study Notes. The best way to build an App?](https://twitter.com/MapleShadow/status/1641690615015694336?s=20)
* TestFlight: Pure SwiftUI
* Weather: Catalyst
* Music: Pure AppKit

### SwiftUI Advantages

1. Navigation: [NavigationSplitView](https://developer.apple.com/documentation/swiftui/navigationsplitview)
2. Drag and Drop: onDrag, onDrop
3. Preferences: enum
4. Multiple Windows: openWindowWithId

### Drawbacks and Workarounds

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

* Transform from ObservableObject to Combine, see also [fatbobman.com](https://www.fatbobman.com/)
* Single Source of Truth

## Swift Charts by Eric Woo

[Pixel Weather at App Store](https://apps.apple.com/us/app/pixel-weather-forecast/id1278650505?itsct=apps_box_link&itscg=30200)

* Chalkduster, [Apple Developer - System Font](https://developer.apple.com/fonts/system-fonts/)
* [`interpolationMethod(.catmullRom)`](https://developer.apple.com/documentation/charts/interpolationmethod/catmullrom?language=_8)
* [Apple Developer - Hello Swift Charts](https://developer.apple.com/videos/play/wwdc2022/10136/)
* [Apple Developer - Swift Charts: Raise the bar](https://developer.apple.com/videos/play/wwdc2022/10137/)
* [Apple Developer - Creating a chart using Swift Charts](https://developer.apple.com/documentation/charts/creating-a-chart-using-swift-charts)

## Developers

* [@aloveric - woo-interactive.com](https://woo-interactive.com/)
* [@caiyue5 - Music Mate](https://musicmate.fun/)
* [@cyongfrank - Oh My Flag](https://yongfrank.github.io/OhMyFlag-WWDC22/)
* [@harryworld - Sorted](https://www.sortedapp.com/)
* [@hzlzh - Lock Launcher](https://locklauncher.com/)
* [@liuyi0922 - miidii.tech](https://www.miidii.tech/)
* [@MapleShadow - justinyan.me](https://justinyan.me/)
* [Wei Li - WinkNotes](https://www.appnice.cn/)

This page is open source. [Improve this page][opensource].

[opensource]: https://github.com/yongfrank/yongfrank.github.io/edit/main/content/posts/accelerator-shanghai.md

<!-- ```txt
活动安排 ｜2023 年 4 月 18 日：
13:30 - 14:00 签到
14:00 - 14:05 欢迎致辞 Jason Wang
14:05 - 14:40 SwiftUI 布局与渲染 罗爽
14:50 - 16:30 SwiftUI 实践
                    - 开始使用 Layout 协议；Harry Ng, Sorted
                    - SwiftUI 快速响应产品设计； ZiLi, 
                    - SwiftUI 高效开发 Mac App；Justin
                    - Swift Charts 实践分享。 Eric Woo
16:30 - 17:00 茶歇 / 自由讨论
``` -->
