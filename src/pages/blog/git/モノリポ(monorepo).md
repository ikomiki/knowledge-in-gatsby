---
title: "モノリポ(monorepo)"
templateKey: blog-post
date: 2020-07-26T03:45:00+09:00
tags: ["git", "monorepo"]
draft: true
---

## モノリポとは

npm で管理する複数の package を、まとめて一つの git リポジトリで管理すること。

<!--more-->

## 通常のリポジトリの場合の問題点

- 複数のリポジトリをまたがった変更の差分を確認しづらい[^1]
- アプリケーションとリポジトリの対応を覚える必要がある[^1]
- リポジトリ数が増えてくると、リポジトリ権限やリポジトリ設定をする労力が増える[^1]
- 複数リポジトリにまたがった課題(issue)を管理しづらい[^1]

## モノリポの利点

- コードが再利用しやすい[^1]
- 開発者同士のコラボレーションが簡単になる[^1]
- コミットを通じて依存がシンプルになる[^1][^7]
- 根本的なリファクタリングや改修が行える(すでにある資産を使い回せる)[^1][^4]
- 多数のリポジトリを管理するコストから解放される（リポジトリ一覧のドキュメントが不要になる）[^1]
- issue の受付場所を 1 つに絞ることが出来る[^4]
- 不人気な `git submodule` を扱わなくて済むようになる[^3]
- （各パッケージに対して）単一のビルド/テスト/リリースのプロセスを適用できる[^4]
- 全てのパッケージを同時にテストできるため、パッケージをまたいで発生するバグの発見が容易になる[^4]
- Github のスターが集中しやすくなる[^4]
- すべてのパッケージにまたがる機能の名前空間を変えられる[^7]
- コード解析が可能に[^7]

## モノリポの問題点

- pull したときに再コンパイルが必要になることが多くなる。ビルドやクローンのコスト増大[^3][^7]
- Slack に進捗の経過を push するフックのようなツールは基本的に使うことが出来ない（トラフィックごとにチャンネルを分けるような方法がまだ存在しないため）[^3]
- コードベースが威圧的になる[^4]
- リポジトリのサイズが肥大化する[^4]
- issue が集中するのでかえって管理の手間が発生する[^4]
- 共通処理パッケージのバージョン管理が出来ない[^5]
- 特に GUI の git クライアントを使っていると動作が遅くなる[^5]
- 依存性解決が簡単になりすぎる[^7]
- コードが社内に公開されてしまう[^7]

> monorepo は、全体的なサービスのリリースを周期的に行っていたり、共通処理パッケージの修正を専任のチームが担当したりする場合は、ある程度の QA 期間を経てリリースできるため、良いかもしれません。また 1 サービスをパッケージを分けて管理したいというだけの場合も有効かなと思います。  
> しかし、 弊社のようにサービスごとにチームが編成されていたり、サービスごとリリースサイクルが違ったりする場合は、サービスごと独立したリリースが行えたり、パッケージのバージョン管理ができたりといった点で、monorepo ではなく manyrepo の方が合うようです。[^5]

## モノリポの採用実績

企業としては Google, Dropbox, NETFLIX, Uber。OSS では BABEL、Synfony, Vue, React がモノリポ構成を取る。

## モノリポを辞めた例

- [モノリポからマルチリポへの移行：コンチネンタル・コーポレーションがどのように Git を管理しているか \| GitHub Resources](https://resources.github.com/videos/How-Continental-manages-distributed-code-base/) [^5]
- [複数サービスの Web フロントエンドを運用する際のリポジトリ構成〜monorepo から manyrepo へ〜 \| スペースマーケットブログ](https://blog.spacemarket.com/code/web-frontend-repository-composition-monorepo-or-manyrepo/)

## モノリポ導入の障害

- 様々なプログラミング言語やビルドシステムを統合する必要がある[^1]
  - [bazel](https://bazel.build)
  - [pantsbuild](https://www.pantsbuild.org)
  - [buck](https://buck.build)

## モノリポを導入する

- [git subtree](https://manpages.debian.org/testing/git-man/git-subtree.1.en.html) により外部リポジトリを取り込む[^1]
- [git sparse-checkout](https://git-scm.com/docs/git-sparse-checkout)により必要なディレクトリだけを参照できるようにする [^2]
- [Lerna を使用する](#lerna-を使用する)

### Nx を使用する[^9]

### Lerna を使用する

Lerna はモノレポの作成、ブートストラップ、各種のタスクランニング、そしてパッケージの公開を実行するためのツールです。[^4][^6]

JavaScript プロジェクトを管理するためのツールであり、npm を使用します。

#### Lerna のメリット

- バージョンを、固定/ロックモード(全てのパッケージバージョンを自動結合する)、あるいは独立モード（パッケージごとにバージョンを持つ）の管理が出来る。[^7]

### Symplify/Monorepo Builder

Composer ベースのモノリポ化ツール。PHP。[^7]

### Splitsh/lite

Laravel で使用されているコード分割ツール。Golang 製。CLI ベースでコードをディレクトリごと分割し、ReadOnly なリポジトリにタグ付きで push してくれる。[^7]

## モノリポで変更された部分だけをビルドする[^8]

### ビルドツールの差分ビルドを活用する[^8]

Gradle、Make、Bazel などの依存性を考慮するビルドツールでは、採用された部分だけをビルドする機能が備わっている。

CI 環境では使えない（キャッシュが残っていたとしても、そのキャッシュとの差分が実際の変更の差分と一致する保証がない）

### Git の差分から特定する[^8]

Git の差分から変更された部分を特定する。`git diff --name-only <commit>` でファイル名一覧を取得する。

ビルドのトリガーとなったプッシュに含まれる差分の取得は、CI ツールに依存する。

- Travis CI は環境変数 `TRAVIS_COMMIT_RANGE`。
- GitHub Action はワークフローのトリガで`on.push.paths`を指定。[実装例あり](https://orangain.hatenablog.com/entry/monorepo-ci-cd)。
- CircleCI ではパイプライン変数 `.git.revision` と `pipeline.git.base_revision` を比較する。ただし新しい feature ブランチプッシュ時には`pipeline.git.base_revision`が空になる。
- CloudBuild ではビルドトリガーの「含まれるファイルフィルター」を設定することで可能。ただし新しい feature ブランチをプッシュしたときには新しいコミットに含まれる差分のみがフィルターの差分になる
- AWS CodePipelines は不明

## 参考資料

[^1]: [開発者が少ないチームはリポジトリを少なくしよう。Git Subtree で履歴を残したままリポジトリを統合する \- Qiita](https://qiita.com/tamanobi/items/6e951a18591a2d1440eb)
[^2]: [モノリポ時代に知っておくと便利な「git sparse\-checkout」 \- kakakakakku blog](https://kakakakakku.hatenablog.com/entry/2020/06/04/104940)
[^3]: [モノリポによって得られる多くの恩恵 \| Yakst](https://yakst.com/ja/posts/5499)
[^4]: [モノレポ入門 \- Gutenberg が採用するリポジトリ戦略 \- チュートリアル \- Capital P \- WordPress メディア](https://capitalp.jp/2019/02/19/%E3%83%A2%E3%83%8E%E3%83%AC%E3%83%9D%E5%85%A5%E9%96%80-gutenberg-%E3%81%8C%E6%8E%A1%E7%94%A8%E3%81%99%E3%82%8B%E3%83%AA%E3%83%9D%E3%82%B8%E3%83%88%E3%83%AA%E6%88%A6%E7%95%A5/)
[^5]: [複数サービスの Web フロントエンドを運用する際のリポジトリ構成〜monorepo から manyrepo へ〜 \| スペースマーケットブログ](https://blog.spacemarket.com/code/web-frontend-repository-composition-monorepo-or-manyrepo/)
[^6]: [Lerna と Yarn Workspaces で Monorepo 管理 \- Cybozu Inside Out \| サイボウズエンジニアのブログ](https://blog.cybozu.io/entry/2020/04/21/080000)
[^7]: [SCOUTER のリポジトリ戦略　 Monolith→MultiRepo→MonoRepo へと移行した理由 \- ログミー Tech](https://logmi.jp/tech/articles/321292)
[^8]: [monorepo の CI/CD で変更された部分だけをビルド/デプロイする \- orangain flavor](https://orangain.hatenablog.com/entry/monorepo-ci-cd)
[^9]: [GitHub \- nrwl/nx: Extensible Dev Tools for Monorepos](https://github.com/nrwl/nx)
