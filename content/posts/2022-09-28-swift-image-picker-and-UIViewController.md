---
categories: study
date: "2022-09-28T16:37:00Z"
title: 'Swift: Image Picker UIKit in the SwiftUI'
---

<!-- markdownlint-disable -->
<html>
<head>
<!-- Primary Meta Tags -->
<title>Swift: Image Picker UIKit in the SwiftUI</title>
<meta name="title" content="Swift: Image Picker UIKit in the SwiftUI">
<meta name="description" content="Swift: Image Picker UIKit in the SwiftUI">

<!-- Open Graph / Facebook -->
<meta property="og:type" content="website">
<meta property="og:url" content="https://yongfrank.github.io/blog/study/2022/09/28/swift-image-picker-and-UIViewController">
<meta property="og:title" content="Swift: Image Picker UIKit in the SwiftUI">
<meta property="og:description" content="Swift: Image Picker UIKit in the SwiftUI">
<meta property="og:image" content="https://raw.githubusercontent.com/yongfrank/blog/main/metadata_img/2022-09-28-swift-image-picker.png">

<!-- Twitter -->
<meta property="twitter:card" content="summary_large_image">
<meta property="twitter:url" content="https://yongfrank.github.io/blog/study/2022/09/28/swift-image-picker-and-UIViewController">
<meta property="twitter:title" content="Swift: Image Picker UIKit in the SwiftUI">
<meta property="twitter:description" content="Swift: Image Picker UIKit in the SwiftUI">
<meta property="twitter:image" content="https://raw.githubusercontent.com/yongfrank/blog/main/metadata_img/2022-09-28-swift-image-picker.png">
</head>
</html>

<!--
 * @Author: Frank Chu
 * @Date: 2022-09-28 16:25:17
 * @LastEditors: Frank Chu
 * @LastEditTime: 2022-09-29 12:15:07
 * @FilePath: /blog/_posts/2022-09-28-swift-image-picker-and-UIViewController.md
 * @Description: 
 * 
 * Copyright (c) 2022 by Frank Chu, All Rights Reserved. 
-->

[Wrapping a UIViewController in a SwiftUI view](https://www.hackingwithswift.com/books/ios-swiftui/wrapping-a-uiviewcontroller-in-a-swiftui-view)

[Using coordinators to manage SwiftUI view controllers](https://www.hackingwithswift.com/books/ios-swiftui/using-coordinators-to-manage-swiftui-view-controllers)

```swift
import SwiftUI
import PhotosUI

struct ContentView: View {
    
    /// This particular struct is designed to show an image,
    /// so we need an optional **Image** view to hold the selected image,
    @State private var imageToBeDisplayedOnTheScreen: Image?
    
    /// plus some state that determines whether the sheet is showing or not.
    @State private var showingImagePicker = false
    
    /// To integrate our ImagePicker view into that we need to start
    /// by add another @State image property that can be passed into the picker:
    @State private var inputImageFromPhotosLibrary: UIImage?
    
    var body: some View {
        VStack {
            imageToBeDisplayedOnTheScreen?
                .resizable()
                .scaledToFit()
            
            Button("Select Image") {
                showingImagePicker = true
            }
        }
        .sheet(isPresented: $showingImagePicker) {
            /// Our `ImagePicker` struct is a valid SwiftUI view,
            /// which means we can now show it in a sheet 
            /// just like any other SwiftUI view.
            ///
            /// - Parameters:
            /// - image: pass the property into image picker. 
            /// It will be updated when the image is selected
            ImagePicker(image: $inputImageFromPhotosLibrary)
        }
        /// It will be updated when the image is selected in `ImagePicker(image:)`
        .onChange(of: inputImageFromPhotosLibrary) { _ in
            loadImage()
        }
    }
    
    func loadImage() {
        guard let imageSelected = inputImageFromPhotosLibrary else { return }
        imageToBeDisplayedOnTheScreen = Image(uiImage: imageSelected)
    }
}

struct ContentView_Previews: PreviewProvider {
    static var previews: some View {
        ContentView()
    }
}


/// Wrapping a UIKit view controller requires us to create a struct
/// that conforms to the `UIViewControllerRepresentable` protocol.
///
/// That protocol builds on View, which means the struct 
/// we’re defining can be used inside a SwiftUI view hierarchy,
/// however we don’t provide a body property
/// because the view’s body is the view controller itself –
/// it just shows whatever UIKit sends back.
struct ImagePicker: UIViewControllerRepresentable {

    @Binding var imageSelectedInImagePickerStruct: UIImage?
    
    /// - Parameters:
    /// - image: pass the property into image picker. It will be updated when the image is selected
    init(image: Binding<UIImage?>) {
        
        //  See Extended Reading Materials
        //  When initializing a binding variable,
        //  the syntax has changed from a $ to an underscore.
        //  https://developer.apple.com/forums/thread/120034
        self._imageSelectedInImagePickerStruct = image
    }
    
    /// These methods have really precise signatures,
    /// so I’m going to show you a neat shortcut.
    /// The reason the methods are long is because SwiftUI needs to know
    /// what type of view controller our struct is wrapping,
    /// so if we just straight up tell Swift that type Xcode 
    /// will help us do the rest.
    ///
    /// That isn’t enough code to compile correctly,
    /// but when Xcode shows an error saying “Type ImagePicker does not conform to
    /// protocol `UIViewControllerRepresentable`”,
    /// please click the red and white circle to the left of the error 
    /// and select “Fix”.
    /// This will make Xcode write the two methods we actually need,
    /// and in fact those methods are actually enough for Swift to figure out
    /// the view controller type so you can delete the typealias line.
    typealias UIViewControllerType = PHPickerViewController
    
    /// Conforming to UIViewControllerRepresentable does require us to
    /// fill in that struct with two methods:
    /// one called `makeUIViewController()`,
    /// which is responsible for creating the initial view controller
    ///
    /// The `makeUIViewController()` method is important,
    /// so please replace its existing “code” line with this:
    func makeUIViewController(context: Context) -> PHPickerViewController {
        
        /// It creates a new photo picker configuration,
        /// asking it to provide us only images,
        var config = PHPickerConfiguration()
        config.filter = .images
        
        /// then uses that config to create and return a `PHPickerViewController`
        /// that does the actual work of selecting an image.
        let picker = PHPickerViewController(configuration: config)
        
        /// The next step is to tell the `PHPickerViewController`
        /// that when something happens it should tell our coordinator.
        /// That code won’t compile,
        /// but before we fix it I want to spend just a moment 
        /// digging in to what just happened.
        ///
        /// The reason our code doesn't compile is that Swift is checking
        /// that our coordinator class is *capable* of acting as
        /// a delegate for ** PHPickerViewController **, finding that it isn't,
        /// and so is refusing to build our code any further.
        /// To fix this we need to modify our ** Coordinator ** class.
        picker.delegate = context.coordinator

        return picker
    }
    
    /// and another called `updateUIViewController()`,
    /// which is designed to let us update the view controller when some SwiftUI state changes.
    ///
    /// We aren’t going to be using` updateUIViewController()`,
    /// so you can just delete the “code” line from there so that the method is empty.
    func updateUIViewController(_ uiViewController: PHPickerViewController, context: Context) {
        
    }
    
    /// However, we hit a problem: although we could show the image picker,
    /// we weren’t able to respond to the user selecting an image or pressing cancel.
    /// To make that happens requires a wholly new concept: coordinators.
    /// First, add this nested class inside the ImagePicker struct
    ///
    /// class inherit from `NSObject`
    /// the photo picker can say things like
    /// “hey, the user selected an image, what do you want to do?
    ///
    /// `PHPickerViewControllerDelegate`
    /// adds functionality for detecting when the user selects an image.
    /// (NSObject lets Objective-C check for the functionality;
    /// this protocol is what actually provides it.)
    class Coordinator: NSObject, PHPickerViewControllerDelegate {
        
        /// Rather than just pass the data down one level,
        /// a better idea is to tell the coordinator what its parent is,
        /// so it can modify values there directly.
        /// That means adding an ImagePicker property
        var parentInClassOfCoordinator: ImagePicker
        
        /// and associated initializer to the Coordinator class.
        /// And now we can modify `makeCoordinator()`
        init(parent: ImagePicker) {
            self.parentInClassOfCoordinator = parent
        }
        
        /// The `picker` method receives two objects we care about:
        /// the picker view controller that the user was interacting with,
        /// plus an array of the users selections because it's possible
        /// to let the user select mutiple images at once.
        func picker(_ picker: PHPickerViewController, didFinishPicking results: [PHPickerResult]) {
            
            /// It's our job to do threee things:
            /// 1. Tell the Picker to dismiss itself
            picker.dismiss(animated: true)
            
            /// 2. Exit if the user made no selection - if they tapped Cancel.
            guard let provider = results.first?.itemProvider else { return }
            
            /// 3. If this has an image we can use, use it
            /// and if so place it into the parent.image
            /// `func canLoadObject(ofClass aClass: NSItemProviderReading.Type) -> Bool`
            if provider.canLoadObject(ofClass: UIImage.self) {
                // loadObject(ofClass aClass: NSItemProviderReading.Type, completionHandler: @escaping @Sendable (NSItemProviderReading?, Error?) -> Void) -> Progress
                provider.loadObject(ofClass: UIImage.self) { image, _ in
                    /// Notice how we need the typecast for UIImage -
                    /// that's because the data we're provided could in theory be anything.
                    self.parentInClassOfCoordinator.imageSelectedInImagePickerStruct = image as? UIImage
                }
            }
                
        }
        
    }
    
    /// SwiftUI won’t automatically use it for the view’s coordinator.
    /// Instead, we need to add a new method called `makeCoordinator()`,
    /// which SwiftUI will automatically call if we implement it.
    /// All this needs to do is create and configure 
    /// an instance of our Coordinator class,
    /// then send it back.
    ///
    /// Right now our Coordinator class doesn’t do anything special,
    /// so we can just send one back by adding this method to the ImagePicker struct:
    ///
    /// and now we just told ImagePicker that
    /// it should have a coordinator to handle communication
    /// from that `PHPickerViewController`.
    func makeCoordinator() -> Coordinator {
        /// And now we can modify `makeCoordinator()`
        /// so that it passes the ImagePicker struct into the coordinator, like this:
        Coordinator(parent: self)
    }
}
```

## Extended Reading Materials

[How do you pass SwiftUI Bindings to other objects in iOS 13 Beta 4?](https://developer.apple.com/forums/thread/120034)

[SwiftUI: How to implement a custom init with @Binding variables](https://developer.apple.com/forums/thread/120034)

[Use UIKit View in the SwiftUI](https://www.fatbobman.com/posts/uikitInSwiftUI/)