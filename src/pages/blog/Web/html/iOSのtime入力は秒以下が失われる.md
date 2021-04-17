---
title: iPhone/iPadの<input type="time">で秒・ミリ秒が失われる問題
templateKey: blog-post
date: 2020-07-09T11:18:00+09:00
tags: [html, iOS]
draft: true
---

iPhone/iPad の&lt;input type="time"&gt;で秒・ミリ秒が失われる問題

<!--more-->

iPhone/iPad の&lt;input type="time"&gt;が時・分までしか入力出来ず、秒・ミリ秒が失われる。

IOS11+(iPhone8/iPadPro10.5)で確認

```javascript
timeを<input type="time">から取得した場合、

/* Invalid */ new Date(`{date} {time}`); // time="mm:hh"のばあいに Invalid Date となる
/* Valid */ new Date(`{date}T{time}+09:00`); // time="mm:hh"のばあいでも正常表示できる
```
