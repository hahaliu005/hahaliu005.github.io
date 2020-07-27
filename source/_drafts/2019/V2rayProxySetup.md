---
title: V2ray Proxy Setup
tags:
  - V2ray
  - Linux
date: 2019-04-01
---

The official site is [Here](https://www.v2ray.com/)
# For the service
* can use to install v2ray server directly
```
bash <(curl -L -s https://install.direct/go.sh)
```

<!-- more -->

* The date must be the same, so update date in the
* Install docker
* Create and edit file /etc/v2ray/config.json
* Use `cat /proc/sys/kernel/random/uuid` to generate uuid
```
{
  "log": {
    "access": "/var/log/v2ray/access.log",
    "error": "/var/log/v2ray/error.log",
    "loglevel": "none"
  },
  "inbound": {
    "port": 15714,
    "protocol": "vmess",
    "settings": {
      "clients": [
        {
          "id": "0b075668-aad9-9171-5a81-6b5d5e0a613e",
          "alterId": 64
        }
      ]
    }
  },
  "outbound": {
    "protocol": "freedom",
    "settings": {
        "domainStrategy": "UseIP"
    }
  },
  "dns": {
    "hosts": {
      "my.domain.com": "1,2,3,4"
    },
    "servers": [
        "8.8.8.8",
        "8.8.4.4",
        "localhost"
    ]
  }
}
```
* Use the official image and run it
```
docker run -d -p 15714:15714 --restart=always --name v2ray \
  -v /etc/v2ray/config.json:/etc/v2ray/config.json \
  -v /var/log/v2ray/:/var/log/v2ray/ \
  v2ray/official
```
# For the client
* Download and uncompress the latest stable release for you platform [Here](https://github.com/v2ray/v2ray-core/releases) 
* Edit config file in release directory `./config.json`
```
{
  "log": {
    "loglevel": "none",
    "access": "/var/log/v2ray/access.log",
    "error": "/var/log/v2ray/error.log"
  },
  "inbound": {
    "port": 1080,
    "protocol": "socks",
    "domainOverride": ["tls","http"],
    "settings": {
      "auth": "noauth",
      "udp": true
    }
  },
  "outbound": {
    "protocol": "vmess",
    "settings": {
      "vnext": [
        {
          "address": "<Your server ip or domain>",
          "port": 15714,
          "users": [
            {
              "id": "0b075668-aad9-9171-5a81-6b5d5e0a613e",
              "alterId": 64
            }
          ]
        }
      ]
    }
  },
  "outboundDetour": [
    {
      "protocol": "freedom",
      "settings": {},
      "tag": "direct"
    }
  ],
  "routing": {
    "strategy": "rules",
    "settings": {
      "domainStrategy": "IPIfNonMatch",
      "rules": [
        {
          "type": "chinasites",
          "outboundTag": "direct"
        },
        {
          "type": "chinaip",
          "outboundTag": "direct"
        },
		{
			"type": "field",
			"ip": [
				"0.0.0.0/8",
				"10.0.0.0/8",
				"100.64.0.0/10",
				"127.0.0.0/8",
				"169.254.0.0/16",
				"172.16.0.0/12",
				"192.0.0.0/24",
				"192.0.2.0/24",
				"192.168.0.0/16",
				"198.18.0.0/15",
				"198.51.100.0/24",
				"203.0.113.0/24",
				"::1/128",
				"fc00::/7",
				"fe80::/10"
			],
			"outboundTag": "direct"
		}
      ]
    }
  }
}
```
* Run v2ray in the release directory, `./v2ray`
* Set proxy for the network, System Preferences => Network => Wi-Fi => Advanced => Proxies => SOCKS Proxy => 127.0.0.1:1080
* Now you can access websites use browser
# For ssh proxy
* Edit config file `~/.ssh/config`, add proxy setting where you want to use proxy
```
Host dev
   ProxyCommand=nc -X 5 -x 127.0.0.1:1080 %h %p
   HostName 1,2,3,4
   Port 2222
   User root
```
* Now `ssh dev` is connect over the proxy 
* If you want use proxy in command line, can use variables
```
ALL_PROXY=socks5://127.0.0.1:1080 brew update
```