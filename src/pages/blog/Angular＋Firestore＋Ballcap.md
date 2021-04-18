---
title: "Angular＋Firestore＋Ballcap"
templateKey: blog-post
date: 2020-07-09T16:31:00+09:00
tags: [Angular, Firebase, "Cloud Firestore", Ballcap]
draft: true
---

## Ballcap とは

Ballcap は Cloud Firestore のデータベーススキーマ設計フレームワークです。
このリポジトリは、WEB および管理者をサポートします。

<!--more-->

## 実施環境

以下の環境で確認しました。

- Angular 10.0.2

## 手順

```bash
npm install -g @angular/cli
```

あらかじめ、Firestore プロジェクトを用意しておく

```bash
ng new PROJECT
cd PROJECT

npm add @1amageek/ballcap
ng add @angular/fire
? Please select a project: （用意したFirestoreプロジェクトを選択する）
```

[AngularFire Quickstart](https://github.com/angular/angularfire/blob/master/docs/install-and-setup.md)の 3、4、5 の手順でプロジェクトを設定する

TODO: 記入中...

## See also

- [1amageek/ballcap\.ts: Cloud Firestore support library for admin\. 🧢](https://github.com/1amageek/ballcap.ts)
- [angular/angularfire: The official Angular library for Firebase\.](https://github.com/angular/angularfire)
