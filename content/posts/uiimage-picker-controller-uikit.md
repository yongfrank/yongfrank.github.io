---
title: "UIImagePickerController in UIKit"
date: 2023-03-03T11:43:07+08:00
---

## Keyword

* UIImagePickerController
* NSObject

## UIImagePickerController & PHPickerViewController

> Reference: [JianShu](https://www.jianshu.com/p/5e7aacfa4374)
>
> 如何在 iOS 14 中使用新推出的 PhotoKit 框架中的 PHPickerViewController 类，并通过 Objective C 和Swift 语言从照片库中选择照片。
>
> 多年来，在 iOS 上选择照片和视频的最简单方法是使用 `UIImagePickerController` 类。该类允许你呈现一个内置的系统 UI 来选择照片或视频，并将其返回到你的应用程序中，而无需构建选择照片的 UI 页面或访问照片库的提示。
>
> 然而，`UIImagePickerController` 也有很多缺点：它相当基础，而且呈现给用户浏览照片库的 UI 也非常有限；一次只能选择一个（图片或者视频），而且只支持基本的过滤功能。在 iOS 14 中，`UIImagePickerController` 被 "软废弃 "了。虽然目前还没有被标记为废弃，但如果你看一下头文件，就会发现 API 标记有这个
>
> `API_TO_BE_DEPRECATED`
>
> iOS 14 中新的 `PHPicker` 类不是在 `UIKit` 框架中的，而是位于 `PhotosUI` 框架中

## Navigation Button & Function for selection

```swift
// inside viewDidLoad
self.navigationItem.leftBarButtonItem = UIBarButton(barButtonSystemItem: .add, target: self, action: #selector(addNewPerson))

// inside class like ViewController
@objc func addNewPerson() {
    // @MainActor class UIImagePickerController : UINavigationController
    let picker = UIImagePickerController()
    picker.allowsEditing = true
    picker.delegate = self
    // Presents a view controller modally. 
    // extension UIViewController {}
    self.present(picker, animated: true)
}

// let controller conform to these Delegate
// second: telling us when the user either selected a picture or cancelled the picker
// third: really is quite pointless here
class ViewController: UICollectionViewController, UIImagePickerControllerDelegate, UINavigationControllerDelegate
```

## UIImagePickerControllerDelegate

* Extract the image from the dictionary that is passed as a parameter.
* Generate a unique filename for it.
* Convert it to a JPEG, then write that JPEG to disk.
* Dismiss the view controller.

```swift
// @protocol UIImagePickerControllerDelegate<NSObject>
func imagePickerController(_ picker: UIImagePickerController, didFinishPickingMediaWithInfo info: [UIImagePickerController.InfoKey : Any]) {
    guard let image = info[.editedImage] as? UIImage else { return }
    let imageName: String = UUID().uuidString
    let imagePath: URL = getDocumentsDirectory().appendingPathComponent(imageName)
    
    if let jpegData: Data = image.jpegData(compressionQuality: 0.8) {
        try? jpegData.write(to: imagePath)
    }
    let person = Person(name: "Unknown", image: imageName)
    people.append(person)
    collectionView.reloadData()
    // @MainActor open class UIViewController {}
    self.dismiss(animated: true)
}

func getDocumentsDirectory() -> URL {
    // default: The shared file manager object for the process.
    // User home directories (/Users).
    // The user’s home directory—the place to install user’s personal items (~).
    let path = FileManager.default.urls(for: .userDirectory, in: .userDomainMask)
    return path[0]
}
```

### FileManager

```swift
// default: The shared file manager object for the process.
// Document directory.
// userDomainMask: The user’s home directory—the place to install user’s personal items (~).
FileManager.default.urls(for: .documentDirectory, in: .userDomainMask)[0].appendingPathComponent("123.jpg")
$R3: Foundation.URL = "file:///Users/yongfrank/Documents/123.jpg"
```

## NSObject

## Reference

[HackingWithSwift](https://www.hackingwithswift.com/100/43)
