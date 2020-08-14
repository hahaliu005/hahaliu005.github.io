---
title: Javascript多行显示省略号
tags:
  - Javascript
date: 2016-02-27
---

单行可以直接使用CSS，多行可以使用JS实现

<!-- more -->

```
$(function () {
  var test = $('.test-container .media');
  var p = test.find('p');
  var text = p.text().split('');
  p.empty();
  var byte = '';
  p.show();
  while (byte = text.shift()) {
  var textStr = p.text();
  if (p.height() >= test.height() && textStr.length > 3) {
  textStr = textStr.substr(0, textStr.length - 3) + '...';
  p.text(textStr);
  break;
  }
  p.append(byte);
  }
})
```