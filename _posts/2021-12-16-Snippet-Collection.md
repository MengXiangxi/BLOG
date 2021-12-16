---
title: "A collection of useful coding snippets"
categories:
  - Coding
tags:
  - cheat sheet
date: 2021-12-16
layout: post
---

I believe every programmer has his/her own repository of coding snippets. I used to manage it with OneNote and EverNote. I am going to manage the core part here.

## python

- Iterate through all files in the current folder.

```python
import os

for root, dirs, files in os.walk(os.getcwd()):
    for name in files:
        print(os.path.join(root, name))
```

## MATLAB

- Iterate through all files in the current folder and remove the root folders.

```matlab
dirInput = dir(fpath);
dirInput(ismember({dirInput.name},{'.','..'})) = []; % remove hidden folder
```
