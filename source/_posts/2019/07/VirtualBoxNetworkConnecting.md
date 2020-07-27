---
title: VirtualBox Network Connecting
tags:
  - VirtualBox
date: 2019-07-01
---

# Bridge connecting
Simply choose `Setting => Bridged Adapter`

# NAT connecting
It need add two network adapter, nat and host only
- `Preferences => Network` to add an Nat network
- `Settings => Network => Adapter 1` chose NAT and keep default configs
- Click 'Tools => Network => Create' on the left top of the software window, Create a network
- `Settings => Network => Adapter 2` select Host-only Adapter and keep default settings
- If your vm had started, restart the network, `systemct restart network`