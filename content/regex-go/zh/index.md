---
title: "Regex Go 已上架 App Store"
date: 2023-04-24T20:54:05+08:00
custom_head: <meta name='apple-itunes-app' content='app-id=6447801504'>
disable_toc: true
layout: "single"
description: "利用 Regex Go 掌握 Regex 正则表达式!"
---
<!-- markdownlint-disable md033 -->
<link rel="stylesheet" href="/regex-go/style.css">
<div style="display: flex; align-items: flex-start; justify-content: space-between">
    <a href="https://github.com/yongfrank/RegexGo">
        <img src="https://img.shields.io/github/stars/yongfrank/RegexGo.svg?style=social" alt="GitHub Stars">
    </a>
    <select
        data-placeholder="Choose a Language..."
        onchange="
            var value = this.value;
            var url = '/regex-go/';
            if (value == 'zh-tw') {
            url += 'zh-tw/';
            } else if (value == 'zh') {
            url += 'zh/';
            }
            window.location.assign(url);
        "    >
        <option value="">English</option>
        <option value="zh-tw">繁體中文</option>
        <option value="zh" selected>简体中文</option>
    </select>
</div>

<a href="https://apps.apple.com/app/regex-go/id6447801504" >
    <img src="/regex-go/images/download-on-the-app-store.svg" alt="download on app store" align="right" width="120px">
</a>

利用 Regex Go 掌握 Regex 正则表达式!

🤗 跟繁琐的文本处理任务说再见吧，通过 Regex Go 的 RegexBuilder，轻松实现字符串操作。🤩

<div style="position: relative; padding: 30% 45%;">
    <iframe style="position: absolute; width: 100%; height: 100%; left: 0; top: 0;" src="//player.bilibili.com/player.html?aid=528191215&bvid=BV1gM411V7r9&cid=1117513738&page=1&as_wide=1&high_quality=1&danmaku=0" frameborder="no" scrolling="no">
    </iframe>
</div>

[![cover image](cover.jpg)](https://apps.apple.com/app/regex-go/id6447801504)
<!-- https://blog.csdn.net/lishimin1012/article/details/88949602 -->
<!-- markdownlint-disable MD033 -->

<style>
@media screen and (max-width: 960px){
    ul#container
    {
    overflow: hidden;
    overflow-x: scroll;
    width: 88%; /* or whatever */
    /* height: 100%; or whatever */
    white-space: nowrap;
    }
    
    ul#container li
    {
    display: inline-block;
    width: 70%; /* or whatever */
    /* height: 100%; or whatever */
    }
}
 
@media screen and (min-width: 960px){
      ul#container
    {
    overflow: hidden;
    overflow-x: scroll;
    width: 96%; /* or whatever */
    /* height: 100%; or whatever */
    white-space: nowrap;
    }
    
    ul#container li
    {
    display: inline-block;
    width: 30%; /* or whatever */
    /* height: 100%; or whatever */
    }
}
</style>
<!-- https://stackoverflow.com/questions/2728715/iphone-scroll-images-horizontally-like-in-appstore -->
<ul id="container">
  <li><image src="../images/1.jpg"></image></li>
  <li><image src="../images/2.jpg"></image></li>
  <li><image src="../images/3.jpg"></image></li>
  <li><image src="../images/4.jpg"></image></li>
  <li><image src="../images/5.jpg"></image></li>
</ul>

## App 里有什么

Regex Go 使用了 Swift Regex 和 RegexBuilder，两个在 WWDC22 推出的框架。广为流传的 Regex 诞生于上世纪 60 - 70 年代，他在文字处理过程中起到了非常大的作用，但也因为他出现的比较早，所以有很多不足的地方，例如难以阅读，难以排错。

而在 WWDC22，RegexBuilder 的诞生，以 DSL 的形式去构建 Regex，让 Regex 更加易读，易于维护。Regex Go 希望让不了解 Regex 的人，能够通过简单的拖拽，就能够使用 Regex，而不需要去学习 Regex 的语法，让更多的人享受到 coding 带来的乐趣，效率的提高。

## 更多细节

[![Follow on twitter](https://img.shields.io/twitter/follow/cyongfrank)](https://twitter.com/intent/follow?screen_name=cyongfrank)
[![About Frank](https://img.shields.io/badge/Find_More_Project-yongfrank.github.io/about-9ef)](https://yongfrank.github.io/about)
[![Blog Page](https://img.shields.io/badge/Blog_Page-yongfrank.github.io-success)](https://yongfrank.github.io/)
<a href="https://apps.apple.com/app/regex-go/id6447801504">
    <img src="../images/download-on-the-app-store.svg" alt="download on app store" style="float: right;">
</a>

从去年开始学习 iOS 到今年，我不仅收获了知识上的成长，更多的是收获了一群志同道合的朋友，他们的帮助让我在学习过程中不再孤单，也让我在学习过程中不断的成长。各种沙龙活动，各种技术分享，是这段经历为我打开了新世界的大门，让我不再局限于学校的课程，而是能够自由的去学习自己感兴趣的知识。从懵懂无知，到上架第一个 App，这一年的成长，让我感到非常幸运。

<style>
.links {
    text-align: center;
    font-weight: 500;
    color: #424245;
    font-size: 0.8em;
    margin-top: 50px;
    margin-bottom: 20px;
}

.links__item {
    color: #424245;
    text-decoration: none;
}

@media (prefers-color-scheme: dark) {
    .links {
        color: rgba(255, 255, 255, 0.2);
    }
    .links__item {
        color: rgba(255, 255, 255, 0.5);
    }
}

.links__item:hover {
    animation: colorChange 0.5s both;
}
@keyframes colorChange {
    from {
        color: rgba(255, 255, 255, 0.5);
    }
    to {
        color: #F2A33C;
    }
}
</style>
<div class="links">
    <!-- <a href="https://chat.nuxt.dev"><img width="20px" src="../images/discord.svg" alt="Discord"></a>&nbsp;&nbsp; -->
    <a href="https://twitter.com/cyongfrank"><img width="20px" src="../images/twitter.svg" alt="Twitter"></a>&nbsp;&nbsp;
    <a href="https://github.com/yongfrank"><img width="20px" src="../images/github.svg" alt="GitHub"></a>
    <br>
    <a href="../privacy-policy/" title="privacy" class="links__item" style="text-decoration: none;">协议</a>
    |
    <a href="https://www.dropbox.com/sh/k43u1bkqd4lsrnc/AABQvkI5rkY8keLz2yAwj6Lta?dl=0" title="PressKit at Dropbox" class="links__item" style="text-decoration: none;">素材</a>
    |
    <a href="mailto:yongfrank@outlook.com" title="Mail" class="links__item" style="text-decoration: none;">联系</a>
    <br>
    <a href="https://twitter.com/cyongfrank" title="Twitter at @cyongfrank" class="links__item" style="text-decoration: none;">@cyongfrank</a>
</div>
