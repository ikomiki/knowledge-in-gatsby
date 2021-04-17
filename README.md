# knowledge-in-gatsby

Gatsby上で実装された、知識データベースサイト。

## リソース

* GitHub - [ikomiki/knowledge-in-gatsby](https://github.com/ikomiki/knowledge-in-gatsby)
* Netlify - [Site overview | ikomiki-knowledge](https://app.netlify.com/sites/ikomiki-knowledge/overview)
* Google Analytics - [アナリティクス | ホーム](https://analytics.google.com/analytics/web/#/p268639173/reports/defaulthome)

## ウェブサイト

* [知識データベース by 伊湖美樹](https://www.ikomiki.com/)
* [Content Manager](https://www.ikomiki.com/admin/#/collections/blog)

## 参考サイト

* [爆速サイト構築に挑戦１.ヘッドレスCMSを導入 - 清水誠メモ](https://makoto-shimizu.com/news/fast-cms/01-launch-gatsby-on-netlify/)
* [爆速サイト構築に挑戦２.サーバーサイドGTMでITP対応 - 清水誠メモ](https://makoto-shimizu.com/news/fast-cms/02-server-side-gtm/)
* [爆速サイト構築に挑戦３.Gatsbyをカスタマイズ - 清水誠メモ](https://makoto-shimizu.com/news/fast-cms/03-customize-gatsby/)


## ローカル開発

### インストール

```bash
npm i -g gatsby-cli
npm i -g netlify-cli

```

### ローカル実行

```bash
netlify dev
```

### サイト

* [ウェブサイト]](http://localhost:8000/)
* [Content Manager](http://localhost:8888/admin/)
* [GraphiQL](http://localhost:8888/___graphql)

### 主な修正対象

* src
  * components
    * Footer.js
    * Navbar.js
  * pages
    * index.md
    * 各ページ
      * index.md
* static
  * admin
    * config.yml - NetlifyのContent Managerの設定
* gatsby-config.js
* gatsby-node.js
