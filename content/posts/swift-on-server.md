---
title: "Swift on Server"
date: 2023-07-21T10:52:27+08:00
---

[Server Side Swift — 玩玩 Vapor 4](https://ken-60401.medium.com/server-side-swift-%E7%8E%A9%E7%8E%A9vapor-4-ff935af56ff9)

```swift
import Vapor

struct Person: Content {
    var name: String
    var age: Int
}

func routes(_ app: Application) throws {
    // ...
    app.get("json", use: getJson)
    // ...
}

func getJson(req: Request) throws -> Person {
    return Person(name: "Frank", age: 20)
}
```

```sh
curl --location 'http://127.0.0.1:8080/json'
```