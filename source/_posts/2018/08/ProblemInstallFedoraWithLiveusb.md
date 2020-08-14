---
title: Problem Install Fedora With Liveusb
tags:
  - Linux
date: 2018-08-05
---

I use ubuntu for a while , yesterday my colleague recommend fedora to me , so I try to install fedora to my lenovo Y450 . 

<!-- more -->

The download are successfully , The file name is 'Fedora-16-i686-Live-Desktop.iso' , I use UltraISO to make liveUSB , don't forget to run UltraISO with administrator , chose the ISO file , Then click start->write to hardware image->open a write window->click write . Then the liveUSB will be created . 

After liveUSB created , reboot system , press F12 to boot from USB HDD+ , then go the fedora install window , click 'install fedora' , Then I han a ploblem when process next , It enter a debug mode display an error below : 

```
dracut Warning:No root device live:"/dev/disk/by-label/Fedora-16-x86-64-Live-Desktop.is"found
Dropping to debug shell
sh:can't access tty:job control turned off
dracut:/#
```

For solove this problem , I reboot system to windows 7 , change USB name to FEDORA-16 , and enter USB file to change some config file , just as *.conf,*.cfg,*.sys , replace all the 'Fedora-16-x86-64-Live-Desktop.iso' to 'FEDORA-16' , Then reboot and process the fedora install , solove this ploblem , the next step of install is so simple .