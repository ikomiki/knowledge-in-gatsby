---
title: "gcloudアカウントの切り替え"
templateKey: blog-post
date: 2020-07-09T11:35:00+09:00
tags: [GCP]
draft: true
---

## gcloud アカウントの切り替え

https://www.sambaiz.net/article/28/

<!--more-->

gcloud のアカウント一覧と切り替え

```bash
$ gcloud auth list
$ gcloud config set account `ACCOUNT`
configにprojectなども設定している場合はconfig自体を作成して切り替えた方が楽。
$ gcloud config configurations create <name>
$ gcloud config configurations activate <name>
$ gcloud config list
...
Your active configuration is: [<name>]

$ gcloud config set account <accout>
$ gcloud config set project <project>

```

kubectl の context 変更

```bash
$ kubectl config current-context
$ kubectl config view # contexts
$ kubectl config use-context minikube
```
