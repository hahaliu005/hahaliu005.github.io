---
title: The Commpression Tools Usage (gzip , tar ...)
tags: 
  - Linux
  - Mac
date: 2018-11-19
---

### The gzip command we usually used :
```
#compress the file
$ gzip filename
#display the compressed file
$ gzcat filename.gz
#uncompress file
$ gzip -d filename
 
#eg: compress the file use the best compress alog , and stag the origin file
$ gzip -9 -c filename > filename.gz
```

<!-- more -->

### The tar command we usually used :
```
#pack and compress
tar -zcvf file.tar.gz dirname
#uncompress
tar -zxvf file.tar.gz
#list the files in the gz file
tar -ztvf file.tar.gz
#only uncompress one file in the .gz file
tar -zxvf file.tar.gz gzdir/filename
#uncompre  .gz file to specify dir
tar -zxvf file.tar.gz -C dirname
```
### unrar
```
unrar x [fileName]
```

### unzip
```
unzip [zipFile] -d [toDirectory]
```