---
categories: study
date: "2022-09-22T15:50:00Z"
title: Swift QRCode and Image Processing
---

<!--
 * @Author: Frank Chu
 * @Date: 2022-09-22 15:49:57
 * @LastEditors: Frank Chu
 * @LastEditTime: 2022-09-22 18:02:05
 * @FilePath: /blog/_posts/2022-09-22-swift-qrcode-image-processing.md
 * @Description: 
 * 
 * Copyright (c) 2022 by Frank Chu, All Rights Reserved. 
-->

## QRCode Generator

[Generating and scaling up a QR code](https://www.hackingwithswift.com/books/ios-swiftui/generating-and-scaling-up-a-qr-code)

```swift
//  First, we need to bring in all the Core Image filters using a new import:
import SwiftUI
import CoreImage.CIFilterBuiltins

func generateQRCode(from string: String) -> UIImage {
    
    //  We need two properties to store an active Core Image context
    let context = CIContext()
    //  and an instance of Core Image’s QR code generator filter
    let filter = CIFilter.qrCodeGenerator()
    
    //  Working with Core Image filters requires us to provide some input data.
    //  Our input for the filter will be a string, 
    //  but the input for the filter is Data, 
    //  so we need to convert that.
    filter.message = Data(string.utf8)
    
    //  then convert the output CIImage into a CGImage, 
    //  then that CGImage into a UIImage.
    if let outputImage = filter.outputImage {
        // Return a rect the defines the bounds of non-(0,0,0,0) pixels
        // open var extent: CGRect { get }
        if let cgimg = context.createCGImage(outputImage, from: outputImage.extent) {
            return UIImage(cgImage: cgimg)
        }
    }
    
    //  If conversion fails for any reason 
    //  we’ll send back the “xmark.circle” image from SF Symbols.
    //  If that can’t be read – which is theoretically possible 
    //  because SF Symbols is stringly typed – then we’ll send back an empty UIImage.
    return UIImage(systemName: "xmark.circle") ?? UIImage()
}

struct ContentView: View {
    var body: some View {
        VStack {
            Image(uiImage: generateQRCode(from: "apple.com"))
            //  take a close look at the QR code – 
            //  do you notice how it’s fuzzy? 
            //  This is because Core Image is generating 
            //  a tiny image, and SwiftUI is trying to 
            //  smooth out the pixels as we scale it up.
            
            //  Line art like QR codes and bar codes is 
            //  a great candidate for disabling image interpolation. 
            //  Try adding and removing .interpolation(.none) modifier 
            //  to the image to see what I mean:
                .interpolation(.none)
                .resizable()
                .scaledToFit()
                .frame(width: 300, height: 300)
        }
    }
}

```

### Extended Reading Materials

[Controlling image interpolation in SwiftUI](https://www.hackingwithswift.com/books/ios-swiftui/controlling-image-interpolation-in-swiftui)

## Image Processing with Core Image and SwiftUI

[Integrating Core Image with SwiftUI](https://www.hackingwithswift.com/books/ios-swiftui/integrating-core-image-with-swiftui)

Apart from SwiftUI’s Image view, the three other image types are:

* UIImage, which comes from UIKit. This is an extremely powerful image type capable of working with a variety of image types, including bitmaps (like PNG), vectors (like SVG), and even sequences that form an animation. UIImage is the standard image type for UIKit, and of the three it’s closest to SwiftUI’s Image type.
* CGImage, which comes from Core Graphics. This is a simpler image type that is really just a two-dimensional array of pixels.
* CIImage, which comes from Core Image. This stores all the information required to produce an image but doesn’t actually turn that into pixels unless it’s asked to. Apple calls CIImage “an image recipe” rather than an actual image.

There is some interoperability between the various image types:

* We can create a UIImage from a CGImage, and create a CGImage from a UIImage.
* We can create a CIImage from a UIImage and from a CGImage, and can create a CGImage from a CIImage.
* We can create a SwiftUI Image from both a UIImage and a CGImage.

### Why we need these complex procedure?

> If we want to create images dynamically, apply Core Image filters, save them to the user’s photo library, and so on, then SwiftUI’s images aren’t up to the job.
>
> Apple gives us three other image types to work with, and cunningly we need to use all three if we want to work with Core Image.

What actually is an Image? As you know, it’s a view, which means it’s something we can position and size inside our SwiftUI view hierarchy. It also handles loading images from our asset catalog and SF Symbols, and it’s capable of loading from a handful of other sources too. However, ultimately it is something that gets displayed – we can’t write its contents to disk or otherwise transform them beyond applying a few simple SwiftUI filters.

If we want to use Core Image, SwiftUI’s Image view is a great end point, but it’s not useful to use elsewhere. That is, if we want to create images dynamically, apply Core Image filters, save them to the user’s photo library, and so on, then SwiftUI’s images aren’t up to the job.

Apple gives us three other image types to work with, and cunningly we need to use all three if we want to work with Core Image. They might sound similar, but there is some subtle distinction between them, and it’s important that you use them correctly if you want to get anything meaningful out of Core Image.
