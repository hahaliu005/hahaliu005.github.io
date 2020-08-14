---
title: Five Kinds Of Sort Algorithm
tags:
  - Algorithm
  - PHP
date: 2018-02-23
---

## Insertion sort
Insert a data element to the new array every time , keep the new array still in order.
The insertion sort just like this :

<!-- more -->

> Beware:I use php to achieve the sort function from small to large,for convenient I use few php array function , But don't impact the sort algorithm; and we assume the array argement will be sorted are numeric which the key value begin with 0.
```php
function insertion_sort($array)
{
    //define a sort container
    $sort = array();
    //get the first element $array[0]
    $sort[0] = array_shift($array);
    while (!empty($array)) {
        //the while loop get element to $value until $array are empty
        $value = array_shift($array);
        for ($i = 0;$i<count($sort);$i++) {
            //if $value little than $sort[$i] insert $value to the position of $sort[$i]
            //every element after $sort[$i] back ward one index
            if ($value <= $sort[$i]) {
                for ($j = count($sort)-1;$j>=$i;$j--) {
                    $sort[$j+1] = $sort[$j];
                }
                $sort[$i] = $value;
                unset($value);
                break;
            }
        }
        //the $value begger than every element of $sort[$i]
        //insert it to the tail
        if (isset($value)) {
            $sort[$i] = $value;
        }
    }
    return $sort;
}
$array = array(49,38,65,97,76,13,27,49);
print_r(insertion_sort($array));

```

## Selection sort
Get a smallest(or bigest) element from the array , put it to the last of the sort array, until all the element be puted from array to sort array.
E.g:
```php
function selection_sort($array)
{
    //define a sort container
    $sort = array();
    while (!empty($array)) {
        //defined $min to get the minimum value
        //and remember the $key used to unset()
        $min = null;
        $offset = null;
        foreach ($array as $key => $value) {
            if (empty($min) or $min>$value) {
                $min = $value;
                $offset = $key;
            }
        }
        $sort[] = $min;
        unset($array[$offset]);
    }
    return $sort;
}
$array = array(49,38,65,97,76,13,27,49);
print_r(selection_sort($array));
```

## Bubble sort
Compare two element , if the first element is smaller(or bigger ) than second element , reverse two element , until all the element sequence are correct.
E.g:
```php
function bubble_sort($array)
{
    $count = count($array);
    for ($i = 0;$i<$count;$i++) {
        for ($j = $count-1;$j>$i;$j--) {
            if ($array[$j-1] > $array[$j]) {
                //reverse two elements
                $tem = $array[$j-1];
                $array[$j-1] = $array[$j];
                $array[$j] = $tem;
            }
        }
    }
    return $array;
}
$array = array(49,38,65,97,76,13,27,49);
print_r(bubble_sort($array));
```

## Quick sort
If the array are array[0 to n] , Select a element array[x] to the base comparition , Then put the other element to the left of array[x] which smaller than array[x] , and put the other element to the right of array[x] which bigger than array[x] . Recursive quick sort left array and right array , until array empty.
E.g:
```php
function quick_sort($array)
{
    if (!empty($array)) {
        $count = count($array);
        //get a element from array then unset it from $array
        $offset = floor($count / 2);
        $value = $array[$offset];
        unset($array[$offset]);
        if (!empty($array)) {
            //initial left array and right array
            $left_array = array();
            $right_array = array();
            foreach ($array as $arr) {
                if ($arr <= $value) {
                    //put the smaller value to the left
                    $left_array[] = $arr;
                } else {
                    //put the bigger value to the right
                    $right_array[] = $arr;
                }
            }
            //recusive quick sort left and right array and merge
            return array_merge(quick_sort($left_array), array($value), quick_sort($right_array));
        } else {
            return array($value);
        }
    } else {
        return array();
    }
}
$array = array(49,38,65,97,76,13,27,49);
print_r(quick_sort($array));
```

## Heap sort
Think the array are complete binary tree , sort the array to a heap , the smallest(or biggest) element will appear in the root of binare tree , switch the root element and the last element . Then recursive heap sort the rest of binary tree node except the last element been switched last time.
E.g:
```php
/**
 * make the $heap[$start to $end] to a big heap
 * @param array $heap the heap array
 * @param int $start the start index key
 * @param int $end the end index key
 */
function heap_adjust(&$heap, $start, $end)
{
    $tem = $heap[$start];
    for ($j = 2*$start;$j<=$end;$j*=2) {
        if ($j<$end and $heap[$j]<$heap[$j+1]) {
            $j = $j+1;
        }
        if ($tem>=$heap[$j]) {
            break;
        }
        $heap[$start]=$heap[$j];
        $start=$j;
    }
    $heap[$start]=$tem;
}
/**
 * the heap sort function
 * @param array $array
 */
function heap_sort($array)
{
    $count = count($array);
    //the array are begin with 0, for convenient,let the index begin with 1
    for ($i = $count;$i>0;$i--) {
        $array[$i] = $array[$i-1];
    }
    unset($array[0]);
    
    for ($i=floor($count/2);$i>0;$i--) {
        heap_adjust($array, $i, $count);
    }
    for ($i = $count;$i>1;$i--) {
        $tem = $array[1];
        $array[1] = $array[$i];
        $array[$i] = $tem;
        heap_adjust($array, 1, $i-1);
    }
    return $array;
}
$array = array(49,38,65,97,76,13,27,49);
print_r(heap_sort($array));
```