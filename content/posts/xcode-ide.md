---
title: "Xcode IDE"
date: 2023-03-27T21:31:43+08:00
---

## IDE Tutorials

* [Xcode 6 Articles: Shortcut ...](https://www.cnblogs.com/lxlx1798/category/1262403.html)
* [Xcode overview](https://www.cnblogs.com/lxlx1798/p/9369458.html)

## Mark Tricks

* [Xcode Mark](https://www.jianshu.com/p/3e136e660d27)
* [`//TODO`: — Make your notes on Xcode stand out](https://medium.com/@cboynton/todo-make-your-notes-on-xcode-stand-out-5f5124ec064c)
* [Xcode中那些让人焕然一新的特殊注释#pragma mark、TODO、FIXME、MARK](https://developer.aliyun.com/article/937303?accounttraceid=ba9d921bd3b745bfafb39602c62ad321gjpa)

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

* [建立方便大家安裝到手機的 Xcode 專案 — 搭配 xcconfig & Team ID](https://medium.com/彼得潘的-swift-ios-app-開發問題解答集/建立方便大家安裝到手機的-xcode-專案-搭配-xcconfig-team-id-fb072ed08b2f)

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