---
title: Object.create()
date: 2021-04-04 13:23:22
tags: js
categories: js
---

`Object.create()` 指定其原型物件與屬性，創建一個新物件。

[使用 Object.create() 實現類別繼承](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Global_Objects/Object/create)

```js
// Shape - 父類別
function Shape() {
  this.x = 0;
  this.y = 0;
}

// 父類別的方法
Shape.prototype.move = function(x, y) {
  this.x += x;
  this.y += y;
  console.info('Shape moved.');
};

// Rectangle - 子類別
function Rectangle() {
  Shape.call(this); // call super constructor.
}

// 子類別擴展(extends)父類別
Rectangle.prototype = Object.create(Shape.prototype);
Rectangle.prototype.constructor = Rectangle;

var rect = new Rectangle();

console.log('Is rect an instance of Rectangle?', rect instanceof Rectangle);// true
console.log('Is rect an instance of Shape?', rect instanceof Shape);// true
rect.move(1, 1); // Outputs, 'Shape moved.'
```

ex

```js
let {...arr} = Object.create({x:1}) // Object.create 將x掛在原形鏈上了 沒有掛在值上 {}
console.log(arr) // undefined
```
