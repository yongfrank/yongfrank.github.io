---
title: "Cocoapods"
date: 2023-05-20T23:10:11+08:00
---

```txt
ATestingModule
ATestingModule-ATestingModule
```

这两个 cocoapods 里的有什么区别

在 CocoaPods 中，一个模块名字的定义是根据 podspec 文件中的 name 字段来的。比如说，你的 podspec 文件可能会有类似这样的一行代码：

```ruby
Pod::Spec.new do |s|
  s.name = "ATestingModule"
  ...
end
```

在这个例子中，模块的名字就是 "ATestingModule"。

那么，这个名字后面跟着 "-ATestingModule" 是什么呢？那是一个 target。在 Xcode 中，一个 target 定义了一组要使用的源文件，一个要构建的产品，以及用于构建该产品的构建设置。

CocoaPods 会为每一个 Pod 创建一个对应的 target，名字通常是 {PodName}-{ModuleName}。在你的例子中，PodName 和 ModuleName 都是 "ATestingModule"，所以你看到的 target 名字就成了 "ATestingModule-ATestingModule"。

如果你只是想要使用这个 Pod，你通常不需要关心这个 target 的名字。当你在你的代码中 import ATestingModule 的时候，你实际上是在引用的是模块，而不是 target。

然而，如果你正在进行一些更复杂的构建配置，比如设置 Build Phases 或者添加编译标志，那么你可能就需要知道这个 target 的名字了。