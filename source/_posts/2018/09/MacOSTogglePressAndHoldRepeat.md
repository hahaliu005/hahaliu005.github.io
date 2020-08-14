---
title: Macos toggle press and hold repeat
tags:
  - MacOS
date: 2018-09-04
---

To enable press and hold repeat character, you can run the command below:
```
defaults write NSGlobalDomain ApplePressAndHoldEnabled -bool false
```

And if you want to set back, run this command:
```
defaults write NSGlobalDomain ApplePressAndHoldEnabled -bool true
```