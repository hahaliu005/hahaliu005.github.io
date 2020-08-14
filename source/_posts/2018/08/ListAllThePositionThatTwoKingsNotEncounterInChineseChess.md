---
title: List All The Position That Two Kings Not Encounter In Chinese Chess
tags:
  - Algorithm
  - PHP
date: 2018-08-05
---

Title like , there are many ways to resolve this question , but if request you can only use one variable , how to do it . 

<!-- more -->

For use one variable to resolve this question , we can set a eight bit binary variable , every four bit express one kings position . the can solve it . the code not list below .

There are some more convenient solution . just like this 

```php
$i = 81;
while ($i--) {
	if ($i / 9 % 3 == $i % 9 % 3) {
		continue;
	}
	printf("A=%d,B=%d\n",$i / 9 + 1,$i % 9 + 1);
}
```

And we can use php array also:
```php
$array = array(0,0);
for ($array[0] = 1;$array[0] <= 9 ; $array[0]++){
	for ($array[1] = 1;$array[1] <= 9;$array[1]++){
		if ($array[0] % 3 != $array[1] % 3) {
			printf("a=%d,b=%d\n",$array[0],$array[1]);
		}
	}
}
```