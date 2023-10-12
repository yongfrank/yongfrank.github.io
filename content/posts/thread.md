---
title: "Thread"
date: 2023-04-08T14:58:09+08:00
---

## Thread Safety

> [确保线程安全的几种方法](https://developer.aliyun.com/article/254282?spm=a2c6h.13262185.profile.388.699e167e7REVuk)

## Swift 多线程方案

* Thread
* Cocoa Operation(Operation, Operation Queue)
* [Dispatch](https://developer.apple.com/documentation/dispatch): Grand Central Dispatch
* [Swift Concurrency](https://juejin.cn/post/7084640887250092062)

## Runloop

> [Runloop explained](https://hit-alibaba.github.io/interview/iOS/ObjC-Basic/Runloop.html)
>
> Runloop 是什么？Runloop 还是比较顾名思义的一个东西，说白了就是一种循环，只不过它这种循环比较高级。一般的 while 循环会导致 CPU 进入忙等待状态，而 Runloop 则是一种“闲”等待，这部分可以类比 Linux 下的 epoll。当没有事件时，Runloop 会进入休眠状态，有事件发生时， Runloop 会去找对应的 Handler 处理事件。Runloop 可以让线程在需要做事的时候忙起来，不需要的话就让线程休眠。
>
> 一个 Timer 一次只能加入到一个 RunLoop 中。我们日常使用的时候，通常就是加入到当前的 runLoop 的 default mode 中，而 ScrollView 在用户滑动时，主线程 RunLoop 会转到 UITrackingRunLoopMode 。而这个时候， Timer 就不会运行。

```swift
class DataTimer: NSObject, ObservableObject {
    @Published var timer = 0
    
    var timerObject = Timer()
    
    @objc func increment() {
        self.timer += 1
    }
    
    func runTimer() {
        timerObject = Timer.scheduledTimer(timeInterval: 1.0, target: self, selector: #selector(increment), userInfo: nil, repeats: true)
//        RunLoop.main.add(timerObject, forMode: .common)
    }

    // 主线程 默认有 runloop
    // 而 GCD 中没有 runloop 
    // https://blog.csdn.net/M316625387/article/details/83787313
    func runTimerOnGCD() {
        DispatchQueue.global().async {
            print(Thread.current)
            self.timerObject = Timer.scheduledTimer(timeInterval: 1.0, target: self, selector: #selector(self.increment), userInfo: nil, repeats: true)
            RunLoop.current.add(Port(), forMode: .default)
            RunLoop.current.run(mode: .default, before: .distantFuture)
//            RunLoop.main.add(self.timerObject, forMode: .common)
        }
    }
}

struct ContentView: View {
    @StateObject var timer = DataTimer()
    
    var body: some View {
        NavigationStack {
            Text("\(timer.timer)")
            List {
                Text("hi")
            }
        }
        .onAppear {
            timer.runTimer()
        }
    }
}
```

## GCD RAC

[iOS开发「RAC」RAC的定时、延时、超时方法](https://www.jianshu.com/p/d5b90f08f2fc)

> GCD的延时方法

```objc
// 延时执行
dispatch_time_t delayTime = dispatch_time(DISPATCH_TIME_NOW, (int64_t)(1.0/** 延时的时间*/ * NSEC_PER_SEC));
dispatch_after(delayTime, dispatch_get_main_queue(), ^{
    NSLog(@"延时1秒时间到~")
});
```

## 锁

[地平线-高精地图开发-面经](https://www.nowcoder.com/discuss/496072248986443776?sourceSSR=search)

0x01 多线程 保证数据一致性
* 互斥锁：通过使用互斥锁来控制多个线程对共享数据的并发访问，从而实现对共享数据的保护。
* 读写锁：读写锁可以同时允许多个读线程访问共享数据，但仅允许单个写线程对共享数据进行修改，进一步提高了效率。
* 原子操作：通过使用原子操作，可以保证在多线程环境下数据操作的原子性，从而避免数据不一致的情况。
* 管程：管程是一种特殊的锁，用于保护共享资源的访问，并具有更高的灵活性和可扩展性。

### atomic

> [iOS探索 细数iOS中的那些锁](https://juejin.cn/post/6844904167010467854)
>
> * 原子性修饰的属性进行了spinlock加锁处理
> * 非原子性的属性除了没加锁，其他逻辑与atomic一般无二
> * atomic只能保证setter、getter方法的线程安全，并不能保证数据安全，所以更多的使用nonatomic来修饰

### @synchronized

### NSLock

> NSLock、NSRecursiveLock、NSCondition和NSConditionLock底层都是对pthread_mutex的封装