---
title: Promise add(...inputs)
date: 2021-07-22 13:14:22
tags: js
categories: js
---

```js
// 假設本地機器無法做加減乘除運算，需要通過遠程請求讓服務端來實現。
// 以加法為例，現有遠程API的模擬實現

const addRemote = async (a, b) => new Promise(resolve => {
  setTimeout(() => resolve(a + b), 1000)
});

// 請實現本地的add方法，調用addRemote，能最優的實現輸入數字的加法。
async function add(...inputs) {
  // 你的實現
}

// 請用示例驗證運行結果:
add(1, 2)
  .then(result => {
    console.log(result); // 3
});


add(3, 5, 2)
  .then(result => {
    console.log(result); // 10
})
```

```js
async function add(...args) {
 let res = 0;
 if (args.length <= 1) return res;
  
  for (const item of args) {
    res = await addRemote(res, item);
  }
  return res;
}
```

<script async src="//jsfiddle.net/JacobHsu/d13r4okg/1/embed/js/"></script>

優化加本地緩存

<script async src="//jsfiddle.net/JacobHsu/va1k2zdo/8/embed/js/"></script>

[Array.prototype.reduce()](https://developer.mozilla.org/zh-TW/docs/orphaned/Web/JavaScript/Reference/Global_Objects/Array/Reduce)

```js
// Promise鏈式調用版本
async function add(...args) {
  return args.reduce((promiseChain, item) => {
    return promiseChain.then(res => {
      return addRemote(res, item);
    });
  }, Promise.resolve(0));

}
```

> 如何將一組用Array包起來的Promise function“依序執行”

```js
const delayPromise = data =>
  new Promise((resolve, reject) => {
    setTimeout(() => {
      console.log(Date());
      console.log(data);
      resolve(data);
    }, 1000);
  });

const problem = [1, 2, 4, 5].map(delayPromise)
```

上面的程式是有問題的，原因是因為map的實作原理是**同步**的將數值**一個一個**帶入delayPromise當中。
為了方便理解，我們可以把map的簡易實作看成下列的程式碼

```js
const mapFunc = (mapper, dataArray) => {
  let res = [];
  for(const i in dataArray) {
    res[i] = mapper(dataArray[i]);
  }
  return res;
}
mapFunc(delayPromise, [1, 2, 4, 5]);
```

可以發現，先前的[1, 2, 4, 5].map(delayPromise)其實是一個用迴圈同步呼叫promise的實作，**因此結果並非依序執行promise**

那到底要怎麼讓promise依序執行呢？

### 用`reduce`將promise串接到Promise.resolve()後面

```js
[1, 2, 4, 5].reduce(
  (p, current) => p.then(() => delayPromise(current)),
  Promise.resolve()
)
/* 等效於 
Promise.resolve()
.then(()=>delayPromise(1))
.then(()=>delayPromise(2))
.then(()=>delayPromise(4))
.then(()=>delayPromise(5))
*/
```

### 用async/await去解決

```js
const asyncWay = async(dataArray) => {
  for(const i in dataArray) {
    await delayPromise(dataArray[i]);
  }
}
asyncWay([1, 2, 3, 4])
```

