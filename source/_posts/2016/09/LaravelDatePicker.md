---
title: Bootstrap的日期组件加入到laravel中
tags:
  - Laravel
date: 2016-09-13
---

将附件中的两个文件放入对应的assets文件夹中并分别require
```
require('./util/bootstrap-datetimepicker');
```

```
@import "util/bootstrap-datetimepicker-build.less";
```

package.json添加这两行, 并npm install
```
  "moment": "^2.10",
  "moment-timezone": "^0.4.0"
```

```html
  <div class="container">
  <div class="row">
  <div class='col-sm-6'>
  <div class="form-group">
  <div class='input-group date' id='datetimepicker1'>
  <input type='text' class="form-control" />
  <span class="input-group-addon">
  <span class="glyphicon glyphicon-calendar"></span>
  </span>
  </div>
  </div>
  </div>
  </div>
  </div>

  $(function () {
  $('#datetimepicker1').datetimepicker({
  format: 'YYYY-MM-DD HH:mm:ss'
  });
  });


  $(function () {
  $('#datetimepicker1').datetimepicker({
  format: 'YYYY-MM-DD HH:mm:ss'
  });
  });
```


参考文档链接: http://eonasdan.github.io/bootstrap-datetimepicker/

bootstrap-datetimepicker.js
bootstrap-datetimepicker-build.less