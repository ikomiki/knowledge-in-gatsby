---
title: "AngularMaterialツールバーアイコンボタンのサイズ変更"
templateKey: blog-post
date: 2020-07-09T11:22:00+09:00
tags: [Angular, typescript]
draft: false
---

### example.component.html

```example.component.html
<button mat-icon-button class="menu-button"><mat-icon svgIcon="header:list" class="menu-icon"></mat-icon></button>
```

### example.component.scss

```example.component.scss
.menu-button {
  width: 50px;
  height: 50px;
  line-height: 50px;
  .menu-icon {
    font-size: 50px;
    width: 50px;
    height: 50px;
    line-height: 50px;
  }
}
```
