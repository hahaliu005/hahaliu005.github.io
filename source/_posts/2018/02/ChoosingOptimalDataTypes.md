---
title: Choosing Optimal Data Types
tags:
  - Mysql
date: 2018-02-13
---

## Smaller is usually better.

In general, try to use the smallest data type that can correctly store and represent your data. Smaller data types are usually faster, because they use less space on the disk, in memory, and in the CPU cache. They also generally require fewer CPU cycles to process.

<!-- more -->

Make sure you don’t underestimate the range of values you need to store, though, because increasing the data type range in multiple places in your schema can be a painful and time-consuming operation. If you’re in doubt as to which is the best data type to use, choose the smallest one that you don’t think you’ll exceed. (If the system is not very busy or doesn’t store much data, or if you’re at an early phase in the design process, you can change it easily later.)

## Simple is good.

Fewer CPU cycles are typically required to process operations on simpler data types. For example, integers are cheaper to compare than characters, because character sets and collations (sorting rules) make character comparisons complicated. Here are two examples: you should store dates and times in MySQL’s built-in types instead of as strings, and you should use integers for IP addresses. We discuss these topics further later.

## Avoid NULL if possible.

You should define fields as NOT NULL whenever you can. A lot of tables include nullable columns even when the application does not need to store NULL (the absence of a value), merely because it’s the default. You should be careful to specify columns as NOT NULL unless you intend to store NULL in them.

It’s harder for MySQL to optimize queries that refer to nullable columns, because they make indexes, index statistics, and value comparisons more complicated. A nullable column uses more storage space and requires special processing inside MySQL. When a nullable column is indexed, it requires an extra byte per entry and can even cause a fixed-size index (such as an index on a single integer column) to be converted to a variable-sized one in MyISAM. Even when you do need to store a “no value” fact in a table, you might not need to use NULL. Consider using zero, a special value, or an empty string instead.
The performance improvement from changing NULL columns to NOT NULL is usually small, so don’t make finding and changing them on an existing schema a priority unless you know they are causing problems. However, if you’re planning to
index columns, avoid making them nullable if possible.

 
> Comes from High Performance Mysql the second edition