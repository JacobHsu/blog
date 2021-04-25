---
title: 隊列（Queue）
date: 2021-04-25 13:23:22
tags: js
categories: js
---

# 數據結構：隊列（Queue）

```js
function Queue() {
  let items = []
  this.enqueue = function(e) {
    items.push(e) // 進隊
  }
  this.dequeue = function() {
    return items.shift() // 出隊
  }
  this.isEmpty = function() {
    return items.length === 0 // 是否是空隊
  }
  this.front = function() {
    return items[0] // 獲取隊頭元素
  }
  this.clear = function() {
    items = [] // 清空隊
  }
  this.size = function() {
    return items.length // 獲取隊列長度
  }
}
```

查找：從對頭開始查找，從時間複雜度為 O(n)
插入或刪除：進棧與出棧的時間複雜度為 O(1)

## 時間複雜度

如果演算法中包含巢狀的迴圈，則基本語句通常是最內層的迴圈體，如果演算法中包含並列的迴圈，則將並列迴圈的時間複雜度相加。例如：

`for (i=1; i<=n; i++)　　x++;`  
`for (i=1; i<=n; i++)　　for (j=1; j<=n; j++)　　x++;`

第一個for迴圈的時間複雜度為`O(n)`，第二個for迴圈的時間複雜度為O(n2)，
則整個演算法的時間複雜度為O(n+n2)=O(n2)。

`Ο(1)`表示基本語句的執行次數是一個常數，一般來說，只要演算法中不存在迴圈語句，其時間複雜度就是O(1)

常見的演算法時間複雜度由小到大依次為：

> O(1)＜O(log2n)＜O(n)＜O(nlog2n)＜O(n2)＜O(n3)＜…＜O(2n)＜O(n!)

O(log2n)、O(n)、O(nlog2n)、O(n2)和O(n3)稱為`多項式時間`
O(2n)和O(n!)稱為`指數時間`  

# 雙端隊列（Deque）

Deque 在原有隊列的基礎上擴充了：隊頭、隊尾都可以進隊出隊

```js
function Deque() {
  let items = []
  this.addFirst = function(e) {
    items.unshift(e) // 方法會添加一個或多個元素至陣列的開頭，並且回傳陣列的新長度
  }
  this.removeFirst = function() {
    return items.shift() // 方法會移除並回傳陣列的第一個元素。此方法會改變陣列的長度
  }
  this.addLast = function(e) {
    items.push(e)
  }
  this.removeLast = function() {
    return items.pop()
  }
}
```

## 經典的雙端隊列問題：翻轉字符串裡的單詞

輸入: "a good   example"  
輸出: "example good a"  
解釋: 如果兩個單詞間有多餘的空格，將反轉後單詞間的空格減少到只含一個。

說明：

無空格字符構成一個單詞。  
輸入字符串可以在前面或者後面包含多餘的空格，但是反轉後的字符不能包括。  
如果兩個單詞間有多餘的空格，將反轉後單詞間的空格減少到只含一個。  

解題思路：使用雙端隊列解題

首先去除字符串左右空格  
逐個讀取字符串中的每個單詞，依次放入雙端隊列的對頭  
再將隊列轉換成字符串輸出（已空格為分隔符）  

![Alt Deque](https://i.imgur.com/8FYQBqO.png "Deque")

```js
var reverseWords = function(s) {
    let left = 0
    let right = s.length - 1
    let queue = []
    let word = ''
    // 首先去除字符串左右空格  
    while (s.charAt(left) === ' ') left ++ 
    while (s.charAt(right) === ' ') right -- 
    // 逐個讀取字符串中的每個單詞，依次放入雙端隊列的對頭  
    while (left <= right) {
        let char = s.charAt(left)
        if (char === ' ' && word) {
            queue.unshift(word) //  "a" -> "good" -> "example"
            word = ''
        } else if (char !== ' '){
            word += char
        }
        left++
    }
    console.log(word) // "a" -> "good" -> "example"
    queue.unshift(word) // ["a"]  -> ["good", "a"] -> ["example", "good", "a"]
    console.log(queue) // ["example", "good", "a"]
    return queue.join(' ') // "example good a"
};
const ans = reverseWords("a good   example")
console.log(ans) // "example good a"
```

### 正则 + JS API

```js
var reverseWords = function(s) {
    return s.trim().replace(/\s+/g, ' ').split(' ').reverse().join(' ')
};
```

## References

[前端进阶算法：一看就懂的队列及配套算法题](https://juejin.im/post/5eb2c1d76fb9a0435210a12c?utm_source=gold_browser_extension)