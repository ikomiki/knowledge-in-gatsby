---
title: "Hugo静的サイトジェネレータによるサイトの構築"
templateKey: blog-post
date: 2020-07-07T14:45:16.084Z
descrition: Hugo静的サイトジェネレータによるサイトの構築
tags: [Hugo]
draft: true
---

## Hugo とは

Hugo はオープンソースの静的サイトジェネレータです。  
GO 言語により開発されており、サイトの生成が爆速であることが特徴としています。

<!--more-->

## 文章のターゲット

以下の環境で実装しました

- macOS 10.15
- VisualStudioCode
- hugo 0.73

## 構築手順

### 環境設定

```bash
brew install hugo
hugo version
hugo new site knowledge-transfer-site && cd $_

git init
git submodule add https://github.com/budparr/gohugo-theme-ananke.git themes/ananke
echo 'theme = "ananke"' >> config.toml

code .
```

```config.toml
baseURL = "https://kb.rasen-lib.com/"
languageCode = "ja-jp"
title = "螺旋図書館ナレッジベース"


rss_sections = ["blog"]
```

### 記事の投稿

```bash
hugo new posts/example1.md
```

```content/_index.md
---
---
# Hugo テストサイト

Hugoのテストページです。
```

```content/post/example1.md
---
title: "テスト記事"
templateKey: blog-post
date: 2020-02-23T13:48:24+09:00
draft: false
tags: [ example, test ]
categories: [ test ]
---

# テスト記事です。

記事はmarkdown形式で記述します。

 * foo
 * bar
 * hoge

![画像を入れることも可能です](/img/dc.jpg)
```

### コンテンツのテスト

```bash
hugo server
```
