---
title: "FirebaseAuthのトークンをRestAPIに渡す"
templateKey: blog-post
date: 2020-07-09T11:28:00+09:00
tags: [Firebase, "Firebase Authentication", Lambda]
draft: true
---

FirebaseAuth のトークンを RestAPI に渡す

<!--more-->

トークンの取得

```javascript
const idToken = firebase.auth().currentUser.getIdTokenResult(true);
const token = idToken.token;
```

トークンをヘッダに入れて送信

```javascript
http.post(
  URL,
  (headers: {
    Authorization: token,
  })
);
```

## --- API Gateway の場合 ---

API Gateway のオーソライザーを作成する。
トークンのソース: Authorization
Lambda: 認可用 Lambda

認可用 Lambda でトークンを検証し、トークンの権限に見合ったポリシーを返す

```javascript
const decodedToken = await admin.auth().verifyIdToken(event.autorizationToken);
let uid;
if (decodedToken) {
  uid = decodedToken.uid;
} else {
  uid = null;
}
if (uid) {
  return 権限のあるポリシー;
} else {
  return 権限のないポリシー;
}
```

## --- EC2 の場合 ---

リクエストのヘッダからトークンを取得する

トークンの有効聖チェックを行う

無効であればエラーレスポンスを返す。有効の場合は引き続き処理を実行する
