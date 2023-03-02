---
categories: study
date: "2022-10-03T16:37:00Z"
title: Swift Codable, JSON, UserDefaults Explained
---

<!--
 * @Author: Frank Chu
 * @Date: 2022-10-03 15:16:00
 * @LastEditors: Frank Chu
 * @LastEditTime: 2022-10-04 10:43:25
 * @FilePath: /blog/_posts/2022-10-03-codable-explained.md
 * @Description: 
 * 
 * Copyright (c) 2022 by Frank Chu, All Rights Reserved. 
-->

## What's JSON

JSON is a file format to store key-value pair.

[It's JSON for Pikachu](https://pokeapi.co/api/v2/pokemon/pikachu)

## What's Codable in the Swift

**Codable** was introduced in **Swift 4.0**, bringing with it incredibly smooth conversion between Swift data types and JSON.

> The code comes from
>
> [Hacking With Swift by Paul Hudson](https://www.hackingwithswift.com/articles/119/codable-cheat-sheet)
>
> Codable cheat sheet
>
> Convert between JSON and Swift types the smart way

## Part I: Codable Part

### Encoding and decoding JSON

```swift
import Foundation
let json = """
[
    {
        "name":"Frank",
        "age": 20
    },
    {
        "name": "Paul",
        "age": 38
    }
]
"""

/// Convert json into a **Data** object because that's 
/// what Codable decoders work with.
let data = Data(json.utf8)

/// Define a Swift Struct that will hold our finished data
struct User: Codable {
    var name: String
    var age: Int
}

/// An object that decodes instances of 
/// a data type from JSON objects.
let decoder = JSONDecoder()

do {
    let decoded = try decoder.decode([User].self, from: data)
    
    /// This will print "Frank", which is name of the firstusr in the JSON
    print(decoded[0].name)
} catch {
    print("Failed to decode JSON")
}
```

### Converting case

Key is `first_name` in JSON, but we usually use `firstName` in Swift.

```swift
import Foundation

let json = """
[
    {
        "first_name": "Frank",
        "last_name": "Chu"
    },
    {
        "first_name": "Paul",
        "last_name": "Hudson"
    }
]
"""

let data = Data(json.utf8)

struct User: Codable {
    var firstName: String
    var lastName: String
}

/// To make this work we need to change only one property in the JSON decoder
let decoder = JSONDecoder()

/// That instructs Swift to map snake case names (`names_written_like_this`) to camel case names (`namesWrittenLikeThis`)
decoder.keyDecodingStrategy = .convertFromSnakeCase

do {
    let decoded = try decoder.decode([User].self, from: data)
    print(decoded)
} catch {
    print("Decode error")
}
```

### Maping different key names

If you have JSON keys that are completely diffrent from your Swift properties, you can map them using a **CodingKeys** enum.

```swift
let json = """
[
    {
        "user_first_name": "Frank",
        "user_last_name": "Chu",
        "user_age": 26
    },
    {
        "user_first_name": "Paul",
        "user_last_name": "Hudson",
        "user_age": 26
    }
]
"""
let data = Data(json.utf8)

/// Those key names aren't great, and really we'd like to convert that data into a struct like this
struct User: Codable {
    var firstName: String
    var lastName: String
    var age: Int
    
    /// To make that happen, we need to declare a CodingKeys enum:
    /// a mapping that **Codable** can use to
    /// convert JSON names into properties for our struct.
    ///
    /// It needs to conform to the **CodingKey** protocol,
    /// which is what makes this work with **Codable** protocol.
    enum CodingKeys: String, CodingKey {
        case firstName = "user_first_name"
        case lastName = "user_last_name"
        case age = "user_age"
    }
}

let decoder = JSONDecoder()

do {
    let decoded = try decoder.decode([User].self, from: data)
    print(decoded)
} catch {
    print(error)
}
```

### Parsing hierarchical data the easy way

Any non-trival JSON is like to have hierarchical data - one collection of data nested inside another.

```swift
import Foundation

let json = """
[
    {
        "name": {
            "first_name": "Taylor",
            "last_name": "Swift"
        },
        "age": 26
    },
    {
        "name": {
            "first_name": "Frank",
            "last_name": "Chu"
        },
        "age": 20
    }
]
"""

let data = Data(json.utf8)

/// **Codable** is able to handle this just fine, as long as you can describle the relationships clearly.
/// This is the easy way to do this is using nested structs.
struct User: Codable {
    struct Name: Codable {
        var firstName: String
        var lastName: String
    }
    
    var name: Name
    var age: Int
}

let decoder = JSONDecoder()
decoder.keyDecodingStrategy = .convertFromSnakeCase

do {
    let decoded = try decoder.decode([User].self, from: data)
    
    /// The downside is that if you want to read a user's first name,
    /// you need to use `user.name.first`,
    /// but at least the actual parsing work is trivial
    /// - our existing code works already!
    if let user = decoded.last {
        print(user.name.firstName)
    }
} catch {
    print(error)
}
```

### Parsing hierarchical data the hard way

If you want to parse hierarchical data into a flat struct - i.e., you want to be able to write `user.firstName` rather than `user.name.firstName` - then you need to do some parsing yourself.

```swift
import Foundation

/// Second, we need to define coding keys
/// that describe where data can be found in the hierarchy.
let json = """
[
    {
        "name": {
            "first_name": "Taylor",
            "last_name": "Swift"
        },
        "age": 26
    },
    {
        "name": {
            "first_name": "Frank",
            "last_name": "Chu"
        },
        "age": 20
    }
]
"""

let data = Data(json.utf8)

/// First, create the struct you want to end up with
struct User: Codable {
    var firstName: String
    var lastName: String
    var age: Int
    
    /// As you can see, at the root there's a key
    /// called "name" and another called "age",
    /// "so" we need to add that as our root coding keys.
    enum CodingKeys: String, CodingKey {
        case name, age
    }
    
    /// Inside "name" were two more keys, `"first_name"` and `"last_name"`,
    /// so we're going to create some coding keys for those two.
    enum NameCodingKeys: String, CodingKey {
        case firstName, lastName
    }
    
    /// Now for the hard part: we need to write a custom initializer and
    /// custom encode method for our type.
    init(from decoder: Decoder) throws {
        /// Inside there, the first thing we need to do is
        /// attempt to pull out a container we can read
        /// using the keys of our **CodingKeys** enum
        let container = try decoder.container(keyedBy: CodingKeys.self)
        
        /// Once that's done we can attempt to read our age property.
        age = try container.decode(Int.self, forKey: .age)
        
        /// Next we need to dig down one level to read our name data.
        let name = try container.nestedContainer(keyedBy: NameCodingKeys.self, forKey: .name)
        
        firstName = try name.decode(String.self, forKey: .firstName)
        lastName = try name.decode(String.self, forKey: .lastName)
    }
    
    /// `encode(to:)` is effectively the reverse of the initializer we just write.
    func encode(to encoder: Encoder) throws {
        var containder = encoder.container(keyedBy: CodingKeys.self)
        try containder.encode(age, forKey: .age)
        
        var name = containder.nestedContainer(keyedBy: NameCodingKeys.self, forKey: .name)
        try name.encode(firstName, forKey: .firstName)
        try name.encode(lastName, forKey: .lastName)
    }
}

let decoder = JSONDecoder()
decoder.keyDecodingStrategy = .convertFromSnakeCase

do {
    let decoded = try decoder.decode([User].self, from: data)
    print(decoded)
} catch {
    print(error)
}
```

## Part II: UserDefaults Part

