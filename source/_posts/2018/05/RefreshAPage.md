---
title: Refresh A Page
tags:
  - PHP
date: 2018-05-04
---

If you want to redirect an user and tell him he will be redirected, e. g. "You will be redirected in about 5 secs. If not, click here." you cannot use header( 'Location: ...' ) as you can't sent any output before the headers are sent.

<!-- more -->

So, either you have to use the HTML meta refresh thingy or you use the following:
```php
<?php
  header (  "refresh:5;url=wherever.php"  );
  echo  'You\'ll be redirected in about 5 secs. If not, click <a href="wherever.php">here</a>.' ;
?>
```
From php menual. [Link](http://php.net/manual/en/function.header.php#97114)