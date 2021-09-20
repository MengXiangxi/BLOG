---
title: "Good practice for collaborative revising a manuscript in LaTeX"
categories:
  - Research
tags:
  - academic writing
  - LaTeX
date: 2021-09-20
layout: post
---

LaTeX is our first option when writing a research paper, unless our collaborators have trouble using it. However, there is no standard solution for collaborative manuscript editing and revising. In my opinion, the best way to do it is to use some macros and do it manually.

Of course, using [Overleaf](https://www.overleaf.com/) makes things a lot easier, but due to various reasons, that is not always possible. Alternatively, we have tried to use the change-tracking mechanism in git, but it did not work out quite well either.

## Sharing the manuscript

Git is an ideal companion with LaTeX, so be sure to always use it (whenever possible). There are some important things to check before creating a git repository.

- Use a [.gitignore](https://github.com/github/gitignore/blob/master/TeX.gitignore) file to exclude unwanted intermediate files.
- Write some related information in the README file. You would want to specify what is the compiling tool chain, and any unconventional macros used.
- Put all figures into a figure folder, and keep the root as clean as possible. Make sure to avoid large files by reasonably compressing figures.

After doing all these, configure the correct permissions and let your collaborator git pull everything to start revising.

## Revising a manuscript

There are two macros that have been validated, [`easyReview`](https://www.ctan.org/pkg/easyreview) and [`changes`](https://www.ctan.org/pkg/changes). Personally I find `easyReview` quite sufficient, but it has some strange conflicts (the mechanism to create ~~strikethrough text~~) so sometimes I had to use `changes`.

In either situation, you have to specify the package in the preamble using the `\usepackage{}` command first. Then you can move on with the revision.

| Purpose | `easyReview` | `changes` |
| --- | --- | --- |
| Include the package | `\usepackage{easyReview}` | `\usepackage{changes}` |
| Inserting | `\add{}` | `\added{}` |
| Deleting | `\remove{}`| `\deleted{}` |
| Replacement | `\replace{old}{new}` | `\replaced{new}{old}` |
| Commenting | `\comment{}{}` | `\comment{}` |
| Highlighting | `\highlight{}` | `\highlight{}` |

However, `easyReview` is kind of simplified, and `changes` is more exquisite, with various configurable features through optional parameters. Both packages have various known problems, especially when it comes to math equations, citations, and floats. Workaround measures are required.

Typing these commands can be tiring. Keyboard shortcut could be used to streamline the process. I have a [programmable keyboard](https://www.logitechg.com/en-us/products/gaming-keyboards/g613-wireless-mechanical-gaming-keyboard.html), and I have mapped these commands to the programmable keys.

As for git, it is generally more polite to submit a pull request, even if you have the permission to directly commit.

## Receiving the revised manuscript.

Although it is possible to solve the problems during the merging process, it should be easier to first merge the revised text and then start to review it.

It is quite easy to manually accept or reject the revisions made. Finally, just delete or comment out the `\usepackage{}` command. Or just leave it there.
