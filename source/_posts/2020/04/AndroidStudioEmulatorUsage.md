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

List avds
```
Library/Android/sdk/emulator/emulator -list-avds
```

Run avd as writable
```
Library/Android/sdk/emulator/emulator -avd Pixel_3_XL_API_29 -writable-system
```

List devices
```
Library/Android/sdk/platform-tools/adb devices
```

For writable permission, If use api >= 29, Run below first
```
Library/Android/sdk/platform-tools/adb -s emulator-5554 root
Library/Android/sdk/platform-tools/adb -s emulator-5554 shell avbctl disable-verification
Library/Android/sdk/platform-tools/adb -s emulator-5554 reboot
```

```
Library/Android/sdk/platform-tools/adb -s emulator-5554 root
Library/Android/sdk/platform-tools/adb -s emulator-5554 remount
```

Then you can pull or push file to android
```
Library/Android/sdk/platform-tools/adb -s emulator-5554 pull /etc/hosts ./
Library/Android/sdk/platform-tools/adb -s emulator-5554 push ./hosts /etc/hosts
```