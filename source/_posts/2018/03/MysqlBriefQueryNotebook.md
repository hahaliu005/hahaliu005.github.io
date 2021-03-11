---
title: Mysql Brief Query Notebook
tags:
  - Mysql
date: 2018-03-11
---

Show table status
```mysql
show table status like 'table_name';
```

<!-- more -->

Create database with default charset
```
CREATE DATABASE test DEFAULT CHARACTER SET UTF8mb4 DEFAULT COLLATE utf8mb4_general_ci;
```


A better way to change storage engine
```mysql
CREATE TABLE innodb_table LIKE myisam_table;
ALTER TABLE innodb_table ENGINE=InnoDB;
INSERT INTO innodb_table SELECT * FROM myisam_table;
```

Create user
```mysql
grant select,insert,update,delete on *.* to username@"hostname" identified by "password";
```

Set user password
```
SET PASSWORD FOR 'root'@'localhost' = 'password'; 
flush privileges;
```

Add column
```
alter table table_name add column <column_name> <column_option>
```

Flush  cache and show session status
```mysql
FLUSH STATUS;
<some sql statement>
SHOW SESSION STATUS LIKE 'Select%';
SHOW SESSION STATUS LIKE 'Handler%';
SHOW SESSION STATUS LIKE 'Sort%';
SHOW SESSION STATUS LIKE 'Created%';  //temporary tables MySQL created for the query
```

Show profiles
```mysql
//set profiling variable to 1
SET profiling = 1;
<some sql statement>
SHOW PROFILES/G
```

Show processlist
```mysql
SHOW PROCESSLIST/G
```

Explain
```mysql
explain <sql statement>/G
```

show index
```mysql
show index table_name/G
```


Show log temply
```
show variables like '%general_log%';
SET GLOBAL general_log = 'ON';
tail -f [general_log_file]
SET GLOBAL general_log = 'OFF';
```