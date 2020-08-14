---
title: Mysql Master And Slave Server
tags:
  - Mysql
date: 2018-01-11
---

# Install mysql in every server that you want to install mysql master or server

<!-- more -->

### Flush and lock tables;
```
flush tables with read lock;
```

### Remember log file and log position
```
show master status;
# eg: 'master-bin.000004' 3234050
```

### Dump data from master
```
mysqldump --all-database -h localhost -u root -p > mysql-master.sql
```

### Unlock tables so that master can work
```
unlock tables;
```

### Import master data to slave
```
mysql -h localhost -u root -p < mysql-master.sql
```

### Login to mysql slave command line set slave options
```
change master to 
master_host='mysql-master',
master_port=3306,
master_user='root',
master_password='111',
master_log_file='master-bin.000004',
master_log_pos=3234050
```

### Start slave
```
start slave;
```