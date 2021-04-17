---
title: "ng-japan meetup 2019 Spring"
templateKey: blog-post
date: 2020-07-09T11:49:00+09:00
tags: [Angular, typescript]
draft: true
---

• デフォルトで差分ロード(es5 と es2015 の両対応。モダンブラウザで読み込みサイズが 7〜20%減量）
• ルーティングの遅延ロードの書き方を業界標準のものに変更 `{path:`/admin`, loadChildren: () => import(`./admin/admin.module`).then(m => m.AdminModule)}`
• CLI のっびる度

スポンサートーク：サイボウズ kintone のプラグインとして angular を入れることができたりする
angular+クロージャ
　 angular/tsickle 　 Typescipt→JSDoc アノテートされた Javascipt を生成
　 angular/clutz 　逆

---

ng-conf
Angular の最大級イベント。ソルトレイクで毎年開催

Youtube に動画公開済み
https://www.youtube.com/watch?v=O0xx5SvjmnU&list=PLOETEcp3DkCpimylVKTDe968yNmNIajlR

---

Angular8

Deffentional Loading
差分ロード(es5 と es2015 の両対応。モダンブラウザで読み込みサイズが 7〜20%減量）

```html
<script type="module" src="app-es2015.js"> モダン
<script nomodule src="app-es5.js"> IE11とか
```

Bazel によるビルド
差分ビルド・フルスタック・スケールアウト
node.js ではない

cli での bazel ビルド環境の実装は、オプトインプレビュー `ng add@angular/bazel --next`
https://bazel.angular.io/

---

ivy

現状、テストの 97%が通る

v9 では標準化する

ファイルサイズ
v7 151kb
v8(Diffential) 131kb
ivy Compat 126kb
v8+IVY 有効 Ivy Compat + Diffential 109kb
Ivy Future API + Differential 14kb

Angular8 で Ivy を利用する利点
メモリ使用量 down
テスト実行速度 Up
いくつもの既存バグの解決

v9 の Ivy で実装される予定
もっと小さくする
コンパイル高速
型チェック

---

Angular

Scalable
Evergreen

Angular2.0 の利点は大規模。小規模は手薄
4〜8、小・中規模への拡大(Elements,　 Ivy)、CLI も含まれる
v9 Angular Photon 実験的プロジェクト　高トラフィック砂 Web 愛と、圧倒的敬老・高速。Wiz(Gmail)と連携

https://www.youtube.com/watch?v=JX5GGu_7JKc

---

angular labs

Bazel
material-experimental
　 CDK→material→your app
　 CDK+MDC Web→material→your app

可能な限り早く
何がネックか Javascript の Load と Parse
読み込まない戦略
Fastest

コードの種類
Code youWill need Three-shaking の残り
might Lasy-Load
maight not クリックハンドラの関数
should not need（bootstrap、re-fetch）

Pelay（シンプル）→Resume（軽量）
サーバーで初期化

click してからクリックハンドラのソースを呼びだす

Day 3 keynote
Not Every SPA

プログレッシブレンダリング、プログレッシブ(React はサードバーティなどで行われているが、Angular は公式で

---

おすすめセッション
スライドをあとで読む

---

Modern Angular への道

Cypress でのテストスイートが便利
Elemctron ベースの画面便利
E2E テストの平子ウィック
テスト失敗時のスクリーンショットが自動保存

upgradeComponent を downgradeComponent 　としたら動いた

---

Bazel か別の何か（仮）

Bazel

インクリメンタルビルド
どのコンパイラでも使える（go とか）
ピュア関数ビルド
　書くのはルールだけ。アクションではない。
　 gulp の場合は書くのはアクション

問題
エラーメッセージがわかりにくい
mv や cp が難しい

angular cli での bazel ビルド環境の実装は、オプトインプレビュー `ng add@angular/bazel --next`
https://bazel.angular.io/

---

小規模チームにおけるドキュメント駆動開発の勧め

Voice
パーソナリティが自らの声を通して声のブログやニュースを放送しているボイスメディア

全てはコンポーネオｔ、モジュール階層を適切に単純化すること
ドキュメント作成の自動化

← 　 compodoc
https://github.com/compodoc/compodoc

Angular アプリケーションの自動ドキュメント化

利点
コンポーネントの階層を見える化
コメントの徹底
必要なメソッドが入ったことをそのまま確認できる
実装不足や不必要なレビューを減らせる
海外かいはつちーむへの開発依頼を単純か
仕様書作成コストの削減

コンポーネント再設定 ← テストの流れを少しずつ作る

NgRX、テストについては Issue 化

AngularDoc
テストや NgRx のアクション、ストア、ディスパッチが見れる
ドキュメントとしてはソースに直接飛ぶので使いにくい

---

Angular7 から 8 にあげてみた

angular

polyfill.ts が大幅に書き換わる

tslint.json のルール名がわかっている
mgechev/codelyzer/issues/791 tslink 側
tsconfig.json
modue:es2015→esnext
target: es5→es2015 古い cli で作ったのを自動アップデートされる

xxx.comoponent.ts
ViewChild/ ContentChild に {static: true}が増える。v8 の破壊的変更。※〜Children 対象外
ngOnInit で有効にんる場合 <div foo> → {static: true}
ngAfterViewInit で有効 <div foo \*ngIf="">→ {static: false}
※ V9 では省略可能になる予定

^^^^^^^

すごい大規模　たのしく作ろう

大規模開発向け

PrimeNG
見た目の良さ、ソースの見通しの良さ
デスクトップで使う場合は使いやすい
モバイル向けではイマイチ
Premium Applicateion Templates for PrimenNG 　一つ 10 万円

ag-Grid
データグリッドを提供。データがばか大きくてもサクサク。
PrimeNG の DataTable だと機能が足りない。

ionic
モバイルファーストの場合は UI 部品は使いやすい
スタック型のページ遷移

JHipster
モダン Web アプリケーションのひな形を素早く作れる Scaffold ツール

OpenAPI Generator
Swagger II SpringFox から API を作る

大規模のトラブル
IE で遅い
タブレットで遅い
→ タブレットでのみ操作がもたつく、コンポーネントの多い画面で顕著
→ タブレットは PC 端末に較べて低スペック
→ChangeDetectionStrategy.OnPush を使うことで解決

本は増えたが
パフォーマンス対策などの運用事例はまだ少ない
Angular がらみの問題はなし

Typescript は品質確保に役立っている

---

Angular はいいぞ！

tipsys 女性専用ソーシャルサービスの開発

よかったことベスト 5 with ionic

1 ng update / schematics
一発アップロード

2 Semver
破壊的変更がゆっくり。ドキュメント化さえている

3 All in One
公式パッケージが他機能　開発中止はめったにない

4 Intercepter
4.3 HttpClient の Intercepter を使えばすべての HTTP 通信をハックして書き換え可能
ログイン認証とかで使える

5 TypeScript / Compile Error

まとめ
Angular8 リリース
https://blog.angular.io/version-8-of-angular-smaller-bundles-cli-apis-and-alignment-with-the-ecosystem-af0261112a27

- Deffentional Loading 　差分ロード。モダンブラウザの読み込みサイズ 7〜20%減量
- Ivy 新レンダリングエンジン

ng-conf 2019 Spring Youtube チャンネル
https://www.youtube.com/watch?v=O0xx5SvjmnU&list=PLOETEcp3DkCpimylVKTDe968yNmNIajlR

Bazel Google 謹製ビルドツール（大規模開発向け。インクリメンタルビルド、node 以外のビルドにも対応）
https://bazel.angular.io/

compodoc Angular アプリケーションの自動ドキュメント化
https://github.com/compodoc/compodoc

AngularDoc 自動ドキュメント化。テスト、NgRx 対応。
https://angulardoc.github.io/#/

Google Closure Compiler JavaScript 最適化ツール
https://developers.google.com/closure/compiler/
https://github.com/angular/tsickle
https://github.com/angular/clutz

PrimeNG Angular の UI ライブラリ
https://www.primefaces.org/primeng/

ag-grid 高性能グリッド
https://www.ag-grid.com/

Ionic スマホ向けライブラリ
https://ionicframework.com/

JHipster ひな形の作成ツール
https://www.jhipster.tech/

OpenAPI Generator API ジェネレータ
https://github.com/OpenAPITools/openapi-generator
