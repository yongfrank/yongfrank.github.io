---
title: "Facebook Yoga / Flexbox"
date: 2023-03-24T13:02:13+08:00
---

## [Repository on GitHub By facebook](https://github.com/facebook/yoga)

[![npm](https://img.shields.io/npm/v/yoga-layout.svg)](https://www.npmjs.com/package/yoga-layout) [![Maven Central](https://img.shields.io/maven-central/v/com.facebook.yoga/yoga)](https://search.maven.org/artifact/com.facebook.yoga/yoga)

## Layout

![Layout-justify-content](https://koenig-media.raywenderlich.com/uploads/2017/05/flexbox_theory_3.png)

![Layout-align-items](https://koenig-media.raywenderlich.com/uploads/2017/05/flexbox_theory_4.png)

## Code

> [Online HTML CSS JavaScript](https://codepen.io/pen/)

### `align-*`

> [align-items](https://developer.mozilla.org/en-US/docs/Web/CSS/align-items)

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Align-items Stretch Demo</title>
    <style>
        .container {
            display: flex;
            height: 200px;
            align-items: stretch;
            background-color: lightgray;
        }

        .item {
            width: 100px;
            margin: 10px;
            background-color: #f1c40f;
            text-align: center;
            font-size: 24px;
            line-height: 2;
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="item">1</div>
        <div class="item">2</div>
        <div class="item">3</div>
    </div>
</body>
</html>
```

### line-*

[line height](https://developer.mozilla.org/en-US/docs/Web/CSS/line-height)

## Yoga / Flexbox

> [Juejin]
> 
> Yoga 是基于 Flexbox 的，也有一些不同。
> 
> Yoga 并没有实现全部 CSS Flexbox。
> 
> 它省略了非布局属性，如设置颜色。
> 
> Yoga 改进了一些 Flexbox 的属性来提供更好的从右到左的支持。
> 
> 最后，Yoga 增加了一个新的比例（AspectRatio）属性来处理在布置某些元素如图片时常见的需求

## Reference

* [Yoga doc](https://yogalayout.com/docs)
* [Yoga Tutorial: Using a Cross-Platform Layout Engine](https://www.kodeco.com/530-yoga-tutorial-using-a-cross-platform-layout-engine)
* [zh - Yoga Tutorial: Using a Cross-Platform Layout Engine](https://blog.csdn.net/kmyhy/article/details/77676104)
* [zh - Yoga - Cross-platform engine][Juejin]
* [zh - Yoga Android](https://www.jianshu.com/p/d4289b16a133)

[Juejin]: https://juejin.cn/post/6844903591233191943