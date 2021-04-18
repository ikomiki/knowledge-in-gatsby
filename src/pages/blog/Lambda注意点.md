---
title: "Lambda注意点"
templateKey: blog-post
date: 2020-07-15T17:57:00+09:00
tags: [Lambda, DynamoDB]
draft: true
---

AWS Lambda、API Gateway、Step Functions でのはまりポイントを列挙します。

<!--more-->

- python で DynamoDB の数値を取得すると Decimail 型になるので、Lambda 返し値にするときは int にキャストする。  
  [Step Function のパラメータに「Lambda で DynamoDB から取得した数値」を使うときは int 型に変換しよう \| Developers\.IO](https://dev.classmethod.jp/articles/step-function-not-use-dynamodb-number/)
