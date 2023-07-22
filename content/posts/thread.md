---
title: "Thread"
date: 2023-04-08T14:58:09+08:00
---

## Thread Safety

> [确保线程安全的几种方法](https://developer.aliyun.com/article/254282?spm=a2c6h.13262185.profile.388.699e167e7REVuk)

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