---
title: New Macbook Setup
tags:
  - MacOS
date: 2020-05-25
---

MacOs base settings.

<!-- more -->

### System Setting
- Desktop & Dock -> Automatically hide and show the Dock
- Language & Region -> add Chinese Simplified
- Keyboard -> Keyboard Shortcuts -> Modifier Keys -> Switch Caps Lock Key to Control
- Keyboard -> Keyboard Shortcuts -> Function Keys -> Check "Use F1,F2,etc,keys as standard function keys"
- Track Pad -> Point & Click -> Tap to Click
- Track Pad -> Point & Click -> Adjust Tracking speed to seven
- Track Pad -> Scroll & Zoom -> Scroll direction: Natural (uncheck)
- Track Pad -> More Gestures -> App Expose (Swipe down with four fingers)
- Keyboard -> Text -> uncheck all the checkbox in the setting page

### Local app setting
* Finder
Preferences -> Advanced -> When performing a search -> Search the Current Folder
Toolbar -> View -> Show Path Bar

### Remove all the app from Dock that don’t use

### Install and setting Chrome(For work)
- Settings -> (checked) Ask where to save each file before downloading
- Setting -> Security -> Uncheck 'Warn you if passwords are exposed in a data breach' (This will disable notice when you submit login form even you are testting your site )

### Install Iterm2 : [Link Here](https://www.iterm2.com/)

### Install brew : [Link Here](https://brew.sh/)

### Install oh-my-zsh : 
* change theme to ‘bira’

### Bind keys for zsh in ~/.zshrc
```
bindkey "^K" forward-word
bindkey "^J" backward-word
```

### Install tmux, htop : brew install tmux htop

### Install sequel-ace:  brew install --cask sequel-ace

### Install MacVIM For temp note tool