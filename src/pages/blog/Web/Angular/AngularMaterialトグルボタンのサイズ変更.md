---
title: "AngularMaterialトグルボタンのサイズ変更"
templateKey: blog-post
date: 2020-07-09T11:11:00+09:00
tags: [Angular, typescript]
draft: false
---

AngularMaterial トグルボタンのサイズ変更

<!--more-->

```styles.scss

// トグルボタン
.mat-button-toggle {
  height: $my-mat-button-toggle-height;
}
.mat-button-toggle-button .mat-button-toggle-label-content {
  line-height: $my-mat-button-toggle-height;
}
.mat-button-toggle-checked {
  background-color: #faa02a; // #fae5ca;
}


```
