---
title: "Angularのロケール遅延読み込み"
templateKey: blog-post
date: 2020-07-16T00:07:00+09:00
tags: [Angular]
draft: true
---

## このドキュメントについて

Angular9 で新しい組み込み I18N サポート`@angular/localize`が導入されました。
それが含む 1 つの概念は、グローバルロケールと呼ばれます。これらは、アプリケーションのコンパイル後に CLI が追加できるさまざまな日付および数値形式のメタデータのバンドルです。

通常、CLI はこれらのバンドルを内部で使用して、異なる言語バージョンの作成を劇的に高速化します。ただし、意図した用途からそれをそらして、メタデータをオンデマンドで動的にロードすることができます。

See also: [Angular を使用したロケールの遅延読み込み\-ANGULARarchitects](https://www.angulararchitects.io/en/aktuelles/lazy-loading-locales-with-angular/)
