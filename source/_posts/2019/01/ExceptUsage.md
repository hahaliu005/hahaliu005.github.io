---
title: Use expect command to interact with mysql in linux system
tags: 
  - Linux
  - Mysql
date: 2019-01-09
---

The "expect" command is not install in some linux system default , in the ubuntu system , you can simplly tap "sudo apt-get install expect" to install this tool.
And then , if you want to use to in the shell script , the first line maybe like this:
```
#!/use/bin/expect
```

<!-- more -->

It just like bash or sh . check the example below:

```
$ expect
expect1.1> spawn mysql -h localhost -u db_runner -p
spawn mysql -h localhost -u db_runner -p
27022
expect1.2> expect "password:"
Enter password: expect1.3> send "mysqlpassword\r"
expect1.4> expect "mysql>"

Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 88
Server version: 5.5.34-0ubuntu0.13.04.1 (Ubuntu)

Copyright (c) 2000, 2013, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> expect1.5> send "show databases;\r"
expect1.6> expect "mysql>"
show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| test               |
+--------------------+
9 rows in set (0.00 sec)

mysql> expect1.7> send "exit;\r"
expect1.8> expect "mysql>"
exit;
Bye
```

In Addition, want to build script automatically, Use `autoexpect`
```
```