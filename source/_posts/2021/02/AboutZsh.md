---
title: About Zsh
tags:
  - Zsh
date: 2021-02-11
---

# Installation
For Mac, already have zsh.

For Linux
```
yum install zsh -y
# or
apt install zsh -y
```

<!-- more -->

# Configuration
Append to ~/.zshrc
```
bindkey "^K" forward-word
bindkey "^J" backward-word
```