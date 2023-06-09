---
title: Grafana Install
tags:
  - Linux
  - Grafana
date: 2023-04-28
---

<!-- more -->

Ref [link](https://grafana.com/docs/grafana/latest/setup-grafana/installation/debian/#2-start-the-server)

```
sudo apt-get install -y apt-transport-https
sudo apt-get install -y software-properties-common wget
sudo wget -q -O /usr/share/keyrings/grafana.key https://apt.grafana.com/gpg.key
echo "deb [signed-by=/usr/share/keyrings/grafana.key] https://apt.grafana.com stable main" | sudo tee -a /etc/apt/sources.list.d/grafana.list
sudo apt-get update
sudo apt-get install grafana
```