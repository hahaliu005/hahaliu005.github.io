---
title: Kubespray Install Kubernetes
tags:
  - Kubespray
  - Kubernetes
date: 2019-06-25
---

## Update system
```
yum install epel-release -y
yum update -y
```
<!-- more -->

## Install python3
```
yum install -y https://centos7.iuscommunity.org/ius-release.rpm
yum install -y python36u python36u-libs python36u-devel python36u-pip
```
## For ssh-copy-id
```
ssh-keygen -C <nodeName>
ssh-copy-id root@<ip>
```
## Download kubespray repo and check to the newest tag version
```
git clone https://github.com/kubernetes-sigs/kubespray.git && \
cd kubespray && \
git checkout v2.10.4
```
## Install requirement
```
pip3.6 install -r requirements.txt
```
```
cp -r inventory/sample inventory/mycluster
declare -a IPS=(<ip1> <ip2>)
CONFIG_FILE=inventory/mycluster/hosts.yml python3.6 contrib/inventory_builder/inventory.py ${IPS[@]}
```
## Start Deploy
```
ansible-playbook -i inventory/mycluster/hosts.yml cluster.yml -b -v
```
