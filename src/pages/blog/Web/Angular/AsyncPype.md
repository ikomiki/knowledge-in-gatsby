---
title: "AsyncPipe"
templateKey: blog-post
date: 2020-07-09T11:48:00+09:00
tags: [Angular, typescript, NgRx]
draft: true
---

AsyncPype

初期値 null 問題

Push Pipe(ngrx component)
https://github.com/ngrx/platform/issues/2052
ゾーンなしで実行できることが改善点

初期値 null 対応法
<ng-container \*ngIf="store\$ | async">

ただしあるいは、直接 store$よみこませない君 (userName$)

combineLatest の無駄撃ちに注意する(セレクタのメモライズ)

store\$は神ステート化してしまうので分ける

バッドノウハウ
state.user.〜 みたいな長ったらしいのは使わない。別ステートにしろよ
