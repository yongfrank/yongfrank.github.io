---
title: "Environment Problems"
date: 2023-05-24T13:28:22+08:00
---

## Python

```venv
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```

### pip source

```sh
pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple
```

## Node.js

```sh
# nvm
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.38.0/install.sh | bash
nvm install --lts
nvm use --lts
```

```sh
# npm
npm config set registry https://registry.npm.taobao.org
npm install
```
