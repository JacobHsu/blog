---
title: let 和 const
date: 2021-05-31 22:22:13
tags: js
categories: es6
---

## let 和 const

块级声明用于声明在指定块的作用域之外无法访问的变量。

当在全局作用域中使用 var 声明的时候，会创建一个新的全局变量作为全局对象的属性。

```js
var value = 1;
console.log(window.value); // 1
```

然而 let 和 const 不会：

```js
let value = 1;
console.log(window.value); // undefined
```

## let 和 const 的区别

const 用于声明常量，其值一旦被设定不能再被修改，否则会报错。

值得一提的是：const 声明**不允许修改绑定，但允许修改值**。这意味着当用 const 声明对象时：

```js
const data = {
    value: 1
}

// 没有问题
data.value = 2;
data.num = 3;

// 报错
data = {}; // Uncaught TypeError: Assignment to constant variable.
```

## 临时死区

临时死区(Temporal Dead Zone)，简写为 TDZ。

let 和 const 声明的变量不会被提升到作用域顶部，如果在声明之前访问这些变量，会导致报错：

```js
console.log(typeof value); // Uncaught ReferenceError: Cannot access 'value' before initialization
let value = 1;
```

这是因为 JavaScript 引擎在扫描代码发现变量声明时，要么将它们提升到作用域顶部(遇到 var 声明)，要么将声明放在 TDZ 中(遇到 let 和 const 声明)。访问 TDZ 中的变量会触发运行时错误。只有执行过变量声明语句后，变量才会从 TDZ 中移出，然后方可访问。

看似很好理解，不保证你不犯错：

var value = "global";

```js
// 例子1
(function() {
    console.log(value);

    let value = 'local';
}());

// 例子2
{
    console.log(value);

    const value = 'local';
};
```

两个例子中，结果并不会打印 "global"，而是报错 `Uncaught ReferenceError: Cannot access 'value' before initialization`，就是因为 TDZ 的缘故。

## References

ES6 系列之 [let 和 const](https://juejin.cn/post/6844903608316592141)
