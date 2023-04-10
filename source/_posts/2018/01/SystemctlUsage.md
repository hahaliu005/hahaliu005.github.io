---
title: Systemctl Usage
tags:
  - Systemctl
  - Linux
date: 2018-01-20
---

<!-- more -->

Ref [link](https://www.ruanyifeng.com/blog/2016/03/systemd-tutorial-commands.html)

Practice [link](https://www.ruanyifeng.com/blog/2016/03/systemd-tutorial-part-two.html)

Enable service
```
systemctl enable serviceName
```

Disable service
```
systemctl disable serviceName
```

Start service
```
systemctl start serviceName
```

Restart service
```
systemctl restart serviceName
```

Follow service log
```
sudo journalctl -u nginx.service -f
```