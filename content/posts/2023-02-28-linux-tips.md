---
title: "2023 02 28 Linux Tips"
date: 2023-02-28T15:54:04+08:00
tags: []
featured_image: ""
description: ""
---
<!-- https://github.com/DavidAnson/markdownlint/issues/199 -->
# Linux 小技巧

## 特殊符号

### ` grave accent

> noun
>
> a mark placed over a vowel in some languages to show how it should be pronounced, as over the e in the French word père 钝重音符，沉音符，抑音符（标在元音上面表发音）
>
> ➡  compare acute accent, circumflex, tilde (1) , umlaut
>
> ➡  see also grave1

```sh
echo `date`
echo $(date)
```

### `$(( ))` 用于数学运算

```sh
echo $(( 1 + 1 ))
```

### USAGE

[google explaination](https://developers.google.com/style/code-syntax)

在命令行工具的用法说明中，通常有一些符号来表示参数的类型和数量。这些符号的含义可能因不同的工具而有所差异，但一般来说，有以下几种常见的规则²：

- []: 表示方括号内的参数是可选的，可以提供也可以省略；
- <>: 表示尖括号内的参数是必需的，必须提供；
- ...: 表示省略号前面的参数可以重复多次，或者表示任意数量和类型的参数；
- |: 表示竖线两边的参数是互斥的，只能选择其中一个。

例如，假设有一个工具叫做 copy，它用于复制文件或目录。它的用法说明可能是这样：

copy \<source> \<destination> [-r] [-v]

这里：

- source 和 destination 是必需的参数，分别表示要复制的源文件或目录和目标位置；
- -r 和 -v 是可选的参数，分别表示递归复制和显示详细信息；
- [] 表示 -r 和 -v 可以提供也可以省略；
- <> 表示 source 和 destination 必须提供；
- ... 没有出现在这个例子中。

你还有其他关于命令行符号的问题吗？

Source: Conversation with Bing, 2/28/2023(1) Document command-line syntax - Google Developers. https://developers.google.com/style/code-syntax Accessed 2/28/2023.
(2) cmd | Microsoft Learn. https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/cmd Accessed 2/28/2023.
(3) linux - bash: how to pass command line arguments containing special .... https://superuser.com/questions/163515/bash-how-to-pass-command-line-arguments-containing-special-characters Accessed 2/28/2023.