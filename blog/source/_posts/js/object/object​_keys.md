---
title: Object​.keys()
date: 2021-04-18 13:23:22
tags: js
categories: js
---

[Object​.keys()](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Global_Objects/Object/keys)

https://jsbin.com/wurocegika/edit?js,console

```js
var arr = ['a', 'b', 'c'];
console.log(Object.keys(arr)); // console: ['0', '1', '2']

// 類似陣列的物件
var obj = { 0: 'a', 1: 'b', 2: 'c' };
console.log(Object.keys(obj)); // console: ['0', '1', '2']

// 擁有隨機 key 排序，類似陣列的物件
var an_obj = { 100: 'a', 2: 'b', 7: 'c' };
console.log(Object.keys(an_obj)); // console: ['2', '7', '100']
```

https://jsbin.com/sidixofelo/edit?js,console 

```js
const types = {
  status: '404',
  users: 'https'
}

const getObj = {};
Object.keys(types).forEach(type => {
    console.log(type) //  "status" "users"
    const camelCaseTypeRes = type + 'Res';
    getObj[type] = camelCaseTypeRes; 
})

console.log(getObj)
// [object Object] {
//  status: "statusRes",
//  users: "usersRes"
// }

```

