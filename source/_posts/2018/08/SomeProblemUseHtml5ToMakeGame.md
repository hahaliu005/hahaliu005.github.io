---
title: Some Problem Use Html5 To Make Game
tags:
  - html
date: 2018-08-05
---

Some Problem Use Html5 To Make Game

<!-- more -->

### Time shaft
calculate fram count every seconds
```javascript
var fps = 60,
	interval = 1000 / fps,
	now = Date.now(),
	lastSecond = Math.floor(now / 1000),
	frameCount = Math.ceil((now % 1000) / fps) ;
window.setInterval(function(){
	var now = Date.now(),
		second = Math.floor(now / 1000),
		milliSecond = now % 1000;
	if(second != lastSecond){
		frameCount = 0;
		lastSecond = second;
	}
	if(frameCount < fps && milliSecond >= frameCount * interval){
		frameCount ++;
		//do render
	}
},1);

```

### Keyboard press status

```javascript
var specialKeys = {},
	keyboardHandler = function (e){
		var isDown = e.type === 'keyDown';
		e.stopPropagation();
		e.preventDefault();
		if(!specialKeys[e.keyCode]){
			keyStates[e.keyCode] = isDown;
		}
	},
	blurHandler = function (){
		keyStates = {};
	};
specialKeys[16] = specialKeys[17] = specialkeys[18] = true;
canvas.addEventListener('keydown',keyboardHandler,false);
canvas.addEventListener('keyup',keyboardHandler,false);
canvas.addEventListener('blur',blurHandler,false);
```

### Mouse event
Cache every mouse event , deal cache every render . and clear dealed event in the cache.

```javascript
var MAX = 30,head=0,tail=0,
	history = [],
	mousemoveHandler = function (e){
		var last = history[head];
		if(e.screenX == last.screenX && e.screenY == last.screenY){
			//ignore duplication
			return;
		}
		
		head = (head + 1) % MAX;
		history[head] = {
			//store attr in event..	
		};
		if(tail == head){
			tail = (tail + 1) % MAX;
		}
	},
	traverseHistory = function (cb){
		var len = (head - tail + MAX) % MAX ,i;
		for(i = 0,i<len;i++){
			cb(history[(i + tail) % MAX]);
		}
	};
canvas.addEventListener('mousemove',mousemoveHandler,false);
```

### Touch device support
same to up
```javascript
var MAX = 30,head = 0,tail = 0,
	history = [],
	touchHandler = function (){
		var last = history[head],
			touch = e.touches[0];
		e.preventDefault();
		if(touch.screenX == last.screenX && touch.screenY == last.screenY){
			//ignore duplication
			return;
		}
		head = (head + 1) % MAX;
		history[head] = {
				//store attr in event..
		};
		if(tail == head){
			tail = (tail + 1) % MAX;
		}
	};
canvas.addEventListener('touchstart',touchHandler,false);
canvas.addEventListener('touchmove',touchHandler,false);
```