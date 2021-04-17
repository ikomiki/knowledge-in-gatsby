---
title: "Firestoreのバックアップ手順"
templateKey: blog-post
date: 2020-07-09T11:29:00+09:00
tags: [Firebase, "Cloud Firestore"]
draft: true
---

## 事前準備

1. Firestore のアカウントを Blaze(従量)プランにする
2. https://cloud.google.com/sdk/docs/quickstarts から、 Google Cloud SDK をインストールする
3. `gcloud info | grep project`により、対象プロジェクトが想定通りであることを確認する。異なる場合は `gcloud config set project [PROJECT_ID]`により切り替える
4. Google Cloud Storage 上で、バックアップ先のバケットを作成する。名前は任意だが、`[PROJECT_ID]-backups-firestore` などにする。バケットのリージョンは Firestore と同じにしておく

## バックアップの実行

1. `gcloud alpha firestore export gs://[BUCKET_NAME]` を実行する

## リストアの実行

1. `gcloud info | grep project`により、対象プロジェクトが想定通りであることを確認する。異なる場合は `gcloud config set project [PROJECT_ID]`により切り替える
2. リストア対象のバックアップを、以下のいずれかの方法で特定する
   - バックアップ時のログから `outputUriPrefix:` を見つけ出し、以降の文字列を取得する
   - `gcloud alpha firestore operations list` コマンドを実行して出力したログから `outputUriPrefix:` を見つけ出し、以降の文字列を取得する
   - Cloud Storage のバックアップ先のバケットからバックアップ先のフォルダを取得する
3. `gcloud alpha firestore import gs://[BUCKET_NAME]/[EXPORT_PREFIX]` を実行する

リストアは、ドキュメントを上書きするように実行されているようです。
リストア先にバックアップ後に追加されたドキュメントが存在している場合、そのドキュメントは削除されずに残り続けました。
そのため、厳密にバックアップ前と同じ内容を復元するためには、何もデータが入っていないデータベースにインポートする必要があると考えられます。

定期バックアップについては、以下の記事が参考になりそうです(未検証)
https://medium.com/google-cloud-jp/firesto-77272bac8762
あるいは AWS の CloudWatch Event から Lamda 経由で REST API を呼びだすとかいう変態的手法もありかもです。
