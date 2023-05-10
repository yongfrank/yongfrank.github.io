---
title: "SwiftUI Example"
date: 2023-04-03T01:39:20+08:00
---

[opensource]: https://github.com/yongfrank/yongfrank.github.io/edit/main/content/posts/swiftui.md

This page is open source. [Improve this page][opensource].

> * [SwiftUI by Example](https://www.hackingwithswift.com/quick-start/swiftui/)
> * [SwiftUI 参考手册](https://www.cocoaz.com/swiftui/swiftui-vs-uikit-cheat-sheet/)

<!-- https://discord.com/channels/967978112509935657/967978112509935663/1105046781521305600 我购买了《Mastering SwiftUI》并将它汉化为简体中文，免费放在了我的 Github 静态服务器中，先读完 https://sspai.com/series/147 再继续阅读 https://ylqylq001.github.io/Mastering-SwiftUI 我相信你很快就能撸出漂亮的 SwiftUI App  -->

<!-- 还有件事强调一下，我放在 Github 静态服务器上的这本书，本来是打算方便自己阅读，偷偷转译的，不要到处传播，哪天因为版权被删除就麻烦了
https://ylqylq001.github.io/Mastering-SwiftUI  -->

## Working with static text (Laying out text neatly)

> * [hws - How to add advanced text styling using AttributedString](https://www.hackingwithswift.com/quick-start/swiftui/how-to-add-advanced-text-styling-using-attributedstring)
> * [hws - How to render Markdown content in text](https://www.hackingwithswift.com/quick-start/swiftui/how-to-render-markdown-content-in-text)
> * [Using Markdown in SwiftUI](https://www.appcoda.com/swiftui-markdown/)
> * [AttributedString](https://www.fatbobman.com/posts/attributedString/)

```swift
struct ContentView: View {
    
    private var content: String? {
        try? 
            String(
                contentsOf: Bundle.main.url(forResource: "README", withExtension: "md")!
            )
    }

    var txt: AttributedString {
        var text = try! AttributedString(markdown: "_Hamlet_ by William Shakespeare **Text in bold**")
        if let range = text.range(of: "William") {
            text[range].backgroundColor = .yellow
        }
        return text
    }
    
    var body: some View {
        VStack {
            Text(content ?? "?")
            Text(txt)
            Text("**Text in bold**")
        }
        .background(.red)
        .padding()
    }
}
```

This automatic Markdown conversion happens because SwiftUI interprets those strings as being instances of LocalizedStringKey – strings that can be localized by our app. This means if you want to create Markdown text from a property or variable, you should mark it explicitly as being LocalizedStringKey to get the Markdown rendering:

```swift
struct ContentView: View {
    let markdownText: LocalizedStringKey = "* This is **bold** text, this is *italic* text, and this is ***bold, italic*** text."

    var body: some View {
        Text(markdownText)
    }
}
```

### Working with AttributedString

[How to let users select text](https://www.hackingwithswift.com/quick-start/swiftui/)

![attributed string](https://www.appcoda.com/wp-content/uploads/2022/03/swiftui-markdown-3.png)

SwiftUI’s text component has a built-in support for AttributedString. You can simply pass it to the Text view for rendering.

## Stacks, grids, scrollviews

Position views in a structured way

> [hws - How to position views in a grid using LazyVGrid and LazyHGrid](https://www.hackingwithswift.com/quick-start/swiftui/how-to-position-views-in-a-grid-using-lazyvgrid-and-lazyhgrid)

![LazyVGrid](https://www.hackingwithswift.com/img/books/quick-start/swiftui/how-to-position-views-in-a-grid-using-lazyvgrid-and-lazyhgrid-1@2x.png)

```swift
import SwiftUI

struct ContentView: View {
    let columnLayout = [GridItem(.adaptive(minimum: 100, maximum: 100))]

    let allColors: [Color] = [.pink, .red, .orange, .yellow, .green, .mint, .teal, .cyan, .blue, .indigo, .purple, .brown, .gray]

    var body: some View {
        ScrollView {
            LazyVGrid(columns: columnLayout) {
                ForEach(allColors.indices, id: \.self) { index in
                    RoundedRectangle(cornerRadius: 4.0)
                        .aspectRatio(1.0, contentMode: ContentMode.fit)
                        .foregroundColor(allColors[index])
                }
            }
        }
        .padding()
    }
}
```

> [hws - How to dismiss the keyboard when the user scrolls](https://www.hackingwithswift.com/quick-start/swiftui/how-to-dismiss-the-keyboard-when-the-user-scrolls)

SwiftUI’s scrollDismissesKeyboard() modifier gives us precise control over how the keyboard should dismiss when the user scrolls around.

For example, we could place a TextField and TextEditor into a scroll view, and have them both dismiss the keyboard interactively like this:

```swift
struct ContentView: View {
    @State private var username = "Anonymous"
    @State private var bio = ""

    var body: some View {
        ScrollView {
            VStack {
                TextField("Name", text: $username)
                    .textFieldStyle(.roundedBorder)
                TextEditor(text: $bio)
                    .frame(height: 400)
                    .border(.quaternary, width: 1)
            }
            .padding(.horizontal)
        }
        .scrollDismissesKeyboard(.interactively)
    }
}
```

* Use `.automatic` to let SwiftUI judge what’s the best thing to do based on the context of the scroll.
* Use `.immediately` to make the keyboard dismiss fully as soon as any scroll happens.
* Use `.interactively` to make the keyboard dismiss inline with the user’s gesture – they need to scroll further to make it dismiss fully.
* Use `.never` if you want the keyboard to remain present during scrolling.


## User interface controls

Respond to interaction and control your program state

> [How to customize the submit button for TextField, SecureField, and TextEditor](https://www.hackingwithswift.com/quick-start/swiftui/how-to-customize-the-submit-button-for-textfield-securefield-and-texteditor)

![keyboard customize](https://www.hackingwithswift.com/img/books/quick-start/swiftui/how-to-customize-the-submit-button-for-textfield-securefield-and-texteditor-1@2x.png)

```swift
struct ContentView: View {
    @State private var username = ""

    var body: some View {
        TextField("Username", text: $username)
            .submitLabel(.join)
    }
}
```

> * [How to create multiline TextField in SwiftUI](https://sarunw.com/posts/swiftui-multiline-textfield/)
> * [iOS 16.0+ supprts multiline TextField.](https://stackoverflow.com/questions/58210457/make-the-textfield-multiline-swiftui)

axis: .horizontal will yield the same result as a normal text field.

```swift
struct MultilineTextFieldExample: View {
    @State var address = ""
    var body: some View {        
        TextField("Address", text: $address, axis: .vertical)

            .textFieldStyle(.roundedBorder)
            .padding()
    }
}
```

> * How to make a TextField or TextEditor have default focus
> * [hws - How to dismiss the keyboard for a TextField](https://www.hackingwithswift.com/quick-start/swiftui/how-to-dismiss-the-keyboard-for-a-textfield)

```swift
struct ContentView: View {
    @State private var name = ""
    @FocusState private var nameIsFocused: Bool

    var body: some View {
        VStack {
            TextField("Enter your name", text: $name)
                .focused($nameIsFocused)

            Button("Submit") {
                nameIsFocused = false
            }
        }
    }
}
```

> * [Apple Developers -  Keyboard toolbar](https://developer.apple.com/documentation/swiftui/toolbaritemplacement/keyboard)
> * [How to hide keyboard when using SwiftUI?](https://stackoverflow.com/questions/56491386/how-to-hide-keyboard-when-using-swiftui)
> * [FocusedValue and FocusedBinding property wrappers in SwiftUI](https://swiftwithmajid.com/2021/03/03/focusedvalue-and-focusedbinding-property-wrappers-in-swiftui/)
> * [Focus Management in SwiftUI: Getting Started](https://www.kodeco.com/31569019-focus-management-in-swiftui-getting-started)

![keyboard toolbar](https://i.stack.imgur.com/7oFYC.gif)

```swift
// ♣️♥️♠️♦️ appear above the keyboard
enum Field {
    case suit
    case rank
}

struct FieldKey: FocusedValueKey {
    typealias Value = Field
}

extension FocusedValues {
    var field: Field? {
        get { self[FieldKey.self] }
        set { self[FieldKey.self] = newValue }
    }
}

struct KeyboardBarDemo: View {
    @FocusedValue(\.field) var field: Field?
    @FocusState private var focusedField: Field?
    @State private var suitText = ""
    @State private var rankText = ""
    
    var body: some View {
        HStack {
            TextField("Suit", text: $suitText)
                .focusedValue(\.field, .suit)
                .focused($focusedField, equals: .suit)
                .textFieldStyle(.roundedBorder)
            TextField("Rank", text: $rankText)
                .focusedValue(\.field, .rank)
                .focused($focusedField, equals: .rank)
                .textFieldStyle(.roundedBorder)
        }
        .toolbar {
            ToolbarItemGroup(placement: .keyboard) {
                if field == .suit {
                    Button("♣️", action: { self.suitText += "♣️" })
                    Button("♥️", action: { self.suitText += "♥️" })
                    Button("♠️", action: { self.suitText += "♠️" })
                    Button("♦️", action: { self.suitText += "♦️" })
                }
                Spacer()
                Button("Done") {
                    focusedField = nil
                }
            }
        }
    }
}
```

> [Adding a clear button to a TextField](https://www.hackingwithswift.com/forums/100-days-of-swift/adding-a-clear-button-to-a-textfield/12079)

Just an update for this. If you add this modifier to TextField then you get the clear button.

```swift
TextField("regex", text: $regexResearch, axis: .vertical)
    .onAppear {
        UITextField.appearance().clearButtonMode = .whileEditing
    }
```

## Responding to events

## Tap and gestures

> * [stackoverflow - SwiftUI TapGesture and LongPressGesture in ScrollView with tap indication not working](https://stackoverflow.com/questions/62733633/swiftui-tapgesture-and-longpressgesture-in-scrollview-with-tap-indication-not-wo)
> * [Apple Developers - Longpress and list scrolling](https://developer.apple.com/forums/thread/127277)
>
> Thank you very much for pointing this out. It solved my problem. I didn't considder having an empty onTapGesture handler would have any impact on the scrolling, but it does. Maybe the common use is to have the tap handler and then add longtap as an additional gesture (makes sense).
Thanks again :-)

## Advanced state

Learn how to bind objects and query the environment

> [How to use @ObservedObject to manage state from external objects](https://www.hackingwithswift.com/quick-start/swiftui/how-to-use-observedobject-to-manage-state-from-external-objects)

```swift
class UserProgress: ObservableObject {
    @Published var score = 0 {
        didSet {
            if score > highScore {
                highScore = score
            }
        }
    }

    @AppStorage("HighScore") var highScore = 0
}

struct InnerView: View {
    @ObservedObject var progress: UserProgress

    var body: some View {
        Button("Increase Score") {
            progress.score += 1
        }
    }
}

struct ContentView: View {
    @StateObject var progress = UserProgress()

    var body: some View {
        VStack {
            Text("Your score is \(progress.score)")
            InnerView(progress: progress)
        }
    }
}
```

## Lists

Create scrolling tables of data

* [How to add a search bar to filter your data](https://www.hackingwithswift.com/quick-start/swiftui/how-to-add-a-search-bar-to-filter-your-data)

## Navigation

Direct your user through data in your app

* [Programmatically hide and show sidebar in split view](https://nilcoalescing.com/blog/ProgrammaticallyHideAndShowSidebarInSplitView/)
* [How to hide and show the sidebar programmatically](https://www.hackingwithswift.com/quick-start/swiftui/how-to-hide-and-show-the-sidebar-programmatically)
* [How to customize a view’s width in NavigationSplitView](https://www.hackingwithswift.com/quick-start/swiftui/how-to-customize-a-views-width-in-navigationsplitview)


## Alerts and menus

> [How to recommend another app using appStoreOverlay()](https://www.hackingwithswift.com/quick-start/swiftui/how-to-recommend-another-app-using-appstoreoverlay)

<video width="100%" src="https://www.hackingwithswift.com/img/books/quick-start/swiftui/how-to-recommend-another-app-using-appstoreoverlay-1.mp4" autoplay loop>
</video>

## Presenting Views

Move your user from one view to another

## Transforming views
Clip, size, scale, spin, and more

## Drawing

Take control of rendering using custom shapes

## Animation

Bring your views to life with movement

> [How to add and remove views with a transition](https://www.hackingwithswift.com/quick-start/swiftui/how-to-add-and-remove-views-with-a-transition)

<video src="https://www.hackingwithswift.com/img/books/quick-start/swiftui/how-to-add-and-remove-views-with-a-transition-1.mp4" width=100% autoplay loop autopictureinpicture>

</video>

```swift
struct ContentView: View {
    @State private var showDetails = false

    var body: some View {
        VStack {
            Button("Press to show details") {
                withAnimation {
                    showDetails.toggle()
                }
            }

            if showDetails {
                Text("Details go here.")
            }
        }
    }
}
```

> [View Transitions and Animations](https://designcode.io/swiftui-handbook-view-transition)
> 
> To show a new view with transition, you can use the if statement with a show boolean state. On that new view, apply a transition modifier. The zIndex modifier ensures that when dismissing, the new view stays on top.

```swift
@State var show = false
ZStack {
    if !show {
        Text("View transition")
    } else {
        VStack { Text("New View") }
            .transition(.move(edge: .bottom))
            .zIndex(1)
    }
}
.onTapGesture {
    show.toggle()
}
```

### Confetti

> [How to Create Confetti Animations in SwiftUI](https://www.appcoda.com/swiftui-confetti-animation/)

![Confetti](https://github.com/simibac/ConfettiSwiftUI/raw/master/Gifs/default.gif)

```swift
// https://github.com/simibac/ConfettiSwiftUI.git

import ConfettiSwiftUI
import SwiftUI

struct ContentView: View {
    
    @State private var counter: Int = 0
    
    var body: some View {
        Button("🎉") {
            counter += 1
        }
        .confettiCannon(counter: $counter)
    }
}
```

## Composing views

Make your UI structure easier to understand

## Cross-platform SwiftUI

learn 

> * [Swift - How to detect orientation changes](https://stackoverflow.com/questions/38894031/swift-how-to-detect-orientation-changes)
> * [SwiftUI如何在iOS16中优雅的检测和适配横竖屏 - 青颜君的文章 - 知乎](https://zhuanlan.zhihu.com/p/568441109)
> * [Food Truck: Building a SwiftUI multiplatform app](https://developer.apple.com/documentation/swiftui/food_truck_building_a_swiftui_multiplatform_app)

```swift
// Deprecated
guard let orientation = UIApplication.shared.windows.first?.windowScene?.interfaceOrientation 
else { return true }

if orientation == .landscapeRight || orientation == .landscapeLeft {
    return false
}

// Food Truck By Apple
#if os(iOS)
@Environment (\.verticalSizeClass) var verticalSizeClass
@Environment(\.horizontalSizeClass) private var sizeClass
#endif
@Environment(\.dynamicTypeSize) private var dynamicType

func isCompact(width: Double) -> Bool {
    #if os(iOS)
    if self.sizeClass == .compact {
        if verticalSizeClass == .compact {
            return false
        }
    }
    #endif
    if self.dynamicType >= dynamicTypeThreshold {
        return true
    }
    if width < self.widthThreshold {
        return true
    }
    return false
}
```

| Device                  | Landscape/Portrait | horizontalSizeClass | verticalSizeClass |
| ------------------- | ------- | ------------------- | ---------------- |
| All iPhone          |  Portrait    | compact             | regular          |
| iPhone (except Max、Plus) | Landscape    | compact             | compact          |
| iPhone (Max、Plus)   | Landscape    | regular             | compact          |
| All iPad            | Landscape & Portrait  | regular             | regular          |

| 设备                  | 横/竖屏 | horizontalSizeClass | verticalSizeClass |
| ------------------- | ------- | ------------------- | ---------------- |
| 所有 iPhone          | 竖屏    | compact             | regular          |
| iPhone (Max、Plus 除外) | 横屏    | compact             | compact          |
| iPhone (Max、Plus)   | 横屏    | regular             | compact          |
| 所有 iPad            | 横、竖屏  | regular             | regular          |


## PreferenceKey

> [SwiftUI 小技巧：透過 PreferenceKey 簡單對齊視圖](https://www.appcoda.com.tw/swiftui-preferencekey/)

![before preference key](https://www.appcoda.com.tw/content/images/2019/12/1_QY4RlNdXQUq5owuQqrwvEw.png)

![after preference key](https://www.appcoda.com.tw/content/images/2019/12/1_wkJwKpz4CqmWmirN4uRYpg.png)

```swift
import SwiftUI

struct ContentView: View {
    @State var value1 = ""
    @State var value2 = ""
    @State var value3 = ""
    @State var width: CGFloat? = nil
    var body: some View {
        Form {
            HStack {
                Text("First Item")
                    .frame(width: width, alignment: .leading)
                    .background(ColumnWidthEqualiserView())
                TextField("Enter first item", text: $value1)
                    .textFieldStyle(RoundedBorderTextFieldStyle())
            }
            HStack {
                Text("Second Item")
                    .frame(width: width, alignment: .leading)
                    .background(ColumnWidthEqualiserView())
                TextField("Enter second item", text: $value2)
                    .textFieldStyle(RoundedBorderTextFieldStyle())
            }
            HStack {
                Text("Final  Item")
                    .frame(width: width, alignment: .leading)
                    .background(ColumnWidthEqualiserView())
                TextField("Enter third item", text: $value3)
                    .textFieldStyle(RoundedBorderTextFieldStyle())
            }
        }.modifier(ColumnWidth(width:$width))
    }
}

struct ColumnWidthPreference: Equatable {
    let width: CGFloat
}

struct ColumnWidthPreferenceKey: PreferenceKey {
    typealias Value = [ColumnWidthPreference]
    
    static var defaultValue: [ColumnWidthPreference] = []
    
    static func reduce(value: inout [ColumnWidthPreference], nextValue: () -> [ColumnWidthPreference]) {
        value.append(contentsOf: nextValue())
    }
}

struct ColumnWidthEqualiserView: View {
    var body: some View {
        GeometryReader { geometry in
            Rectangle()
                .fill(Color.clear)
                .preference(
                    key: ColumnWidthPreferenceKey.self,
                    value: [ColumnWidthPreference(width: geometry.frame(in: CoordinateSpace.global).width)]
                )
        }
    }
}

struct ColumnWidth: ViewModifier {
    @Binding var width: CGFloat?
    func body(content: Content) -> some View {
        content
            .onPreferenceChange(ColumnWidthPreferenceKey.self) { preferences in
                for p in preferences {
                    let oldWidth = self.width ?? CGFloat.zero
                    if p.width > oldWidth {
                        self.width = p.width + 20
                    }
                }
            }
    }
}
```

## UIKit Integration

> [访问 SwiftUI 内部的 UIKit 组件 - 猫克杯的文章 - 知乎](https://zhuanlan.zhihu.com/p/133444068)

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