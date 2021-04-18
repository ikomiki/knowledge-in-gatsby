---
title: "Cloud Functions のTips"
templateKey: blog-post
date: 2020-07-09T11:33:00+09:00
tags: [Firebase, "Cloud Functions"]
draft: true
---

## Cloud Functions の Tips

<!--more>-->

1. Cold Start 対策する
   1. グローバルスコープでモジュールをロードしない
   2. HTTP 関数で Firestore をなるべく使わない
   3. 全ての関数を定期的に実行する
      1. スリープを避けられる(Lambda からある有名手法)間隔は 10 分程度
   4. 参考: https://tech.ginco.io/post/ginco-engineer-meetup-2018-cloud-functions/ Ginco における Cloud Functions の利用とその高速化
   5. 適切なリージョンで利用する
   6. Firestore のデータ取得の機会を減らす。Firestore への初回接続は Cold Start する。可能な限りは Client から直接つなぐ
      1. リクエストのユーザ確認を Firebase Authentication で行うようにして、Firestore のデータ取得を行わないようにする
      2. Runtime を Node8 にすることで、若干軽減される
2. テストに firebase-functions-test を使う
   1. mock が使える
      1. Admin SDK の接続プロジェクト
      2. コンテキストオブジェクト
      3. functions.config()
3. retry はデフォルトで行わないので注意
   1. 関数にバグがあると大量に実行されてしまうため、エラーログを残して運用でカバー推奨
      1. 例) 10 秒ごとに retry、7 日成功しなかったら終了
      2. https://github.com/firebase/firebase-functions/issues/297
4. （現状では）同一名の関数を複数リージョン二デプロイできない
   1. 関数名に postfix をつけて叩き分ける対応もあり(GetDataJP)
   2.

index.js

```javascript
if (!process.env.FUNCTION_NAME || process.env.FUNCTION_NAME === "Func1") {
  exports.Func1 = require("./funcs/func1");
}

if (!process.env.FUNCTION_NAME || process.env.FUNCTION_NAME === "Func2") {
  exports.Func2 = require("./funcs/func2");
}
```

ColdStart 時に全ての関数が評価され、関数の実行時にはその関数のみが評価されるようにする

```javascript
const functions = require('firebase-functions');

module.exports = functions.region('asia-northeast1') // ここでregion指定を追加
                          .https
                          .onRequest((req, res) => {

```

```javascript
const functions = require("firebase-functions");

module.exports = functions
  .region("asia-northeast1")
  .https.onCall((data, ctx) => {
    //トリガーをonCallへ変更
    // 認証済みユーザからのリクエストかチェック
    if (!ctx.auth) {
      throw new functions.https.HttpsError(
        "unauthenticated",
        "This function must be called while authenticated."
      );
    }

    // ……
  });
```
