---
title: "hugoプロジェクトのamplifyホスティング"
templateKey: blog-post
date: 2020-07-09T15:15:00+09:00
tags: [AWS, "Amplify", Hugo]
draft: false
---

## hugo プロジェクトの amplify ホスティング

<!--more-->

### 初期化

```bash
amplify init
（略）
? Source Directory Path:  content
? Distribution Directory Path: public
? Build Command:  hugo
? Start Command: npm run-script start

? Do you want to use an AWS profile? Yes
? Please choose the profile you want to use (任意のプロファイル)
```

```bash
amplify add hosting
? Select the plugin module to execute Hosting with Amplify Console (Managed hosting with custom domains, Continuous deployment)

? Choose a type Manual deployment
# ↑環境によってはCDなど他のデプロイ方法を採る
```

### デプロイ

```bash
amplify publish
```

必要であれば`config.toml`の`baseURL`をデプロイ先 URL に変更して上げ直す。

デプロイ先の URL は、実行後の標準出力、ないし AWS コンソールの AWS Amplify ページで確認できます。
