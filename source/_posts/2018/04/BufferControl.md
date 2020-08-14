---
title: Buffer Control
tags:
  - PHP
date: 2018-04-22
---

There are various kinds of buffer. buffer in PHP , in apache server , and in brower .There are something diferences.

<!-- more -->

In PHP there are some function and option about buffer for example: ob_start().etc.

### Two option about buffer in php.ini is:

- output_buffering: you can set it to on , off ,or int number.
- implicit_flush: you can set it to on/off .when you set it on , then it will fllow a flush() behind echo .

### The diference about flush() and ob_flush()

- when you use flush(). it will immediately output the buffer cached in the server (for example: apache server);
- And use ob_flush(), it will immediately output the buffer cached in PHP.
- You should use ob_flush() first, The PHP buffer will output to server cache ,but it havn't output to brower  already , The use flush() , It will output to brower.

### A example just like something below:
output a number every second:
```php
ob_start();         //start buffer
for ($i = 0; $i<10; $i++) {
    echo $i . "<br />";
    ob_flush(); //flush buffer
    flush();    //then flush server buffer
    sleep();    //sleep servel seconds
}
```

but if you use browser to access this code . it  usually don't output variable immediately , then we can add more space let the output bigger than 4096bytes :
```php
ob_start();         //start buffer
for ($i = 0; $i<10; $i++) {
    echo str_repeat(' ',4096);//output 4096 space to let the brower output the $i immediately
    echo $i . "<br />";
    ob_flush(); //flush buffer
    flush();    //then flush server buffer
    sleep();    //sleep servel seconds
}
```