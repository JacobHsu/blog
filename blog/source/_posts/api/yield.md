---
title: yield generator函數
date: 2021-09-15 22:22:13
tags: API 
categories: API
---

### generator函數

`generator函數`跟普通函數在寫法上的區別就是，多了一個星號`*`，並且只有在generator函數中才能使用`yield`，
什麼是yield呢，他相當於generator函數執行的`中途暫停點`，  
比如下方有3個暫停點。而怎麼才能暫停後繼續走呢？  
那就得使用到next方法，next方法執行後會返回一個對象，對象中有`value`和`done`兩個屬性  

* value：暫停點後面接的值，也就是yield後面接的值
* done：是否generator函數已走完，沒走完為false，走完為true

```js
function* gen() {
  yield 1
  yield 2
  yield 3
  return 4
}
const g = gen()
console.log(g.next()) // { value: 1, done: false }
console.log(g.next()) // { value: 2, done: false }
console.log(g.next()) // { value: 3, done: false }
console.log(g.next()) // { value: 4, done: true }
```

### yield後面接函數

yield後面接函數的話，到了對應暫停點yield，會馬上執行此函數，並且該函數的執行返回值，會被當做此暫停點對象的`value`

```js
function fn(num) {
  console.log(num)
  return num
}
function* gen() {
  yield fn(1)
  yield fn(2)
  return 3
}
const g = gen()
console.log(g.next()) // { value: 1, done: false }
console.log(g.next()) // { value:2, done: false }
console.log(g.next()) // { value: 3, done: true }

```

### yield後面接Promise

函數執行返回值會當做暫停點對象的value值，那麼下面例子就可以理解了，前兩個的value都是pending狀態的Promise對象

```js
function fn(num) {
  return new Promise(resolve => {
    setTimeout(() => {
      resolve(num)
    }, 1000)
  })
}
function* gen() {
  yield fn(1)
  yield fn(2)
  return 3
}
const g = gen()
console.log(g.next()) // { value: Promise { <pending> }, done: false }
console.log(g.next()) // { value: Promise { <pending> }, done: false }
console.log(g.next()) // { value: 3, done: true }
```

其實我們想要的結果是，兩個Promise的結果1 和 2，那怎麼做呢？很簡單，使用Promise的`then`方法就行了

```js
const g = gen()
const next1 = g.next()
next1.value.then(res1 => {
  console.log(next1) // 1秒後輸出 { value: Promise { 1 }, done: false }
  console.log(res1) // 1秒後輸出 1

  const next2 = g.next()
  next2.value.then(res2 => {
    console.log(next2) // 2秒後輸出 { value: Promise { 2 }, done: false }
    console.log(res2) // 2秒後輸出 2
    console.log(g.next()) // 2秒後輸出 { value: 3, done: true }
  })
})
```

### next函數傳參

`generator`函數可以用`next方法來傳參`，並且可以通過`yield來接收這個參數`，注意兩點

* 第一次next傳參是沒用的，只有從第二次開始next傳參才有用
* next傳值時，要記住順序是，先右邊yield，後左邊接收參數

```js
function* gen() {
  const num1 = yield 1
  console.log(num1)
  const num2 = yield 2
  console.log(num2)
  return 3
}
const g = gen()
console.log(g.next()) // { value: 1, done: false }
console.log(g.next(11111))
// 11111
//  { value: 2, done: false }
console.log(g.next(22222)) 
// 22222
// { value: 3, done: true }
```

![Alt next](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/c49ec193e19249d2876fba7909f89acc~tplv-k3u1fbpfcp-watermark.awebp "next")

### Promise+next傳參

yield後面接Promise next函數傳參  

```js
function fn(nums) {
  return new Promise(resolve => {
    setTimeout(() => {
      resolve(nums * 2)
    }, 1000)
  })
}
function* gen() {
  const num1 = yield fn(1)
  const num2 = yield fn(num1)
  const num3 = yield fn(num2)
  return num3
}
const g = gen()
const next1 = g.next()
next1.value.then(res1 => {
  console.log(next1) // 1秒後同時輸出 { value: Promise { 2 }, done: false }
  console.log(res1) // 1秒後同時輸出 2

  const next2 = g.next(res1) // 傳入上次的res1
  next2.value.then(res2 => {
    console.log(next2) // 2秒後同時輸出 { value: Promise { 4 }, done: false }
    console.log(res2) // 2秒後同時輸出 4

    const next3 = g.next(res2) // 傳入上次的res2
    next3.value.then(res3 => {
      console.log(next3) // 3秒後同時輸出 { value: Promise { 8 }, done: false }
      console.log(res3) // 3秒後同時輸出 8

       // 傳入上次的res3
      console.log(g.next(res3)) // 3秒後同時輸出 { value: 8, done: true }
    })
  })
})
```

![Alt Promise+next](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/8db7c759079a404ebab41b9aacc90c77~tplv-k3u1fbpfcp-watermark.awebp "Promise+next")

#### References

[7張圖，async/await原理](https://juejin.cn/post/6993358764481085453)
