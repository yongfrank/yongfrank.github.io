---
title: "Swift Snippet"
date: 2023-04-08T11:44:42+08:00
---

## Enum

> [Swift Enum Usage](https://www.jianshu.com/p/9c7a07163e5b)

```swift
enum Device {
    case iPad, iPhone, AppleTV, AppleWatch
    
}
extension Device: CustomStringConvertible{
    
    func introduced() -> String {
        
        switch self {
        case .iPad: return "iPad"
        case .iPhone: return "iPhone"
        case .AppleWatch: return "AppleWatch"
        case .AppleTV: return "AppleTV"
        }
    }
 
    var description: String {
        
        switch self {
        case .iPad: return "iPad"
        case .iPhone: return "iPhone"
        case .AppleWatch: return "AppleWatch"
        case .AppleTV: return "AppleTV"
        }
    }
}

print(Device.AppleTV.description)

print(Device.iPhone.introduced())
```
