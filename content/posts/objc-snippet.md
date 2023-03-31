---
title: "Objc Snippet"
date: 2023-03-29T21:18:40+08:00
---

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

## [Initialization](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/ProgrammingWithObjectiveC/WorkingwithObjects/WorkingwithObjects.html#//apple_ref/doc/uid/TP40011210-CH4-SW1)

Use new to Create an Object If No Arguments Are Needed for Initialization
It’s also possible to create an instance of a class using the new class method. This method is provided by NSObject and doesn’t need to be overridden in your own subclasses.

It’s effectively the same as calling alloc and init with no arguments:

```objc
XYZObject *object = [XYZObject new];
// is effectively the same as:
XYZObject *object = [[XYZObject alloc] init];
```

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