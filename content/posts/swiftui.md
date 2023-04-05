---
title: "SwiftUI Example"
date: 2023-04-03T01:39:20+08:00
---

> [SwiftUI by Example](https://www.hackingwithswift.com/quick-start/swiftui/)

## Working with static text (Laying out text neatly)

> * [How to add advanced text styling using AttributedString](https://www.hackingwithswift.com/quick-start/swiftui/how-to-add-advanced-text-styling-using-attributedstring)
> * [Using Markdown in SwiftUI](https://www.appcoda.com/swiftui-markdown/)

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

## Navigation

Direct your user through data in your app

* [Programmatically hide and show sidebar in split view](https://nilcoalescing.com/blog/ProgrammaticallyHideAndShowSidebarInSplitView/)
* [How to hide and show the sidebar programmatically](https://www.hackingwithswift.com/quick-start/swiftui/how-to-hide-and-show-the-sidebar-programmatically)
* [How to customize a view’s width in NavigationSplitView](https://www.hackingwithswift.com/quick-start/swiftui/how-to-customize-a-views-width-in-navigationsplitview)

## Lists

Create scrolling tables of data

* [How to add a search bar to filter your data](https://www.hackingwithswift.com/quick-start/swiftui/how-to-add-a-search-bar-to-filter-your-data)