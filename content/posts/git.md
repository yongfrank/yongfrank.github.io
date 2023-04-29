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