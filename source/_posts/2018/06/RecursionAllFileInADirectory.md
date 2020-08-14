---
title: Recursion All File In A Directory
tags:
  - PHP
date: 2018-06-03
---

Get all file direction and save it to an reference array, The function like below.

<!-- more -->

```
public function scanfile($dir, &$dir_array)
{
    if (is_dir($dir)) {
        $scandirs = scandir($dir);
        foreach ($scandirs as $scandir) {
            if ($scandir != '.' and $scandir != '..') {
                $scandir = $dir.'/'.$scandir;
                if (is_file($scandir)) {
                    $dir_array[] = $scandir;
                } elseif (is_dir($scandir)) {
                    scanfile($scandir, $dir_array);
                }
            }
        }
    }
}
```

Then use it like this:
```
echo "<pre>";
$dir_array = array();
$dir = './simpletest';
scanfile($dir,$dir_array);
print_r($dir_array);
```

And the result may be :
```
Array
(
    [0] => ./simpletest/a.php
    [1] => ./simpletest/b/c.php
    [2] => ./simpletest/d/e/f.php
```

Use example, we can use this function to replace some content which we need e.g:
```
$dir_array = array();  
$dir = './simpletest';  
scanfile($dir,$dir_array);  
foreach ($dir_array as $dir_ar){  
    $name_arr = explode('.',basename($dir_ar));  
    $name_arr = end($name_arr);  
    if ($name_arr == 'php'){  
        $content = file_get_contents($dir_ar);  
        $content = preg_replace('/&new/','new',$content);  
        file_put_contents($dir_ar, $content);  
    }  
}  
```