---
title: Vagrantfile Example
tags:
  - Vagrant
date: 2018-10-28
---

Some common example about vagrantfile configuration.

<!-- more -->

```
# -*- mode: ruby -*-
# vi: set ft=ruby :
#

machineCount = 3

Vagrant.configure("2") do | config |
  (1..machineCount).each do | count |
    countStr = String(count)
    name = 'node' + countStr
    config.vm.define name do | machine |
      machine.vm.box = "centos/7"
      machine.vm.box_check_update = false
      machine.vm.hostname = name

      machine.vm.network :private_network, ip: '192.168.1.10' + countStr
      machine.vm.network "forwarded_port", guest: 80, host: '909' + countStr

      machine.vm.provider "virtualbox" do | v |
        v.name = name
	v.linked_clone = true
        v.memory = 4096
        v.cpus = 4
      end
    end
  end
end
```

```
# -*- mode: ruby -*-
# vi: set ft=ruby :
#

servers = [
	{ :name => "kube-node1", :ip => "172.27.129.105" },
	{ :name => "kube-node2", :ip => "172.27.129.111" },
	{ :name => "kube-node3", :ip => "172.27.129.112" }
]

Vagrant.configure("2") do | config |
  servers.each do | server |
    config.vm.define server[:name] do | machine |
      machine.vm.box = "centos/7"
      machine.vm.box_check_update = false
      machine.vm.hostname = server[:name]

      machine.vm.network :private_network, ip: server[:ip]

      machine.vm.provider "virtualbox" do | v |
        v.name = server[:name]
        v.linked_clone = true
        v.memory = 4096
        v.cpus = 4
      end
    end
  end
end
```