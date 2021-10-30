---
title: Array.prototype.sort()
date: 2021-10-30 13:23:22
tags: js
categories: js
---

sort 方法可以直接使用函式運算式 (en-US)（以及閉包（closures） (en-US)）：


```js
var numbers = [4, 2, 5, 1, 3];
numbers.sort(function(a, b) {
  return a - b;
});
console.log(numbers);

// [1, 2, 3, 4, 5]
```

```js
var numbers = [4, 2, 5];
numbers.sort(function(a, b) {
  console.log(a, b);
// 2 4
// 5 2
// 5 4
  return a - b;
});
console.log(numbers);
// (3) [2, 4, 5]
```

Array.prototype.indexOf()

```js
const beasts = ['ant', 'bison', 'camel', 'duck', 'bison'];

console.log(beasts.indexOf('bison'));
// expected output: 1

const age = [33, 11, 55, 22, 66];

console.log(age.indexOf(11));
// expected output: 1
```

一個對象數組按照另一個數組排序

```js
sortFunc = (propName, referArr) => (prev, next) =>
  referArr.indexOf(prev[propName]) - referArr.indexOf(next[propName]);
// 按照年齡age的順序給person排序
const age = [33, 11, 55, 22, 66];
const person = [
  { age: 55, weight: 50 },
  { age: 22, weight: 42 },
  { age: 11, weight: 15 },
  { age: 66, weight: 56 },
  { age: 33, weight: 68 },
];
person.sort(sortFunc('age', age)); // referArr: age
// 結果：
// [
//  {"age": 33,"weight": 68},
//  {"age": 11,"weight": 15},
//  {"age": 55,"weight": 50},
//  {"age": 22,"weight": 42},
//  {"age": 66,"weight": 56}
// ] 
```