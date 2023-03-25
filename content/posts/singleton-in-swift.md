---
title: "Singleton in Swift"
date: 2023-03-25T01:39:04+08:00
---

## static variable and class variable

> [What’s the difference between a static variable and a class variable?](https://www.hackingwithswift.com/example-code/language/whats-the-difference-between-a-static-variable-and-a-class-variable)
>
> Both the static and class keywords allow us to attach variables to a class rather than to instances of a class. For example, you might create a Student class with properties such as name and age, then create a static numberOfStudents property that is owned by the Student class itself rather than individual instances.
>
> Where static and class differ is how they support `inheritance`: When you make a static property it becomes owned by the class and cannot be changed by subclasses, whereas when you use class it may be overridden if needed.

```swift
class Person {
    static var count: Int {
        return 250
    }

    class var averageAge: Double {
        return 30
    }
}

class Student: Person {
    // THIS ISN'T ALLOWED
    // override static var count: Int {
    //    return 150
    // }

    // THIS IS ALLOWED
    override class var averageAge: Double {
        return 19.5
    }
}
```

## MVC and Singleton

> [MVC Explain](https://onevcat.com/2018/05/mvc-wrong-use/)

![MVC standford university](https://onevcat.com/assets/images/2018/mvc.png)

```swift
class ToDoStore {
    static let shared = ToDoStore()
    
    private(set) var items: [ToDoItem] = []
    private init() {}
    
    func append(item: ToDoItem) {
        items.append(item)
    }
    
    var count: Int {
        return items.count
    }
    
    func item(at index: Int) -> ToDoItem {
        return items[index]
    }
}

class ToDoListViewController: UITableViewController {
    @objc func addButtonPressed(_ sender: Any) {
        let store = ToDoStore.shared
        let newCount = store.count + 1
        let title = "ToDo Item \(newCount)"
        
        store.append(item: .init(title: title))
    }
}

extension ToDoListViewController {
    override func tableView(_ tableView: UITableView, trailingSwipeActionsConfigurationForRowAt indexPath: IndexPath) -> UISwipeActionsConfiguration? {
        let deleteAction = UIContextualAction(style: .destructive, title: "Delete") { _, _, done in
            ToDoStore.shared.remove(at: indexPath.row)
            done(true)
        }
        return UISwipeActionsConfiguration(actions: [deleteAction])
    }
}
```

## @StateObject and @ObservedObject

> [How to use @ObservedObject to manage state from external objects](https://www.hackingwithswift.com/quick-start/swiftui/how-to-use-observedobject-to-manage-state-from-external-objects)
>
> You should not use this property wrapper to create the initial instance of an observable object – that’s what @StateObject is for.

```swift
class {
    @Published var score: Int = 0
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
    // This isn't recommended
    // @ObservedObject var progress = UserProgress()

    @StateObject var progress = UserProgress()

    var body: some View {
        VStack {
            Text("Your score is \(progress.score)")
            InnerView(progress: progress)
        }
    }
}
```
<!-- markdownlint-disable MD033 -->
<video loop muted autoplay playsinline>
    <source src="https://www.hackingwithswift.com/img/books/quick-start/swiftui/how-to-use-observedobject-to-manage-state-from-external-objects-1.mp4" type="video/mp4">
</video>

<!-- markdown-lint -->
> [StateObject & ObservedObject](https://onevcat.com/2020/06/stateobject/)
>
> Once you understand the issues with @ObservedObject, the significance of @StateObject becomes apparent. @StateObject is an upgrade of @State and is specifically designed to store ObservableObject class instances. While @State is created to store struct states, @StateObject is created to store ObservableObject class instances. It ensures that the instance of this class will not be recreated with the view, thus solving the problem.

![@ObservedObject Problem](https://onevcat.com/assets/images/2020/stateobject_reset.gif)

```swift
struct ContentView: View {
    @State private var showRealName = false
    var body: some View {
        VStack {
            Button("Toggle Name") {
                showRealName.toggle()
            }
            Text("Current User: \(showRealName ? "Wei Wang" : "onevcat")")
            ScorePlate().padding(.top, 20)
        }
    }
}

class Model: ObservableObject {
    init() { print("Model Created") }
    @Published var score: Int = 0
}

struct ScorePlate: View {
    
    @ObservedObject var model = Model()
    @State private var niceScore = false
    
    var body: some View {
        VStack {
            Button("+1") {
                if model.score > 3 {
                    niceScore = true
                }
                model.score += 1
            }
            Text("Score: \(model.score)")
            Text("Nice? \(niceScore ? "YES" : "NO")")
            ScoreText(model: model).padding(.top, 20)
        }
    }
}

struct ScoreText: View {
    @ObservedObject var model: Model
    
    var body: some View {
        if model.score > 10 {
            return Text("Fantastic")
        } else if model.score > 3 {
            return Text("Good")
        } else {
            return Text("Ummmm...")
        }
    }
}
```

## Custom Subclass of NSObject

> [Custom subclasses of NSObject](https://www.hackingwithswift.com/read/10/5/custom-subclasses-of-nsobject)
>
> NSObject is what's called a universal base class for all Cocoa Touch classes. That means all UIKit classes ultimately come from NSObject, including all of UIKit. You don't have to inherit from NSObject in Swift, but you did in Objective-C and in fact there are some behaviors you can only have if you do inherit from it. More on that in project 12, but for now just make sure you inherit from NSObject.
>
> [hws - project 12](https://www.hackingwithswift.com/read/12/3/fixing-project-10-nscoding)
>
> Why do we need a class here when a struct will do just as well? (And in fact better, because structs come with a default initializer!)
>
> Why do we need to inherit from NSObject?
>
> It's time for the answers to become clear. You see, working with NSCoding requires you to use objects, or, in the case of strings, arrays and dictionaries, structs that are interchangeable with objects. If we made the Person class into a struct, we couldn't use it with NSCoding.
>
> The reason we inherit from NSObject is again because it's required to use NSCoding – although cunningly Swift won't mention that to you, your app will just crash.
