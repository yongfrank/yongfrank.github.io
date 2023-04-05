---
title: "Combine"
date: 2023-04-03T00:34:18+08:00
---

## Sink

```swift
// 每秒增加 10 % 进度条的代码
import Combine
import Foundation

@MainActor
public class RegularExpressionModel: ObservableObject {
    @Published var progress = 0.0
    
    private var cancellable: AnyCancellable?

    init() {
        startProgressUpdate()
    }

    private func startProgressUpdate() {
        cancellable = Timer.publish(every: 1, on: .main, in: .common)
            .autoconnect()
            .sink { [weak self] _ in
                self?.updateProgress()
            }
    }

    private func updateProgress() {
        progress += 0.1
        if progress == 1.0000 {
            cancellable?.cancel()
        }
    }

    deinit {
        cancellable?.cancel()
    }
}
```

在英语中，sink 这个词有许多不同的意思，包括“水槽”，“下沉”，“降低”，“沉没”等等。在 Combine 框架中，sink 表示订阅者将从源发布者接收新的事件，并执行一个闭包，因此，我们可以将 sink 视为一个“接收器”，用于接收流中的新值并进行后续处理。这个单词可能被用来表示值“下沉”或“降低”到更低级别的代码中进行处理，或者表示订阅者正在“吸收”发布者发出的事件。在 Combine 中，sink 是订阅一个发布者的一种方式，因此这个术语可能也反映了这一点，即“吸收”或“消耗”流中的事件。



