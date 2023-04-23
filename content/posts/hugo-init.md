---
title: "Hugo Init"
date: 2023-03-02T11:06:36+08:00
---

This article is about how to init a hugo site.

<!--more-->

## Hugo QuickStart

> [Hugo 基础知识](https://github.com/heartnn/hugo-theme-test/blob/master/README.md)
> [Content Summaries Hugo generates summaries of your content.](https://gohugo.io/content-management/summaries/)

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

<meta property="og:url" content="{{ .Permalink }}" /> sets the URL of the page being shared. .Permalink is a Hugo variable that holds the permanent link (URL) to the current page being rendered.

<meta property="og:site_name" content="{{ $.Site.Title }}" /> sets the name of the website or app that the content is being shared from. $.Site.Title is a Hugo variable that holds the title of the website or app.

The og: prefix stands for "Open Graph" and is a protocol developed by Facebook to standardize metadata for social media sharing. By including these meta tags in the <head> section of your HTML, you are providing social media platforms with specific information about your website's content, which can improve the appearance and accuracy of shared links.

### open graph in iMessage

> [Implementing iMessage Link Previews with Open Graph](https://scottbartell.com/2019/03/05/implementing-imessage-link-previews/)

```html
<meta property="og:title" content="iPhone" />
<meta property="og:image" content="https://example.com/image.png" />
```

### favicon

> * [利用 Favicon 为 Hugo 静态站点添加图标](https://ibrights.github.io/post/blog20210527/)
> * [Favicon Generator. For real.](https://realfavicongenerator.net/)