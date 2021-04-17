---
title: "Angularアンチパターン"
templateKey: blog-post
date: 2020-07-09T11:07:00+09:00
tags: [Angular, typescript]
draft: true
---

Angular アンチパターン

<!--more-->

コンポーネントの初期化処理を constructor で書かない
@Input()が評価されない

コンポーネントの共通初期化処理を基底クラスの ngOnInit()では書かない。そもそも継承せず、関数として全てのコンポーネントがその処理を呼ぶようにする

Input にオブジェクトや配列を渡さない。ngOnChanges()の発火が出ないため。渡すのはプリミティブな値にするようにする。

Input に関数を渡さない。Output で実装する。

テンプレート内に DI した Sevice をそのまま書かない。関数も書かない。変更検知のたびに関数呼び出しと結果比較が起きるため、パフォーマンスが低下する。いったんプロパティに受けるのが望ましい

つねに AoT コンパイルで開発し、Aot を意識するようにする。

ViewChild で子 Component のメソッドを直接呼ばない。

ChangeDetectionStrategy を常に書く。OnPush が推奨、バグが起きた場合は消さずに Default にする

*ngIf と*nContainer を一つのタグで書かない。例外で失敗する

css で host-context や>>>を使わない。変更に弱い使用になってしまうため

Pipe を流用するのに既存 Pipe を DI しない。失敗する。新しい Pipe の内部で古い Pipe をコンストラクトする。

```ts
  constructor(@Inject(LOCALE_ID) locale: string) {
    this.decimalPipe = new DecimalPipe(locale)
```
