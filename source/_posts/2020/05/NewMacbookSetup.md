---
title: New Macbook Setup
tags:
  - MacOS
date: 2020-05-25
---

MacOs base settings.

<!-- more -->

### System Setting
- Displays -> Resolution -> Scaled -> Choose the third (Larger Text) scale
- Dock & Menu Bar -> Automatically hide and show the Dock
- Dock & Menu Bar -> Automatically hide and show the menu bar
- Language & Region -> add Chinese Simplified
- Keyboard -> Keyboard -> Modifier Keys -> Switch Caps Lock Key to Control
- Keyboard -> Keyboard -> Touch Bar shows F1, F2, etc
- Track Pad -> Point & Click -> Tap to Click
- Track Pad -> Point & Click -> Look up & data detectors (tap with three fingers)
- Track Pad -> Point & Click -> Adjust Tracking speed to six
- Track Pad -> Scroll & Zoom -> Scroll direction: Natural (uncheck)
- Track Pad -> More Gestures -> App Expose (Swipe down with four fingers)
- Accessibility -> Pointer Control -> TrackPad Options -> Enable dragging (three finger drag)
- Keyboard -> Text -> uncheck all the checkbox in the setting page
- Siri -> Disable siri

### Local app setting
* Finder
Preferences -> Advanced -> When performing a search -> Search the Current Folder
Toolbar -> View -> Show Path Bar

### Install wireguard

### Remove all the app from Dock that don’t use

### Install and setting Chrome(For work)
- Settings -> (checked) Ask where to save each file before downloading
- Setting -> Security -> Uncheck 'Warn you if passwords are exposed in a data breach' (This will disable notice when you submit login form even you are testting your site )
 
### Install and setting Edge(For study)
- Settings -> (checked) Ask where to save each file before downloading
 
### Install baidu wubi : [Download Link](https://srf.baidu.com/input/mac.html)
- Set input source switch hotkey: System Setting -> Keyboard -> ShortCuts -> Input Sources

### Install Iterm2 : [Link Here](https://www.iterm2.com/)
* Preference -> Profiles -> Window -> Transparency
* Hide menubar : Preference -> Profiles -> Window -> Style -> Full height left of screen
* RemapKeys : Preference -> Profiles -> Keys -> (^j | Send Hex Code | 0x1B 0x62) (^k | Send Hex Code | 0x1B 0x66)
* Change font size: Preference -> Profile -> Text -> Font -> Change font size to 14

### Install brew : [Link Here](https://brew.sh/)

### Install oh-my-zsh : 
* change theme to ‘bira’

### Bind keys for zsh in ~/.zshrc
```
bindkey "^K" forward-word
bindkey "^J" backward-word
```

### Install Alfred: [Link Here](https://www.alfredapp.com/help/v3/)

### Install tmux, htop : brew install tmux htop

### Install sequel-ace:  brew install --cask sequel-ace

### Install MacVIM For temp note tool