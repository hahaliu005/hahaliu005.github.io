---
title: About Function Quote Parameter In PHP
tags:
  - PHP
date: 2018-09-04
---

In general , we can use quote in function like this .
```php
function index1(&$test)
{
    $test .= 'inner';
}
$test = 'outer';
index1($test);
echo $test;//output: outerinner
```

<!-- more -->

We can see the outer variable test be changed.
Then we change the code like this :
```php
function index1(&$test)
{
    unset($test);
    $test = 'inner';
}
$test = 'outer';
index1($test);
echo $test; // what's the result?
```

It seems like will output nothing , because the variable was been unset and the new variable be set 'inner' was not in the outer space , but the correct result is it will output 'outer'.

There was another example to use global variable :
```php
function index2()
{
    global $test;
    unset($test);
    $test = 'inner';
}
global $test;
$test = 'outer';
index2();
echo $test;//the result will also output : outer
```

So I think , actully , the quote parameter in the function only pass a data address , like pointer in C language . With this result , we can go though the example below samplly :
```php
function index1(&$array)
{
    unset($array);
}
$test_array = array('one'=>'two','three'=>'four');
index1($test_array);
print_r($test_array);
/**
 * result
 * Array ( [one] => two [three] => four )
 */
function index2(&$array)
{
    unset($array['one']);//beware of this place
}
$test_array2 = array('one'=>'two','three'=>'four');
index2($test_array2);
print_r($test_array2);
/**
 * result
 * Array ( [three] => four )
 */
```

