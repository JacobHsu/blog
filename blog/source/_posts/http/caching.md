---
title: 前端緩存
date: 2021-08-10 22:22:13
tags: HTTP 
categories: HTTP
---

HTTP緩存

就是將http請求獲取的頁面資源存儲在本地，之後再加載直接從緩存中獲取而不用請求服務器，從而響應更快。

## 強緩存

第一次請求時，服務器把資源的過期時間通過響應頭中的Expires和Cache-Control兩個字段告訴瀏覽器，之後再請求這個資源的話，會判斷有沒有過期，沒有過期就直接拿來用，不向服務器發起請求，這就是強緩存

`Expires`

用來指定資源到期絕對時間，服務器響應時，添加在響應頭中。

`expires: Wed, 22 Nov 2021 08:41:00 GMT`

注意：如果服務器和瀏覽器端時間不一致的話可能導致失敗。比如現在時間是8月1，expires過期時間是8月2，客戶端把電腦時間改成了8月3，那就用不了這個緩存

`Cache-Control`

指定資源過期時間秒，如下，表示在這個請求正確返回後的300秒內，資源可以使用，否則過期

`cache-control:max-age=300`

### 為什麼指定緩存過期時間需要兩個字段呢？

因為有的瀏覽器只認識 Cache-Control，有的瀏覽器不認識，不認識的情況下再找 Expires

Expires 和 Cache-Control 的區別

* `Expires` 是`HTTP/1.0`中的，`Cache-Control` 是`HTTP/1.1`中的;
* `Expires` 是為了兼容，在不支持 HTTP/1.1 的情況下才會發生作用
* 兩者同時存在的話 `Cache-Control` 優先級高於 `Expires`;

### 強緩存的缺點

就是緩存過期之後，**不管資源有沒有變化，都會重新發起請求，重新獲取資源**

而我們希望的是在資源文件沒有更新的情況下，即使過期了也不重新獲取資源，繼續使用舊資源

所以協商緩存它來了，在強緩存過期的情況下，再走協商緩存的流程，判斷文件有沒有更新

## 協商緩存

### Last-Modified/If-Modified-Since

第一次請求資源時，服務器除了會返回給瀏覽器上面說的過期時間，還會在響應頭添加 `Last-Modified` 字段，告訴瀏覽器該資源的最後修改時間

`last-modified: Fri, 27 Oct 2021 08:35:57 GMT` (服務器響應頭回)

然後瀏覽器再次請求的時候就把這個時間再通過另一個字段`If-Modified-Since`，發送給服務器

`if-modified-since: Fri, 27 Oct 2021 08:35:57 GMT`  (瀏覽器再次請求送)

服務器再把這兩個字段的時間對比，
如果是一樣的，就說明文件沒有被更新過，就返回`狀態碼304`和空響應體給瀏覽器，瀏覽器直接拿過期了的資源繼續使用即可；
如果對比不一樣說明資源有更新，就返回`狀態碼200`和新的資源

所以說`Last-Modified/If-Modified-Since`它倆是成對的，是為了對比文件修改時間

### Last-Modified/If-Modified-Since的缺點

如果本地打開了緩存文件，即使沒有對文件進行修改，但還是會造成`Last-Modified`被修改，服務器端不能命中緩存導致發送相同資源

### ETag/If-None-Match

第一次請求資源時，服務器除了會在響應頭上返回`Expires`、`Cache-Control`、`Last-Modified`，還在返回`Etag`字段，表示當前資源文件的一個唯一標識。這個標識符由服務器基於文件內容編碼生成，能**精準感知文件的變化**，只要文件內容不同，ETag就會重新生成

`etag: W/"132489-1627839023000"`

然後瀏覽器再次請求的時候就把這個文件標識 再通過另一個字段 `If-None-Match`，發送給服務器

`if-none-match: W/"132489-1627839023000"`

服務器再把這兩個字段的時間對比，
如果發現是一樣的，就說明文件沒有被更新過，就返回`狀態碼304`和空響應體給瀏覽器，瀏覽器直接拿過期了的資源繼續使用；
如果對比不一樣說明資源有更新，就返回`狀態碼200`和新的資源

### Last-Modified 和 ETag 的區別

* `Etag` 感知文件精準度要高於 Last-Modified
* 同時使用時，服務器校驗優先級 Etag/If-None-Match
* `Last-Modified` 性能上要優於 Etag，因為 Etag 生成過程中需要服務器付出額外開銷，會影響服務器端的性能，所以它並不能完全替代 Last-Modified，只能作為補充和強化

### 強緩存與協商緩存的區別 

* 優先查找強緩存，沒有命中再查找協商緩存
* 強緩存不發請求到服務器，所以有時候資源更新了瀏覽器還不知道，但是協商緩存會發請求到服務器，資源是否有更新，服務器肯定知道
* 目前項目大多數使用緩存文案

協商緩存一般存儲：`HTML`
強緩存一般存儲：`css`, `image`, `js`，文件名帶上 `hash`

#### References

為什麼第二次打開頁面快？[吃透前端緩存](https://juejin.cn/post/6993358764481085453)