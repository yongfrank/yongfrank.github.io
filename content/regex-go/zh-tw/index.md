---
title: "Regex Go 已上架 App Store"
date: 2023-04-24T20:54:05+08:00
custom_head: <meta name='apple-itunes-app' content='app-id=6447801504'>
disable_toc: true
layout: "single"
description: "利用 Regex Go 掌握 Regex 正規表示式！"
---
<!-- Markdownlint-disable MD033 -->
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
        "
    >
        <option value="">English</option>
        <option value="zh-tw" selected>繁體中文</option>
        <option value="zh">简体中文</option>
    </select>
</div>

<a href="https://apps.apple.com/app/regex-go/id6447801504" >
    <img src="/regex-go/images/download-on-the-app-store.svg" alt="download on app store" align="right" width="120px">
</a>

利用 Regex Go 掌握 Regex 正規表示式！

🤗 跟繁瑣的文本處理任務說再見吧，透過 Regex Go 的 RegexBuilder，輕鬆實現字符串操作。🤩

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

## App 裡有什麼

Regex Go 使用了 Swift Regex 和 RegexBuilder，兩個在 WWDC22 推出的框架。廣為流傳的 Regex 誕生於上世紀 60 - 70 年代，他在文字處理過程中起到了非常大的作用，但也因為他出現的比較早，所以有很多不足的地方，例如難以閱讀，難以排錯。

而在 WWDC22，RegexBuilder 的誕生，以 DSL 的形式去構建 Regex，讓 Regex 更加易讀，易於維護。Regex Go 希望讓不了解 Regex 的人，能夠通過簡單的拖拉，就能夠使用 Regex，而不需要去學習 Regex 的語法，讓更多的人享受到 coding 帶來的樂趣，效率的提高。

## 更多細節

[![Follow on twitter](https://img.shields.io/twitter/follow/cyongfrank)](https://twitter.com/intent/follow?screen_name=cyongfrank)
[![About Frank](https://img.shields.io/badge/Find_More_Project-yongfrank.github.io/about-9ef)](https://yongfrank.github.io/about)
[![Blog Page](https://img.shields.io/badge/Blog_Page-yongfrank.github.io-success)](https://yongfrank.github.io/)
<a href="https://apps.apple.com/app/regex-go/id6447801504">
    <img src="../images/download-on-the-app-store.svg" alt="download on app store" style="float: right;">
</a>

從去年開始學習 iOS 到今年，我不僅收穫了知識上的成長，更多的是收穫了一群志同道合的朋友，他們的幫助讓我在學習過程中不再孤單，也讓我在學習過程中不斷地成長。各種沙龍活動，各種技術分享，是這段經歷為我打開了新世界的大門，讓我不再局限於學校的課程，而是能夠自由地去學習自己感興趣的知識。從懵懂無知，到上架第一個 App，這一年的成長，讓我感到非常幸運。

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
    <a href="../privacy-policy/" title="privacy" class="links__item" style="text-decoration: none;">協議</a>
    |
    <a href="https://www.dropbox.com/sh/k43u1bkqd4lsrnc/AABQvkI5rkY8keLz2yAwj6Lta?dl=0" title="PressKit at Dropbox" class="links__item" style="text-decoration: none;">素材</a>
    |
    <a href="mailto:yongfrank@outlook.com" title="Mail" class="links__item" style="text-decoration: none;">聯繫</a>
    <br>
    <a href="https://twitter.com/cyongfrank" title="Twitter at @cyongfrank" class="links__item" style="text-decoration: none;">@cyongfrank</a>
</div>
