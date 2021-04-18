---
title: "gitでのローカルファイルの無視"
templateKey: blog-post
date: 2020-07-09T11:37:00+09:00
tags: [git]
draft: false
---

[既に git 管理しているファイルをあえて無視したい \- Qiita](https://qiita.com/usamik26/items/56d0d3ba7a1300625f92)

```bash
git update-index --skip-worktree [ファイル名]
```

この設定を取り消すには次のようにします。

```bash
git update-index --no-skip-worktree [ファイル名]
```

上記の設定がされているファイルを確認するには、次のようにします。

```bash
git ls-files -v | grep ^S
```
