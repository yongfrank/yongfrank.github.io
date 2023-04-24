---
title: "UIKit Intro in Objective-C & Swift"
date: 2023-03-27T13:31:46+08:00
---

Everything about UIKit in Objective-C & Swift when I was coding. 

> [hws - UIKit Example Code](https://www.hackingwithswift.com/example-code/uikit/)

<!--more-->

## QuickStart

* [Create a UIButton in Code with Objective-C](https://supereasyapps.com/blog/2014/8/4/create-a-uibutton-in-code-with-objective-c)

## Debug Tools

> * [FLEX - Flipboard Explorer](https://github.com/FLEXTool/FLEX)
> * [FLEX 一个轻量级App多维分析工具](https://www.jianshu.com/p/1d8fc9206204)
> * [1.2 万 Star！一个 iOS 应用调试利器](https://zhuanlan.zhihu.com/p/434459667)

![Flex](https://user-images.githubusercontent.com/8371943/70185687-e842c800-16af-11ea-8ef9-9e071380a462.gif)

```swift
#if os(iOS)
import FLEX
#endif

#if DEBUG
//import FLEX
#endif

#if DEBUG
ToolbarItemGroup {
    Button("Flex") {
        FLEXManager.shared.showExplorer()
    }
//                darkModeButton
}
#endif
```

### Button
<!-- markdownlint-disable MD010 -->
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
* [[Swift] UICollectionView data source and delegates](https://xuebaonline.com/UICollectionView%20data%20source%20and%20delegates/)
* [纯代码创建UICollectionView步骤以及简单使用](https://www.jianshu.com/p/16c9d466f88c)

```objc
// ViewController.h
// <> Means Controller conform delegate inside <>
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

```swift
import UIKit

class ViewController: UIViewController {
    
    var collectionView: UICollectionView!

    override func viewDidLoad() {
        super.viewDidLoad()
        // Do any additional setup after loading the view, typically from a nib.
        self.view.backgroundColor = .green
        
        let flowLayout = UICollectionViewFlowLayout()
        self.collectionView = UICollectionView(frame: self.view.safeAreaLayoutGuide.layoutFrame, collectionViewLayout: flowLayout)
        
        flowLayout.scrollDirection = .horizontal
        flowLayout.itemSize = CGSizeMake(180, 180)
        
        collectionView.delegate = self
        collectionView.dataSource = self
        
        collectionView.register(ExampleCell.self, forCellWithReuseIdentifier: String(describing: type(of: ExampleCell.self)))
        
        self.view.addSubview(collectionView)
    }

    override func didReceiveMemoryWarning() {
        super.didReceiveMemoryWarning()
        // Dispose of any resources that can be recreated.
    }
}

extension ViewController: UICollectionViewDataSource {
    func collectionView(_ collectionView: UICollectionView, cellForItemAt indexPath: IndexPath) -> UICollectionViewCell {
        let cell = collectionView.dequeueReusableCell(withReuseIdentifier: String(describing: type(of: ExampleCell.self)), for: indexPath) as! ExampleCell
        cell.titleLabel.text = String(indexPath.row)
        return cell
    }
    
    func numberOfSections(in collectionView: UICollectionView) -> Int {
        1
    }
    
    func collectionView(_ collectionView: UICollectionView, numberOfItemsInSection section: Int) -> Int {
        10
    }
}

extension ViewController: UICollectionViewDelegate {
    func collectionView(_ collectionView: UICollectionView, didSelectItemAt indexPath: IndexPath) {
        print("Selected item at index: \(indexPath.row)")
    }
}

class ExampleCell: UICollectionViewCell {
    let titleLabel = UILabel()
    
    override init(frame: CGRect) {
        super.init(frame: frame)
        
        self.initUI()
    }
    
    required init?(coder: NSCoder) {
        fatalError("init(coder:) has not been implemented")
    }
    
    private func initUI() {
        self.backgroundColor = .red
        
        // Text for index
        titleLabel.frame = CGRect(x: 50, y: 50, width: 50, height: 20)
        self.addSubview(titleLabel)
    }
}
```

## No Storyboard

* [Day-11 使用UIKit框架建立iOS App專案(不使用storyboard)](https://ithelp.ithome.com.tw/m/articles/10298398)

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

## Lazy Instantiation

> See more in [objc-snippet](../objc-snippet#lazy-instantiation)

## Border

> [kingfisher - onevcat on twitter](https://twitter.com/onevcat/status/1600020125746311168?s=20)

```objc
@interface ViewController ()
@property (weak, nonatomic) IBOutlet UIImageView *imageView;
@property (weak, nonatomic) IBOutlet UIImageView *secondImgView;

@end

@implementation ViewController

- (void)viewDidLoad {
    [super viewDidLoad];
    // Do any additional setup after loading the view.
    self.imageView.layer.cornerRadius = self.imageView.frame.size.width / 2;
    self.imageView.layer.masksToBounds = true;
    self.imageView.layer.borderWidth = 2.0;
    self.imageView.layer.borderColor = [UIColor whiteColor].CGColor;
    self.secondImgView.layer.cornerRadius = self.imageView.frame.size.width / 2;
    self.secondImgView.layer.borderWidth = 2.0;
    self.secondImgView.layer.borderColor = [UIColor whiteColor].CGColor;
}

@end
```

```swift
import SwiftUI
struct ContentView: View {
    
    var body: some View {
        HStack {
            ForEach(0..<3, id: \.self) { i in
                Image("icon")
                    .resizable()
                    .frame(width: 100, height: 100)
                    .overlay {
                        Circle()
                            .stroke(.blue, lineWidth: 5)
                    }
                //.cornerRadius(50)
                    .offset(x: CGFloat(-i * 50), y: 0)
            }
        }
    }
    
}
```

在 SwiftUI 中，视图的修改器是一种声明式方法，用于描述视图的外观和行为。在这种情况下，为了实现与 UIKit 类似的效果，我们需要将边框绘制为一个圆形视图的叠加层。

SwiftUI 与 UIKit 的设计理念有很大差异。在 UIKit 中，我们直接操作视图的层（layer）来实现边框、圆角等效果。而在 SwiftUI 中，我们通过组合和修改视图来实现所需的效果。这种声明式的方式允许 SwiftUI 更好地优化渲染过程，并提供更易读、更易维护的代码。

在上面的 SwiftUI 示例中，我们使用了 overlay 修改器来添加一个描边圆形，从而实现了类似于 UIKit 中的边框效果。虽然这种方法与 UIKit 的操作方式不同，但它非常符合 SwiftUI 的设计原则，并能很好地满足需求。

## ResponsiveScaleWidth

```objc
#define ResponsiveScaleWidth(length) (((length)/375.0f) * [[UIScreen mainScreen] bounds].size.width)
```
