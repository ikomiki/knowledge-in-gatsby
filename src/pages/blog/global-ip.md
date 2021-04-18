---
title: "node.jsでグローバルIPの取得"
templateKey: blog-post
date: 2020-07-09T03:17:00+09:00
tags: ["node.js", javascript]
draft: true
---

node.js でグローバル IP の取得を取得するコード

<!--more-->

```javascript
var http = require('http');
yield new Promise(function(resolve, reject)
  http.get('http://ifconfig.me', (res) => {
    let body = '';
    res.setEncoding('utf8');

    res.on('data', (chunk) => {
        body += chunk;
    });

    res.on('end', (res) => {
        res = body;
        console.log(res);
        resolve(res);
    });
  }).on('error', (e) => {
    console.log(e.message); //エラー時
    reject(e);
  });
});


```
