---
title: Object.concat()
date: 2021-04-06 13:23:22
tags: js
categories: js
---

`Object.concat()` 方法被用來合併兩個或多個陣列。此方法不會改變現有的陣列，回傳一個包含呼叫者陣列本身的值，作為代替的是回傳一個新陣列。

```js
const array1 = ['a', 'b', 'c'];
const array2 = ['d', 'e', 'f'];
const array3 = array1.concat(array2);
// const groupAll = [...array1, ...array2] // es6
console.log(array3);
// expected output: Array ["a", "b", "c", "d", "e", "f"]
```
