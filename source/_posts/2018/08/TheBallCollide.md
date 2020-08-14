---
title: The ball collide
tags:
  - Algorithm
  - PHP
date: 2018-08-05
---

When a ball or something run along a line , some time later , it will arrive the sideline , then rebound and continue to run until arrive the next sideline . This condition just like the graphic below :

<!-- more -->

![](01.gif)

For example , one little ball in a rectangle area , start at coordinate (startX,startY) , it move face to coordinate(endX,endY) , and it will continue to run until (x1,y1) , then rebound and move to (x2,y2) , then ...... it will do this loop forever if you don't want it to stop .

Then the core function just like this : this function return the coordinate where the ball will arrive to next time .

```php
/*
* get the next coordinate 
* startX,startY  the start point coordiante
* endX,endY  the end point coordiante
* width,height the rectangle area width and height
* return an array with x and y ,note: the x and y hava large odds 
* will return float , for the need ,can parse it to int
*/
function getDestination(startX,startY,endX,endY,width,height){
	//if the start and end point at the same point
	if(startX == endX && startY == endY){
		return new Array(width/2,height/2);
	}
	//start and end point in the rectangle area 
	if((endX < width && endX > 0) && (endY < height && endY > 0)){
		var radianUp = endY - startY;
		var radianDown = endX - startX;
		if(radianDown == 0){
			if(radianUp < 0){
				return new Array(endX,0);
			}else{
				return new Array(endX,height);
			}
		}
		if(radianUp == 0){
			if(radianDown > 0){
				return new Array(width,endY);
			}else{
				return new Array(0,endY);
			}
		}
		var radian = radianUp/radianDown;
		if(radianUp < 0){
			if(radianDown > 0){
				var y = radian * (width - endX) + endY;
				if(y < 0){
					return new Array(1 / radian * (0 - endY) + endX,0);
				}else{
					return new Array(width,y);
				}
			}else{
				var y = radian * (0 - endX) + endY;
				if(y < 0){
					return new Array(1 / radian * (0 - endY) + endX,0);
				}else{
					return new Array(0,y);
				}
			}
		}else{
			if(radianDown > 0){
				var y = radian * (width - endX) + endY;
				if(y > height){
					return new Array(1 / radian * (height - endY) + endX,height);
				}else{
					return new Array(width,y);
				}
			}else{
				var y = radian * (0 - endX) + endY;
				if(y > height){
					return new Array(1 / radian * (height - endY) + endX,height);
				}else{
					return new Array(0,y);
				}
			}
		}
	}else if(endX <= 0) {//on the x left sideline
		var radianUp = endY - startY;
		var radianDown = endX + startX;
		if(radianDown == 0 || radianUp == 0){
			return new Array(width,endY);
		}
		var radian = radianUp/radianDown;
		if(radianUp < 0){
			var y = radian * (width - endX) + endY;
			if(y < 0){
				return new Array(1 / radian * (0 - endY) + endX,0);
			}else{
				return new Array(width,y);
			}
		}else{
			var y = radian * (width - endX) + endY;
			if(y > height){
				return new Array(1 / radian * (height - endY) + endX,height);
			}else{
				return new Array(width,y);
			}
		}
	}else if(endX >= width){//on the x right sideline
		var radianUp = endY - startY;
		var radianDown = endX - 2 * width + startX;
		if(radianDown == 0 || radianUp == 0){
			return new Array(0,endY);
		}
		var radian = radianUp/radianDown;
		if(radianUp < 0){
			var y = radian * (0 - endX) + endY;
			if(y < 0){
				return new Array(1 / radian * (0 - endY) + endX,0);
			}else{
				return new Array(0,y);
			}
		}else{
			var y = radian * (0 - endX) + endY;
			if(y > height){
				return new Array(1 / radian * (height - endY) + endX,height);
			}else{
				return new Array(0,y);
			}
		}
	}else if(endY <= 0){//on the y up sideline
		var radianUp = endY + startY;
		var radianDown = endX - startX;
		if(radianDown == 0 || radianUp == 0){
			return new Array(endX,height);
		}
		var radian = radianUp/radianDown;
		if(radianDown > 0){
			var y = radian * (width - endX) + endY;
			if(y > height){
				return new Array(1 / radian * (height - endY) + endX,height);
			}else{
				return new Array(width,y);
			}
		}else{
			var y = radian * (0 - endX) + endY;
			if(y > height){
				return new Array(1 / radian * (height - endY) + endX,height);
			}else{
				return new Array(0,y);
			}
		}
	}else if(endY >= height){//on the y bottom sideline
		var radianUp = endY - 2 * height - startY;
		var radianDown = endX - startX;
		if(radianDown == 0 || radianUp == 0){
			return new Array(endX,0);
		}
		var radian = radianUp/radianDown;
		if(radianDown > 0){
			var y = radian * (width - endX) + endY;
			if(y < 0){
				return new Array(1 / radian * (0 - endY) + endX,0);
			}else{
				return new Array(width,y);
			}
		}else{
			var y = radian * (0 - endX) + endY;
			if(y < 0){
				return new Array(1 / radian * (0 - endY) + endX,0);
			}else{
				return new Array(0,y);
			}
		}
	}
}
```