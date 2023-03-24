---
title: "Hugo Init"
date: 2023-03-02T11:06:36+08:00
---


# Init hugo site

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