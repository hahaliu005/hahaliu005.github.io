---
title: Image Carousel
tags: 
  - Javascript
date: 2018-07-05
---

So many website have had this function , and I try to write this function use Jquery + CSS fundamentally . Maybe I can clear it to a single function some day then I can make it more  conveniency to use.

<!-- more -->

This is the actual effect picture , we can find a UI to make it more pretty :

![](01.gif)

At last , I copy my code to this place . It's so long that I can't remember it :

picture_rotation.html
```
<html>
<head>
	<script type="text/javascript" src="http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4.min.js"></script>
	<script type="text/javascript" src="./picture_rotation.js"></script>
	<link rel="stylesheet" type="text/css" href="./picture_rotation.css" />
</head>
<body>
	<div id="main">
	<div id="img1">
		<img src="./img/1.jpg" class="img_1"/>
		<img src="./img/2.jpg" class="img_2"/>
		<img src="./img/3.jpg" class="img_3"/>
		<img src="./img/4.jpg" class="img_4"/>
	</div>
	</div>
	<div id="main2">
		<div id="leftButton"></div>
		<div id="img2">
			<div id="imgBar">
				<img src="./img/1.jpg" class="img_1"/>
				<img src="./img/2.jpg" class="img_2"/>
				<img src="./img/3.jpg" class="img_3"/>
				<img src="./img/4.jpg" class="img_4"/>
			</div>
		</div>
		<div id="rightButton"></div>
	</div>
</body>
</html>
```

picture_rotation.js
```
$().ready(function(){
	var img1 = $('#img1');
	var img2 = $('#imgBar');
	//this two variable flag can avoid animate and setInterval conflict
	var run = 1;
	var run2 = 1;
	var intervalId;
	intervalId = setInterval(img_run,2000);
	$('#rightButton').click(function(){
		if(run){
			run = 0;
			var position = img2.position()
			img2.animate({left:(position.left == -66) ? position.left : (position.left - 66)},'slow',function(){run = 1;});
		}
	});
	$('#leftButton').click(function(){
		if(run){
			run=0;
			var position = img2.position();
			img2.animate({left:position.left ? (position.left + 66) : position.left},'slow',function(){run=1});
		}
	});
	$('#imgBar img').click(function(){
		var attr = $(this).attr('class');
		var big_img = $('.' + attr + ':eq(0)');
		var position = big_img.position();
		img1.animate({left:'-' + position.left},'slow');
	});
	$('#imgBar img').mouseover(function(){
		clearInterval(intervalId);
	});
	$('#imgBar img').mouseout(function(){
		intervalId = setInterval(img_run,2000);
	});
	function img_run(){
		if(run2){
			run = 0;
			var position = img1.position();
			img1.animate({left:(position.left == -900) ? 0 : (position.left - 300)},'slow',function(){run=1});
		}
 
	}
})
```

picture_rotation.css
```
#main{
	width:300px;
	height:150px;
	background-color:lightgreen;
	position:relative;
	overflow:hidden;
}
#main #img1{
	display:block;
	width:1200px;
	height:150px;
	position:absolute;
}
#main #img1 img{
	float:left;
	display:block;
	width:300px;
	height:150px;
}
#main2
{
    height: 100px;
    overflow: hidden;
    width: 300px;
}
#img2
{
    float: left;
    height: 100px;
    overflow: hidden;
    position: relative;
    width: 200px;
}
#img2 img {
    display: block;
    float: left;
    height: 60px;
    margin-left: 6px;
    margin-top: 20px;
    width: 60px;
}
#img2 img:hover{
	cursor:pointer;
}
#imgBar
{
    height: 100px;
    position: absolute;
    width: 264px;
}
#leftButton,#rightButton {
    float: left;
    height: 100px;
    width: 50px;
}
#leftButton{
	background-image:url('./img/leftButton.png');
}
#rightButton{
	background-image:url('./img/rightButton.png');
}
#leftButton:hover,#rightButton:hover {
    cursor: pointer;
}
```