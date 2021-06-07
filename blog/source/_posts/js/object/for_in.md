---
title: for...in 順序
date: 2021-06-07 13:23:22
tags: js
categories: js
---

在學習 JavaScript 語言的 `for...in` 循環時，總是會被告知：用它循環對象，循環出來的屬性順序並不可靠，所以不要在 `for...in` 中做依賴對象屬性順序的邏輯判斷。

簡單歸結成一句話就是：**先遍歷出整數屬性（integer properties，按照升序），然後其他屬性按照創建時候的順序遍歷出來。**

我們來看一個例子：

```js
let codes = {
  "49": "Germany",
  "41": "Switzerland",
  "44": "Great Britain",
  "1": "USA"
};

for(let code in codes) {
  alert(code); // 1, 41, 44, 49
}
```

* "49" 是整數屬性，因為 `String(Math.trunc(Number('49'))` 的結果還是 "49"。
* "+49" 不是整數屬性，因為 `String(Math.trunc(Number('+49'))` 的結果是 "49"，不是 "+49"。
* "1.2" 不是整數屬性，因為 `String(Math.trunc(Number('1.2'))` 的結果是 "1"，不是 "1.2"。

上面的例子中，如果想按照創建順序循環出來，可以用一個 討巧 的方法：

```js
let codes = {
  "+49": "Germany",
  "+41": "Switzerland",
  "+44": "Great Britain",
  // ..,
  "+1": "USA"
};

for(let code in codes) {
  console.log( +code ); // 49, 41, 44, 1
}
```

References: 

[JavaScript for...in 循環出來的對象屬性順序到底是什麼規律？](https://juejin.cn/post/6844903555401252871)
