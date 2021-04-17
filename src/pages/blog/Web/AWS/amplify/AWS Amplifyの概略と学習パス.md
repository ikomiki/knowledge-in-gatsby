---
title: "AWS Amplifyの概略と学習パス"
templateKey: blog-post
date: 2020-07-15T19:24:00+09:00
tags: [Amplify, Angular]
draft: false
---

## AWS Amplify とは

[AWS Amplify](https://aws.amazon.com/jp/amplify/) は、ウェブでフルスタックアプリケーションを構築出来るようにするツールとサービスのセットです。

<!--more-->

漫画での紹介記事: [AWS マンガ 第 3 話「高負荷に強いサイトを構築しろ！」\(1/5\) \| AWS](https://aws.amazon.com/jp/campaigns/manga/vol3-1/?sc_icampaign=awareness_awsmanga_amplify&sc_ichannel=ha&sc_icontent=awssm-2417&sc_iplace=banner&trk=ha_awssm-2417_awareness_awsmanga_amplify_banner)

Amplify は以下の 3 つのコンポ − ネントで構成されています。

- [CLI ツールチェーン](https://aws.amazon.com/jp/amplify/features/#CLI_toolchain)
- [ライブラリ](https://aws.amazon.com/jp/amplify/features/#Libraries)
- [UI コンポーネント](https://aws.amazon.com/jp/amplify/features/#UI_components)

以下のようなクラウド機能が使用出来ます。

- ウェブホスティング(BASIC 認証対応!)
- 認証(Cognito)
- データ(AppSync)
- API(GraphiSQL および REST)
- ストレージ(S3)
- その他、分析(Pinpoint、Kinesis)、インタラクション(チャットボット。Lex)、AI/ML(Amazon Machine Learning)、PubSub(IoT、MQTT)、プッシュ通知(Pinpoint)など

以下のアプリケーションがサポートされます。

- iOS
- Android
- ウェブ
- React Native

Web アプリケーションでは、以下のフレームワークに対応しています。

- React
- Angular
- Vue
- Ionic

以下の UI コンポーネントを使用できます。

- 認証
- その他。ストレージ(フォトピッカーやアルバムなど)、インタラクション(チャットボット)

## 学習パス

AWS Amplify の学習パスとしては、公式チュートリアルを通したうえで、Developer.IO の記事（および、ページ内の動画）を熟読することがよいかと考えます。

### [Getting started \- Amplify Docs](https://docs.amplify.aws/start/q/integration/angular)

Angular での Amplify の CLI、ライブラリ、UI コンポーネントのチュートリアルです。 [^angular9の注意]

### Developers.IO 内の記事

- [AWS のモバイルアプリ向けサービス・ツールが「AWS Amplify」に統合されました！ \#reinvent \| Developers\.IO](https://dev.classmethod.jp/articles/amplified/)  
  サービス発表時の紹介記事。
- [HIGOBASHI\.AWS 第 9 回 AWS re:Invent 2018 報告会「モダンな Web アプリを楽々デプロイ！！ AWS Amplify Console」を話しました \#higobashiaws \#reinvent \| Developers\.IO](https://dev.classmethod.jp/articles/higobashi-aws-9-amplify-console/)  
  紹介記事。
- [Angular \+ Amplify DataStore を試してみる \#reinvent \| Developers\.IO](https://dev.classmethod.jp/articles/angular-amplify-datastore/)  
  Angular での DataStore
- [Amplify で Cognito の Hosted UI を利用した認証を最低限の実装で動かしてみて動作を理解する \| Developers\.IO](https://dev.classmethod.jp/articles/learn-authentication-using-cognitos-hosted-ui-with-amplify/)  
  素の Javascript でのログインページ実装
- [AWS Amplify と Angular8 を使って Cognito で AWS Management Console にログインするページを作ってみる \| Developers\.IO](https://dev.classmethod.jp/articles/aws-management-console-login-with-cognito-user-using-amplify-and-angular8/)  
  Angular8 でのログインページ実装 [^angular9の注意]
- [「Amplify と AppSync でモダンでイケてる Web サービスを爆速で立ち上げようぜ 」を話しました \#devio2020 \| Developers\.IO](https://dev.classmethod.jp/articles/amplify-appsync-bakusoku-devio2020/)  
  Angular での AppSync

## サイト

- [Amplify Community \- Welcome](https://amplify.aws/community/)  
  Amplify のコミュニティ。英語。
- [AWS Amplify ｜ Developers\.IO](https://dev.classmethod.jp/tag/aws-amplify/)  
  Developers.IO 内の AWS Amplify のページです。

[^angular9の注意]: 記事執筆時での Amplify の UI コンポーネントは Ivy 非対応のため、Ivy を無効化する必要があります。参考: [バージョン 9 で Ivy をオプトアウトする ](https://angular.jp/guide/ivy#%E3%83%90%E3%83%BC%E3%82%B8%E3%83%A7%E3%83%B39%E3%81%A7ivy%E3%82%92%E3%82%AA%E3%83%97%E3%83%88%E3%82%A2%E3%82%A6%E3%83%88%E3%81%99%E3%82%8B)
