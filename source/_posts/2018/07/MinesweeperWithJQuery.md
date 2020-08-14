---
title: Minesweeper With JQuery
tag:
  - JQuery
  - Javascript
date: 2018-07-05
---

This was a Minesweeper with jquery , some place are not very well  and seemed not completed completely . But the core function are writed in it . upload two game picture.

<!-- more -->

![](01.gif)

![](02.gif)

Then this is the code:

minesweeper.html
```html
<html>
<head>
	<script type="text/javascript" src="http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4.min.js"></script>
	<script type="text/javascript" src="./minesweeper.js"></script>
	<link rel="stylesheet" href="./minesweeper.css" />
</head>
<body>
<p id="mine1"></p>
<p id="mine2"></p>
<div id="main"></div>
</body>
</html>
```

minesweeper.css
```css
td{
	width:20px;
	height:20px;
	border:none;
}
.cell{
	background-image:url('./img/cell.png');
}
.flag{
	background-image:url('./img/flag.png');
}
.mine{
	background-image:url('./img/mine.png');
}
.indetermined{
	background-image:url('./img/indetermined.png');
}
.cleared{
	background-image:url('./img/cleared.png');
}
```

minesweeper.js
```javascript
$().ready(function(){
	var rows = 16;
	var cols = 30;
	var gElement = new Array();
	var gRate = new Array();
	var mines = 99;
	var gArea = '';
	gArea = '<table>';
	for(i=0;i<rows;i++){
		gArea += '<tr>';
		gElement[i] = new Array();
		gRate[i] = new Array();
		for(j=0;j<cols;j++){
			gArea += '<td id="cell_' + i + '_' + j + '" class="cell">';
			gElement[i][j] = 0;
			gRate[i][j] = 0;
			gArea += '</td>';
		}
		gArea += '</tr>';
	}
	gArea += '</table>';
	$('#main').append(gArea);
	var k = Math.min(rows*cols,mines);
	//the surplus mines and real surplus mines
	var surplusMines = realSurplusMines = k;
	$('#mine1').html(surplusMines);
	$('#mine2').html(realSurplusMines);
	while(k > 0){
		i = Math.floor(Math.random()*rows);
		j = Math.floor(Math.random()*cols);
		if(! gElement[i][j]){
			gElement[i][j] = 1;
			k--;
		}
	}
	//get the rate of every block 
	for(i=0;i<rows;i++){
		for(j=0;j<cols;j++){
			p = 0;
			if(gElement[i][j]){
				gRate[i][j] = 9;
			}else{
				for(m=i-1;m<=i+1;m++){
					for(n=j-1;n<=j+1;n++){
						if((typeof(gElement[m]) !== 'undefined') && (typeof(gElement[m][n]) !== 'undefined') && gElement[m][n]){
							p++;
						}
					}
				}
				gRate[i][j] = p;
			}
		}
	}
	var regExp = new RegExp(/.+_(\d+)_(\d+)$/);
	//disable mouse right button menu
	$(window).bind("contextmenu", function(e){ return false;});
	//event bind
	$('td').bind({
		'contextmenu':function(event){
			var clickClass = $(this).attr('class');
			var clickId = $(this).attr('id');
			var match = regExp.exec(clickId);
			switch(clickClass){
				case 'cell':
					$('#cell_' + match[1] + '_' + match[2]).attr('class','flag');
					gRate[match[1]][match[2]] == 9 ? realSurplusMines-- : '';
					//surplus mines surface
					surplusMines --;
					$('#mine1').html(surplusMines);
					$('#mine2').html(realSurplusMines);
					//when all mines are being find
					if(realSurplusMines == 0){
						alert('success');
					}
					break;
				case 'flag':
					$('#cell_' + match[1] + '_' + match[2]).attr('class','indetermined');
					//cancel the mines flag
					gRate[match[1]][match[2]] == 9 ? realSurplusMines++ : '';
					surplusMines ++ ;
					$('#mine1').html(surplusMines);
					$('#mine2').html(realSurplusMines);
					break;
				case 'indetermined':
					$('#cell_' + match[1] + '_' + match[2]).attr('class','cell');
					break;
			}
		},
		'click':function(event){
			var clickClass = $(this).attr('class');
			if(clickClass == 'cell'){
				var clickId = $(this).attr('id');
				var match = regExp.exec(clickId);
				var dir = '';
				if(! gElement[match[1]][match[2]]){
					i=match[1];
					j=match[2];
					setCleared(i,j);
				}else{
					alert('game over');
				}
			}
		}
	})
 
	function setCleared(x,y){
		x = parseInt(x);
		y = parseInt(y);
		var r,c;
		if(! $('#cell_' + x + '_' + y).hasClass('cleared')){
			$('#cell_' + x + '_' + y).attr('class','cleared');
			gRate[x][y] && gRate[x][y] != 9 ? $('#cell_' + x + '_' + y).html(gRate[x][y]) : '';
		}
		if(gRate[x][y]){//not eq to zero
			for(r = -1 ; r < 2 ; r++){
				for(c = -1 ; c < 2 ; c++){
					if(typeof(gRate[x + r]) != 'undefined' && typeof(gRate[x+r][y+c]) != 'undefined' && ! gRate[x+r][y+c] && ! $('#cell_' + (x+r) + '_' + (y+c)).hasClass('cleared')){
						setCleared(x+r,y+c);
					}
				}
			}
		}else{//eq to zero
			for(r = -1 ; r < 2 ; r++){
				for(c = -1 ; c < 2 ; c++){
					if(typeof(gRate[x + r]) != 'undefined' && typeof(gRate[x+r][y+c]) != 'undefined' && ! $('#cell_' + (x+r) + '_' + (y+c)).hasClass('cleared')){
						$('#cell_' + (x+r) + '_' + (y+c)).attr('class','cleared');
						gRate[x+r][y+c] && gRate[x+r][y+c] != 9 ? $('#cell_' + (x+r) + '_' + (y+c)).html(gRate[x+r][y+c]) : '';
						! gRate[x+r][y+c] ? setCleared(x+r,y+c) : '' ;
					}
				}
			}
		}
 
	}
})
```