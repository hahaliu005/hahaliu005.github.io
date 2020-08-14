---
title: vertical align center
tags:
  - HTML
  - CSS
date: 2016-11-04
---

```html
<div class="outer">
  <div class="inner">
    <div class="subInner">
      <div class="subInner2">subinner</div>
    </div>
  </div>
</div>
```

使用此居中时, 确认父元素含有position定位
```css
.vertical-align-center (@width, @height) {
  position: absolute;
  width: @width;
  height: @height;
  top: 50%;
  left: 50%;
  margin-top: -@width/2;
  margin-left: -@height/2;
}
.outer{
  @width: 200px;
  @height: 200px;
  position: absolute;
  width: @width;
  height: @height;
  background-color: lightblue;
  .inner {
    .vertical-align-center(150px, 150px);
    background-color: lightgreen;
    .subInner {
      .vertical-align-center(50px, 50px);
      background-color: red;
      .subInner2 {
        .vertical-align-center(25px, 25px);
        background-color: blue;
      }
    }
  }
}
```