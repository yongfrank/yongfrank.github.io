---
title: "Xcode IDE"
date: 2023-03-27T21:31:43+08:00
---

## IDE Tutorials

> * [Xcode 6 Articles: Shortcut ...](https://www.cnblogs.com/lxlx1798/category/1262403.html)
> * [Xcode overview](https://www.cnblogs.com/lxlx1798/p/9369458.html)

## Mark Tricks

> * [Xcode Mark](https://www.jianshu.com/p/3e136e660d27)
> * [`//TODO`: — Make your notes on Xcode stand out](https://medium.com/@cboynton/todo-make-your-notes-on-xcode-stand-out-5f5124ec064c)
> * [Xcode中那些让人焕然一新的特殊注释#pragma mark、TODO、FIXME、MARK](https://developer.aliyun.com/article/937303?accounttraceid=ba9d921bd3b745bfafb39602c62ad321gjpa)

```swift
TODO: Mark a task that needs to be done, using the format: // TODO:
FIXME: Mark a bug that needs to be fixed, using the format: // FIXME:
!!!: Mark a section that needs to be reviewed, using the format: // !!!:
???: Mark a section that needs to be clarified, using the format: // ???:
MARK: Mark a section of code, using the format: // MARK:

TODO: 标示处有功能代码待编写，使用方法：// TODO:
FIXME: 标示处代码需要修正，使用方法：// FIXME:
!!!: 标示处代码需要注意，使用方法：// !!!:
???: 标示处代码有疑问，使用方法：// ???:
MARK: 标记，和#pragma mark效果相同，使用方法：// MARK:

#pragma mark -
#pragma mark TODO:  标示处有功能代码待编写
#pragma mark FIXME: 标示处代码需要修正
#pragma mark !!!:  标示处代码需要注意
#pragma mark ???:  标示处代码有疑问
#pragma mark MARK:  标记，和#pragma mark效果相同
///TODO:标示处有功能代码待编写
///FIXME:标示处代码需要修正
///!!!:标示处代码需要注意
///???:标示处代码有疑问
///MARK:标记，和#pragma mark效果相同
```

![FIXME](https://miro.medium.com/v2/resize:fit:620/format:webp/1*yTw3RV5gwGnbRnCotn7MDQ.jpeg)

```swift
//FIXME: Remove all references to jiltedExLover
```

![TODO](https://miro.medium.com/v2/resize:fit:638/format:webp/1*_LdkLsLMuLDQPiHNLmLyGA.jpeg)

### Building Phase - Generating Warnings and Errors with Comments


![Select the project, then move to the build phases](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*8nnN5Fkh97Vqbdy6uDDwBA.jpeg)

![The run script box - Buiding Phases Instruction](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*rsw9Lzpa-lR6xpHNEAy9ag.jpeg)

![Xcode sh location](https://ucc.alicdn.com/pic/developer-ecology/bc069e85049c470fbbcb469a226ac2ab.gif)

```sh
TAGS="TODO:|FIXME:"
ERRORTAG="ERROR:"
find "${SRCROOT}" \( -name "*.h" -or -name "*.m" -or -name "*.swift" \) -print0 | xargs -0 egrep --with-filename --line-number --only-matching "($TAGS).*\$|($ERRORTAG).*\$" | perl -p -e "s/($TAGS)/ warning: \$1/" | perl -p -e "s/($ERRORTAG)/ error: \$1/"
```

```sh
# Full type: Including Mark, ???, !!!

TAGS="TODO:|FIXME:|MARK:|\?\?\?:|\!\!\!:"
ERRORTAG="ERROR:"
find "${SRCROOT}" \( -name "*.h" -or -name "*.m" -or -name "*.swift" \) -print0 | xargs -0 egrep --with-filename --line-number --only-matching "($TAGS).*\$|($ERRORTAG).*\$" | perl -p -e "s/($TAGS)/ warning: \$1/" | perl -p -e "s/($ERRORTAG)/ error: \$1/"
```

![Warning](https://ucc.alicdn.com/pic/developer-ecology/fb59cff9f75e436daf6b6f92a32cada8.png)

### Explanation of Bourne shell Script

This script is used for searching and highlighting specific tags and errors in source code files during build time. The script uses the find command to locate files with extensions .h, .m, and .swift in the SRCROOT directory. The egrep command is used to search for lines that match the regular expressions specified in TAGS and ERRORTAG.

If any line matches the tags or errors, it is displayed as a warning or error message using the perl command. The warning or error message is then displayed with the file name, line number, and the actual line containing the tag or error.

> Now maybe you’re thinking, what’s the point? Don’t I still have to dig through the program, going file by file, clicking the dropdown menu, trying to find that TODO tag? What if the project has dozens of files? Won’t my TODO and FIXME tags get lost in the mess and forgotten? If only there was a way to get Xcode to give you a warning if a TODO tag is left unanswered!
> 
> Click on the little plus sign at the trop right and select Run New Script Phase. The idea here is that you’re adding another step to the build process when Xcode builds your project. If you unwrap the arrow for the run script item, you’ll see a black box that will hold script that Xcode will run on every build.
>
> Now build your program. If you have any //TODO: or //FIXME: tags, Xcode will throw you a warning. If you write in an //ERROR: tag, Xcode will throw you an error and not run. (Note: you may have to move your script up in the build order.)
>
> Employ this little trick, and never leave another item on your TODO list undone!

![To Do list nothing emoji](https://miro.medium.com/v2/resize:fit:1000/1*0xhrWT7uRVlQWgdt20h-Bg.gif)

### #pragma mark -

```objc
#pragma mark -
#pragma mark - TableViewDelegate
```

> [Xcode中那些让人焕然一新的特殊注释#pragma mark、TODO、FIXME、MARK](https://developer.aliyun.com/article/937303?accounttraceid=ba9d921bd3b745bfafb39602c62ad321gjpa)
> 
> 从技术角度讲，以 #pragma 开头的代码是一条编译器指令，一个特定于程序或编译器的指令。它们不一定适用于其它编译器或其它环境。如果编译器不能识别该指令，则会将其忽略。
>
> 其作用是，告诉Xcode编译器，要在编辑器窗格顶部的方法和函数弹出菜单中将代码分隔开，如下图所示：
>
> From a technical point of view, the code starting with #pragma is a compiler directive, a specific directive for the program or compiler. They may not be applicable to other compilers or other environments. If the compiler cannot recognize the directive, it will be ignored.
>
> Its purpose is to tell the Xcode compiler to separate the code in the editor window top method and function pop-up menu, as shown in the following figure:

![#pragma mark -](https://ucc.alicdn.com/pic/developer-ecology/269a1ac714b14b42b3abb0088dc79549.png)

## DISAMBIGUATOR for Downloadable Project 

> * [建立方便大家安裝到手機的 Xcode 專案 — 搭配 xcconfig & Team ID](https://medium.com/彼得潘的-swift-ios-app-開發問題解答集/建立方便大家安裝到手機的-xcode-專案-搭配-xcconfig-team-id-fb072ed08b2f)

```
//
// See the LICENSE.txt file for this sample’s licensing information.
//
// SampleCode.xcconfig
//

// The `SAMPLE_CODE_DISAMBIGUATOR` configuration is to make it easier to build
// and run a sample code project. Once you set your project's development team,
// you'll have a unique bundle identifier. This is because the bundle identifier
// is derived based on the 'SAMPLE_CODE_DISAMBIGUATOR' value. Do not use this
// approach in your own projects—it's only useful for sample code projects because
// they are frequently downloaded and don't have a development team set.
SAMPLE_CODE_DISAMBIGUATOR=${DEVELOPMENT_TEAM}
```

## Xcode 14.3 Bug for Pod

[Unable to build project in Xcode 14.3 beta due to missing arc dir at /Applications/Xcode-beta.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/lib](https://developer.apple.com/forums/thread/725300)

```podfile
post_install do |installer|
  installer.generated_projects.each do |project|
    project.targets.each do |target|
        target.build_configurations.each do |config|
            config.build_settings['IPHONEOS_DEPLOYMENT_TARGET'] = '11.0'
         end
    end
  end
end
```

## Swift Package Manager through Proxy

> * [Xcode SPM Problem in mainland CN](https://www.v2ex.com/t/864822#r_11859057)
> * [求 xcode package 依赖正确下载姿势，拉取 aws-sdk- Swift 两个多小时了](https://www.v2ex.com/t/877764)

[ClashX](https://github.com/yichengchen/clashX#install) Pro 开增强已解决!(这个免费)

Download ClashX Pro With enhanced mode and Native Apple Silicon support at [AppCenter](https://install.appcenter.ms/users/clashx/apps/clashx-pro/distribution_groups/public) for free permanently.

> [libgit2 for Xcode 【已失效】Xcode GUI 添加 SPM 依赖的时候访问不了 github，无视 git config proxy 配置解决方案](https://www.cnblogs.com/jerrywossion/p/15719012.html)

此 [openradar](http://ww.openradar.appspot.com/FB9447729) 中提出者指出了原因：Xcode 调用 libgit2 时传入了 GIT_PROXY_NONE，无视了 git config 中的 proxy 配置。作者说用了自己打的 libgit2 包可以解决问题，但没说具体怎么操作。

看了一下 libgit2 里的声明：
```
	/**
	 * Do not attempt to connect through a proxy
	 *
	 * If built against libcurl, it itself may attempt to connect
	 * to a proxy if the environment variables specify it.
	 */
	GIT_PROXY_NONE,
```
里面说如果构建时使用了 curl （一般都会使用 curl？）会遵循 curl 的配置，于是在 ~/.curlrc 中加入一行（具体 proxy 依你自己的配置来指定）：

```
proxy = socks5://127.0.0.1:1080
```

~~验证了一下，Xcode 可以正常使用 proxy 添加 SPM 依赖了~~。更新：似乎不好使

注：
上面是为了解决在 Xcode GUI 中添加新的 SPM 依赖时遇到的问题，由于 Xcode 的项目不支持 Package.swift，目前只能使用 GUI 来添加。也可以使用旁路由来解决，Xcode 所在的机器只需要设置网关为旁路由的网关即可，旁路由上跑 proxy。

如果项目之前已经添加过 SPM 了，解析时卡住的话可以用命令行操作，具体可参见 [stackoverflow](https://stackoverflow.com/a/66768248/4148083)

> * [Swift Package Manager through Proxy](https://juejin.cn/post/6946451335948697636)
> * [Xcode SPM](https://gist.github.com/liang2kl/15237e23b06e737f036b8c1c3e5c0102)
> * [Xcode SPM 走代理抓取package](https://www.jianshu.com/p/c3d565c8be98)

```sh
export all_proxy=<your_proxy>:<your_port>

xcodebuild -resolvePackageDependencies -scmProvider system

export https_proxy=http://127.0.0.1:YOURPORT http_proxy=http://127.0.0.1:YOURPORT all_proxy=socks5://127.0.0.1:YOURPORT

# 如果是project 使用以下命令
xcodebuild -resolvePackageDependencies -scmProvider system -project YOURPROJECT.xcodeproj

# 如果是workspace 需要指定scheme，或者使用-list
xcodebuild -resolvePackageDependencies -scmProvider system -list -workspace HackrNewsApp.xcworkspace
```

> * [xcode spm with proxy](https://www.bolee.fun/xcode-spm-with-proxy.html)

经过查询Xcode的使用的自带 `git` 程序为 `com.apple.dt.Xcode.sourcecontrol.Git`，在 Xcode 里面的 SPM 下载 github 时候可以到控制台查看具体程序`ps aux|grep com.apple.dt.Xcode`，显示结果如下：

![xcode spm proxy git](https://www.bolee.fun/images/post/xcode-spm-proxy-git-show.jpg)

> * [解决swift package manager fetch慢的问题](https://www.cnblogs.com/vlucht/p/15015875.html)

```sh
# 前提: 你有一个代理

# 因为直接打开Xcode是不会走代理的。

# 以你需要现退出Xcode，然后在命令行里输入

open -a Xcode.app

# 保险起见你还可以在这之前加一句
export ALL_PROXY=http://127.0.0.1:<PORT>
```

### Local SPM

> [Getting 'no such module' error when importing a Swift Package Manager dependency](https://stackoverflow.com/questions/57165778/getting-no-such-module-error-when-importing-a-swift-package-manager-dependency)

## 编译架构和 Apple Silicon

> [Xcode 中使用 SPM 和 Build Configuration 的一些坑](https://onevcat.com/2022/10/spm-in-xcode/#%E7%BC%96%E8%AF%91%E6%9E%B6%E6%9E%84%E5%92%8C-apple-silicon)
>
> 在模拟器上排除 arm64 导致的问题
> 
> 在 Apple Silicon 的时代，默认情况下 Xcode 会使用 arm64 架构运行。这时候，自带的 iOS 模拟器也会跑在 arm64 下。如果你在项目里使用了一些老旧的以二进制发布的库，比如 fat binary 做的 framework，或者是不包含模拟器 arm64 的 .a 的文件，那么很可能在 Apple Silicon 的 mac 上，以模拟器为目标进行链接时，看到类似这样的错误：
>
> `building for iOS Simulator, but linking in object file built for iOS, for architecture arm64`
>
> 这是因为虽然库中包含了 arm64，但是其中标明了它是用在实际设备而非模拟器上的。网络上常见的办法，会教你在 EXCLUDED_ARCHS 里为 simulator 添加 arm64，用来把这个架构排除出去。
>
> `EXCLUDED_ARCHS[sdk=iphonesimulator*] = arm64`
>
> 这是一个治标不治本的“快速疗法”，加入这个设定可以让你编译通过并运行，但是你需要清楚了解到这么做的弊端：因为 arm64 被排除了，所以在 iOS 模拟器上，只有 x86_64 这一个架构选择。这意味着你的整个 app 都会以 x86_64 进行编译，然后跑在 x86_64 的模拟器上。而在 Apple Silicon 的 mac 上，这个模拟器其实是使用 Rosetta 2 跑起来的，这意味着性能的大幅下降。
>
> 对于 .debug，ONLY_ACTIVE_ARCH 为 true，编译目标为 arm64 的 iOS 模拟器，因此 SPM 只会给出 arm64-apple-ios-simulator 版本的编译结果。但是项目本身设定了 EXCLUDED_ARCHS arm64，它在链接包时，需要的其实是 x86_64 模拟器版本的包。砰！
> 
> 对于老旧二进制的依赖，最正确的做法是催促维护者赶快适配 xcframework。另一种可行的方案，是 hack 一下二进制，修改 arm64 slice 的目标字段，“欺骗” Xcode 让它认为这个二进制的 arm64 就是为模拟器编译的。这种方法在这里有详细解释，作者也发布了相关的 arm64-to-sim 工具，如有需要，可以暂时酌情使用。



