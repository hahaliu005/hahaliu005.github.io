---
title: Find all files and do something
tags:
  - Shell
date: 2019-09-16
---

Gist about find all files and do something

<!-- more -->

```bash
find . -type f -iname "*.mp4" -print0 | while IFS= read -r -d $'\0' line; do
    echo $line
    // eg:Remove .mp4 extension
    echo ${line%.mp4}
done
```