---
title: "Objc Snippet"
date: 2023-03-29T21:18:40+08:00
---

## Type unsafe

* [isEqual](https://developer.apple.com/documentation/objectivec/1418956-nsobject/1418795-isequal)
* [isEqualTo](https://developer.apple.com/documentation/objectivec/nsobject/1393823-isequalto)
* [OC底层原理之isEqual](https://juejin.cn/post/7127912661714468872)
* [isEqual](https://existorlive.github.io/2020/05/26/isEqual-AND-hash-of-NSObject.html)

```objc
NSLog(@"%d", false == 0);
NSLog(@"%d", nil == 0);
NSLog(@"%d", 0 == 0);
NSLog(@"%d", [false isEqual:0]);

if (nil == 0) {
    NSLog(@"equal");
}
```

## Class in Objective-C

* [Apple Developer: Programming With Objective-C](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/ProgrammingWithObjectiveC/Introduction/Introduction.html)
* [Apple Developer: Start Developing Mac Apps Today](https://developer.apple.com/library/archive/referencelibrary/GettingStarted/RoadMapOSX/chapters/01_Introduction.html)

### [QuickStart](https://www.tutorialspoint.com/objective_c/objective_c_quick_guide.htm)

```objc
@interface Person : NSObject

@property (nonatomic, strong) NSString *name;
@property (nonatomic, assign) NSInteger age;
@property (nonatomic, assign) int birthYear;

- (instancetype)initWithName:(NSString *)name age:(NSInteger)age;
- (void)sayHello;

@end

@implementation Person
@synthesize birthYear;

- (instancetype)initWithName:(NSString *)name age:(NSInteger)age {
    self = [super init];
    if (self) {
        _name = name;
        _age = age;
    }
    return self;
}

- (void)sayHello {
    NSLog(@"Hello, my name is %@ and I am %ld years old.", self.name, (long)self.age);
}

@end

int main(int argc, const char * argv[]) {
    @autoreleasepool {
        // insert code here...
        NSLog(@"Hello, World!");
        Person *person = [[Person alloc] initWithName:@"John Doe" age:25];
        person.birthYear = 2022;
        [person sayHello];
        NSLog(@"%d", person.birthYear);
    }
    return 0;
}
```

### @synthesize

@synthesize height;

@synthesize is an Objective-C directive used to automatically generate getter and setter methods for a class property.

Before the release of Objective-C 2.0, developers had to write both the getter and setter methods themselves, which could be quite verbose and repetitive. However, with @synthesize, the compiler can generate these methods automatically based on the declared property, greatly simplifying the code.

For example, suppose you have a property height declared in a class Person. Instead of writing the getter and setter methods yourself, you can simply write:

```objc
@implementation Person
@synthesize height;
@end
```

This will automatically generate the getter and setter methods for the height property.

Note that with the release of Objective-C 2.0, @synthesize is no longer strictly necessary. Instead, you can use the @property directive to declare the property and the compiler will automatically generate the getter and setter methods by default. However, @synthesize can still be useful in some cases, such as when you need to customize the behavior of the generated methods.

## Property in objc

```objc
@interface MyObject : NSObject {
    int memberVar1; // 显式地声明实例变量
    id  memberVar2;
}

@interface HotDiscussionModel : NSObject

@property (nonatomic, assign) NSString *guid;
@property (nonatomic, assign) NSString *picUrl;
@property (nonatomic, assign) NSString *hotLink;

@end
```

这两段代码的区别在于定义实例变量的方式不同。

第一段代码在 @interface 语句中声明了实例变量 memberVar1 和 memberVar2。这种方式是早期 Objective-C 的写法，在现代 Objective-C 中不太常见了。

而第二段代码则使用了现代 Objective-C 的属性声明方式，使用 @property 来定义属性，不需要显式地声明实例变量。编译器会自动为属性生成对应的实例变量 _guid、_picUrl 和 _hotLink。

需要注意的是，在第二段代码中，属性的 assign 关键字表示该属性的 setter 方法会直接将传入的参数赋值给实例变量，不会进行内存管理。而 retain 和 copy 则会自动处理内存管理，retain 表示 setter 方法会对传入的对象进行引用计数 +1 的操作，而 copy 表示 setter 方法会对传入的对象进行一份拷贝，然后进行引用计数的操作。通常情况下，对于对象类型的属性，我们应该使用 copy 或 strong 来进行内存管理，而对于基本数据类型的属性，我们可以使用 assign。

## Property 修饰符

> [iOS 中定义属性时的atomic、nonatomic、copy、assign、strong、weak几个特性的区别](https://www.jianshu.com/p/4fb51144d4ca)

### atomic

* 默认属性。
* 当前进程进行到一半，其他线程来访问当前线程，可以保证先执行完毕当前线程。
* 只是保证setter/getter 完整，不是线程安全。

### nonatomic

* 非默认属性。
* 两个线程同时访问同一个属性将会导致无法预计的结果。
* 优点是程序运行速度快。

### copy

* 是owner，不是reference（引用）。当对象可变时，可设置为copy，用于获取此时值的副本。
* 新的对象引用计数为1，与原始对象引用计数无关，且原始对象引用计数不会改变。
* 使用copy创建的新对象也是强引用，使用完成后需要负责释放该对象。
* copy特性可以减少对象对上下文的依赖。新对象、原始对象中任一对象的值改变不会影响另一对象的值。
* 要想设置该对象的特性为copy，该对象必须遵守NSCopying协议，Foundation类默认实现了NSCopying协议，所以只需要为自定义的类实现该协议即可。更全面了解NSCopying协议，查看 深复制、浅复制、copy、mutableCopy 一文。

### assign

* 与copy相反，只是reference，不是owner。只返回指针。
* 用于float、int、BOOL等类型。
* 释放后再发送消息会导致程序崩溃。

### strong

* 默认属性
* strong = retain iOS引入ARC后，用strong替代了retain。
* 创建一个强引用的指针，引用对象引用计数加1。
* strong特性表示两个对象内存地址相同（建立一个指针，进行指针拷贝），内容会一直保持相同，直到更改一方内存地址，或将其设置为nil。
* 如果有多个对象同时引用一个属性，任一对象对该属性的修改都会影响其他对象获取的值。
* 所有实例变量、局部变量默认都是strong。

### weak

* 只是reference，不是owner。即引用计数不会加1。
* 可将weak对象设为nil，向nil发送消息，什么都不会执行，程序也不会崩溃。
* 代理使用weak。delegate几乎一直own代理对象，所以代理对象应该对代理使用weak，否则会形成循环引用（retain cycle）。但也有例外，如果代理对象的生命周期比代理短，代理对象也可以使用strong。
* IBOutlet常用weak。

> 关于strong和weak对比的一个形象例子：
>
> 假设对象是一条小狗，小狗想跑走（be deallocated)。
>
> strong类型就像是拴狗的绳子，只要有一条绳子栓住狗，它就不能跑走，如果有五条绳子拴着同一条狗（五个strong类型指向同一个对象），只有当五条绳子都释放狗才可以跑走。
>
> weak类型就像是小孩子看着小狗说：看这里有小狗。只要还有绳子拴着小狗，小孩子们就可以继续指着小狗说：看这里有小狗。当绳子释放了的时候，不管有多少小孩子依旧在指着小狗说：看这里的小狗。小狗都会跑掉。
>
> 当最后一个strong指针不再指向这个对象，这个对象就会被释放，此时，所有指向这个对象的weak指针都将被清空。

## 初始化 [Initialization](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/ProgrammingWithObjectiveC/WorkingwithObjects/WorkingwithObjects.html#//apple_ref/doc/uid/TP40011210-CH4-SW1)

Use new to Create an Object If No Arguments Are Needed for Initialization
It’s also possible to create an instance of a class using the new class method. This method is provided by NSObject and doesn’t need to be overridden in your own subclasses.

It’s effectively the same as calling alloc and init with no arguments:

```objc
XYZObject *object = [XYZObject new];
// is effectively the same as:
XYZObject *object = [[XYZObject alloc] init];
```

## Lazy instantiation & 下划线变量

> [Objective-C 懒加载没有调用？](https://www.jianshu.com/p/986adc6fe3ca)

总结：self.对属性的get和set方法间接调用，下划线_是直接对实例变量操作。

> 前面已经说到，在iOS 5之后，使用`@property`定义一个属性后，系统会默认生成`getter`和`setter`方法。我们申明了一个属性，但并不是立即就要使用这个对象，没必要把所有的属性都放在`viewDidLoad`方法中初始化，等到要使用时再加载(初始化)。

（1）、当开发者使用懒加载本质就是重写了 `getter()` 方法；

（2）、懒加载在加载时必须判空；

（3）、懒加载判空必须使用下划线，如下面代码，Xcode 100% 会报错的，前面已经说明，self 就是调用了 setter 跟 getter 方法，懒加载本质就是重写了 getter 方法，但在此处属性本身还没初始化,是 nil, 但是 getter 返回的也是 nil, 那在判断时就会进入死循环；

```objc
//错误的懒加载示范一
-(NSArray*)PageddatafromServerList {   
    if(self.PageddatafromServerList == nil)   {     
        self.PageddatafromServerList = [NSArray array];    
    }  
    return self.PageddatafromServerList;
}

//错误的懒加载示范二
-(NSArray*)PageddatafromServerList {   
    if(self.PageddatafromServerList) //此处一定要判空   
    {     
        self.PageddatafromServerList = [NSArray array];    
    }  
    return self.PageddatafromServerList;
}

//正确的懒加载方式
- (NSArray *)DepartmentArray {   
    if(_DepartmentArray == nil) {      
        _DepartmentArray = [NSArray array];   
    }
    return _DepartmentArray;
}
```

![懒加载问题代码](https://upload-images.jianshu.io/upload_images/1214383-1d112281de77c800.png?imageMogr2/auto-orient/strip|imageView2/2/w/651/format/webp)

> 分析：最上面贴出的代码出现的原因：
>
> 由于这个写这个代码的哥们使用了懒加载，而懒加载本质上是重写了属性的 `getter` 方法，本文第二条也说明了 `self.` 和 `_` 的区别，所以在赋值时使用 `_dataArray`，就没有调 `dataArray` 的 `getter` 方法，懒加载根本就没有调！！！所以出现的情况就是给 `dataArray` 赋值后依旧是 `nil`！
>
> 直接就举个🌰来说明吧：我买了个超级省电的台灯，回家后我给台灯通上电后发现怎么折腾这个台灯都不亮，为什么呢？因为我没有摁台灯的开关！！

```objc
// 那么懒加载的正确打开方式是怎样的呢？请看代码：
@interface EOCClass : NSObject
@property (strong, nonatomic) NSArray *LazyLoading;
-(void)LazyLoadingData;
@end

-(void)LazyLoadingData {
    NSLog(@"_LazyLoading============%@",_LazyLoading);    
    NSLog(@"self.LazyLoading=============%@",self.LazyLoading);    
    NSLog(@"=============%@",[self LazyLoading]);
}

-(NSArray*)LazyLoading {   
    if(!_LazyLoading) {     
        _LazyLoading= [NSArray array];   
    }  
    return _LazyLoading;
}
```

![懒加载图片](https://upload-images.jianshu.io/upload_images/1214383-624cf0347e44f040.png?imageMogr2/auto-orient/strip|imageView2/2/w/593/format/webp)

> 那么，通过上面的代码，可以看出，使用了懒加载后，当要使用这个对象时可以用`self.` 调用该对象或者直接调用这个对象的 `getter` 方法，如果用下划线调用实例变量那么懒加载就没有调用，最终造成的结果就是赋值了也是 `nil`;

### Another Example on [iOS面试宝典](https://heron-newland.github.io/assets/knowledges/index.html)

> [什么是懒加载](https://heron-newland.github.io/assets/knowledges/%E4%BB%80%E4%B9%88%E6%98%AF%E6%87%92%E5%8A%A0%E8%BD%BD.html)
>
> 懒汉模式，只在用到的时候才去初始化，也可以理解成延时加载,一般懒加载写在getter方法中。我觉得最好也最简单的一个列子就是 `tableView` 中图片的加载显示了。一个延时载，避免内存过高，一个异步加载，避免线程堵塞。

### Swift & Objc Lazy

[objective-c和swift中懒加载的区别](https://blog.csdn.net/I_HOPE_SOAR/article/details/121306121)

oc中懒加载的写法就是，如果一个变量为空，则进行一定操作，否则return原来的值

而swift中只用写一个lazy标识符,swift中的懒加载只有第一次调用此变量时才执行闭包中的内容，不论是否为空，后面都不会执行了。

所以两者的区别就在于，如果调用了一次oc中的懒加载，再把变量设置为空，第二次还是会进入懒加载。

而swift语言中，调用了一次懒加载，再把变量设置为空，第二次就不会进入懒加载了。

```swift
//swift
import Foundation

class Test {
    var name: String
    lazy var greeting: String? = {
        return "hello"
    }()
    
    lazy var eatting: String = {
        return "rice"
    }()
    
    init(name: String) {
        self.name = name
    }
}

var test = Test(name: "init")

let a = test.greeting//第一次调用
print(a!)//输出hello
test.greeting = nil//设置为空
let b = test.greeting//第二次调用
print(b)//输出nil
```

```objc
//objectivec
#import <Foundation/Foundation.h>

@interface Test : NSObject

@property (nonatomic) NSString *name;

@end

@implementation Test

//lazy load
- (NSString *)name {
    if(!_name) {
        _name = @"hello";
    }
    return _name;
}

- (void)printt {
    NSLog(@"%@",_name);
}

@end

int main(int argc, const char * argv[]) {
    @autoreleasepool {
        Test *test = [[Test alloc] init];
        NSLog(@"%@",test.name);//第一次调用，输出hello
        test.name = nil;
        NSLog(@"%@",test.name);//第二次调用，输出hello两次进入懒加载
    }
    return 0;
}
```

### See more on

* [Objective-C学习之懒加载（延迟加载）](https://blog.csdn.net/yxys01/article/details/51489938)
* [Lazy instantiation in Objective-C/ iPhone development](https://stackoverflow.com/questions/11769562/lazy-instantiation-in-objective-c-iphone-development)

## Struct in Objective-C

* [OC中定义了一个结构体(struct)，设置结构体的值](https://blog.csdn.net/srn214/article/details/47788231)

```objc
// main.m 文件

#import <Foundation/Foundation.h>
#import "Dog.h"
#import "Person.h"
#import "Student.h"

int main(int argc, const char * argv[]) {

    @autoreleasepool {
        Person *p1 = [[Person alloc] init];

        // 给对象中的结构体(struct)赋值
        // 如果直接用c语言的方式直接赋值就会报错，如s->birthday={1990,12,11};就会抛出错误
        // 声明并初始化一个struct类型临时变量，再整个赋值给birthday成员。

        Date d = {2015, 12, 22};
        p1.birthday = d;
    }
    return 0;
}


// Person.h 文件

#import <Foundation/Foundation.h>
// 生日
typedef struct {
    int year; // 年
    int month; // 月
    int day; // 日
} Date;

// 性别
typedef enum {
    XingBieMan,
    XingBieWoman
} XingBie;

@interface Person : NSObject

// 姓名的setter和getter
@property (nonatomic, retain) NSString *name;

// 生日的setter和getter
@property (nonatomic, assign) Date birthday;

// 年龄的setter和getter
@property (nonatomic, assign) int age;

// 性别
@property (nonatomic, assign) XingBie sex;

// 身高
@property (nonatomic, assign) double height;

// 体重
@property (nonatomic, assign) double weight;

@end
```

## Turn JSON File into Model Object

```json
{
    "name": "John Doe",
    "age": 30
}
```

```objc
// Person.h 文件内容：
#import <Foundation/Foundation.h>

@interface Person : NSObject

@property (nonatomic, strong) NSString *name;
@property (nonatomic, assign) NSInteger age;

- (instancetype)initWithDictionary:(NSDictionary *)dictionary;

@end
```

```objc
// Person.m 文件内容：
#import "Person.h"

@implementation Person

- (instancetype)initWithDictionary:(NSDictionary *)dictionary {
    self = [super init];
    if (self) {
        _name = dictionary[@"name"];
        _age = [dictionary[@"age"] integerValue];
    }
    return self;
}

@end
```

```objc
// main.m 文件内容：
#import <Foundation/Foundation.h>
#import "Person.h"

int main(int argc, const char * argv[]) {
    @autoreleasepool {
        // 1. 读取 JSON 文件
        // If you use Xcode CLI
        NSString *filePath = @"/Users/frank/Example/CLITesting/CLITesting/person.json";
        // If you use Xcode App
        // NSString *filePath = [[NSBundle mainBundle] pathForResource:@"data" ofType:@"json"];
        NSData *jsonData = [NSData dataWithContentsOfFile:filePath];

//        NSLog(@"%@", jsonData);
        // 2. 使用 NSJSONSerialization 将 JSON 数据转换为字典
        NSError *error;
        NSDictionary *jsonDictionary = [NSJSONSerialization
                                        JSONObjectWithData:jsonData
                                        options:kNilOptions
                                        error:&error];

        // 3. 判断是否转换成功
        if (error) {
            NSLog(@"Error converting JSON to NSDictionary: %@", error.localizedDescription);
        } else {
            // 4. 使用字典初始化 Person 对象
            Person *person = [[Person alloc] initWithDictionary:jsonDictionary];
            NSLog(@"Person name: %@, age: %ld", person.name, (long)person.age);
        }
    }
    return 0;
}
```

### Array of JSON Objects

```json
[
    {
        "name": "John Doe",
        "age": 30
    },
    {
        "name": "Frank Doe",
        "age": 330
    },
]
```

```objc
// 1. 读取 JSON 文件
NSString *filePath = @"/Users/frank/Example/CLITesting/CLITesting/person.json";
NSData *jsonData = [NSData dataWithContentsOfFile:filePath];

//        NSLog(@"%@", jsonData);
// 2. 使用 NSJSONSerialization 将 JSON 数据转换为字典
NSError *error;
NSDictionary *jsonDictionary = [NSJSONSerialization
                                JSONObjectWithData:jsonData
                                options:kNilOptions
                                error:&error];

// 3. 判断是否转换成功
if (error) {
    NSLog(@"Error converting JSON to NSDictionary: %@", error.localizedDescription);
} else {
    // 4. 使用字典初始化 Person 对象
//            Person *person = [[Person alloc] initWithDictionary:jsonDictionary];
//            NSLog(@"Person name: %@, age: %ld", person.name, (long)person.age);
    NSMutableArray *personArray = [NSMutableArray array];
    NSLog(@"%@", jsonDictionary);
    for (NSDictionary *dict in jsonDictionary) {
        Person *person = [[Person alloc] init];
        person.age = [dict[@"age"] integerValue];
        person.name = dict[@"name"];
        [personArray addObject:person];

    }
    NSLog(@"%@", personArray);
}
```

### Another Example

```objc
//
//  main.m
//  CLITesting
//
//  Created by hb24155 on 3/30/23.
//

#import <Foundation/Foundation.h>

@interface HotDetail : NSObject

@property (nonatomic, strong) NSString *contentId;
@property (nonatomic, strong) NSArray<NSString *> *headImageList;
@property (nonatomic, strong) NSString *pvCount;
@property (nonatomic, strong) NSString *iconUrl;
@property (nonatomic, strong) NSString *title;
@property (nonatomic, strong) NSString *jumpUrl;

- (instancetype)initWithDictionary:(NSDictionary *)dictionary;

@end

@implementation HotDetail

- (instancetype)initWithDictionary:(NSDictionary *)dictionary {
    self = [super init];
    if (self) {
        _contentId = dictionary[@"contentId"];
        _headImageList = dictionary[@"headImageList"];
        _pvCount = dictionary[@"pvCount"];
        _iconUrl = dictionary[@"iconUrl"];
        _title = dictionary[@"title"];
        _jumpUrl = dictionary[@"jumpUrl"];
    }
    return self;
}

@end

@interface HotDiscussion : NSObject

@property (nonatomic, strong) NSString *guid;
@property (nonatomic, strong) NSString *picUrl;
@property (nonatomic, strong) NSString *hotLink;
@property (nonatomic, strong) NSArray<HotDetail *> *hotDetailList;

- (instancetype)initWithDictionary:(NSDictionary *)dictionary;

@end

@implementation HotDiscussion

- (instancetype)initWithDictionary:(NSDictionary *)dictionary {
    self = [super init];
    if (self) {
        _guid = dictionary[@"guid"];
        _picUrl = dictionary[@"picUrl"];
        _hotLink = dictionary[@"hotLink"];
        
        NSMutableArray *details = [NSMutableArray array];
        for (NSDictionary *detailDict in dictionary[@"hotDetailList"]) {
            HotDetail *detail = [[HotDetail alloc] initWithDictionary:detailDict];
            [details addObject:detail];
        }
        _hotDetailList = [details copy];
    }
    return self;
}

@end



int main(int argc, const char * argv[]) {
    @autoreleasepool {
        // insert code here...
        NSLog(@"Hello, World!");
        NSString *jsonString = @"{\"hotDiscussion\": {\"guid\": \"do mollit pro\",\"picUrl\": \"adipisicing deserunt ea ex\",\"hotLink\": \"occaecat\",\"hotDetailList\": [{\"contentId\": \"quis mollit sit\",\"headImageList\": [\"labore deserunt sint\",\"cup\",\"aliqua sint magna\"],\"pvCount\": \"laboris id adipisicing\",\"iconUrl\": \"ut non\",\"title\": \"aute exercitation nisi tempor\",\"jumpUrl\": \"ut ut eu cupidatat\"},{\"contentId\": \"nostrud exercitation quis cupidatat eiusmod\",\"headImageList\": [\"incididunt sunt\",\"exercitation enim officia aute\"],\"pvCount\": \"labore\",\"iconUrl\": \"sed tempor magna nulla\",\"title\": \"in amet dolor minim\",\"jumpUrl\": \"amet\"}]}}";

        NSData *jsonData = [jsonString dataUsingEncoding:NSUTF8StringEncoding];
        NSError *error;
        NSDictionary *jsonDict = [NSJSONSerialization JSONObjectWithData:jsonData options:NSJSONReadingMutableContainers error:&error];
        NSLog(@"%@", jsonDict);
    }
    return 0;
}

/*
 "hotDiscussion": {
     "guid": "do mollit pro",
     "picUrl": "adipisicing deserunt ea ex",
     "hotLink": "occaecat",
     "hotDetailList": [
         {
             "contentId": "quis mollit sit",
             "headImageList": [
                 "labore deserunt sint",
                 "cup",
                 "aliqua sint magna"
             ],
             "pvCount": "laboris id adipisicing",
             "iconUrl": "ut non",
             "title": "aute exercitation nisi tempor",
             "jumpUrl": "ut ut eu cupidatat"
         },
         {
             "contentId": "nostrud exercitation quis cupidatat eiusmod",
             "headImageList": [
                 "incididunt sunt",
                 "exercitation enim officia aute"
             ],
             "pvCount": "labore",
             "iconUrl": "sed tempor magna nulla",
             "title": "in amet dolor minim",
             "jumpUrl": "amet"
         },
     ]
 }
 */
```

## ==, isEqual, isEqualTo

> [Difference between isEqualTo: and isEqual:](https://stackoverflow.com/questions/7096691/difference-between-isequalto-and-isequal)

isEqual: is part of the NSObject protocol and is meant for comparing objects.

isEqual: 方法是 NSObject 类的一个方法，它用于比较两个对象的内容是否相等。具体来说，它比较两个对象的地址是否相同，如果相同，则返回 YES；否则，它将比较两个对象的属性值、实例变量、方法等是否相同，如果相同，则返回 YES；否则，返回 NO。

isEqualTo: is part of the Cocoa AppleScript support infrastructure (specifically, NSComparisonMethods, which allow AppleScript to compare Cocoa objects). It's normally the same as isEqual:, but can be overridden if you want equality to work differently internally and in a script.

isEqualTo: 方法是 Cocoa AppleScript 支持基础设施的一部分（具体来说，它是 NSComparisonMethods 的一部分，它允许 AppleScript 比较 Cocoa 对象）。它通常与 isEqual: 相同，但如果你想在内部和脚本中使用不同的相等性，则可以覆盖它。

> [isEqual ，isEqualToString ， == 三者的区别](https://www.jianshu.com/p/1d05bf21d8f9)

isEqual: 首先判断两个对象是否类型一致， 在判断具体内容是否一致，如果类型不同直接return no.如先判断是否都是 NSString，在判断string的内容。

isEqualToString: 这个直接判断字符串内容。

==是直接比较指向的地址。

```objc
- (BOOL)isEqual:(id)other {  
    if (other == self) { 
        return YES;
    }

    if (!other || ![other isKindOfClass:[self class]]) {
        return NO;
    }

    return [self isEqualToWidget:other]; 
}

- (BOOL)isEqualToWidget:(MyWidget *)aWidget {    
    if (self == aWidget) return YES;    
    if (![(id)[self name] isEqual:[aWidget name]]) return NO;    
    if (![[self data] isEqualToData:[aWidget data]]) return NO;    
    return YES;
}
```

* 首先都会判断 指针是否相等，相等直接返回YES，
* 不相等再判断是否是同类对象或非空，空或非同类对象直接返回NO，
* 而后依次判断对象对应的属性是否相等，若均相等，返回YES
* 这样就不难理解 isEqualToString 的实现内部的了

> [iOS开发 之 不要告诉我你真的懂isEqual与hash!](https://www.jianshu.com/p/915356e280fc)

为什么要有isEqual方法?
isEqual方法的作用大家肯定是知道的:

> 判断两个对象是否相等

但是判断相等不是已经有==运算符了么, 为什么还要isEqual方法?

这是因为:

> 对于基本类型, ==运算符比较的是值; 对于对象类型, ==运算符比较的是对象的地址(即是否为同一对象)

注意: 上述==运算符的说明适用于Objective-C和Java等不支持运算符重载的语言, 支持运算符重载的语言有C++

所以要理清==运算符和isEqual方法的区别, 问题就集中在

什么叫比较对象的地址, 什么叫比较对象

```objc
UIColor *color1 = [UIColor colorWithRed:0.5 green:0.5 blue:0.5 alpha:1.0];
UIColor *color2 = [UIColor colorWithRed:0.5 green:0.5 blue:0.5 alpha:1.0];
NSLog(@"color1 == color2 = %@", color1 == color2 ? @"YES" : @"NO");
NSLog(@"[color1 isEqual:color2] = %@", [color1 isEqual:color2] ? @"YES" : @"NO");
// color1 == color2 = NO
// [color1 isEqual:color2] = YES
```

从上面的例子可以看出, ==运算符只是简单地判断是否是同一个对象, 而isEqual方法可以判断对象是否相同, 例如UIColor对象表示的color是否相同

### ==, ===, isEqual: Objc & Swift

> [onevcat: 判等](https://swifter.tips/equal/)

我们在 Objective-C 时代，通常使用 -isEqualToString: 来在已经能确定比较对象和待比较对象都是 NSString 的时候进行字符串判等。Swift 中的 String 类型中是没有 -isEqualToString: 或者 -isEqual: 这样的方法的，因为这些毕竟是 NSObject 的东西。在 Swift 的字符串内容判等，我们简单地使用 == 操作符来进行

在判等上 Swift 的行为和 Objective-C 有着巨大的差别。在 Objective-C 中 == 这个符号的意思是判断两个对象是否指向同一块内存地址。其实很多时候这并不是我们经常所期望的判等，我们更关心的往往还是对象的内容相同，而这种意义的相等即使两个对象引用的不是同一块内存地址时，也是可以做到的。Objective-C 中我们通常通过对 -isEqual: 进行重写，或者更进一步去实现类似 -isEqualToString: 这样的 -isEqualToClass: 的带有类型信息的方法来进行内容判等。如果我们没有在任意子类重写 -isEqual: 的话，在调用这个方法时会直接使用 NSObject 中的版本，去直接进行 Objective-C 的 == 判断。

在 Swift 中情况大不一样，Swift 里的 == 是一个操作符的声明，在 Equatable 里声明了这个操作符的接口方法：

```swift
protocol Equatable {
    func ==(lhs: Self, rhs: Self) -> Bool
}
```

对于原来 Objective-C 中使用 == 进行的对象指针的判定，在 Swift 中提供的是另一个操作符 ===。在 Swift 中 === 只有一种重载：

`func ===(lhs: AnyObject?, rhs: AnyObject?) -> Bool`

它用来判断两个 AnyObject 是否是同一个引用。

对于判等，和它紧密相关的一个话题就是哈希，因为哈希是一个稍微复杂的话题，所以我将它分成了一个单节。但是如果在实际项目中你需要重载 == 或者重写 -isEqual: 来进行判等的话，很可能你也会想看看有关哈希的内容，重载了判等的话，我们还需要提供一个可靠的哈希算法使得判等的对象在字典中作为 key 时不会发生奇怪的事情。

## Category

* [深入理解Objective-C：Category](https://tech.meituan.com/2015/03/03/diveintocategory.html)
* [【iOS面试粮食】OC语言—Category(分类)和类扩展(extension)、关联对象](https://juejin.cn/post/6844903968691191815)

## iOS Constant - `static NSString * const kStr = @""`

> [iOS 不要用宏来定义你的常量](https://toutiao.io/posts/kw76e7/preview)

iOS 不要用宏来定义你的常量

最近在工程里看到很多不规范的使用，于是来写一篇博客来让不是很清楚的小朋友们，少埋点坑。

首先，预处理命令他不是一个常量！！！！

我们来看一段代码

```objc
#define avatar @"60"
if (false) {
#define avatar @"80"
}
NSLog(avatar);
```

### 首先，预处理命令他不是一个常量！！！！

这段代码会输出多少，我们将“avatar”定义为了60，然后在一个永远不会执行的代码里面重新定义了“avatar”为80，if语句中的代码永远不会执行，但是在编译时期，编译器会编译这段代码，而这个时候编译器就会将avatar这个名字替换为@“80”，所以这段代码最后的输出结果就是80。

当然这个时候编译器是会有一个警告的，但是不知道有多少同学会忽略这个警告。或者你会告诉我你对警告十分敏感，不会放过他的，但是记住你不是一个人在写代码，可能在别人的页面他给你重新定义了你的define，给你挖了一个大坑，还找不着………

### 用const来定义一个常量

const修饰符定义的变量是不可变的，比如说你需要定义一个动画时间的常量，你可以这么做：

```objc
static const NSTimeInterval kAnimateDuration = 0.3;
```

当你试图去修改“ kAnimateDuration”的值的时候，编译器会报错。更加重要的是用这种方法定义的常量是带有类型信息的，而这点则是define不具备的。

也许你已经发现了，如果你像这样定义：

```objc
static const NSString * kUserName = @"StrongX";
```

你是可以修改userName的值的，（说好的常量呢～～～）

首先我们需要确定的是以下两种写法是一样的：

```objc
static NSString const * kUserName = @"StrongX";
static const NSString * kUserName = @"StrongX";
```

也就是说const放在类型前还是类型后是一样的效果。然后不同效果的是下面这种写法：

```objc
static NSString * const kUserName = @"StrongX";
```

const 修饰的是他右边的部分，也就是说：

```objc
static NSString const * kUserName = static NSString const (* kUserName )

static NSString * const kUserName = static NSString * const (kUserName)
```

当const修饰的是(userName)的时候，不可变的是userName;星号在C语言中表示指针指向符，也就是说这个时候userName指向的内存块地址不可变，而内存保存的内容是可变的，我们来做个尝试：

```objc
NSLog(@"内存地址： %x",& kUserName);
kUserName = @"superXLX";
NSLog(@"内存地址： %x",& kUserName);
```

以上NSLog会打印*userName指向的内存块地址，而他的输出是：

我们已经发现当我们改变内存的内存的时候他的地址并没有发生改变，也就是说这是符合“const”修饰符的规定的。
而当我们的修饰符是这样的时候：

```objc
static NSString * const kUserName = @"StrongX";
```

我们则无法改变userName的值。

所以当我们需要定义一个不可变的常量的时候 ，我们还是需要将“const”修饰符放到“*”指针指向符后边才对。

### 一定要同时使用static和const来定义你的变量

上面已经说了const是用来定义一个常量。而static在C语言中（OC中延用）则表明此变量只在改变量的输出文件中可用(.m文件)，如果你不加“static”符号，那么编译器就会对该变量创建一个“外部符号”，后果是什么呢？

你可以尝试在不同编译文件中加入以下代码：

```objc
NSString * const kUserName = @"StrongX";
```

可能尽管文件之间并没有相互引用，不存在属性名重复的问题（因为这并不是一个属性，这是一个外部符号）,但是编译器还是报错了:

他会告诉你在两个目标文件(.0文件是.m文件编译后的输出文件)有一个重复的符号。(OC中没有类似C++中的名字空间的概念)

所以当你在你自己的.m文件中需要声明一个只有你自己可见的局部变量(k开头)的变量的时候一定要同时使用“static”和“const”两个符号。

### 定义工程中的全局变量

在我们的工程中一定会定义很多全局常量，很多人的做法是会创建一个“ constant.h”文件，在这个文件中用#define声明许多常量，然后将这个头文件引入“pch”文件中，不能说这么做不对，但是如同上面说的那样define可能被修改，当然在命名规范的情况下这种情况很少出现，并且这样做的效率很高。

然而苹果更推荐另外一种做法:”extern”，这样做的优势是保持常量绝对不会被修改，并且一定初始化还带有类型信息。

我们在”constants.h”文件中，声明常量：

```objc
extern NSString *const XUserName;
```

然后在“constants.m”中定义他：

```objc
NSString *const XUserName = @"StrongX";
```

用“extern”定义的常量必须也只能初始化一次，不满足必须以及只能一次的条件那么编译器就会提醒你。在定义全局变量的时候需要要注意你的命名，你可以使用规定好的前缀来命名。

“define”和“extern”各有各的优势，不过我个人还是比较推荐使用“extern”.(因为之前在一个工程中被define坑惨了——！)。

> [what's the different between const NSString and static NSString](https://stackoverflow.com/questions/12815189/whats-the-different-between-const-nsstring-and-static-nsstring)

I want to use class variable. the following two approaches work well, but I don't know what's the different between them.

```objc
static NSString *str1 = @"str1";
NSString *const str2 = @"str2";
```

you can change the location to where str1 is pointing to but cannot do the same for str2 as it is a const pointer

```objc
// this will work :
str1 = @"Hello";

// while this won't:
str2 = @"Hello"; 
```

## @weakify 和 @strongify

@weakify 是一个宏，用于创建一个指向自身的弱引用。在这种情况下，它用于避免闭包中的循环引用。循环引用会导致内存泄漏，因为对象不能正确释放。@weakify 和 @strongify 通常在 Objective-C 中与 ReactiveCocoa 和 ReactiveObjC 库一起使用。

在闭包中，当你需要引用 self 时，通常需要考虑到循环引用的问题。使用 @weakify(self)，您可以在闭包内部创建一个弱引用的自身。然后，您需要使用 @strongify(self) 宏将弱引用转换回强引用，以便在闭包内部安全地使用 self。

以下是一个完整的示例，说明了如何在 Objective-C 中使用 @weakify 和 @strongify：

```objc
#import <ReactiveObjC/ReactiveObjC.h>
#import "YourClass.h"

@implementation YourClass

- (instancetype)init {
    self = [super init];
    if (self) {
        [self setup];
    }
    return self;
}

- (void)setup {
    @weakify(self)
    [[RACObserve(self, someProperty) skip:1] subscribeNext:^(id x) {
        @strongify(self)
        // 在这里安全地使用 self，而不会导致循环引用
        [self doSomething];
    }];
}

- (void)doSomething {
    // ...
}

@end
```

在此示例中，我们使用 RACObserve 监听名为 someProperty 的属性的变化。当属性发生变化时，我们的闭包会被调用。在闭包中，我们使用 @strongify(self) 将弱引用转换回强引用，以便安全地使用 self。这种方法避免了循环引用问题。

## Type Size

> Values of type 'NSUInteger' should not be used as format arguments; add an explicit cast to 'unsigned long' instead
>
> Replace '%d", ' with '%lu", (unsigned long)'

NSUInteger 是 Objective-C 中的一个无符号整数类型，用于表示对象的数量或容器中的元素数量。在使用 NSUInteger 作为格式化字符串的参数时，您需要使用 %lu 格式化符号来指定它的类型。这是因为在 64 位架构下，NSUInteger 的大小为 8 个字节，而 %d 只能处理 4 个字节的有符号整数。

以下是一个示例，展示了如何使用 %lu 格式化符号来输出 NSUInteger 值：

```objc
NSUInteger count = 10;
NSLog(@"The count is: %lu", count);
```

在上面的示例中，我们首先创建了一个名为 count 的 NSUInteger 变量，其值为 10。然后，我们使用 NSLog 函数输出一个带有格式化字符串的消息，并使用 %lu 格式化符号将 count 变量的值输出为一个无符号长整型。这将确保在 64 位架构下正确处理 NSUInteger 类型的值。

请注意，在 32 位架构下，NSUInteger 的大小为 4 个字节，因此您可以使用 %u 格式化符号来指定它的类型。

## Ternary Operator

```objc
NSString *content = searchContent ?: kDefaultSearchText;

// equation
NSString *content;
if (searchContent) {
    content = searchContent;
} else {
    content = kDefaultSearchText;
}
```

This is also called the null coalescing operator, and it is used to simplify conditional assignment. If searchContent is not nil, then it is assigned to content, otherwise kDefaultSearchText is assigned to content.

这个语法也被称为 null coalescing operator，用于简化条件赋值操作，如果 searchContent 不为空，则将其赋值给 content，否则将 kDefaultSearchText 赋值给 content。
