---
title: "IAMユーザ管理"
templateKey: blog-post
date: 2020-07-15T17:24:00+09:00
tags: ["IAM"]
draft: false
---

AWS のセキュリティのベストプラクティスを列挙します。

<!--more-->

- ユーザの権限はグループで行う。IAM のユーザの使い回しはしない。
- 以下を有効にする。Cloud Trail, Config, GuardDuty, Security Hub, MFA
- [Landing Zone \| 実装 \| AWS ソリューション](https://aws.amazon.com/jp/solutions/implementations/aws-landing-zone/)、[特徴 \- AWS Control Tower \| AWS](https://aws.amazon.com/jp/controltower/features/)により AWS マルチアカウントを構築する
- SQS を挟んで Frontend と Back エンドにする
- 計算資源を使い捨てる。
  - ブートストラッピング(Cloud-init を使用したソフトウェアインストール、各種設定)
  - ゴールデンイメージ(特定の状態でスナップショットを取得)
  - コンテナ(ECS の Docker サポート)

See also: [AWS Lambda 関数を使用するためのベストプラクティス\-AWS Lambda](https://docs.aws.amazon.com/lambda/latest/dg/best-practices.html)
