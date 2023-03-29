---
title: "UIKit Intro in OC"
date: 2023-03-27T13:31:46+08:00
---

## QuickStart

* [Create a UIButton in Code with Objective-C](https://supereasyapps.com/blog/2014/8/4/create-a-uibutton-in-code-with-objective-c)

### Button

```objc
// in file ViewController.m
- (void)viewDidLoad {
    [super viewDidLoad];

    UIButton *button = [UIButton buttonWithType:
									UIButtonTypeSystem];
    [button setTitle:@"Press Me" forState:UIControlStateNormal];
    [button sizeToFit];

	// Set a new (x,y) point for the button's center
    button.center = CGPointMake(320 / 2, 60);

	// Add an action in current code file (i.e. target)
    [button addTarget:self 
			action:@selector(buttonPressed:）
			forControlEvents:UIControlEventTouchUpInside];

    [self.view addSubview:button];
} 

- (void)buttonPressed:(UIButton *)button {
     NSLog(@"Button Pressed");
}
```

### Protocol & UICollectionView

* [[OC] UIcollectionView and UIcollectionViewCell](https://www.cnblogs.com/OranBlog/p/9208782.html)

```objc
// ViewController.h
@interface ViewController : UIViewController <UICollectionViewDelegate, UICollectionViewDataSource>
@end
```

```objc
@interface ViewController ()
@property (nonatomic, strong) UICollectionView *collectionView;
@end
```

```objc
@implementation ViewController
- (void)viewDidLoad {
	[super viewDidLoad];
	[self loadCollectioView];
}

- (void)loadCollectioView {
    // Layout Init
    // let layout = UICollectionViewFlowLayout()
    UICollectionViewFlowLayout *flowlayout = [[UICollectionViewFlowLayout alloc] init];
    
    // self.collectionView = UICollectionView(frame: .zero, collectionViewLayout: layout)
    _collectionView = [[UICollectionView alloc]initWithFrame:self.view.frame collectionViewLayout:flowlayout];
    
    // Cell Size
    flowlayout.itemSize = CGSizeMake(280, 280);
    
    // head Size
    flowlayout.headerReferenceSize = CGSizeMake(self.view.frame.size.width, 40);
    
    // Cell vertical spacing
    flowlayout.minimumLineSpacing = 10;
    
    // Cell horizontal spacing
    flowlayout.minimumInteritemSpacing = 20;
    
    // Cell to container edge
    flowlayout.sectionInset = UIEdgeInsetsMake(20, 20, 20, 20); // Up left down right
    
    // collectionView init
    
    // Register Cell
    // collectionView.register(UICollectionViewCell.self, forCellWithReuseIdentifier: "cell")
    [_collectionView registerClass:[UICollectionViewCell class] forCellWithReuseIdentifier:@"cell"];
    
    // Set delegate
    // collectionView.dataSource = self
    // collectionView.delegate = self
    _collectionView.dataSource = self;
    _collectionView.delegate = self;
    
    // Background color
    _collectionView.backgroundColor = [UIColor greenColor];
    
    // Felxible Width & Height
    _collectionView.autoresizingMask = UIViewAutoresizingFlexibleWidth | UIViewAutoresizingFlexibleHeight;
    
    // Add View
    [self.view addSubview:_collectionView];
    
    // Size settings
    [_collectionView setFrame:self.view.frame];
}

- (__kindof UICollectionViewCell *)collectionView:(UICollectionView *)collectionView cellForItemAtIndexPath:(NSIndexPath *)indexPath {
    
    // Cell Reuse, identifier is same as the registration(@"cell")
    // let cell = collectionView.dequeueReusableCell(withReuseIdentifier: "cell", for: indexPath)
    UICollectionViewCell *cell = [collectionView dequeueReusableCellWithReuseIdentifier:@"cell" forIndexPath:indexPath];
    if (!cell) {
        cell = [[UICollectionViewCell alloc] init];
//        [cell.contentView setBackgroundColor:[UIColor systemBlueColor]];
    }
    // cell.contentView.backgroundColor = .systemBlue
    [cell.contentView setBackgroundColor:[UIColor systemBlueColor]];
    return cell;
}

- (NSInteger)collectionView:(UICollectionView *)collectionView numberOfItemsInSection:(NSInteger)section {
    return 120;
}
@end
```

## Loading Method of AppDelegate, SceneDelegate and Controller AppDelegate、SceneDelegate、控制器的加载方式

* [AppDelegate, SceneDelegate, Controller](https://blog.csdn.net/weixin_46926959/article/details/120076013)

> iOS13 之前，AppDelegate的职责是：全权处理App生命周期和UI生命周期
>
> Before iOS13, the AppDelegate is responsible for handling the entire App lifecycle and the UI lifecycle.
>
> iOS 13 之后，AppDelegate的职责是：处理App生命周期和SceneDelegate 生命周期
>
> After iOS 13, the AppDelegate is responsible for handling the App lifecycle and the SceneDelegate lifecycle.
>

### StartUp Process 启动过程

main函数 -> 自动释放池 -> UIApplicationMain（永不返回，保证程序不会被销毁）-> 创建应用程序对象UIApplication ->创建应用程序的代理对象AppDelegate -> IOS13之前，将AppDelegate的window实例化，设置为keyWindow主窗口 -> 加载配置文件指定的storyboard

main function -> autorelease pool -> UIApplicationMain (never returns, to ensure that the program will not be destroyed) -> create the application object UIApplication -> create the application delegate object AppDelegate -> IOS13 before, instantiate the AppDelegate's window, set it as the keyWindow main window -> load the storyboard specified in the configuration file

## Target for Project in Xcode

* [How to Properly Remove Main.Storyboard (for iOS 13+)](https://ioscoachfrank.com/remove-main-storyboard.html)

```xml
<!-- Info.plist -->
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
	<key>UIApplicationSceneManifest</key>
	<dict>
		<key>UIApplicationSupportsMultipleScenes</key>
		<false/>
		<key>UISceneConfigurations</key>
		<dict>
			<key>UIWindowSceneSessionRoleApplication</key>
			<array>
				<dict>
					<key>UISceneConfigurationName</key>
					<string>Default Configuration</string>
					<key>UISceneDelegateClassName</key>
					<string>SceneDelegate</string>
				</dict>
			</array>
		</dict>
	</dict>
</dict>
</plist>

<key>Storyboard Name</key>
```

## Delegate

* [Delegate Pattern In Swift](https://medium.com/@armanabkar/delegate-pattern-in-ios-a26f2b329097)
* [The Holy Grail of UIKit: Delegate Pattern](https://tarikdahic.com/posts/the-holy-grail-of-uikit-delegate-pattern/)
* [UIKit in SwiftUI](https://zhuanlan.zhihu.com/p/402897951)

## SwiftUI In UIKit(Objective-C)

* [Using SwiftUI in Objective-C](https://www.jianshu.com/p/549facb59caf)

```swift
import SwiftUI
struct MainViewInterface: View {
     var body: some View {
          Text("Hello, World!")
   }
}

@objc
class MainView: NSObject {
    @objc func makeMainView() -> UIViewController {
        return UIHostingController(rootView:  MainViewInterface())
    }
}
```

```objc
// #import "<projectName>-Swift.h"
// Lookup in Targets - Build Settings - Search for Swift - Objective-C Generated Interface Header Name
#import "testSwift-Swift.h" 

@implementation ViewController

- (void)viewDidLoad {
	[super viewDidLoad];

	UIViewController *vc = [[MainView new] makeMainView];
	vc.view.frame = CGRectMake(100, 400, 200, 100);
	[self.view addSubview:vc.view];
}

@end
```