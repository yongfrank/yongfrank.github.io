---
title: "Swift Concurrency"
date: 2023-09-08T17:32:57+08:00
---

> [Swift Concurrency by Example](https://www.hackingwithswift.com/quick-start/concurrency/)

## Introduction

> [Concurrency in top-level code](https://www.hackingwithswift.com/swift/5.7/top-level-concurrency)
>
> [玩了分手厨房，我理解了协程是什么](https://mp.weixin.qq.com/s/FjEVCxvmsu9hoMqS2zhvMA)
>
> [通过 Swift 的 async await 语法糖进行 URLSession](https://moe.jimmy0w0.me/2022/05/26/swift-urlsession-in-async-await/)

```swift
import Foundation

let apiURL = "https://my.api.mockaroo.com/movie_ticket.json?key=8795e870"

struct MovieTicket: Codable {
    let movieTitle, movieGenre: String
    let movieTicketPrice: Float
    let movieDuration: Int
}

enum TicketError: Error {
    case urlError
    case invalidServerResponse
}
```

### DataTask, closure

[How to create continuations that can throw errors](https://www.hackingwithswift.com/quick-start/concurrency/how-to-create-continuations-that-can-throw-errors)

```swift
func getDataFromClosure(completionHandler: @escaping (Result<[MovieTicket], Error>) -> Void) {
    guard let url = URL(string: apiURL) else {
        completionHandler(.failure(TicketError.urlError))
        return
    }
    
    URLSession.shared.dataTask(with: URLRequest(url: url)) { data, response, error in
        if let error = error {
            completionHandler(.failure(error))
        }
        
        guard let response = response as? HTTPURLResponse,
              response.statusCode == 200,
              let data = data
        else {
            completionHandler(.failure(TicketError.invalidServerResponse))
            return
        }
        
        do {
            let tickets = try JSONDecoder().decode([MovieTicket].self, from: data)
            completionHandler(.success(tickets))
        } catch {
            completionHandler(.failure(error))
        }
    }.resume()
}

getDataFromClosure { result in
    switch result {
    case .success(let tickets):
        for (index, ticket) in tickets.enumerated() {
            print("\(index): \(ticket.movieTitle) [genre] - \(ticket.movieGenre)")
        }
    case .failure(let failure):
        print(failure.localizedDescription)
    }
    exit(0)
}

// https://developer.apple.com/forums/thread/713085
// The standard workaround is to add a call to dispatchMain() which ‘parks’ the main thread,
// preventing the tool from exiting. If you then want to exit after your network request is complete, call exit(_:) directly.
dispatchMain()
```

### Concurrency

```swift
func getDataFromConcurrency() async throws -> [MovieTicket] {
    guard let url = URL(string: apiURL) else { throw TicketError.urlError }

    let (data, response) = try await URLSession.shared.data(from: url)

    guard let response = response as? HTTPURLResponse,
          response.statusCode == 200
    else { throw TicketError.invalidServerResponse }

    let tickets = try JSONDecoder().decode([MovieTicket].self, from: data)

    return tickets
}

do {
    let tickets = try await getDataFromConcurrency()
    for (index, ticket) in tickets.enumerated() {
        print("\(index): \(ticket.movieTitle) [genre] - \(ticket.movieGenre)")
    }
} catch {
    print(error.localizedDescription)
}
```

## Async/await

## Sequences and streams

## Tasks and task groups

## Actors

> [How to use @MainActor to run code on the main queue](https://www.hackingwithswift.com/quick-start/concurrency/how-to-use-mainactor-to-run-code-on-the-main-queue)

Error: Publishing changes from background threads is not allowed; make sure to publish values from the main thread (via operators like receive(on:)) on model updates.

## Solutions