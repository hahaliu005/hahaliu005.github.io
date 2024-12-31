---
title: OpenWRT Install
tags:
  - openwrt
  - Linux
date: 2024-02-16
---

## For USB content.
Image write tool: [balenaetcher](https://etcher.balena.io)
Image file: build by this [link](https://openwrt.ai)

## Write image file to nanopi r2s use image write tool.

## Set dnsmasq for test domains
'System -> Advance Settings -> dnsmasq' add below setting
```
address=/.test/192.168.0.200
```

## Set openclash.
'Plugin Settings - Operation mode - Select Mode' check fake-ip
'Plugin Settings - Operation mode - Proxy Mode' check Global Proxy Mode
'Plugin Settings - Operation mode - Bypass Gateway Compatible' must be checked

## Start openclash.

## Open Panel , Select correct proxy source.