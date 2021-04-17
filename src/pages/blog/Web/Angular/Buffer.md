---
title: "AngularでのBufferの使用"
templateKey: blog-post
date: 2020-07-09T11:03:00+09:00
tags: [Angular, typescript]
draft: true
---

Angular での Buffer の使用

<!--more-->

```
npm i -D @types/node
npm i -S buffer
```

tsconfig.{app|lib}.json を修正 (tsconfig.spec.json は、既に追加されていることを確認する)

```
    "types": [
      "node" // ←左記を追加。このコメント自体は含めない
    ],
```

polyfills.ts 末尾に追加

```
const anyWindow: any = <any>window;
anyWindow.global = anyWindow.global || window;
global.Buffer = global.Buffer || require('buffer').Buffer;
```
