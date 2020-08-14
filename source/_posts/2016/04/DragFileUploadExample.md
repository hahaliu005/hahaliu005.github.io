---
title: Drag file upload example
tags:
  - HTML
  - CSS
  - JQuery
date: 2016-04-09
---

A Gist for drage file upload

<!-- more -->

```html
<div id="form-drop">
  <div>
  <h1>Drop file to here, or click button to browser file</h1>
  </div>
  <input type="file" name="file">
</div>
<div id="message"></div>

<script type="text/javascript" src="/js/app.js"></script>
<script type="text/javascript">
  $(function(){
  $(document).on({
  dragleave:function(e){ //拖离
  e.preventDefault();
  },
  drop:function(e){ //拖后放
  e.preventDefault();
  },
  dragenter:function(e){ //拖进
  e.preventDefault();
  },
  dragover:function(e){ //拖来拖去
  e.preventDefault();
  }
  });

  var drop = document.getElementById('form-drop'); //拖拽区域
  $('#form-drop').on('drop', function (e) {
  return upload(e);
  }).on('change', 'input[name=file]', function (e) {
  gevent = e;
  return upload(e);
  });
  });

  function upload(jqueryEvent)
  {
  jqueryEvent.preventDefault();
  // for change event , it was jqueryevent.target.files
  // for drop event , it was jquery.originalEvent.dataTransfer.files
  fileList = jqueryEvent.target.files || jqueryEvent.originalEvent.dataTransfer.files;
  if(fileList.length == 0){
  return false;
  }

  if (! fileList[0].name.match('\.jpg$')) {
  return false;
  }

  var formData = new FormData();
  formData.append('file', fileList[0]);
  $.ajax({
  url: '/upload',
  type: 'POST',
  cache: false,
  headers: {
  'X-CSRF-TOKEN': $('meta[name="csrf-token"]').attr('content')
  },
  data: formData,
  processData: false,
  contentType: false
  }).done(function(res) {
  $('#message').append(res)
  }).fail(function(res) {
  });
  }
</script>
```