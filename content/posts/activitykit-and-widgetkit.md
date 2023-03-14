---
title: "ActivityKit & WidgetKit on iOS"
date: 2023-03-13T17:23:10+08:00
---

[ActivityKit]: https://developer.apple.com/documentation/activitykit
[WidgetKit]: https://developer.apple.com/documentation/widgetkit

## Live Activity

### 相关限制

* iOS 16.1 及以上
* 更新动态数据大小不能超过 4KB
* 只能在应用处于前台时从应用启动 LiveActivity。但是，可以在应用程序在后台运行时从应用程序更新或结束实时活动——例如，使用 [Background Task]。

[Background Task]: https://developer.apple.com/documentation/backgroundtasks

### 生命周期与约束

* [利用 Live Activities 显示实时数据][Displaying live data with Live Activities]

与 Widget 相比，实时活动使用不同的机制来接收更新。Live Activities 不是使用时间轴机制，而是使用 ActivityKit 从您的应用程序接收更新的数据，并通过 ActivityKit push notifications 远程接收。

Live Activities use a different mechanism to receive updates compared to widgets. Instead of using a timeline mechanism, Live Activities receive updated data from your app with ActivityKit and remotely with ActivityKit push notifications.

Live Activity 最多可以运行八小时，除非您的应用程序或其他人终止了它。过了这个8小时的时限，系统自动结束。当实时活动结束时，系统会立即将其从灵动岛中移除。但是，实时活动会保留在锁定屏幕上，直到有人将其删除或在系统将其删除之前再保留最多四个小时。因此，实时活动会在锁定屏幕上保留最多 12 小时。

A Live Activity can be active for up to eight hours unless your app or a person ends it. After this 8-hour limit, the system automatically ends it. When a Live Activity ends, the system immediately removes it from the Dynamic Island. However, the Live Activity remains on the Lock Screen until a person removes it or for up to four additional hours before the system removes it — whichever comes first. As a result, a Live Activity remains on the Lock Screen for a maximum of twelve hours.

每个 Live Activity 都在自己的沙盒中运行，并且与 Widget 不同，它无法访问网络或接收位置更新。要更新活动实时活动的动态数据，请在您的应用程序中使用 ActivityKit 框架或允许您的 Live Activity 接收 ActivityKit 推送通知，如[使用 ActivityKit 推送通知更新和结束您的实时活动]中所述。

Each Live Activity runs in its own sandbox, and — unlike a widget — it can’t access the network or receive location updates. To update the dynamic data of an active Live Activity, use the ActivityKit framework in your app or allow your Live Activities to receive ActivityKit push notifications as described in Updating and ending your Live Activity with ActivityKit push notifications.

除了使用 ActivityKit 从您的应用程序更新和结束 Live Activity 之外，还可以使用您从服务器发送到 Apple 推送通知服务 (APNs) 的 ActivityKit 推送通知来更新或结束 Live Activity。要了解有关使用推送通知更新实时活动的更多信息，请参阅使用 ActivityKit 推送通知更新和结束实时活动。

In addition to updating and ending a Live Activity from your app with ActivityKit, update or end a Live Activity with an ActivityKit push notification that you send from your server to the Apple Push Notification service (APNs). To learn more about using push notifications to update your Live Activities, see Updating and ending your Live Activity with ActivityKit push notifications.

> [利用 Live Activities 显示实时数据][Displaying live data with Live Activities]
>
> ActivityKit 更新和 ActivityKit 推送通知的更新动态数据大小不能超过 4KB。The updated dynamic data for both ActivityKit updates and ActivityKit push notifications can’t exceed 4KB in size.

[使用 ActivityKit 推送通知更新和结束您的实时活动]: https://developer.apple.com/documentation/activitykit/updating-and-ending-your-live-activity-with-activitykit-push-notifications

描述实时活动用户界面的代码是应用程序小部件扩展的一部分。尽管实时活动利用了 WidgetKit 的功能，但它们并不是小部件。与用于更新小部件用户界面的时间轴机制相比，您可以使用 ActivityKit 或 ActivityKit Push Notifications 从您的应用程序更新 Live Activity。

The code that describes the user interface of your Live Activity is part of your app’s widget extension. However, although Live Activities leverage WidgetKit’s functionality, they aren’t widgets. In contrast to the timeline mechanism you use to update your widgets’ user interface, you update a Live Activity from your app with ActivityKit or with ActivityKit push notifications.

 

### 设计指南 design guidance

* [Live Activities Design Guidance]
* [Live Activity 在锁屏以及灵动岛的尺寸][displaying live data with live activities]

### Live Activity Sizes

![Minimal(Attatched / Detached)](https://developer.apple.com/design/human-interface-guidelines/components/system-experiences/live-activities/images/type-minimal_2x.png)

![Compact](https://developer.apple.com/design/human-interface-guidelines/components/system-experiences/live-activities/images/type-compact_2x.png)

![Expanded](https://docs-assets.developer.apple.com/published/dec913bb4b4e8771fc8e55dbd4bdfb41/expanded-layout@2x.png)
<!-- ![Expanded](https://docs-assets.developer.apple.com/published/d865a4848645ca3e7e1b401fa6c94851/type-expanded@2x.png) -->

![Lock Screen](https://developer.apple.com/design/human-interface-guidelines/components/system-experiences/live-activities/images/live-activity-notch_2x.png)

![Lock Screen Example](https://www.macobserver.com/wp-content/uploads/2022/12/Live-Activities-Update-Frequency.jpg)

![How to Enable More Frequent Updates for Live Activities on iPhone](https://images.macrumors.com/t/gb6rFHGiyg7gQsCb3yXJDtaPj1A=/1600x0/article-new/2022/09/iOS-16-Live-Activities-Sports-MLB.jpeg)

| Screen dimensions (portrait) | Compact leading | Compact trailing | Minimal (width given as a range) | Expanded (height given as a range) | Lock Screen   |
|-----------------------------|----------------|------------------|---------------------------------|-----------------------------------|---------------|
| 430x932                     | 62.33x36.67    | 62.33x36.67      | 36.67–45x36.67                 | 408x84–160                        | 408x84–160    |
| 393x852                     | 52.33x36.67    | 52.33x36.67      | 36.67–45x36.67                 | 371x84–160                        | 371x84–160    |

> 如果 Live Activity 的高度超过 160 points，系统可能会在锁屏上截断它。The system may truncate a Live Activity on the Lock Screen if its height exceeds 160 points.
>
> 如果 Live Activity 的高度超过 160 points，系统可能会在灵动岛上截断它。The system may truncate a Live Activity in the Dynamic Island if its height exceeds 160 points.

[displaying live data with live activities]: https://developer.apple.com/documentation/activitykit/displaying-live-data-with-live-activities
[Live Activities Design Guidance]: https://developer.apple.com/design/human-interface-guidelines/components/system-experiences/live-activities

## 2. Coding 代码实现

1. 【创建 Extension】Create a widget extension Target(check "including Live Activity")
2. 【权限设置】Info.plist - Supports Live Activities - YES
3. 【数据结构】Add code that defines an [ActivityAttributes] structure to describe the static and dynamic data of your Live Activity.
4. 【灵动岛，锁屏 UI】Use the ActivityAttributes you defined to create the [ActivityConfiguration]. 
5. Add code to configure, start, update, and end your Live Activities.
6. 【Update Data】update your Live Activity using the [update(_:alertConfiguration:)] function and include an [AlertConfiguration].
7. Create a deep link into your app [widgetURL(_:)]
8. [Configure the Live Activity]

[update(_:alertConfiguration:)]: https://developer.apple.com/documentation/activitykit/activity/update(_:alertconfiguration:)
[AlertConfiguration]: https://developer.apple.com/documentation/activitykit/alertconfiguration
[widgetURL(_:)]: https://developer.apple.com/documentation/activitykit/activity/widgeturl(_:)
[Configure the Live Activity]: https://developer.apple.com/documentation/activitykit/displaying-live-data-with-live-activities#Configure-the-Live-Activity

```swift

// Define a set of static and dynamic data
struct OrderAttributes: ActivityAttributes {
    public struct ContentState: Codable, Hashable {
        var status: Status = .unstarted
    }
    var orderNumber: Int
}

// Add Live Activities to the widget extension
struct OrderStatusLiveActivity: Widget {
    var body: some WidgetConfiguration {
        ActivityConfiguration(for: OrderAttributes.self) { context in
            // Create the presentation that appears on the Lock Screen and as a
            // banner on the Home Screen of devices that don't support the
            // Dynamic Island.
            // ...
            OrderStatusLiveActivityView(context: context)
        } dynamicIsland: { context in
        // Create the presentations that appear in the Dynamic Island.
            // ...
            DynamicIslandView(context: context)
        }
    }
}

struct OrderLockerScreen_Previews: PreviewProvider {
    static let attributes = OrderAttributes(orderNumber: 24155)
    static let contentState = OrderAttributes.ContentState(status: .cycling)

    static var previews: some View {
        
        attributes
            .previewContext(contentState, viewKind: .content)
            .previewDisplayName("Notification")
    }
}

// If your app already offers widgets, add the Live Activity to your WidgetBundle.
import WidgetKit
import SwiftUI

@main
struct OrderStatusBundle: WidgetBundle {
    var body: some Widget {
        OrderLockerScreen()
        OrderStatus()
        OrderStatusLiveActivity()
    }
}
```

## WidgetKit

* [How to Build a Widget in Swift with WidgetKit](https://youtu.be/jucm6e9M6LA)
* [【iOS】WidgetKit Intro](https://zenn.dev/naoya_maeda/articles/e5c5af8ec567c9)

### 相关限制与使小组件保持更新 Keeping a widget up to date

WidgetKit 在一个单独的进程中代表您呈现视图。因此，即使小部件在屏幕上，您的小部件扩展也不会一直处于活动状态。尽管您的小部件并不总是处于活动状态，但您可以通过多种方式使其内容保持最新。

WidgetKit renders the views on your behalf in a separate process. As a result, your widget extension is not continually active, even if the widget is onscreen. Despite your widget not always being active, there are several ways you can keep its content up to date.

* 在预算范围内计划重新加载 Plan reloads within a budget
  * 对于用户经常查看的小部件，每日预算通常包括 40 到 70 次刷新。此速率大致相当于每 15 到 60 分钟重新加载一次小部件，但由于所涉及的许多因素，这些间隔通常会有所不同。Widget 小组件对用户可见的频率和时间。Widget 小组件的上次重新加载时间。Widget 小组件的包含应用程序是否处于活动状态。
* 为可预测的事件生成时间表 Generate a timeline for predictable events
  * WidgetKit 在重新加载小部件之前强加了最短时间。您的时间线提供商应创建至少相隔约 5 分钟的时间线条目。许多小部件都有可预测的时间点，在这些时间点更新它们的内容是有意义的。例如，显示天气信息的小部件可能会全天每小时更新一次温度。股票市场小部件可以在开市时间频繁更新其内容，但在周末根本不更新。通过提前计划这些时间，WidgetKit 会在适当的时间到来时自动刷新您的小部件。
* 当时间线改变时通知 WidgetKit：Inform WidgetKit when a timeline changes
  * 当某些事情影响了小部件的当前时间线时，您的应用程序可以告诉 WidgetKit 请求新的时间线。
* 显示动态日期：Display dynamic dates
  * 即使您的小部件不会持续运行，它也可以显示 WidgetKit 实时更新的基于时间的信息。例如，它可能会显示一个倒计时计时器，即使您的小部件扩展未运行，它也会继续倒计时。
* 后台网络请求完成后更新：Update after background network requests complete
  * 当您的小部件扩展处于活动状态时，例如提供snapshot或时timeline，它可以启动后台网络请求。例如，获取队友当前状态的游戏小部件，或获取带有图像缩略图的标题的新闻小部件。发出异步后台网络请求可让您快速将控制权返回给系统，从而降低因响应时间过长而被终止的风险。

![Widget](https://storage.googleapis.com/zenn-user-upload/4646e6df778f-20221003.png)
![Lock Screen accessoryCircular, accessoryRectangle, accessoryInline](https://storage.googleapis.com/zenn-user-upload/2813fb5e2f9f-20221003.jpg)
![应用程序识别在小部件中点击的链接](https://storage.googleapis.com/zenn-user-upload/93a97df084d4-20221005.png)
![Lock Screen Widget](https://storage.googleapis.com/zenn-user-upload/18a17e216337-20221001.gif)

### 在预算范围内计划重新加载 Plan reloads within a budget

由于额外的网络和处理，重新加载小部件会消耗系统资源并导致电池耗尽。要减少这种性能影响并保持全天的电池寿命，请将您请求的更新频率和数量限制在必要的范围内。

Reloading widgets consumes system resources and causes battery drain due to additional networking and processing. To reduce this performance impact and maintain all-day battery life, limit the frequency and number of updates you request to what’s necessary.

为了管理系统负载，WidgetKit 使用预算来分配一天中的小部件重新加载。预算分配是动态的，考虑了许多因素，包括：

To manage system load, WidgetKit uses a budget to distribute widget reloads over the course of the day. The budget allocation is dynamic and takes many factors into account, including:

* 小部件对用户可见的频率和时间。The frequency and times the widget is visible to the user.
* 小部件的上次重新加载时间。The widget’s last reload time.
* 小部件的包含应用程序是否处于活动状态。Whether the widget’s containing app is active.

WidgetKit 为用户添加到其设备的每个活动小部件维护不同的预算。例如，如果用户添加两个可配置体育小部件实例，显示两个不同球队的信息，则每个小部件都有自己的预算。

> 小部件的预算适用于 24 小时的时间段。WidgetKit 将 24 小时窗口调整为用户的日常使用模式，这意味着每日预算不一定会在午夜准确重置。对于用户经常查看的小部件，每日预算通常包括 40 到 70 次刷新。此速率大致相当于每 15 到 60 分钟重新加载一次小部件，但由于所涉及的许多因素，这些间隔通常会有所不同。

系统需要几天时间来了解用户的行为。在此学习期间，您的小部件可能会收到比平时更多的重新加载。

许多小部件都有可预测的时间点，在这些时间点更新它们的内容是有意义的。例如，显示天气信息的小部件可能会全天每小时更新一次温度。股票市场小部件可以在开市时间频繁更新其内容，但在周末根本不更新。通过提前计划这些时间，WidgetKit 会在适当的时间到来时自动刷新您的小部件。

### 为可预测的事件生成时间表

如果您的小部件可以预测它应该重新加载的时间点，最好的方法是为尽可能多的未来日期生成时间线。对于您显示的内容，使时间轴中条目的间隔尽可能大。WidgetKit 在重新加载小部件之前强加了最短时间。您的时间线提供商应创建至少相隔约 5 分钟的时间线条目。WidgetKit 可能会跨多个小部件合并重新加载，从而影响重新加载小部件的确切时间。

If your widget can predict points in time that it should reload, the best approach is to generate a timeline for as many future dates as possible. Keep the interval of entries in the timeline as large as possible for the content you display. WidgetKit imposes a minimum amount of time before it reloads a widget. Your timeline provider should create timeline entries that are at least about 5 minutes apart. WidgetKit may coalesce reloads across multiple widgets, affecting the exact time a widget is reloaded.

许多小部件都有可预测的时间点，在这些时间点更新它们的内容是有意义的。例如，显示天气信息的小部件可能会全天每小时更新一次温度。股票市场小部件可以在开市时间频繁更新其内容，但在周末根本不更新。通过提前计划这些时间，WidgetKit 会在适当的时间到来时自动刷新您的小部件。

### 当时间线改变时通知 WidgetKit：Inform WidgetKit when a timeline changes

当某些事情影响了小部件的当前时间线时，您的应用程序可以告诉 WidgetKit 请求新的时间线。

Your app can tell WidgetKit to request a new timeline when something affects a widget’s current timeline.

### 显示动态日期：Display dynamic dates

即使您的小部件不会持续运行，它也可以显示 WidgetKit 实时更新的基于时间的信息。例如，它可能会显示一个倒计时计时器，即使您的小部件扩展未运行，它也会继续倒计时。

### 后台网络请求完成后更新：Update after background network requests complete

当您的小部件扩展处于活动状态时，例如提供snapshot或时timeline，它可以启动后台网络请求。

When your widget extension is active, like when providing a snapshot or timeline, it can initiate background network requests.

例如，获取队友当前状态的游戏小部件，或获取带有图像缩略图的标题的新闻小部件。发出异步后台网络请求可让您快速将控制权返回给系统，从而降低因响应时间过长而被终止的风险。

For example, a game widget that fetches your teammate’s current status, or a news widget that fetches headlines with image thumbnails. Making asynchronous background network requests let you return control to the system quickly, reducing the risk of being terminated for taking too long to respond.

[WidgetKit]: https://developer.apple.com/documentation/widgetkit

[ActivityAttributes]: https://developer.apple.com/documentation/widgetkit/activityattributes
[ActivityConfiguration]: https://developer.apple.com/documentation/WidgetKit/ActivityConfiguration

```swift
import WidgetKit
import SwiftUI
import Intents

// TimelineProvider传递一个类型的对象，
// 该对象定义了一个方法来通知 WidgetKit 何时更新小部件，
// 以及一个方法来在每个时间将一个称为时间线条目的对象传递给 WidgetKit。
struct Provider: IntentTimelineProvider {
    // WidgetKit 在显示占位符视图时调用的方法。
    // TimelineEntry 将对象传递给占位符 View。
    // 当设备的环境发生变化时，例如当设备的动态类型设置发生变化时，
    // 将显示占位符视图。占位符视图（如下图）用于小部件的外观，没有任何实际内容或数据可显示。
    func placeholder(in context: Context) -> SimpleEntry {
        SimpleEntry(date: Date(), configuration: ConfigurationIntent())
    }

    // 当小部件首次出现在小部件库中时由 WidgetKit 调用的方法。
    // 在完成参数中传递一个对象 TimelineEntry，以向 WidgetKit 提供小部件的 SwiftUI 视图。
    func getSnapshot(
        for configuration: ConfigurationIntent,
        in context: Context,
        completion: @escaping (SimpleEntry) -> ()
    ) {
        let entry = SimpleEntry(date: Date(), configuration: configuration)
        completion(entry)
    }

    // 从小部件库中选择小部件时由 WidgetKit 调用的方法。
    // 创建一个对象，该对象是一组 TimelineEntry 对象数组和一个表示更新下一个时间线的时间的对象。
    // 然后将生成的对象作为参数传递，以通知 WidgetKit 小部件更新时间。 
    func getTimeline(
        for configuration: ConfigurationIntent,
        in context: Context,
        completion: @escaping (Timeline<Entry>) -> ()
    ) {
        var entries: [SimpleEntry] = []

        // Generate a timeline consisting of five entries an hour apart, starting from the current date.
        let currentDate = Date()
        for hourOffset in 0 ..< 5 {
            let entryDate = Calendar.current.date(byAdding: .hour, value: hourOffset, to: currentDate)!
            let entry = SimpleEntry(date: entryDate, configuration: configuration)
            entries.append(entry)
        }
        let timeline = Timeline(entries: entries, policy: .atEnd)
        completion(timeline)
    }
}

// 时间条目，有关于更新 Widget 的频率
// 在示例源代码中，尾随闭包省略了外部参数名称，
// 但 TimelineEntry 传递了一个采用一种类型的对象并返回
// SwiftUI 视图的闭包。这个返回的 SwiftUI 视图 View 将显示在 Widget 中。
struct SimpleEntry: TimelineEntry {
    let date: Date
    let configuration: ConfigurationIntent
}

// SwiftUI View on Widget
struct OrderStatusEntryView : View {
    var entry: Provider.Entry
    @Environment(\.widgetFamily) var family

    var body: some View {
        ZStack {
//            BackgroundBlueColor()
            if family == .systemSmall || family == .systemLarge || family == .systemMedium {
                ContainerRelativeShape()
                    .fill(.gray.gradient)
            }
            VStack {
                Text(entry.date, style: .time)
                if family != .accessoryCircular {
                    Text("hello bike")
                } else {
                    Text("HB")
                }
            }
        }
    }
}

struct SampleWidget2: Widget {
    var body: some WidgetConfiguration {
        IntentConfiguration(kind: "others", intent: ConfigurationIntent.self, provider: Provider()) { entry in
                SampleWidgetEntryView()
        }
    }
}

// Widget协议定义WidgetConfiguration类型的属性。body
struct OrderStatus: Widget {
    // Kind Parameter
    // WidgetKit 允许您为一个 Widget Extension 创建多个 widget。
    // 这样做时，kind它充当区分每个小部件的标识符。
    // 例如，当点击一个小部件启动一个应用程序时，应用程序kind可以识别它是通过哪种类型的小部件启动的。
    let kind: String = "OrderStatus"
    
    // 小部件设置Configuration定义了小部件范围的设置，例如小部件的类型和支持的小部件的大小，如下所述。
    // 当用户不需要自定义小部件时使用。
    // 例如，Photos 应用程序的 widget 仅显示用户存储在 Photos
    // 应用程序中的照片，并且不允许用户自定义 widget。
    var body: some WidgetConfiguration {
        // StaticConfiguration body 对 propertiesWidgetConfiguration 使用以下两个符合协议的属性之一：
        // IntentConfiguration 意图配置
        // 用于允许用户自定义小部件。
        // 例如，Weather App 小部件允许用户自定义显示天气的区域。
        IntentConfiguration(
            kind: kind,
            intent: ConfigurationIntent.self,
            provider: Provider()
        ) { entry in
            OrderStatusEntryView(entry: entry)
        }
        // 指定在用户配置小部件时显示的描述性文本。
        .configurationDisplayName("LiveActivity Test")
        .description("This is an example widget.")
        // 指定小部件支持的大小。
        .supportedFamilies(self.supportFamilies)
        
    }
    
    private var supportFamilies: [WidgetFamily] {
        if #available(iOSApplicationExtension 16.0, *) {
            return [
                .systemSmall,
                .systemMedium,
                .systemLarge,
                .systemExtraLarge,
                .accessoryInline,
                .accessoryCircular,
                .accessoryRectangular
            ]
        } else {
            return [
                .systemSmall,
                .systemMedium,
                .systemLarge
            ]
        }
    }
}

struct SampleWidgetEntryView : View {
    @Environment(\.widgetFamily) var family
//    var entry: Provider.Entry

    var body: some View {
        VStack() {
            Link(destination: URL(string: "https://baidu.com")!, label: {
                Text("Top Link")
                    .padding()
            })
            Link(destination: URL(string: "https://apple.com")!, label: {
                Text("Middle Link")
                    .padding()
            })
            Link(destination: URL(string: "https://google.com")!, label: {
                Text("Bottom Link")
                    .padding()
            })
        }
    }
}

struct OrderStatus_Previews: PreviewProvider {
    static var previews: some View {
        OrderStatusEntryView(
            entry: SimpleEntry(
                date: Date(),
                configuration: ConfigurationIntent()
            )
        )
        .previewContext(
            WidgetPreviewContext(family: .systemSmall)
        )
        OrderStatusEntryView(
            entry: SimpleEntry(
                date: Date(),
                configuration: ConfigurationIntent()
            )
        )
        .previewContext(
            WidgetPreviewContext(family: .accessoryRectangular)
        )
        OrderStatusEntryView(
            entry: SimpleEntry(
                date: Date(),
                configuration: ConfigurationIntent()
            )
        )
        .previewContext(
            WidgetPreviewContext(family: .accessoryCircular)
        )
        SampleWidgetEntryView()
            .previewContext(
                WidgetPreviewContext(family: .systemLarge)
            )
    }
}
```

## References

* [Adding widgets to the Lock Screen and watch faces]
* [How to Customize Live Activities Update Frequency](https://www.macobserver.com/tips/quick-tip/how-to-customize-live-activities-update-frequency/)
* [Live Activity]


[Live Activity]: https://developer.apple.com/documentation/activitykit/displaying-live-data-with-live-activities#Update-the-Live-Activity

[Adding widgets to the Lock Screen and watch faces]: https://developer.apple.com/documentation/widgetkit/adding_widgets_to_the_lock_screen_and_watch_faces

[Displaying live data with Live Activities]: https://developer.apple.com/documentation/activitykit/displaying-live-data-with-live-activities#Overview