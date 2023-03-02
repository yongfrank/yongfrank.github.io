---
categories: study
date: "2022-09-23T09:50:00Z"
title: Swift Network URLSession
---

<!-- markdownlint-disable -->
<html>
<head>
<!-- Primary Meta Tags -->
<title>Swift Network URLSession</title>
<meta name="title" content="Swift Network URLSession">
<meta name="description" content="The URLSession class and related classes provide an API for downloading data from and uploading data to endpoints indicated by URLs.">

<!-- Open Graph / Facebook -->
<meta property="og:type" content="website">
<meta property="og:url" content="https://yongfrank.github.io/blog/study/2022/09/23/swift-network">
<meta property="og:title" content="Swift Network URLSession">
<meta property="og:description" content="The URLSession class and related classes provide an API for downloading data from and uploading data to endpoints indicated by URLs.">
<meta property="og:image" content="https://raw.githubusercontent.com/yongfrank/blog/main/metadata_img/2022-09-23-swift-network.png">

<!-- Twitter -->
<meta property="twitter:card" content="summary_large_image">
<meta property="twitter:url" content="https://yongfrank.github.io/blog/study/2022/09/23/swift-network">
<meta property="twitter:title" content="Swift Network URLSession">
<meta property="twitter:description" content="The URLSession class and related classes provide an API for downloading data from and uploading data to endpoints indicated by URLs.">
<meta property="twitter:image" content="https://raw.githubusercontent.com/yongfrank/blog/main/metadata_img/2022-09-23-swift-network.png">
</head>
</html>

<!--
 * @Author: Frank Chu
 * @Date: 2022-09-23 08:23:03
 * @LastEditors: Frank Chu
 * @LastEditTime: 2022-09-26 09:44:49
 * @FilePath: /blog/_posts/2022-09-23-swift-network.md
 * @Description: 
 * 
 * Copyright (c) 2022 by Frank Chu, All Rights Reserved. 
-->

<!-- The URLSession class and related classes provide an API for downloading data from and uploading data to endpoints indicated by URLs. Your app can also use this API to perform background downloads when your app isn’t running or, in iOS, while your app is suspended. You can use the related URLSessionDelegate and URLSessionTaskDelegate to support authentication and receive events like redirection and task completion. -->

[Sending and receiving Codable data with URLSession and SwiftUI](https://www.hackingwithswift.com/books/ios-swiftui/sending-and-receiving-codable-data-with-urlsession-and-swiftui)

[Loading an image from a remote server](https://www.hackingwithswift.com/books/ios-swiftui/loading-an-image-from-a-remote-server)

```swift
import SwiftUI

struct InternetNetwork: View {
    
    @StateObject var vm = ViewModel()
    
    var body: some View {
        
        if #available(iOS 16.0, *) {
            NavigationStack {
                contentOfMusic
            }
        } else {
            NavigationView {
                contentOfMusic
            }
        }
    }
    
    var contentOfMusic: some View {
        Form {
            TextField("Enter singer", text: $vm.inputForSearching)
                .textFieldStyle(.roundedBorder)
            Text(vm.url)
            
            //  We want that to be run as soon as our List is shown,
            //  but we can’t just use onAppear() here
            //  because that doesn’t know how to handle sleeping functions –
            //  it expects its function to be synchronous.
            //
            //  task() - This can call functions
            //  that might go to sleep for a while;
            //  all Swift asks us to do is mark those functions
            //  with a second keyword, await,
            //  so we’re explicitly acknowledging that a sleep might happen.
            List(vm.results, id: \.trackId) { result in
                HStack {
                    
                    // Music Info Display
                    VStack(alignment: .leading) {
                        Text(result.trackName)
                            .font(.headline)
                        Text(result.collectionName)
                    }
                    
                    Spacer()
                    
                    // Image View
                    AsyncImage(url: URL(string: result.artworkUrl100)) { image in
                        image
                    } placeholder: {
                        Color.gray
                    }
                    .frame(width: 100, height: 100)
                }
            }
            .onChange(of: vm.inputForSearching) { _ in
                Task {
                    //  It's like do try which is made for error catching.
                    await vm.loadData(from: vm.url)
                }
            }
        }
    }
}

struct InternetNetwork_Previews: PreviewProvider {
    static var previews: some View {
        InternetNetwork()
    }
}

//  [SwiftUI] Publishing changes from background threads is not allowed;
//  make sure to publish values from the main thread
//  (via operators like receive(on:)) on model updates.

//  see Extended Reading Materials
//  SOLVED: Expression requiring global actor 'MainActor' cannot appear in default-value expression of property '_vm'; this is an error in Swift 6
//  https://www.hackingwithswift.com/forums/swiftui/expression-requiring-global-actor-mainactor-cannot-appear-in-default-value-expression-of-property-vm-this-is-an-error-in-swift-6/13695
@MainActor
class ViewModel: ObservableObject {
    @Published var results: [Result] = [Result]()
    @Published var inputForSearching: String = "" {
        
        //  See Entended Reading Materials
        //  How to take action when a property changes
        //  https://www.hackingwithswift.com/quick-start/beginners/how-to-take-action-when-a-property-changes
        didSet {
            self.url = self.searchLinkGenerator(for: self.inputForSearching)
        }
    }
    @Published var url: String = ""
    
    static var shared = ViewModel()
    
    //  Rather than forcing our entire progress to stop
    //  while the networking happens,
    //  Swift gives us the ability to say
    //  “this work will take some time,
    //  so please wait for it to complete
    //  while the rest of the app carries on running as usual.”
    //
    //  Notice the new async keyword in there –
    //  we’re telling Swift this function might want to go to sleep
    //  in order to complete its work.
    func loadData(from url: String) async {
        //  1. Creating the URL we want to read.
        //      This needs to have a precise format:
        //      “itunes.apple.com” followed by a series of parameters
        //  you can find the full set of parameters
        //  if you do a web search for “iTunes Search API”.
        //  In our case we’ll be using the search term
        //  “Taylor Swift” and the entity “song”.
        //  https://itunes.apple.com/search?term=taylor+swift&entity=song
        guard let url = URL(string: url) else {
            print("ERROR: guard let url error")
            return
        }
        
        //  2. Fetching the data for that URL.
        //      Just as importantly, an error might also be thrown here
        //      – maybe the user isn’t currently connected to the internet.
        do {
            //  public func data(from url: URL, delegate: URLSessionTaskDelegate? = nil) async throws -> (Data, URLResponse)
            let (data, _) = try await URLSession.shared.data(from: url)
            //  3. Decoding the result of that data into a Response struct.
            if let decodedResponse = try? JSONDecoder().decode(Response.self, from: data) {
                results = decodedResponse.results
            } else {
                print("ERROR: decode fail")
            }
        } catch {
            print("Invalid data")
        }
        
    }
    
    func searchLinkGenerator(for name: String) -> String {
        //  https://itunes.apple.com/search?term=taylor+swift&entity=song
        let linkBefore = "https://itunes.apple.com/search?term="
        let linkAfter = "&entity=song"
        
        return linkBefore + name.replacingOccurrences(of: " ", with: "+") + linkAfter
    }
}

struct Response: Codable {
    var results: [Result]
}

//  To sum that up, a hashable is a type that has hashValue
//  in the form of an integer that can be compared across different types.
struct Result: Hashable, Codable {
    var trackId: Int
    var collectionName: String
    var trackName: String
    var artworkUrl100: String
}
```

## Extended Reading Materials

[Hashable Protocols in Swift](https://medium.com/@JoyceMatos/hashable-protocols-in-swift-baf0cabeaebd)

[SOLVED: Expression requiring global actor 'MainActor' cannot appear in default-value expression of property '_vm'; this is an error in Swift 6](https://www.hackingwithswift.com/forums/swiftui/expression-requiring-global-actor-mainactor-cannot-appear-in-default-value-expression-of-property-vm-this-is-an-error-in-swift-6/13695)

[URLSession.shared.data(from: url) fails in Swift Playgrounds for Mac](https://developer.apple.com/forums/thread/708856)

[How to take action when a property changes](https://www.hackingwithswift.com/quick-start/beginners/how-to-take-action-when-a-property-changes)