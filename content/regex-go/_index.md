---
title: "Regex Go is Available on App Store"
date: 2023-04-22T20:54:05+08:00
custom_head: <meta name='apple-itunes-app' content='app-id=6447801504'>
disable_toc: true
layout: "single"
# url: "/regex-go/"
description: "Master Regex with Regex Go!"
---
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
        <option value="" selected>English</option>
        <option value="zh-tw">繁體中文</option>
        <option value="zh">简体中文</option>
    </select>
</div>

<a href="https://apps.apple.com/app/regex-go/id6447801504" >
    <img src="/regex-go/images/download-on-the-app-store.svg" alt="download on app store" align="right" width="120px">
</a>

Master Regex with Regex Go!

🤗 Say goodbye to tedious text processing tasks with Regex Go - your ultimate tool for seamless string manipulation using Regex and user-friendly RegexBuilder 🤩.

<div style="position: relative; padding: 30% 45%;">
    <iframe style="position: absolute; width: 100%; height: 100%; left: 0; top: 0;" src="https://www.youtube.com/embed/nNWsuZMPHtk" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>
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
  <li><image src="./images/1.jpg"></image></li>
  <li><image src="./images/2.jpg"></image></li>
  <li><image src="./images/3.jpg"></image></li>
  <li><image src="./images/4.jpg"></image></li>
  <li><image src="./images/5.jpg"></image></li>
</ul>


## What's inside the App

Regex Go utilized two powerful frameworks, Swift Regex and RegexBuilder, which were both introduced at WWDC22. Regular Expressions (regex) have been a popular text processing tool since the 1960s-1970s, enabling us to quickly search and match strings in text. However, using regex can be challenging due to its complex syntax and difficulty in reading and debugging.

At WWDC22, RegexBuilder was introduced as a DSL (domain-specific language) to construct regex in a more readable and maintainable manner. Regex Go's aim is to make it easier for individuals who are unfamiliar with Regex to use it. Users can easily drag and drop to promptly construct their own regex without having to learn the intricate syntax and rules. This will allow more individuals to appreciate the joy of coding and increase their work efficiency.

## More Details

[![Follow on twitter](https://img.shields.io/twitter/follow/cyongfrank)](https://twitter.com/intent/follow?screen_name=cyongfrank)
[![About Frank](https://img.shields.io/badge/Find_More_Project-yongfrank.github.io/about-9ef)](https://yongfrank.github.io/about)
[![Blog Page](https://img.shields.io/badge/Blog_Page-yongfrank.github.io-success)](https://yongfrank.github.io/)
<a href="https://apps.apple.com/app/regex-go/id6447801504">
    <img src="./images/download-on-the-app-store.svg" alt="download on app store" style="float: right;">
</a>

Throughout the past year, from when I began learning iOS development to now, I have not only gained knowledge and skills, but also a community of like-minded friends. Their support and assistance have prevented me from feeling alone during the learning process and have propelled me toward growth. With various salons and technology-sharing events, this journey has opened new doors to a world beyond the limitations of school curriculums, allowing me to freely explore and learn about topics of interest. From a state of naivety and ignorance to launching my first app, this year of personal development has made me feel incredibly fortunate.

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
    <!-- <a href="https://chat.nuxt.dev"><img width="20px" src="./images/discord.svg" alt="Discord"></a>&nbsp;&nbsp; -->
    <a href="https://twitter.com/cyongfrank"><img width="20px" src="./images/twitter.svg" alt="Twitter"></a>&nbsp;&nbsp;
    <a href="https://github.com/yongfrank"><img width="20px" src="./images/github.svg" alt="GitHub"></a>
    <br>
    <a href="./privacy-policy/" title="privacy" class="links__item" style="text-decoration: none;">Privacy</a>
    |
    <a href="https://www.dropbox.com/sh/k43u1bkqd4lsrnc/AABQvkI5rkY8keLz2yAwj6Lta?dl=0" title="PressKit at Dropbox" class="links__item" style="text-decoration: none;">PressKit</a>
    |
    <a href="mailto:yongfrank@outlook.com" title="Mail" class="links__item" style="text-decoration: none;">Contact</a>
    <br>
    <a href="https://twitter.com/cyongfrank" title="Twitter at @cyongfrank" class="links__item" style="text-decoration: none;">@cyongfrank</a>
</div>
