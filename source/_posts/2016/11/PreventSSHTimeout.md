---
title: Prevent SSH timeout
tags:
  - SSH
date: 2016-11-20
---

SSH session timeouts due to inactivity are annoying. Here’s how to keep them alive and prevent the SSH timeout:

<!-- more -->

Preventing SSH timeouts is done by sending a “null packet” between the client and the server at a specified interval that is smaller than the timeout value. It doesn’t matter if the packet is sent from the client or the server, as long as there is some communication going on between the two.

If you setup your SSH client to send the “null packets”, you’ll prevent timeouts on all the SSH connections you make from your computer. If you setup the SSH server to send the “null packets”, you’ll prevent timeouts on all the SSH connections every client makes to the server. Fortunately, the setups are not exclusive, so you may setup both your client and all your servers and everything will run smoothly.

### Prevent SSH timeout on the client side
If you’re on Mac or Linux, you can edit your local SSH config file in~/.ssh/config and add the following line:
```
ServerAliveInterval 120
```
This will send a “null packet” every 120 seconds on your SSH connections to keep them alive.

### Prevent SSH timeout on the server side
If you’re a server admin, you can add the following to your SSH daemon config in /etc/ssh/sshd_config on your servers to prevent the clients to time out – so they don’t have to modify their local SSH config:
```
ClientAliveInterval 120
ClientAliveCountMax 720
```
This will make the server send the clients a “null packet” every 120 seconds and not disconnect them until the client have been inactive for 720 intervals (120 seconds * 720 = 86400 seconds = 24 hours).


