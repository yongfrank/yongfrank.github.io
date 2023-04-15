---
title: "SwiftUI Example"
date: 2023-04-03T01:39:20+08:00
---

> * [SwiftUI by Example](https://www.hackingwithswift.com/quick-start/swiftui/)
> * [SwiftUI 参考手册](https://www.cocoaz.com/swiftui/swiftui-vs-uikit-cheat-sheet/)

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

[How to let users select text](https://www.hackingwithswift.com/quick-start/swiftui/)

## User interface controls

Respond to interaction and control your program state

> [How to customize the submit button for TextField, SecureField, and TextEditor](https://www.hackingwithswift.com/quick-start/swiftui/how-to-customize-the-submit-button-for-textfield-securefield-and-texteditor)

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
> * [How to dismiss the keyboard for a TextField](https://www.hackingwithswift.com/quick-start/swiftui/how-to-dismiss-the-keyboard-for-a-textfield)

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
