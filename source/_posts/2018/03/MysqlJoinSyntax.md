---
title: Mysql Join Syntax
tags:
  - Mysql
date: 2018-03-21
---

There are four kinds of join syntax:

<!-- more -->

### inner join;
```mysql
select * from table1 inner join table2 on table1.id = table2.id;
```
it is the same as the normal sql:
```mysql
select * from table1,table2 where table1.id = taable2.id;
```

### left outer join
```mysql
select * from table1 left outer join table2 on table1.id = table2.id;
```

### right outer join
```
select * from table1 right outer join table2 on table1.id = table2.id;
```

### full outer join:
```
select * from table1 full outer join table2 on table1.id = table2.id;
```

> correct: mysql don't support full outer join , we can use union just like ` SELECT * FROM t1 LEFT JOIN t2 ON t1.id = t2.id UNION SELECT * FROM t1 RIGHT JOIN t2 ON t1.id = t2.id`