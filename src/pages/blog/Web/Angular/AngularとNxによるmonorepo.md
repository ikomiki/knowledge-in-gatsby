---
title: "AngularとNxによるmonorepo"
templateKey: blog-post
date: 2020-07-27T17:23:00+09:00
tags: ["Angular", "Nx", "monorepo"]
draft: false
---

## Nx とは

[Nx: Extensible Dev Tools for Monorepos](https://nx.dev/angular) は monorepo 用の拡張可能な開発ツールです。  
Nx は Microsoft、Google、Facebook で採用されています。

Nx は、1 つのアプリケーションを構築する 1 つのチームから、すべて同じワークスペースで複数のフロントエンドおよびバックエンドアプリケーションを構築する多くのチームまで、開発を拡張するのに役立ちます。Nx を使用する場合、開発者は高度な CLI（エディタープラグインを使用）、制御されたコード共有および一貫したコード生成のための機能を備えた全体的な開発経験があります。

Nx は以下をサポートしています。

- **TypeScript** AltJS 言語。Microsoft 開発による型付き言語。
- **React**
- **Angular**
- [cypress](https://www.cypress.io/) JavaScript テストフレームワーク
- [Jest](https://jestjs.io/ja/) JavaScript のテスト
- Prettier
- [NestJS \- A progressive Node\.js framework](https://nestjs.com/) サーバのフルスタックフレーム。Angular 風。
- [Next\.js by Vercel \- The React Framework](https://nextjs.org/) React のオールインワンパッケージ
- [Storybook](https://nx.dev/angular/plugins/storybook/overview) UI コンポーネントの開発環境。Nxrl 開発
- [Ionic](https://ionicframework.com/) ウェブ技術によるスマホアプリ開発ライブラリ。内部的に Angular、React、Vue、素の JavaScript に対応

開発元である [Nrwl](https://nrwl.io/) は、他に [Nx Console](https://nx.dev/angular/cli/console) (旧名 Angular Console)を開発しているところです。

<!--more-->

## 概略

Nx は AngularCLI の代わりには **なりません**。内部では、Nx は Angular CLI を使用し手コードを生成し、タスクを実行します。  
Nx CLI は高度なコード分析と計算キャッシュを使用して、可能な場合は以前の計算結果を再利用します。Angular CLI ははそれを行いません。

Angular CLI 6 移行、1 つのワークスペースにさまざまなタイプのプロジェクトを追加出来ますが、monorepo スタイルの開発を可能にするには充分ではありません。

## [Nx CLI](https://nx.dev/angular/cli/overview)

Nx は基本的に CLI で操作します。
VSCode のための拡張機能[Nx Console](https://nx.dev/angular/cli/console)を使うことで、コマンドを探す時間を短縮出来ます。

### Nx CLI のインストール

```bash
npm install -g @nrwl/cli
```

### コードの生成

```bash
nx generate @nrwl/angular:application myapp --style=scss
```

### Nx CLI の開発用サーバ

```bash
nx run [project]:serve
# (production設定)
nx run [project]:serve:production
# 省略記法
nx serve [project]
nx serve [project] --configuration=production
nx serve [project] --prod
```

### 一般的なターゲット

```bash
nx build [project]
nx lint [project]
nx serve [project]
nx e2e [project]
nx test [project]
```

### 複数のプロジェクトのタスクの実行

```bash
# 全てのプロジェクトを順番に実行
nx run-many --target=build --all
# 全てのプロジェクトを並行実行
nx run-many --target=build --all --parallel --maxParallel=8
# 選択したプロジェクト
nx run-many --target=build --projects=app1,app2
# 選択したプロジェクトと依存
nx run-many --target=build --projects=app1,app2 --with-deps
# 前回失敗したプロジェクト
nx run-many --target=build --all --only-failed
# Nx固有ではないフラグを渡すと、ビルダーに渡されます
nx run-many --target=build --all --prod

# 影響
# 現在のコード変更(Gitブランチ)によって、全てのプロジェクトに対して同じターゲット実行
nx affected --target=build
# 並行実行
nx affected --target=build --parallel --maxParallel=8
# コード変更の定義を変更
nx affected --target=build --parallel --maxParallel=8 --base=origin/development --head=$CI_BRANCH_NAME
# ショートカット
nx affected:build
nx affected:test
nx affected:lint
nx affected:e2e

# その他のコマンド
# 影響を受けるプロジェクトに関する情報をワークスペースに出力
nx print-affected
# プロジェクト刊の依存的な視覚的なグラフを起動
nx dep-graph
# 景教を受ける全てのプロジェクトが強調表示された依存関係グラフを起動
nx affected:dep-graph
# インストールされ、利用可能なすべての婦ログインを一覧表示
nx list
# 使用するプラグインに関する基本情報を出力
nx report
# コードをフォーマットする
nx format:write
# コードがフォーマットされていることを確認する
nx format:check
```

### [環境変数の読み込み](https://nx.dev/angular/cli/overview#loading-environment-variables)

すでにロードされている変数が見つかった場合、無視されます。

1. workspaceRoot/apps/my-app/.local.env
2. workspaceRoot/apps/my-app/.env
3. workspaceRoot/.local.env
4. workspaceRoot/.env

## [Nx Console](https://nx.dev/angular/cli/console)

## チュートリアル

### [Step 1: Create Application](https://nx.dev/angular/tutorial/01-create-application)

```bash
# CLIのインストール
npm install -g @nrwl/cli

# 新しいワークスペースの作成（ウィザード）
npx create-nx-workspace@latest
? Workspace name (e.g., org name)     myorg
? What to create in the new workspace angular
? Application name                    todos
? Default stylesheet format           CSS

cd PROJECT_NAME

nx serve todos
# もし↑で起動しなかった場合は、以下を実行する
npx nx serve todos
```

### [Step 2: Add E2E Tests](https://nx.dev/angular/tutorial/02-add-e2e-test)

apps/todos-e2e/src/support/app.po.ts に以下を追加

```apps/todos-e2e/src/support/app.po.ts
export const getTodos = () => cy.get('li.todo');
export const getAddTodoButton = () => cy.get('button#add-todo');
```

apps/todos-e2e/src/integration/app.spec.ts を更新

```apps/todos-e2e/src/integration/app.spec.ts
import { getAddTodoButton, getTodos } from '../support/app.po';

describe('TodoApps', () => {
  beforeEach(() => cy.visit('/'));

  it('should display todos', () => {
    getTodos().should((t) => expect(t.length).equal(2));
    getAddTodoButton().click();
    getTodos().should((t) => expect(t.length).equal(3));
  });
});
```

`nx serve`を停止した上で、以下を実行する

```bash
nx e2e todos-e2e --watch
```

Cypress アプリケーションが起動するのに、 `Run all specs`などを実行してテストする。  
結果は、 `li.todo`について `expected 0 to equal 2`のアサーションエラーとなる。次のセクションでエラーを解決する。

### [Step 3: Display Todos](https://nx.dev/angular/tutorial/03-display-todos)

apps/todos/src/app/app.component.ts を更新

```apps/todos/src/app/app.component.ts
import { Component } from '@angular/core';

interface Todo {
  title: string;
}

@Component({
  selector: 'myorg-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css'],
})
export class AppComponent {
  todos: Todo[] = [{ title: 'Todo 1' }, { title: 'Todo 2' }];

  addTodo() {
    this.todos.push({
      title: `New todo ${Math.floor(Math.random() * 1000)}`,
    });
  }
}
```

apps/todos/src/app/app.component.html を差し替え

```apps/todos/src/app/app.component.html
<h1>Todos</h1>

<ul>
  <li *ngFor="let t of todos" class="todo">{{ t.title }}</li>
</ul>

<button id="add-todo" (click)="addTodo()">Add Todo</button>
```

テストブラウザの左ペインの右上隅にある再読込ボタンで再読込する。

いったんテストプロセスを止めて、以下のコマンドでヘッドレステストに成功することを確認する。

```bash
nx e2e todos-e2e --headless
```

### [Step 4: Connect to an API](https://nx.dev/angular/tutorial/04-connect-to-api)

apps/todos/src/app/app.module.ts を更新

```apps/todos/src/app/app.module.ts
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';

import { AppComponent } from './app.component';
import { HttpClientModule } from '@angular/common/http';

@NgModule({
  declarations: [AppComponent],
  imports: [BrowserModule, HttpClientModule],
  providers: [],
  bootstrap: [AppComponent],
})
export class AppModule {}
```

apps/todos/src/app/app.component.ts を更新

```apps/todos/src/app/app.component.ts
import { Component } from '@angular/core';
import { HttpClient } from '@angular/common/http';

interface Todo {
  title: string;
}

@Component({
  selector: 'myorg-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css'],
})
export class AppComponent {
  todos: Todo[] = [];

  constructor(private http: HttpClient) {
    this.fetch();
  }

  fetch() {
    this.http.get<Todo[]>('/api/todos').subscribe((t) => (this.todos = t));
  }

  addTodo() {
    this.http.post('/api/addTodo', {}).subscribe(() => {
      this.fetch();
    });
  }
}
```

### [Step 5: Add Node Application Implementing API](https://nx.dev/angular/tutorial/05-add-node-app)

```bash
npm install --save-dev @nrwl/nest
nx g @nrwl/nest:app api --frontendProject=todos
nx serve api
```

apps/api/src/app/app.service.ts を更新

```apps/api/src/app/app.service.ts
import { Injectable } from '@nestjs/common';

interface Todo {
  title: string;
}

@Injectable()
export class AppService {
  todos: Todo[] = [{ title: 'Todo 1' }, { title: 'Todo 2' }];

  getData(): Todo[] {
    return this.todos;
  }

  addTodo() {
    this.todos.push({
      title: `New todo ${Math.floor(Math.random() * 1000)}`,
    });
  }
}
```

apps/api/src/app/app.controller.ts を更新

```apps/api/src/app/app.controller.ts
import { Controller, Get, Post } from '@nestjs/common';

import { AppService } from './app.service';

@Controller()
export class AppController {
  constructor(private readonly appService: AppService) {}

  @Get('todos')
  getData() {
    return this.appService.getData();
  }

  @Post('addTodo')
  addTodo() {
    return this.appService.addTodo();
  }
}
```

[http://localhost:3333/api/todos](http://localhost:3333/api/todos) の表示を確認。

`npx nx serve todos` を再起動し、[http://localhost:4200/](http://localhost:4200/)の表示を確認。

### [Step 7: Share Code](https://nx.dev/angular/tutorial/07-share-code)

```bash
nx g @nrwl/workspace:lib data
```

libs/data/src/lib/data.ts を更新

```libs/data/src/lib/data.ts
export interface Todo {
  title: string;
}
```

※VSCode 使用時、`myorg/data`パッケージが認識できるようにするため、TS サーバを再起動する。

apps/api/src/app/app.service.ts を置き換え

```apps/api/src/app/app.service.ts
import { Injectable } from '@nestjs/common';
import { Todo } from '@myorg/data';

@Injectable()
export class AppService {
  todos: Todo[] = [{ title: 'Todo 1' }, { title: 'Todo 2' }];

  getData(): Todo[] {
    return this.todos;
  }

  addTodo() {
    this.todos.push({
      title: `New todo ${Math.floor(Math.random() * 1000)}`,
    });
  }
}
```

apps/todos/src/app/app.component.ts を置き換え

```apps/todos/src/app/app.component.ts
import { Component } from '@angular/core';
import { HttpClient } from '@angular/common/http';
import { Todo } from '@myorg/data';

@Component({
  selector: 'myorg-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css'],
})
export class AppComponent {
  todos: Todo[] = [];

  constructor(private http: HttpClient) {
    this.fetch();
  }

  fetch() {
    this.http.get<Todo[]>('/api/todos').subscribe((t) => (this.todos = t));
  }

  addTodo() {
    this.http.post('/api/addTodo', {}).subscribe(() => {
      this.fetch();
    });
  }
}
```

### [Step 8: Create Libs](https://nx.dev/angular/tutorial/08-create-libs)

```bash
nx g @nrwl/angular:lib ui
# スタイルシートは、さしあたりCSS
nx g component todos --project=ui --export
```

libs/ui/src/lib/todos/todos.component.ts を更新

```
import { Component, OnInit, Input } from '@angular/core';
import { Todo } from '@myorg/data';

@Component({
  selector: 'myorg-todos',
  templateUrl: './todos.component.html',
  styleUrls: ['./todos.component.css'],
})
export class TodosComponent implements OnInit {
  @Input() todos: Todo[];

  constructor() {}

  ngOnInit() {}
}
```

libs/ui/src/lib/todos/todos.component.html を更新

```libs/ui/src/lib/todos/todos.component.html
<ul>
  <li *ngFor="let t of todos">{{ t.title }}</li>
</ul>
```

※VSCode 使用時、TS サーバを再起動する。

apps/todos/src/app/app.module.ts を更新

```apps/todos/src/app/app.module.ts
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';

import { AppComponent } from './app.component';
import { HttpClientModule } from '@angular/common/http';
import { UiModule } from '@myorg/ui';

@NgModule({
  declarations: [AppComponent],
  imports: [BrowserModule, HttpClientModule, UiModule],
  providers: [],
  bootstrap: [AppComponent],
})
export class AppModule {}
```

apps/todos/src/app/app.component.html 更新

```apps/todos/src/app/app.component.html
<h1>Todos</h1>

<myorg-todos [todos]="todos"></myorg-todos>

<button (click)="addTodo()">Add Todo</button>
```

### [Step 9: Dep Graph](https://nx.dev/angular/tutorial/09-dep-graph)

```bash
npm run dep-graph
```

ブラウザが開き、依存関係図が表示される。

### [Step 10: Computation Caching](https://nx.dev/angular/tutorial/10-computation-caching)

```bash
nx build todos
# ビルド終了後、もう一度同じ操作を実行する。
nx build todos

# 複数のプロジェクトのビルド
nx run-many --target=build --projects=todos,api

# 依存関係をたどってlint
nx lint todos --with-deps
```

### [Step 11: Test Affected Projects](https://nx.dev/angular/tutorial/11-test-affected-projects)

```bash
git add .
git commit -am 'init'
git checkout -b testbranch
```

ソースを変更する

```bash
npm run affected:apps
# 影響を受けるアプリケーションの範囲を表示
npm run affected:libs
# 影響を受けるライブラリの範囲を表示
nx affected:test
# 変更の影響を受けるプロジェクトのみを再テストする

# The following are equivalent
nx affected --target=build
nx affected:build
```

## 学習資料

- [Nx: Extensible Dev Tools for Monorepos](https://nx.dev/angular) 公式ページ
  - [Why Nx?](https://nx.dev/angular/getting-started/why-nx) Angular+Nx の公式導入(英語)
  - [Step 1: Create Application](https://nx.dev/angular/tutorial/01-create-application) Angular+Nx の公式チュートリアル(英語)
  - [Transitioning to Nx](https://nx.dev/angular/migration/migration-angular) Angular CLI からの移行(英語)
  - [Nx CLI](https://nx.dev/angular/cli/overview) Nx CLI コマンドラインツール(英語)
  - [Plugins](https://nx.dev/angular/plugins/overview) プラグイン
  - [Nx Workspace](https://nx.dev/angular/workspace/workspace-overview) Nx ワークスペース
- [Nx Workspaces Course, by Nrwl \- YouTube](https://www.youtube.com/watch?v=2mYLe9Kp9VM&list=PLakNactNC1dH38AfqmwabvOszDmKriGco) 公式動画リスト(英語)
