title: Event
date: 2021-04-03 13:22:13
tags: js
categories: js
---

無限下拉加載圖片的頁面 綁定事件

## 代理

https://jsbin.com/hajohit/edit?html,js,output

```html
  <div id="div1">
    <a href="#">a1</a>
    <a href="#">a2</a>
    <a href="#">a3</a>
    <a href="#">a4</a>
     <!--會新增更多a標籤-->
  </div>
```

為所有a標籤 新增 點擊事件
```js
// 通過事件冒泡 去上層綁定
var div1 = document.getElementById('div1')
div1.addEventListener('click', function(e){
  //監聽是div1 但target可以告知點擊從哪出發的
  var target = e.target
  if(target.nodeName ==='A') {
    alert(target.innerHTML); //a1
  }  
})
```
