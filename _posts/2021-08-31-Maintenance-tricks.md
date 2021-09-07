---
title: "Tricks in maintenance of coding environments"
categories:
  - Coding
tags:
  - cheat sheet
date: 2021-08-31
layout: post
---

<p> Various toolchains have been set up to facilitate daily chores. However, setting up and maintaining the environment becomes troublesome. Here, some common commands and methods are summarized.</p>

## Jupyter Notebook

- To set breakpoint

```python
from IPython.core.debugger import set_trace

set_trace()
```

- A workaround to skip a certain cell when running the notebook

```python
%%script skip-cell
```

- To use a conda environment as a kernel

```bash
conda info --envs
conda activate myenv
python -m ipykernel install --user --name myenv --display-name "Python (myenv)"

# To uninstall
jupyter kernelspec list
jupyter kernelspec uninstall myenv
```

## Python

- Useful pip mirrors

```bash
# 清华源
pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple
# 阿里源
pip config set global.index-url https://mirrors.aliyun.com/pypi/simple/
# 腾讯源
pip config set global.index-url http://mirrors.cloud.tencent.com/pypi/simple
# 豆瓣源
pip config set global.index-url http://pypi.douban.com/simple/
```

## Git

- Proxy

```bash
git config --global https.proxy http://127.0.0.1:1080
git config --global https.proxy https://127.0.0.1:1080
git config --global --unset http.proxy
git config --global --unset https.proxy
```

- SSH key

In git bash

```git
ls -al ~/.ssh
ssh-keygen -t ed25519 -C "your_email@example.com"
ssh-agent
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
ssh -T git@github.com
```

## R

- To update R on Windows

```R
install.packages("installr") # if not previously installed
library(installr)
updateR()
```
