---
title: "AngularMaterialテーマ色の差し替え"
templateKey: blog-post
date: 2020-07-09T11:10:00+09:00
tags: [Angular, typescript]
draft: false
---

AngularMaterial テーマ色の差し替え

<!--more-->

```
@import '~@angular/material/theming';

$mat-custom: (
    0:#FFFFFF,
    50: #fffde7,
    100: #fff9c4,
    200: #fff59d,
    300: #fff176,
    400: #ffee58,
    500: #2196F3,
    600: #fae5ca,
    700: #0072D6,
    800: #1B485C,
    900: rgb(2, 2, 2),
    A100: #ffff8d,
    A200: #ffff00,
    A400: #ffea00,
    A700: #FF4618,
    contrast: (
      0:$dark-primary-text,
      50: $dark-primary-text,
      100: $dark-primary-text,
      200: $dark-primary-text,
      300: $dark-primary-text,
      400: $dark-primary-text,
      500: $light-primary-text,
      600: $light-primary-text,
      700: $light-primary-text,
      800: $light-primary-text,
      900: $light-primary-text,
      A100: $light-primary-text,
      A200: $light-primary-text,
      A400: $light-primary-text,
      A700: $light-primary-text,
    )
  );
$cms-theme-primary: mat-palette($mat-custom, 500, 100, 800);
$cms-theme-accent: mat-palette($mat-custom, A700, 0, A100);
$cms-theme-warn: mat-palette($mat-red);
$cms-theme: mat-light-theme($cms-theme-primary, $cms-theme-accent, $cms-theme-warn);
@include angular-material-theme($cms-theme);
```
