---
title: "Mac Hint"
date: 2023-04-14T16:50:52+08:00
---

## Username, Full Name, Local Network Name

[YouTube - How To Set Up a New Mac (Step-By-Step Guide)](https://youtu.be/snZpO8rDq7U)

## Storage

> [Reclaiming storage/space from 'System Data' in macOS: A tutorial on understanding the System Data usage.](https://blog.greggant.com/posts/2022/04/10/reclaiming-space-from-system-data-in-macos.html)

> [mac 系统数据 100 多 G 了，有什么清理的办法吗？](https://www.v2ex.com/t/893091)
>
> 看到你有 Xcode 。那么推荐你先试试八爷开发的 Cleaner For Xcode 。另外，一般的清理也可以使用 Disk Drill 以及 Cleaner One Pro ，这两个的免费版就足够使用了。

![system data](https://pic.liuzaoqi.com/picgo/202211061642821.png)

> [Search Result with V2EX and google](https://www.google.com/search?q=mac+%E7%B3%BB%E7%BB%9F%E6%95%B0%E6%8D%AE+site%3Av2ex.com&rlz=1C5CHFA_en__1048__1048&oq=mac+%E7%B3%BB%E7%BB%9F%E6%95%B0%E6%8D%AE+site%3Av2ex.com&aqs=chrome..69i57.3306j0j4&sourceid=chrome&ie=UTF-8)

## Mac Keyboard

> [mac按键难回弹（按下去软软的）](https://blog.csdn.net/Castlehe/article/details/124538924)

原因

一般是键盘里面有一些东西塞住了。

操作

* 以按键A为例，把键盘与桌面垂直放着（左侧电源插孔朝着桌面），然后不断去按A键，
* 如果这个方向不行，换另一侧，即把键盘放置成耳机孔朝着桌面的方向，再去按A键
* 按的时候看手感，差不多就行

## Focus Stealer

> [How do I tell which app stole my focus in OS X?](https://superuser.com/questions/734007/how-do-i-tell-which-app-stole-my-focus-in-os-x)
>
> [mac 使用时窗口经常无故自己切换](https://www.v2ex.com/t/626682#reply13)

```python
#!/usr/bin/python                                                                                                       
    
try:
    from AppKit import NSWorkspace
except ImportError:
    print("Can't import AppKit -- try `pip install PyObjC`")
    print("(see instructions for running in a venv with PyObjC)")
    exit(1)
    
from datetime import datetime
from time import sleep
    
last_active_name = None
while True:
    active_app = NSWorkspace.sharedWorkspace().activeApplication()
    if active_app['NSApplicationName'] != last_active_name:
        last_active_name = active_app['NSApplicationName']
        print('%s: %s [%s]' % (
            datetime.now().strftime('%Y-%m-%d %H:%M:%S'),
            active_app['NSApplicationName'],
            active_app['NSApplicationPath']
        ))
    sleep(1)
```

1. Create a directory, and save the script above into it as "focus-stealer.py"
2. In a terminal in that directory enter these commands:

```sh
/usr/bin/python3 -m venv venv
./venv/bin/python -m pip install PyObjC
```

(This creates a new, isolated Python virtual environment, just for this script, and installs PyObjC into it. This avoids modifying your system Python installation or needing to use sudo.)

3. Now you can run the script. In a terminal in that same directory run:

```sh
./venv/bin/python ./focus-stealer.py
```

(In the future, you can skip directly to this step—no need to reinstall things.)

## .DS_Store

.DS_Store 是 macOS 操作系统中的一个隐藏文件，用于存储目录的自定义属性和元数据。它通常在文件系统的每个文件夹中生成，并保存与该文件夹相关的视图选项、图标位置、背景图像等信息。.DS_Store 文件对于目录的正常功能没有直接影响，但它可能在文件传输、版本控制或共享文件夹时引起一些问题。

在大多数情况下，.DS_Store 文件是不需要手动处理或修改的。它们会被 macOS 系统自动管理。然而，如果你需要在文件共享、版本控制或文件传输中排除 .DS_Store 文件，你可以使用一些方法来处理它们，例如：

## Rename files, folders and disks on Mac

> [Apple Support](https://support.apple.com/en-hk/guide/mac-help/mchlp1144/mac)