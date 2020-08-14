---
title: Cron Usage
tags:
  - Linux
date: 2016-11-22
---

### 以特定用户运行cron定时任务
运行以下命令, 进入编辑界面编辑cron定时任务
crontab -u [username] -e

<!-- more -->

注意事项: 所有执行文件路径, 应用路径最好均使用绝对路径
如果定时任务未运行, 可查看定时任务日志, 看是否报错
tail -f /var/log/cron