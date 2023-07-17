---
title: "Git"
date: 2023-04-29T12:10:01+08:00
---

> [Git-LFS](https://haway.30cm.gg/git-lfs/)

```sh
# https://formulae.brew.sh/formula/git-lfs
brew install git-lfs
```

```sh
git lfs track 10mb.psd
git add .gitattributes
git commit -m 'Add PSD file'
git push origin master
```

```sh
git lfs pull
```

## SourceTree Customized Full Name and Email SourceTree 客制化名字和邮箱

[git, sourcetree中针对不同仓库设置不同账户](https://blog.csdn.net/weixin_43404937/article/details/122564683)

```sh
git config --global user.name "your name"
git config --global user.email "your email-address"
```

```sh
git config --local user.name "your name"
git config --local user.email "your email-address"
```