---
title: Mysql Transaction
tags:
  - Mysql
date: 2018-09-04
---

There's two ways for MYSQL transaction.
### Use begin,rollback,commit

* begin start a transaction
* rollback transaction rollback
* commit transaction commit

<!-- more -->

### Set mysql ini autocommit
In default mysql will commit automatically. It means when you commit a query ,it will excute it directly

* set autocommit=0 forbid commit automatically
* set autocommit=1 allow commit automatically

> Note: In mysql only INNODB and BDB engine support transaction (or other new storage engineer too).

### Chang type of mysql engine

```
table table_name type=InnoDB;
alter table table_name engine=InnoDB;
```

### Transaction example:
```mysql
BEGIN;
SELECT book_number FROM book WHERE  book_id = 123 for update;
// ...
UPDATE book SET book_number = book_number - 1 WHERE  book_id = 123;
COMMIT;
```
```mysql
START TRANSACTION;
SELECT balance FROM checking WHERE customer_id = 10233276;
UPDATE checking SET balance = balance - 200.00 WHERE customer_id = 10233276;
UPDATE savings  SET balance = balance + 200.00 WHERE customer_id = 10233276;
COMMIT;
```

Some sql engine don't support transaction, we can use table lock
support MyISAM & InnoDB ,
```sql
LOCK TABLES `user` WRITE;
INSERT INTO `user` (`id`, `username`, `sex`) VALUES (NULL, 'test1', '0');
UNLOCK TABLES;
```