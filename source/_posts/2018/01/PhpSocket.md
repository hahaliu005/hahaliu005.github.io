---
title: Php Socket
tags:
  - PHP
date: 2018-01-14
---

This is a simple socket about S/C peers . With this simple code . we can associate more.

<!-- more -->

### server.php
```php
$protocol = getprotobyname('tcp');
//create a socket
$socket = socket_create(AF_INET, SOCK_STREAM, $protocol);
socket_bind($socket, 'localhost', 3307);
socket_listen($socket);
echo "socket listenning\r\n";
//accept socket connection
while (true) {
    $connection = socket_accept($socket);
    echo "accept a socket\r\n";
    $input = socket_read($connection, 1024);
    echo "read data :".$input."\r\n";
    //reverse the string readed
    $input=trim($input);
    $output=strrev($input) ."\r\n";
    //output the reverse string
    socket_write($connection, $output, strlen($output));
}
//close socket
socket_close($connection);
socket_close($socket);
```

### client.php
```php
$protocol = getprotobyname('tcp');
$socket = socket_create(AF_INET, SOCK_STREAM, $protocol);
if (socket_connect($socket, 'localhost', 3307)) {
    echo "socket connected\r\n";
} else {
    echo "cann't connect to socket".socket_last_error($socket)."\r\n";
}
$send_data = "the sended data";
if (socket_write($socket, $send_data, strlen($send_data))) {
    echo "socket write data:".$send_data."\r\n";
} else {
    echo "socket write data failed:".socket_last_error($socket)."\r\n";
}
if ($buffer = socket_read($socket, 1024)) {
    echo "socket read data : ".$buffer."\r\n";
} else {
    echo "socket read data failed:".socket_last_error($socket)."\r\n";
}
socket_close($socket);
```

### Then we can use php command line to execute server and client:
Run server
```
$ php server.php
socket listenning
```
Run client
```
$ php client.php
socket connected
socket write data:the sended data
socket read data : atad dednes eht
```
After client connected server side will ouput:
```
accept a socket
read data :the sended data
```