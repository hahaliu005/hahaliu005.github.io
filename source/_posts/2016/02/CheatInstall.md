---
title: Cheat Install
tags:
  - Linux
date: 2016-02-12
---

There are several methods for installing cheat on your system.

### pip
pip is the recommended installation method for most users. Simply run:
```
sudo apt-get install python-pip python-dev build-essential
[sudo] pip install cheat
```

<!-- more -->

it is also possible to install cheat as user only by running
```
pip install --user cheat
```

then you have extend your PATH to include $HOME/.local/bin:
```
export PATH="$HOME/.local/bin:$PATH"
```

This line can be added to your bashrc/zshrc.

### homebrew
To install cheat using homebrew, run:
```
brew install cheat
```

### manually
First, install the dependencies:
```
[sudo] pip install docopt pygments appdirs
```

Then clone this repository:
```
git clone git@github.com:chrisallenlane/cheat.git
```

Lastly, cd into the cloned directory, then run:
```
[sudo] python setup.py install
```