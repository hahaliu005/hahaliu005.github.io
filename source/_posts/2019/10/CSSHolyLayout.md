---
title: CSS 圣杯布局
tags:
  - CSS
  - HTML
date: 2019-10-13
---

Gist about holy layout

<!-- more -->

```html
<body>
  <div id="header">#header</div>
  <div id="container">
    <div id="center">
      Center
    </div>
    <div id="left">
      Left
    </div>
    <div id="right">
      Right
    </div>
  </div>
  <div id="footer">#footer</div>
</body>
```

```css
body {
  min-width: 550px;
}
#container {
  padding-left: 200px;
  padding-right: 150px;
  background: rebeccapurple;
}
#center,
#left,
#right {
  float: left;
  position: relative;
}
#center {
  width: 100%;
  height: 200px;
  background: cyan;
}
#left {
  height: 200px;
  width: 200px;
  background: navy;
  margin-left: -100%;
  right: 200px;
}
#right {
  margin-right: -150px;
  height: 200px;
  width: 150px;
  background: orchid;
}
#header {
  height: 50px;
  background: lightblue;
}
#footer {
  height: 50px;
  background: orange;
  clear: both;
}
```