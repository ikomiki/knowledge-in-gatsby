---
title: "AWS AmplifyをAngularに"
templateKey: blog-post
date: 2020-07-09T11:46:00+09:00
tags: [Angular, typescript, "AWS Amplify"]
draft: true
---

https://aws-amplify.github.io/docs/js/angular

ng new HOGEHOGE

npm i -S aws-amplify aws-amplify-angular

https://aws-amplify.github.io/docs/cli-toolchain/usage?sdk=js#iam-policy-for-the-cli
CLI のアクセスキーのユーザにポリシーを割り当てる

amplify init

プロジェクトはなるべく一意にする。amplify-test などだと S3 バケットで失敗することがある
environment は dev,stg,prod など

amplify auth
さしあたりデフォルトで

amplify storage
さしあたりデフォルトで
（アクセス許可はさしあたり全て。必要なら delete を削るなどする）

amplify push
反映

https://aws-amplify.github.io/docs/js/angular#importing-the-amplify-angular-module-and-the-amplify-provider
移行を実装
