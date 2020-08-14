---
title: Tetris Game With JQuery+CSS
tags:
  - JQuery
  - CSS
date: 2018-09-05
---

I use jQuery make a tetris game , and I only test it on firefox4.0 , for run correctly on other browser , I will write more .

<!-- more -->

This is a game printscreen:
![](01.gif)

And this is the total file code , I really think it's too long , but who care!!

```html
<html>
<head>
<script type="text/javascript" src="http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4.min.js"></script>
<style type="text/css" >
td{
	width:20px;
	height:20px;
	border:1px solid #ddd;
}
#main {
    margin: auto;
    width: 1000px;
}
#left {
    float: left;
    height: 400px;
    width: 300px;
}
#right {
    float: left;
}
.cell{
	background-color:#ccc;
}
.green{
	background-color:green;
}
.blue{
	background-color:blue;
}
.mediumorchid{
	background-color:mediumorchid;
}
.coral{
	background-color:coral;
}
.darkorange{
	background-color:darkorange;
}
.darkviolet{
	background-color:darkviolet;
}
.goldenrod{
	background-color:goldenrod;
}
.salmon{
	background-color:salmon;
}
.maroon{
	background-color:maroon;
}
.lightslategray{
	background-color:lightslategray;
}
 
</style>
</head>
<body>
<div id = 'main'>
	<div id = 'left'>
		<p>please use firefox4.0</p>
		<p>move left : J</p>
		<p>move righ : L</p>
		<p>speed movedown : K</p>
		<p>clockwise rotation : F</p>
		<p>anticlockwise rotation : S</p>
		<p id="score">score:<span></span></p>
		<p id="stage">stage:<span></span></p>
		<p><input type="button" id="start" value="start"/></p>
		<p id="preBlock"></p>
	</div>
	<div id = 'right'></div>
</div>
<script type="text/javascript">
$().ready(function(){
 
//initialize
 
 
//rows and cols
var gheight = 20;
var gwidth = 10;
//keyboard value
var leftKey = 106; //J
var rightKey = 108;//L
var speedKey = 107;//I
var spin = 102;//F
var rSpin = 115;//S
 
//difficulty
var stage = 1;
 
//block move down speed(millisecond)
var speed = 1000 - (stage - 1) * 100;
//clear rows number,every ten rows level up 1
var clearNum = 0;
//total score
var score = 0;
//based on clear rows to set score
var scoreArr = new Array([100],[300],[600],[1200]);
//blocks's color style
var color = new Array('cell','green','blue','mediumorchid','coral','darkorange','darkviolet','goldenrod','salmon','maroon','lightslategray');
//the block array
var block1 = new Array(
[[[0],[0],[0],[0]],[[1],[1],[1],[1]],[[0],[0],[0],[0]],[[0],[0],[0],[0]]],
[[[0],[1],[0],[0]],[[0],[1],[0],[0]],[[0],[1],[0],[0]],[[0],[1],[0],[0]]],
[[[0],[0],[0],[0]],[[1],[1],[1],[1]],[[0],[0],[0],[0]],[[0],[0],[0],[0]]],
[[[0],[1],[0],[0]],[[0],[1],[0],[0]],[[0],[1],[0],[0]],[[0],[1],[0],[0]]]
);
var block2 = new Array(
[[[0],[0],[0],[0]],[[1],[0],[0],[0]],[[1],[1],[1],[0]],[[0],[0],[0],[0]]],
[[[0],[1],[0],[0]],[[0],[1],[0],[0]],[[1],[1],[0],[0]],[[0],[0],[0],[0]]],
[[[0],[0],[0],[0]],[[1],[1],[1],[0]],[[0],[0],[1],[0]],[[0],[0],[0],[0]]],
[[[1],[1],[0],[0]],[[1],[0],[0],[0]],[[1],[0],[0],[0]],[[0],[0],[0],[0]]]
);
var block3 = new Array(
[[[0],[0],[0],[0]],[[0],[1],[0],[0]],[[1],[1],[1],[0]],[[0],[0],[0],[0]]],
[[[0],[0],[1],[0]],[[0],[1],[1],[0]],[[0],[0],[1],[0]],[[0],[0],[0],[0]]],
[[[1],[1],[1],[0]],[[0],[1],[0],[0]],[[0],[0],[0],[0]],[[0],[0],[0],[0]]],
[[[1],[0],[0],[0]],[[1],[1],[0],[0]],[[1],[0],[0],[0]],[[0],[0],[0],[0]]]
);
var block4 = new Array(
[[[0],[0],[0],[0]],[[0],[0],[1],[0]],[[1],[1],[1],[0]],[[0],[0],[0],[0]]],
[[[1],[1],[0],[0]],[[0],[1],[0],[0]],[[0],[1],[0],[0]],[[0],[0],[0],[0]]],
[[[0],[0],[0],[0]],[[1],[1],[1],[0]],[[1],[0],[0],[0]],[[0],[0],[0],[0]]],
[[[1],[0],[0],[0]],[[1],[0],[0],[0]],[[1],[1],[0],[0]],[[0],[0],[0],[0]]]
);
var block5 = new Array(
[[[1],[0],[0],[0]],[[1],[1],[0],[0]],[[0],[1],[0],[0]],[[0],[0],[0],[0]]],
[[[0],[0],[0],[0]],[[0],[1],[1],[0]],[[1],[1],[0],[0]],[[0],[0],[0],[0]]],
[[[1],[0],[0],[0]],[[1],[1],[0],[0]],[[0],[1],[0],[0]],[[0],[0],[0],[0]]],
[[[0],[0],[0],[0]],[[0],[1],[1],[0]],[[1],[1],[0],[0]],[[0],[0],[0],[0]]]
);
var block6 = new Array(
[[[0],[1],[0],[0]],[[1],[1],[0],[0]],[[1],[0],[0],[0]],[[0],[0],[0],[0]]],
[[[0],[0],[0],[0]],[[1],[1],[0],[0]],[[0],[1],[1],[0]],[[0],[0],[0],[0]]],
[[[0],[1],[0],[0]],[[1],[1],[0],[0]],[[1],[0],[0],[0]],[[0],[0],[0],[0]]],
[[[0],[0],[0],[0]],[[1],[1],[0],[0]],[[0],[1],[1],[0]],[[0],[0],[0],[0]]]
);
var block7 = new Array(
[[[0],[0],[0],[0]],[[0],[1],[1],[0]],[[0],[1],[1],[0]],[[0],[0],[0],[0]]],
[[[0],[0],[0],[0]],[[0],[1],[1],[0]],[[0],[1],[1],[0]],[[0],[0],[0],[0]]],
[[[0],[0],[0],[0]],[[0],[1],[1],[0]],[[0],[1],[1],[0]],[[0],[0],[0],[0]]],
[[[0],[0],[0],[0]],[[0],[1],[1],[0]],[[0],[1],[1],[0]],[[0],[0],[0],[0]]]
);
var block = new Array(block1,block2,block3,block4,block5,block6,block7);
//the block offset
var gX = Math.floor(gwidth/2)-2;
var gY = 0;
var aX = 0;
var aY = 0;
 
var intervalId = 0;
var randBlockArray = new Array();
var randBlock = new Array();
 
//initialize global array
var gElement = new Array();
var gStyle = new Array();
for(i=0;i<gheight+1;i++){
	gElement[i] = new Array();
	gStyle[i] = new Array();
	for(j=0;j<gwidth+2;j++){
		gElement[i][j] = 0;
		gStyle[i][j] = 0;
	}
}
for(i=0;i<gheight+1;i++){
	gElement[i][0] = 1;
	gElement[i][gwidth+1] = 1;
	gStyle[i][0] = 1;
	gStyle[i][gwidth+1] = 1;
}
for(j=0;j<gwidth+2;j++){
	gElement[gheight][j] = 1;
	gStyle[gheight][j] = 1;
}
//this string to generate game area
var garea='<table class="main">';
for(i=0;i<gheight;i++){
	garea += '<tr class="rows">';
	for(j=0;j<gwidth;j++){
		garea += '<td ' + 'id="' + 'cell_' + i + '_' + j + '"></td>';
	}
	garea += '</tr>';
}
garea += '</table>';
$('#right').append(garea);
$('#score span').html(score);
$('#stage span').html(stage);
var preRandBlockArray = randBlocks();
var randBlockArray = randBlocks();
var randBlock = randBlockArray[0];
var minX = getMinX(randBlock);
var minY = getMinY(randBlock);
var maxX = getMaxX(randBlock);
var maxY = getMaxY(randBlock);
preBlockFlush();
flush();
$('#start').click(function(){
	try{
		if(intervalId != 0){
			throw "game had started!";
		}
		aX = gX;
		aY = gY;
		$(window).bind({
			keypress:function(event){
				if(event.which == leftKey){
					moveLeft();
				}else if(event.which == rightKey){
					moveRight();
				}else if (event.which == spin){
					spins();
				}else if(event.which == rSpin){
					rSpins();
				}else if(event.which == speedKey){
					moveDown();
				}
			},
		})
		for(i=minY;i<=maxY;i++){
			for(j=minX;j<=maxX;j++){
				if(randBlock[i][j] == 1){
					gElement[aY+i][aX+j+1] = 1;
					gStyle[aY+i][aX+j+1] = randBlockArray[3];
				}
			}
		}
		preBlockFlush();
		flush();
		intervalId = setInterval(moveDown,speed);
	}catch(err){
		alert(err);
	}
})
 
//block move down one grid
function moveDown(){
	aY++;
	var flag = 1;
	
	for(i=minX;i<=maxX;i++){
		for(j=maxY;j>=minY;j--){
			if(randBlock[j][i] == 1){
				if(gElement[aY+j][aX+i+1] == 1){
					flag = 0;
				}
				break;
			}
		}
		if(! flag){
			break;
		}
	}
	
	if(! flag){//when block cann't move down
		clearInterval(intervalId);
		clearRows();
		randBlockArray = preRandBlockArray.slice(0);
		preRandBlockArray = randBlocks();
		randBlock = randBlockArray[0];
		minX = getMinX(randBlock);
		minY = getMinY(randBlock);
		maxX = getMaxX(randBlock);
		maxY = getMaxY(randBlock);
		aX = gX;
		aY = gY;
		if(! gameOver()){
			preBlockFlush();
			intervalId = setInterval(moveDown,speed);
		}else{
			alert('gameover');
		}
	}else{//when block can move down 
		//clear
		for(i=minY;i<=maxY;i++){
			for(j=minX;j<=maxX;j++){
				if(randBlock[i][j] == 1){
					gElement[aY+i-1][aX+j+1] = 0;
					gStyle[aY+i-1][aX+j+1] = 0;
				}
			}
		}
		//add
		for(i=minY;i<=maxY;i++){
			for(j=minX;j<=maxX;j++){
				if(randBlock[i][j] == 1){
					gElement[aY+i][aX+j+1] = 1 ;
					gStyle[aY+i][aX+j+1] = randBlockArray[3] ;
				}
			}
		}
		flush();
	}
}
//check whether game is over
function gameOver(){
	for(i=maxY;i>=minY;i--){
		for(j=minX;j<=maxX;j++){
			if(gElement[aY+i][aX+j+1] & randBlock[i][j]){
				return true;
			}
		}
	}
	return false;
}
//move left
function moveLeft(){
	aX--;
	var flag = 1;
	for(i=minY;i<=maxY;i++){
		for(j=minX;j<=maxX;j++){
			if(randBlock[i][j] == 1){
				if(gElement[aY+i][aX+j+1] == 1){
					flag = 0;
				}
				break;
			}
		}
		if(! flag){
			break;
		}
	}
	if(flag){
		for(i=minY;i<=maxY;i++){
			for(j=minX;j<=maxX;j++){
				if(randBlock[i][j] == 1){
					gElement[aY+i][aX+j+2] = 0;
					gStyle[aY+i][aX+j+2] = 0;
				}
			}
		}
		for(i=minY;i<=maxY;i++){
			for(j=minX;j<=maxX;j++){
				if(randBlock[i][j] == 1){
					gElement[aY+i][aX+j+1] = 1;
					gStyle[aY+i][aX+j+1] = randBlockArray[3];
				}
			}
		}
		flush();
	}else{
		aX++;
	}
}
//random a block and return some data as an big array
function randBlocks(){
	var rand1 = Math.round(Math.random()*6)
	var randBlocks = block[rand1];
	var rand2 = Math.round(Math.random()*(randBlocks.length-1));
	var randStyle = Math.round(Math.random()*(color.length -2)) + 1;
	return new Array(randBlocks[rand2],rand1,rand2,randStyle);
}
//move right
function moveRight(){
	aX++;
	var flag = 1;
	for(i=minY;i<=maxY;i++){
		for(j=maxX;j>=minX;j--){
			if(randBlock[i][j] == 1){
				if(gElement[aY+i][aX+j+1] == 1){
					flag = 0;
				}
				break;
			}
		}
		if(! flag){
			break;
		}
	}
	if(flag){
		for(i=minY;i<=maxY;i++){
			for(j=minX;j<=maxX;j++){
				if(randBlock[i][j] == 1){
					gElement[aY+i][aX+j] = 0;
					gStyle[aY+i][aX+j] = 0;
				}
			}
		}
		for(i=minY;i<=maxY;i++){
			for(j=minX;j<=maxX;j++){
				if(randBlock[i][j] == 1){
					gElement[aY+i][aX+j+1] = 1;
					gStyle[aY+i][aX+j+1] = randBlockArray[3];
				}
			}
		}
		flush();
	}else{
		aX--;
	}
}
//block spin(clockwise)
function spins(){
	var offset = ((randBlockArray[2] - 1) == -1) ? 3 : (randBlockArray[2] - 1)
	var newElement = new Array();
	var newBlock = block[randBlockArray[1]][offset];
	var flag = 1;
	for(i=0;i<4;i++){
		newElement[i] = new Array();
		for(j=0;j<4;j++){
			if(randBlock[i][j] == 1){
				newElement[i][j] = 0;
			}else{
				newElement[i][j] = gElement[aY + i][aX + j + 1];
			}
			if(newBlock[i][j] & newElement[i][j]){
				flag = 0;
				break;
			}
		}
		if(! flag){
			break;
		}
	}
	if(flag){
		for(i=minX;i<=maxX;i++){
			for(j=minY;j<=maxY;j++){
				if(randBlock[j][i] == 1){
					gElement[aY+j][aX+i+1] = 0;
					gStyle[aY+j][aX+i+1] = 0;
				}
			}
		}
		randBlockArray = new Array(newBlock,randBlockArray[1],(offset),randBlockArray[3]);
		randBlock = newBlock;
		minX = getMinX(randBlock);
		minY = getMinY(randBlock);
		maxX = getMaxX(randBlock);
		maxY = getMaxY(randBlock);
		for(i=minX;i<=maxX;i++){
			for(j=minY;j<=maxY;j++){
				if(randBlock[j][i] == 1){
					gElement[aY+j][aX+i+1] = 1;
					gStyle[aY+j][aX+i+1] = randBlockArray[3];
				}
			}
		}
		flush();
	}
}
//block spin(anticlockwise)
function rSpins(){
	var offset = (randBlockArray[2] + 1) % 4;
	var newElement = new Array();
	var newBlock = block[randBlockArray[1]][offset];
	var flag = 1;
	for(i=0;i<4;i++){
		newElement[i] = new Array();
		for(j=0;j<4;j++){
			if(randBlock[i][j] == 1){
				newElement[i][j] = 0;
			}else{
				newElement[i][j] = gElement[aY + i][aX + j + 1];
			}
			if(newBlock[i][j] & newElement[i][j]){
				flag = 0;
				break;
			}
		}
		if(! flag){
			break;
		}
	}
	if(flag){
		for(i=minX;i<=maxX;i++){
			for(j=minY;j<=maxY;j++){
				if(randBlock[j][i] == 1){
					gElement[aY+j][aX+i+1] = 0;
					gStyle[aY+j][aX+i+1] = 0;
				}
			}
		}
		randBlockArray = new Array(newBlock,randBlockArray[1],(offset),randBlockArray[3]);
		randBlock = newBlock;
		minX = getMinX(randBlock);
		minY = getMinY(randBlock);
		maxX = getMaxX(randBlock);
		maxY = getMaxY(randBlock);
		for(i=minX;i<=maxX;i++){
			for(j=minY;j<=maxY;j++){
				if(randBlock[j][i] == 1){
					gElement[aY+j][aX+i+1] = 1;
					gStyle[aY+j][aX+i+1] = randBlockArray[3];
				}
			}
		}
		flush();
	}
}
//check clear rows
function clearRows(){
	var newElement = new Array();
	var newStyle = new Array();
	for(i=0;i<gheight+1;i++){
		newElement[i] = new Array();
		newStyle[i] = new Array();
		for(j=0;j<gwidth+2;j++){
			newElement[i][j] = 0;
			newStyle[i][j] = 0;
		}
	}
	for(i=0;i<gheight+1;i++){
		newElement[i][0] = 1;
		newStyle[i][0] = 1;
		newElement[i][gwidth+1] = 1;
		newStyle[i][gwidth+1] = 1;
	}
	for(j=0;j<gwidth+2;j++){
		newElement[gheight][j] = 1;
		newStyle[gheight][j] = 1;
	}
	var height = gheight;
	var r = 0;
	var flag = 1;
	for(i=gheight-1;i>=0;i--){
		flag = 1;
		for(j=1;j<=gwidth+1;j++){
			if(gElement[i][j] == 0){
				newElement[--height] = gElement[i];
				newStyle[height] = gStyle[i];
				flag = 0;
				break;
			}
		}
		if(flag){
			r++;
		}
	}
	if(r>0){
		score += parseInt(scoreArr[r-1]);
		$('#score span').html(score);
		clearNum += r;
		if(Math.ceil(clearNum / 10) > stage){
			stage = (stage + 1) % 10 ;
			speed = 1000 - (stage - 1) * 100;
			$('#stage span').html(stage);
		}
	}
	gElement = newElement;
	gStyle = newStyle;
	flush();
}
	
 
 
 
 
//flush game area
function flush(){
	for(i=0;i<gheight;i++){
		for(j=1;j<gwidth+1;j++){
			if(gElement[i][j] == 1){
				$('#cell_' + i + '_' + (j - 1)).attr('class',color[gStyle[i][j]]);
			}else if(gElement[i][j] == 0){
				$('#cell_' + i + '_' + (j - 1)).attr('class',color[0]);
			}
		}
	}
}
//flush block area
function preBlockFlush(){
	var preStr = '<table>';
	for(i=0;i<4;i++){
		preStr += '<tr>';
		for(j=0;j<4;j++){
			if(preRandBlockArray[0][i][j] == 1){
				preStr += '<td class="' + color[preRandBlockArray[3]] + '"></td>';
			}else{
				preStr += '<td class="cell"></td>';
			}
		}
		preStr += '</tr>';
	}
	preStr += '</table>';
	$('#preBlock').html(preStr);
}
 
//get block min Y
function getMinY(block){
	for(i=0;i<block.length;i++){
		for(j=0;j<block[i].length;j++){
			if(block[i][j] == 1){
				return i;
			}
		}
	}
}
//min X
function getMinX(block){
	for(j=0;j<block[0].length;j++){
		for(i=0;i<block.length;i++){
			if(block[i][j] == 1){
				return j;
			}
		}
	}
}
//max Y
function getMaxY(block){
	for(i=block.length-1;i>=0;i--){
		for(j=block[i].length-1;j>=0;j--){
			if(block[i][j] == 1){
				return i;
			}
		}
	}
}
//max X
function getMaxX(block){
	for(j=block[0].length-1;j>=0;j--){
		for(i=block.length-1;i>=0;i--){
			if(block[i][j] == 1){
				return j;
			}
		}
	}
}
})
</script>
</body>
</html>
```