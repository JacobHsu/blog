---
title: Object.assign()
date: 2021-04-05 13:23:22
tags: js
categories: js
---

用來複製  

[Object.assign()](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Global_Objects/Object/assign)被用來複製一個或多個物件自身所有可數的屬性到另一個目標物件。回傳的值為該目標物件。

`Object.assign()`只會從來源物件將自身可列舉的**屬性複製**到目標物件。  

##　複製物件

```js
var obj = { a: 1 };
var copy = Object.assign({}, obj);
console.log(copy); // { a: 1 }
```

##  合併物件

```js
var o1 = { a: 1 };
var o2 = { b: 2 };
var o3 = { c: 3 };

var obj = Object.assign(o1, o2, o3);
console.log(obj); // { a: 1, b: 2, c: 3 }
console.log(o1);  // { a: 1, b: 2, c: 3 }, 目標物件本身也被改變。
```

## 有相同屬性時合併物件區段

```js
var o1 = { a: 1, b: 1, c: 1 };
var o2 = { b: 2, c: 2 };
var o3 = { c: 3 };

var obj = Object.assign({}, o1, o2, o3);
console.log(obj); // { a: 1, b: 2, c: 3 }，屬性c為o3.c的值，最後一個出現的屬性c。
```