---
title: Mysql Trigger
tags:
  - Mysql
date: 2018-03-27
---

This is about a very simply trigger but which is very useful :

<!-- more -->

Syntax:
```sql
CREATE TRIGGER <trigger name> <--
{ BEFORE | AFTER }
{ INSERT | UPDATE | DELETE }
ON <table name>
FOR EACH ROW
<the sql general statement which will be triggered>
```

E.g:
```mysql
create trigger table_insert after insert on table1 for each row insert into table2(`field`) value('value');
```

When you create trigger 'table_insert' like the code above , then insert a row in to table1, it also will insert a row into table2 field2=>value;

Want to see you created trigger code: `show triggers`;

For delete trigger: `drop trigger <trigger name>`

> Note: You must have enough proviledge to create trigger , if you haven't  it will faile.