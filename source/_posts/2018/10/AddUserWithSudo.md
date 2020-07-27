---
title: Add User With Sudo
tags:
  - Linux
date: 2018-10-21
---

If you want to add an new user with sudo , you can use:
```
$ sudo adduser username
```

<!-- more -->

This will add an user 

Edit the sudo file:

* `sudo visudo`
* ctrl+o   is write
* ctrl+c   is cancel
* ctrl+x   is exit

And there was an add exist group to exists user command:
```
$ sudo usermod -a -G groupname username
```

### If you want to add a user with sudo, Just add this user to wheel group.
```
adduser [username]
usermod -a -G wheel [username]
```
