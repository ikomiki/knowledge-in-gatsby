---
title: "Angular9＋Scullyによる高パフォーマンスブログサイトの構築"
templateKey: blog-post
date: 2020-07-09T03:20:00+09:00
tags: [Angular, typescript]
draft: true
---

Angular での静的サイトジェネレータであるところの[Scully](https://github.com/scullyio/scully)を使用した手順メモ

Scully はまだアルファ版のため、参考程度。

<!--more-->

```bash
npm -g i @angular/cli@next
ng new ng-blog-app --routing  --style scss
cd ng-blog-app
ng add @scullyio/init

ng g module home --routing && ng g component home/home
ng g module blog --routing && ng g component blog/blog-list && ng g component blog/blog-detail

```

```src/app/app.component.html (部分)
const routes: Routes = [{
  path: '',
  loadChildren: () => import('./home/home.module').then(m => m.HomeModule),
}, {
  path: 'blog',
  loadChildren: () => import('./blog/blog.module').then(m => m.BlogModule),
}];
```

```src/app/home/home-routing.module.ts (部分)
const routes: Routes = [{
  path: '',
  component: HomeComponent,
}];
```

```src/app/blog/blog-routing.module.ts (部分)
const routes: Routes = [{
  path: '',
  pathMatch: 'full',
  component: BlogListComponent,
}, {
  path: ':slug-id',
  component: BlogDetailComponent,
}];
```

```
ng g @scullyio/init:blog
```
