---
title: "Swift on Server"
date: 2023-07-21T10:52:27+08:00
---

* [Server Side Swift — 玩玩 Vapor 4](https://ken-60401.medium.com/server-side-swift-%E7%8E%A9%E7%8E%A9vapor-4-ff935af56ff9)
* [Server Side Swift Using Vapor](https://www.youtube.com/watch?v=2tACpHQeHfI)

## GET, JSON

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

### Colon

```swift
// curl --location 'http://127.0.0.1:8080/customers/12'
app.get("customers", ":customerId" , use: getCustomerId)

func getCustomerId(req: Request) async throws -> String {
    guard let customerId = req.parameters.get("customerId", as: Int.self) else {
        throw Abort(.badRequest)
    }
    return "\(customerId) is welcoming"
}
```

## POST

```swift
import Foundation
import Vapor

struct Movie: Content {
    let title: String
    let year: Int
}

/*
curl --location 'http://127.0.0.1:8080/movies' \
--header 'Content-Type: application/json' \
--data '{
    "title": "Up",
    "year": 2020
}'
*/
app.post("movies") { req async throws in
    let movie = try req.content.decode(Movie.self)
    return movie
}
```

## Query

```swift
import Foundation
import Vapor

struct HotelQuery: Content {
    let sort: String
    let search: String?
}

// curl --location 'http://127.0.0.1:8080/hotels?sort=desc'
app.get("hotels") { req async throws in
    let hotelQuery = try req.query.decode(HotelQuery.self)
    return hotelQuery
}
```

## MVC Pattern Design

```swift

func routes(_ app: Application) throws {   
    try app.register(collection: MoviesController())
}

struct MoviesController: RouteCollection {
    func boot(routes: Vapor.RoutesBuilder) throws {
        let movies = routes.grouped("movies")
        
        // /movies
        movies.get(use: index)
        
        // /movies/20
        movies.get(":moviesId", use: index)
    }
    
    func index(req: Request) async throws -> String {
        return "index"
    }
    
    func show(req: Request) async throws -> String {
        // 5xx case internalServerError
        guard let moviesIndex = req.parameters.get("moviesId") else { throw Abort(.internalServerError) }
        return "\(moviesIndex)"
    }   
}

import Vapor

struct Movie: Content {
    let title: String
    let year: Int
}

```

## Middleware

[Vapor.codes](https://docs.vapor.codes/advanced/middleware/?h=middle)

Middleware is a logic chain between the client and a Vapor route handler. It allows you to perform operations on incoming requests before they get to the route handler and on outgoing responses before they go to the client.

### Log Middleware

```swift
func routes(_ app: Application) throws {
    app.middleware.use(LogMiddleware())
}

import Vapor

struct LogMiddleware: AsyncMiddleware {
    func respond(to request: Vapor.Request, chainingTo next: Vapor.AsyncResponder) async throws -> Vapor.Response {
        print("LOG MIDDLEWARE")
        return try await next.respond(to: request)
    }
}
```

### Authentication Middleware

```swift
/**
curl --location 'http://127.0.0.1:8080/members' \
--header 'Authorization: Bearer 123'
*/

import Vapor

struct AuthenticationMiddleware: AsyncMiddleware {
    func respond(to request: Vapor.Request, chainingTo next: Vapor.AsyncResponder) async throws -> Vapor.Response {
        
        // Headers: Authorization: Bearer 
        guard let auth = request.headers.bearerAuthorization else {
            throw Abort(.unauthorized)
        }
        print(auth.token)
        return try await next.respond(to: request)
    }
}

app.grouped(AuthenticationMiddleware()).group("members") { route in
    route.get { req async -> String in
        return "Members index"
    }
    route.get("hello") { req async -> String in
        return "Members Hello"
    }
}
```

## Setting Fluent ORM

ORM: Object Relational Mapping

[PostgreSQL as a Service](https://www.elephantsql.com) Perfectly configured and optimized PostgreSQL databases ready in 2 minutes.

## Migration

```swift
// configure.swift
app.databases.use(.postgres(hostname: "", username: "", password: "", database: ""), as: .psql)
app.migrations.add(CreateMoviesTableMigration())
```

```sh
vapor run migrate
```
