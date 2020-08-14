---
title: A Simple Maze Function
tags:
  - PHP
date: 2018-06-29
---

I use php write a maze function , it based on algorithm of random travel graph.

<!-- more -->

maze.php
```
<?php
/**
 * be used to negerate a maze array
 * @param $rows how many rows the maze have
 * @param $cols how many cols the maze have
 * 
 * @return array the array just like below:
 * array(                      *an element
 * 	[1]=>array(                *this key defines the row
 * 		[5]=>array(			   *this key defines the col
 * 			[0]=>1,			   *top: the value 1 defines the direction of this elements was not beent broken
 * 			[1]=>0,			   *right:the value 0 defines the direction of this elements was beent broken
 * 			[2]=>0,			   *bottom:same to up
 * 			[3]=>1,			   *left: same to up
 * 		)......
 * 	)......
 * )
 */
function creat_maze($rows = 10,$cols = 10){
	set_time_limit(0);
	if (!is_numeric($rows) or !is_numeric($cols) or $rows <= 0 or $cols <= 0){
		return array();
	}
	$maze = array(); //the begin of the maze
	$tree = array(); //the tree
	$stack = array(); //the stack
	$accessed_maze = array(); //be accessed maze
	$broken_wall = array(); //the wall between two maze element status(broken or not been broken)
	
	//initialize the maze and tree
	for ($i = 1;$i <= $rows;$i++){
		for ($j = 1;$j <= $cols;$j++){
			$maze[$i][$j] = ($i-1)*$cols + $j; //define every element's value 
			$tree[$maze[$i][$j]] = array($maze[$i][$j]);
		}
	}
	
	//at the loop's begin of value
	$begin_row = 1;
	$begin_col = 1;
	$stack = array(array($begin_row,$begin_col));
	$accessed_maze[$begin_row][$begin_col] = true;
	
	//the loop
	while (count($tree) != 1){
		//define the for directions : top right bottom left
		$directions = array(array(0,-1),array(1,0),array(0,1),array(-1,0));
		
		$flag = false; //set a flag to taken that whether it gets a dead path
		while (count($directions) != 0 and !$flag){
			$dir_key = array_rand($directions);
			$direction = $directions[$dir_key];
			$now_row = $begin_row + $direction[0];
			$now_col = $begin_col + $direction[1];
			if (isset($maze[$now_row][$now_col]) and !isset($accessed_maze[$now_row][$now_col])){
				//array_push($tree[1], $maze[$now_row][$now_col]);
				unset($tree[$maze[$now_row][$now_col]]);
				$accessed_maze[$now_row][$now_col] = true;
				$broken_wall[$maze[$begin_row][$begin_col]][$maze[$now_row][$now_col]] = true;//broke the wall
				array_push($stack, array($now_row,$now_col));
				$begin_row = $now_row;
				$begin_col = $now_col;
				$flag = true;
			}
			unset($directions[$dir_key]);
		}
		if (!$flag){
			$back = array_pop($stack);
			$begin_row = $back[0];
			$begin_col = $back[1];
		}
	}
	//reset the maze's construction
	$re_maze = array(); //the maze will be reseted in this array
	foreach ($maze as $row=>$row_value){
		foreach ($row_value as $col=>$col_value){
			//define the for directions : top right bottom left
			$directions = array(array(0,-1),array(1,0),array(0,1),array(-1,0));
			$cache_value = array(1,1,1,1);
			while (!empty($directions) ){
				$direction = array_shift($directions);
				$next_row = $row + $direction[0];
				$next_col = $col + $direction[1];
				if (isset($maze[$next_row][$next_col])){
					if (isset($broken_wall[$maze[$row][$col]][$maze[$next_row][$next_col]]) or isset($broken_wall[$maze[$next_row][$next_col]][$maze[$row][$col]])){
						if (($direction[0] == 0) and ($direction[1] == -1)){
							$cache_value[3] = 0;
						}elseif (($direction[0] == 1) and ($direction[1] == 0)){
							$cache_value[2] = 0;
						}elseif (($direction[0] == 0) and ($direction[1] == 1)){
							$cache_value[1] = 0;
						}elseif (($direction[0] == -1) and ($direction[1] == 0)){
							$cache_value[0] = 0;
						}
					}
				}
			}
			$re_maze[$row][$col] = $cache_value;
		}
	}
	return $re_maze;
}
/* end of the file maze.php*/
```

Then I use jQuery CSS PHP HTML and include the maze function to write a simple maze game:
index.php
```
<html>  
<head>  
<meta http-equiv="Content-Type" content="text/html;charset=utf-8">  
<title>A maze </title>  
<?php   
error_reporting(E_ALL);  
$maze = array();  
if (isset($_POST['rows']) and isset($_POST['cols'])){  
    require './maze.php';//include maze function  
    $maze = creat_maze($_POST['rows'],$_POST['cols']);  
}  
?>  
<!-- include jquery library -->  
<script type = "text/javascript" src = "https://code.jquery.com/jquery-1.4.2.min.js"></script>  
<script type = "text/javascript" >
$().ready(function(){  
    $('form').submit(function(){  
        var valid = false;  
        valid = ($('#rows').val() != '') && ($('#cols').val() != '');  
        return valid;  
    })  
    <?php if (isset($_POST['rows']) and isset($_POST['cols'])){?>  
        var now_row = 1;  
        var now_col = 1;  
        start = '#cell'+now_row+'_'+now_col;  
        $(start).addClass('active');  
        console.log($(start));
          
        sele_cell = '#cell'+now_row+'_'+now_col;  
        $(window).keydown(function(event){  
            console.log(event);
            if(event.keyCode == 38){//up  
                if(! $(sele_cell).hasClass('top')){  
                    $(sele_cell).removeClass('active');  
                    now_row--;  
                    next_cell = '#cell'+now_row+'_'+now_col;  
                    $(next_cell).addClass('active');  
                    if(now_row == <?php echo $_POST['rows'];?> && now_col == <?php echo $_POST['cols'];?>){  
                        alert('congratulation');  
                    }  
                }  
            }else if(event.keyCode == 39){//right  
                if(! $(sele_cell).hasClass('right')){  
                    $(sele_cell).removeClass('active');  
                    now_col++;  
                    next_cell = '#cell'+now_row+'_'+now_col;  
                    $(next_cell).addClass('active');  
                    if(now_row == <?php echo $_POST['rows'];?> && now_col == <?php echo $_POST['cols'];?>){  
                        alert('congratulation');  
                    }  
                }  
            }else if(event.keyCode == 40){//down  
                if(! $(sele_cell).hasClass('bottom')){  
                    $(sele_cell).removeClass('active');  
                    now_row++;  
                    next_cell = '#cell'+now_row+'_'+now_col;  
                    $(next_cell).addClass('active');  
                    if(now_row == <?php echo $_POST['rows'];?> && now_col == <?php echo $_POST['cols'];?>){  
                        alert('congratulation');  
                    }  
                }  
            }else if(event.keyCode == 37){//left  
                if(! $(sele_cell).hasClass('left')){  
                    $(sele_cell).removeClass('active');  
                    now_col--;  
                    next_cell = '#cell'+now_row+'_'+now_col;  
                    $(next_cell).addClass('active');  
                    if(now_row == <?php echo $_POST['rows'];?> && now_col == <?php echo $_POST['cols'];?>){  
                        alert('congratulation');  
                    }  
                }  
            }  
            sele_cell = '#cell'+now_row+'_'+now_col;  
        })  
    <?php }?>  
})  
</script>  
<style type="text/css"  >
body{  
    margin:0 auto;  
}  
.cell{  
    float:left;  
    background:#ccc;  
    height:18px;  
    width:18px;  
    border:1px solid #ccc;  
}  
.clear{  
    clear:both;  
}  
.top{  
    border-top:1px solid #000;  
}  
.right{  
    border-right:1px solid #000;  
}  
.bottom{  
    border-bottom:1px solid #000;  
}  
.left{  
    border-left:1px solid #000;  
}  
#cell1_1{  
    border-left:1px solid #fff;  
    background-color:greed;  
}  
<?php if (isset($_POST['rows']) and isset($_POST['cols'])){?>  
    #cell<?php echo $_POST['rows']."_".$_POST['cols']?>{  
        border-right:1px solid #fff;  
        background-color:red;  
    }  
    #maze{  
        height:<?php echo 20*$_POST['rows'] + $_POST['rows'];?>px;  
        width:<?php echo 20*$_POST['cols']+$_POST['cols'];?>px;  
    }  
<?php }?>  
#out_maze{  
    margin:0 auto;  
    overflow:auto;  
}  
.active{  
    background-color:green;  
}
</style>  
</head>  
<body>  
<p>Set the rows and cols </p>  
<p>Move the green dot to the red dot then you win</p>  
<form action="./" id = "form" method = "POST" name = "form">  
<label for = "rows">Set rows : </label><input id = "rows" type = "text" name = "rows" value = "<?php echo isset($_POST['rows']) ? $_POST['rows'] : ''?>"/>  
<label for = "cols">Set cols : </label><input id = "cols" type = "text" name = "cols" value = "<?php echo isset($_POST['cols']) ? $_POST['cols'] : ''?>"/>  
<input id = "submit" type = "submit" name = "submit" value = "begin">  
</form>  
<div id = "out_maze">  
    <div id = "maze">  
        <?php if (!empty($maze)){?>  
            <?php foreach ($maze as $key_r => $cell_row){?>  
                <?php foreach ($cell_row as $key_c => $cell){?>  
                    <div class = "cell   
                    <?php   
                        if ($cell[0] == 1) echo " top";  
                        if ($cell[1] == 1) echo " right";  
                        if ($cell[2] == 1) echo " bottom";  
                        if ($cell[3] == 1) echo " left";  
                    ?>  
                    " id = "<?php echo "cell".$key_r."_".$key_c;?>"></div>  
                <?php }?>  
            <div class = "clear"></div>  
            <?php }?>  
        <?php }?>  
    </div>  
</div>  
</body>  
</html>  
```
I debug it in Chrome and it works well.

Set the maze rows and columns then begin.
![](leanote://file/getImage?fileId=5b627c79ab64412c2c000e45)
Press direction key to move the green point to the red point then you win haha

