---
title: About Insert Sql On Duplicate Key Update
tags:
  - Mysql
date: 2018-06-02
---

Here was a useful insert sql like this :

insert into `table` (`field1`,`field2`,`field3`) values('value1','value2','value3')  on   duplicate key update   `id`=`id`,`count`=`count`+1;

<!-- more -->

It means if the values of field1,field2,field3 value1, value2,value3 are exist, it will update the exist row , otherwise , it will insert the row with value1, value2 ,value3.

Before use this you must set `field1`,`field2`,`field3` was unique index, and think about the performance.