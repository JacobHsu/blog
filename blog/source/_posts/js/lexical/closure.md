---
title: Closure
date: 2021-07-28 22:22:13
tags: js
categories: es6
---

## Closure

> 閉包是指有權訪問另一個函數作用域中的變量的函數
當函數可以記住並訪問所在的詞法作用域時，就產生了閉包，
即使函數是在當前詞法作用域之外執行。

```js
function makeFunc() {
  var name = "Mozilla";
  function displayName() {
    alert(name);
  }
  return displayName;
}

var myFunc = makeFunc();
myFunc();
```

函式內的局部變數，只會在函式的執行期間存在。當 makeFunc() 執行完，你可能會預期 name 變數再也無法使用。

若你執行這個程式碼，字串 Mozilla 會以 JavaScript alert 提示顯示出來。

箇中理由和 JavaScript 函式的閉包有關。
閉包（Closure）是函式以及該函式被宣告時所在的作用域環境（lexical environment）的組合。

在這個例子中，`myFunc` 是 displayName 在 makeFunc 運行時所建立的實例（instance）參照。  

> myFunc函數是在當前詞法作用域之外 displayName 可以記住並訪問所在的詞法作用域

`displayName` 的實例保有了其作用域環境的參照，作用域裡則內含 name 變數。因此，在調用 myFunc 時，` name 變數被保存、並能作它用。` 「Mozilla」一詞也因此傳給了 alert。
>  myFunc 有權訪問另一個函數作用域 displayName 中的變量的函數

**什麼是閉包**

閉包是一種特殊的對象，它由兩部分組成：執行上下文（代號 A），
以及在該執行上下文中創建的函數 （代號 B），
當 B 執行時，如果訪問了 A 中變量對象的值，那麼閉包就會產生，且在 Chrome 中使用這個執行上下文 A 的函數名代指閉包。

**一般如何產生閉包**

返回函數
函數當做參數傳遞

## References

[閉包- JavaScript | MDN](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Closures)
[javascript 近乎神话般的概念：闭包](https://juejin.cn/post/6844904161268482062?utm_source=gold_browser_extension)