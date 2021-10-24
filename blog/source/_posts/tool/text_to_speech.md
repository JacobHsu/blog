---
title: TTS（Text To Speech）
date: 2021-10-25 22:22:13
tags: tool
categories: tool 
---

在項目中需要對 ajax 請求返回的消息進行語音播報，str 為需要播報的信息（適應於錯誤信息語音提示等場景）：

```js
//語音播報
function voiceAnnouncements(str) {
  // 百度語音合成：或者使用新版地址https://tsn.baidu.com/text2audio
  var url =
    'http://tts.baidu.com/text2audio?lan=zh&ie=UTF-8&spd=5&text=' +
    encodeURI(str);
  var n = new Audio(url);
  n.src = url;
  n.play();
}
voiceAnnouncements(`
人有悲歡離合，月有陰晴圓缺，此事古難全。但願人長久，千里共嬋娟。
`);
```

參數解釋：

lan：固定值 zh。語言選擇,目前只有中英文混合模式，填寫固定值 zh  
ie:編碼方式  
spd：語速，取值 0-9，默認為 5 中語速  
text：合成的文本，使用 UTF-8 編碼。小於 512 個中文字或者英文數字。（文本在百度服務器內轉換為 GBK 後，長度必須小於 1024 字節）  

ak1394 / [react-native-tts](https://github.com/ak1394/react-native-tts)
