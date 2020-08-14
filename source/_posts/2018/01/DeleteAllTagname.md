---
title: Delete All Tagname
tags:
  - HTML
  - Javascript
date: 2018-01-15
---

Description: When use getElementByTagName('div'); the var divs was an object if you remove(div[i]) directly,
when you remove(div[0]),then div[1] will became div[0] ,then when you remove[1], it doesn't work.note.

<!-- more -->

```html
<!DOCTYPE html>
<html>
<head>
<title>Template</title>
</head>
<body>
<h1></h1>
<div>
    the first div
</div>
<div>
   the secend div
</div>
<div>
  the third div
</div>
<script type="text/javascript">
var divs = document.getElementsByTagName('div');
//then how to delete all the div tag 
//the error way it not work correctly
for(var i = 0;i<divs.length;i++){
    remove(divs[i]);
}

//the first correct way
var divarray = []
for(var i = 0;i<divs.length;i++){
  divarray.push(divs[i]);
}
for(var i = 0;i<divarray.length;i++){
  remove(divarray[i]);
}

//the second correct simply way 
while(divs[0]){
  remove(divs[0]);
}

function remove(elem) {
    elem.parentNode.removeChild(elem);
}
</script>
</body>
</html>
```