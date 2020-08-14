---
title: Android Studio Emulator Change Hosts
tags:
  - Android
date: 2020-04-17
---

```
Library/Android/sdk/emulator/emulator -avd Nexus_5X_API_26_APIs -writable-system
Library/Android/sdk/platform-tools/adb root
Library/Android/sdk/platform-tools/adb remount
```

<!-- more -->

> Please use Nexus_6_API_28  (28 API), higher version cause you do not have permission to write file