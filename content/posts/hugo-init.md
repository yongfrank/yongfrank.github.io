---
title: "Hugo Init"
date: 2023-03-02T11:06:36+08:00
---

This article is about how to init a hugo site.

<!--more-->

## Hugo QuickStart

> * [Hugo 基础知识](https://github.com/heartnn/hugo-theme-test/blob/master/README.md)
> * [Hugo 从入门到会用](https://olowolo.com/post/hugo-quick-start/)
> * [Content Summaries Hugo generates summaries of your content.](https://gohugo.io/content-management/summaries/)

## Init hugo site

* [Hugo Article](https://gohugo.io/getting-started/quick-start/)
* [Hugo Host on GitHub Article](https://gohugo.io/hosting-and-deployment/hosting-on-github/#types-of-github-pages)

## Install Hugo

```sh
brew install hugo

hugo new site quickstart
cd quickstart
git init
git submodule add https://github.com/theNewDynamic/gohugo-theme-ananke themes/ananke
echo "theme = 'ananke'" >> config.toml
hugo server

hugo new posts/my-first-post.md

hugo server
```

## Update config.toml

```toml
baseURL = 'http://example.org/'
languageCode = 'en-us'
title = 'My New Hugo Site'
theme = 'ananke'
```

## GitHub Action

Create a file in `.github/workflows/gh-pages.yml`

```yml
name: github pages

on:
  push:
    branches:
      - main  # Set a branch that will trigger a deployment
  pull_request:

jobs:
  deploy:
    runs-on: ubuntu-22.04
    steps:
      - uses: actions/checkout@v3
        with:
          submodules: true  # Fetch Hugo themes (true OR recursive)
          fetch-depth: 0    # Fetch all history for .GitInfo and .Lastmod

      - name: Setup Hugo
        uses: peaceiris/actions-hugo@v2
        with:
          hugo-version: 'latest'
          # extended: true

      - name: Build
        run: hugo --minify

      - name: Deploy
        uses: peaceiris/actions-gh-pages@v3
        if: github.ref == 'refs/heads/main'
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./public
```

## Remove Submodule

[zh - Git Submodule](https://www.littlezhang.com/2020/01/%E5%A6%82%E4%BD%95%E6%8A%8A-git-submodule-%E5%8F%98%E6%88%90%E6%99%AE%E9%80%9A%E6%96%87%E4%BB%B6%E5%A4%B9/)

```sh
git rm --cached submodule_path  # delete reference to submodule HEAD 
git rm .gitmodules              # delete .gitmodule file
rm -rf submodule_path/.git      # delete submodule path .git file
```

## Google Analytics

[Setting Up Google Analytics on Hugo](http://cloudywithachanceofdevops.com/posts/2018/05/17/setting-up-google-analytics-on-hugo/#step-2-getting-your-google-analytics-tracking-id-for-your-property)

## Local Area Network Access Hugo

> [设置局域网内访问 hugo 本地服务 server](https://it-boyer.github.io/post/old/设置局域网内访问hugo本地服务server/)

```sh
hugo server --bind="0.0.0.0"

# specific port
hugo server --bind="0.0.0.0" -p 1234

hugo server --baseURL=http://192.168.1.7 --bind=192.168.1.7   
```

## MetaTags

These are meta tags used for social media sharing, specifically for Facebook's Open Graph Protocol.

`<meta property="og:url" content="{{ .Permalink }}" />` sets the URL of the page being shared. .Permalink is a Hugo variable that holds the permanent link (URL) to the current page being rendered.

`<meta property="og:site_name" content="{{ $.Site.Title }}" />` sets the name of the website or app that the content is being shared from. $.Site.Title is a Hugo variable that holds the title of the website or app.

The og: prefix stands for "Open Graph" and is a protocol developed by Facebook to standardize metadata for social media sharing. By including these meta tags in the `<head>` section of your HTML, you are providing social media platforms with specific information about your website's content, which can improve the appearance and accuracy of shared links.

### open graph in iMessage

> * [Implementing iMessage Link Previews with Open Graph](https://scottbartell.com/2019/03/05/implementing-imessage-link-previews/)
> * [iMessage Link Preview Tool Article](https://renderform.io/blog/posts/imessage-link-preview/)
> * [iMessage Link Preview Tool](https://renderform.io/tools/imessage-link-preview-tool/)
> * [Best Practices for Link Previews in Messages](https://developer.apple.com/library/archive/technotes/tn2444/_index.html)

```html
<meta property="og:title" content="iPhone" />
<meta property="og:image" content="https://example.com/image.png" />
```

### Summary Large Summary Image in Twitter

> [Summary Card with Large Image](https://developer.twitter.com/en/docs/twitter-for-websites/cards/overview/summary-card-with-large-image)

### Open Graph Preview Tools

> * [The Easiest Way to Preview and Generate Open Graph Meta Tags](https://www.opengraph.xyz/)
> * [Meta Tags](https://metatags.io/)

## Favicon

> * [利用 Favicon 为 Hugo 静态站点添加图标](https://ibrights.github.io/post/blog20210527/)
> * [Favicon Generator. For real.](https://realfavicongenerator.net/)

## Page bundles

> [Put multi-content.md into one folder](https://discourse.gohugo.io/t/put-multiple-content-md-files-into-one-file/26198)

### Page Organization

> * [Content Organization](https://gohugo.io/content-management/organization/)
> * [Subpages without _index.md file](https://discourse.gohugo.io/t/subpages-without-index-md-file/32358/3)

## disable list in _index.md

在Hugo中，要禁用一个_index.md文件中的列表页面，您可以通过在页面前置参数（front matter）中设置layout参数为"single"来实现。这将使Hugo使用单页布局模板渲染此页面，而不是默认的列表模板。

根据您使用的配置文件类型（TOML或YAML），以下是两个示例：

TOML格式的_index.md前置参数：

```makefile
Copy code
+++
title = "Your Page Title"
date = 2023-04-23T00:00:00Z
layout = "single"
+++
```

YAML格式的_index.md前置参数：
```yaml
---
title: "Your Page Title"
date: 2023-04-23T00:00:00Z
layout: "single"
---
```

这样，Hugo将使用"single"布局模板而不是列表模板渲染_index.md文件。请注意，如果您的主题没有提供名为"single"的布局模板，您可能需要自定义一个。在这种情况下，请查阅Hugo文档以了解如何创建自定义布局。