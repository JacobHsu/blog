---
title: 網頁 DOM 事件的效能優化：Debounce 和 Throttle
date: 2021-08-04 22:22:13
tags: DOM
categories: HTML
---

## 特定網頁效能問題改善

### 背景簡介

各版本的瀏覽器實作時，為了確保滑鼠移動、滾動、改變視窗大小 (mousemove, scroll, resize) 等事件能夠及時回應維持使用者體驗，觸發的頻率會比較高。也就是說，使用者在一個正常的操作中，有可能在短時間內觸發非常多次事件處理器 (event handler)。  

如果為這些短時間內觸發非常多次的事件處理器綁定一些 DOM 節點操作，就會引發大量消耗效能的 DOM 計算，不斷重新計算 DOM 元素的絕對位置，造成頁面緩慢，甚至瀏覽器直接崩潰。  

關於這個優化的一個知名的事件是：2011 年時，Twitter 頁面 scroll 時會變得緩慢  

當時用的解法相對簡單（設定計算量大的事件函數每 250 ms 執行一次），而目前已有眾多可行的解法處理這個問題，較常見的解法有 throttling 和 debouncing 等等。

### 解決方法

開始之前，先看一下兩種方法 debounce 和 throttle 的視覺化模擬，直觀就能感受兩種方法的區別。

debounce and throttle
[http://demo.nimius.net/debounce_throttle/](http://demo.nimius.net/debounce_throttle/)  

### 去抖動 debounce

模仿電器開關處理的方法，把多個訊號合併成一個訊號。讓一個函式在連續觸發時只執行一次。一個常見的用法是使用者連續輸入基本資訊後，才觸發事件處理器進行格式確認。

```js
function debounce(func, delay) {
  var timer = null;
  return function () {
    var context = this;
    var args = arguments;
    clearTimeout(timer);
    timer = setTimeout(function () {
      func.apply(context, args)
    }, delay);
  }
}
```

程式碼很直觀，先設一個計時器 (timer)，保存當下脈絡後 (context, args)，只要太早進來 (小於 delay) 就會重置計時器，直到成功執行 setTimeout 內的函式後結束。

注意這裡 debounce 回傳的是一個閉包 (closure)，是 js 的一個重要特性，不這樣寫的話 timer 就必須是全域變數，以防止每次呼叫 timer 都被重置產生錯誤。

### 函數節流 throttle

函數節流讓一個函數不要執行得太頻繁，也就是控制函數最高呼叫頻率，減少一些過快的呼叫來節流。一個常見的用法是減少 scroll 的觸發頻率，因為 scroll 常常綁定一些消耗資源的 render 的事件。

```js
function throttle(func, threshhold) {
  var last, timer;
  if (threshhold) threshhold = 250;
  return function () {
    var context = this
    var args = arguments
    var now = +new Date()
    if (last && now < last + threshhold) {
      clearTimeout(timer)
      timer = setTimeout(function () {
        last = now
        func.apply(context, args)
      }, threshhold)
    } else {
      last = now
      fn.apply(context, args)
    }
  }
}
```

與 debouncing 的程式邏輯相似，只多了一個時間間隔的判斷。

### Debounce 和 Throttle 的 library

俗話說「不要自己製造輪子」，建議直接使用 underscore 或 Lodash 的實作比較穩定。

[Lodash](https://lodash.com/)

[Underscore](http://underscorejs.org/)

[vueuse](https://vueuse.org/) ex: [hooks/event/useEventListener.ts](https://github.com/anncwb/vue-vben-admin/blob/main/src/hooks/event/useEventListener.ts)

## References

[dom-debounce-throttle](https://mropengate.blogspot.com/2017/12/dom-debounce-throttle.html)