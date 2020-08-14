---
title: About Empty Function In Php
tags:
  - PHP
date: 2018-06-17
---

For empty() you must set var like this:
```
$var = 'value':
empty($var);
```

<!-- more -->

If you use it directly like this :
```
empty('value');
```
It is error!

The $var be set under will return TRUE:
* "" (an empty string)
* 0 (0 as an integer)
* "0" (0 as a string)
* NULL
* FALSE
* array() (an empty array)

var $var; (a variable declared, but without a value in a class)
Come from php menual partly.