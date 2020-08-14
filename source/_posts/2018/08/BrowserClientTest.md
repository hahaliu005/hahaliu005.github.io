---
title: Browser Client Test
tags:
  - Javascript
date: 2018-08-05
---

Since now , there are so many browser and system that we must think of the difference of these thing use javascript to do the same thing , and at first , we should check 

<!-- more -->

the info of this client . Then I find a client test code . we can use it at first in our code :

```javascript
var client = function(){
	//use of engine
	var engine = {
		ie:0,
		gecko:0,
		webkit:0,
		khtml:0,
		opera:0,
		
		//the full version
		ver:null
	};
	//browser
	var browser = {
		//main browser
		ie:0,
		firefox:0,
		konq:0,
		opera:0,
		chrome:0,
		safari:0,
		
		//the version
		ver:null
	}
	//platform,device and system
	var system = {
		win:false,
		mac:false,
		xll:false,
		//movable device
		iphone:false,
		ipod:false,
		nokiaN:false,
		winMobile:false,
		macMobile:false,
		//game system
		wii:false,
		ps:false
	}
	//test engine and browser
	var ua = navigator.userAgent;
	if(window.opera){
		engine.ver = browser.ver = window.opera.version();
		engine.opera = browser.opera = parseFloat(engine.ver);
	}else if(/AppleWebKit\/(S+)/.test(ua)){
		engine.ver = RegExp["$1"];
		engine.webkit = parseFloat(engine.ver);
		//check chrome or safari
		if(/Chrome\/(\S+)/.test(ua)){
			browser.ver = RegExp["$1"];
			browser.chrome = parseFloat(browser.ver);
		}else if(/Version\/(\S+)/.test(ua)){
			browser.ver = RegExp["$1"];
			browser.safari = parseFloat(browser.ver);
		}else{
			//defined the version
			var safariVersion = 1;
			if(engine.webkit < 100){
				safariVersion = 1;
			}else if(engine.webkit < 312){
				safariVersion = 1.2;
			}else if(engine.webkit < 412 ){
				safariVersion = 1.3;
			}else{
				safariVersion = 2;
			}
			browser.safari = browser.ver = safariVersion;
		}
	}else if(/KHTML\/(\S+)/.test(ua) || /Konqueror\/([^;]+)/.test(ua)){
		engine.ver = browser.ver = RegExp["$1"];
		engine.khtml = browser.konq = parseFloat(engine.ver);
	}else if(/rv:([^\)]+)\) Gecko\/\d{8}/.test(ua)){
		engine.ver = RegExp["$1"];
		engine.gecko = parseFloat(engine.ver);
		//if this is firefox
		if(/Firefox\/(\S+)/.test(ua)){
			browser.ver = RegExp["$1"];
			browser.firefox = parseFloat(browser.ver);
		}
	}else if(/MSIE ([^;]+)/.test(ua)){
		engine.ver = browser.ver = RegExp["$1"];
		engine.ie = browser.ie = parseFloat(engine.ver);
	}
	//check browser
	browser.ie = engine.ie;
	browser.opera = engine.opera;
	//check platform
	var p = navigator.platform;
	system.win = p.indexOf("Win") == 0;
	system.mac = p.indexOf("Mac") == 0;
	system.xll = (p == "Xll") || (p.indexOf("Linux") == 0);
	
	//check windows system
	if(system.win){
		if(/Win(?:dows )?([^do]{2})\s?(\d+\.\d+)?/.test(ua)){
			if(RegExp["$1"] == "NT"){
				switch(RegExp["$2"]){
					case "5.0":
						system.win = "2000";
						break;
					case "5.1":
						system.win = "XP";
						break;
					case "6.0":
						system.win = "Vista";
						break;
					default:
						system.win = "NT";
						break;
				}
			}else if(RegExp["$1"] == "9x"){
				system.win = "ME";
			}else{
				system.win = RegExp["$1"];
			}
		}
	}
	//movable device
	system.iphone = ua.indexOf("iPhone") > -1;
	system.ipod = ua.indexOf("ipod") > -1;
	system.nokiaN = ua.indexOf("NokiaN") > -1;
	system.winMobile = (system.win == "CE");
	system.macMobile = (system.iphone || system.ipod);
	//game system
	system.wii = ua.indexOf("Wii") > -1;
	system.ps = /playstation/i.test(ua);
	
	//return these object
	return {
		engine:engine,
		browser:browser,
		system:system
	};
}();
```
And we can use it like this :

```
if(client.browser.ie && client.broswer.ver > 6.0){
	//some code compatible to ie 6.0+
}
```

> The code are reference from professional javascript for web developers by Nicholas C.Zakas