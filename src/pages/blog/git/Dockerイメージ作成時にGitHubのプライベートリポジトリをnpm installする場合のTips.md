---
title: "Dockerイメージ作成時にGitHubのプライベートリポジトリをnpm installする場合のTips"
templateKey: blog-post
date: 2020-07-26T05:58:00+09:00
tags: ["git", "モノリポ"]
draft: false
---

[複数サービスの Web フロントエンドを運用する際のリポジトリ構成〜monorepo から manyrepo へ〜 \| スペースマーケットブログ](https://blog.spacemarket.com/code/web-frontend-repository-composition-monorepo-or-manyrepo/#post-5413-md-6)

<!--more-->

```dockerfile
USER root
# ssh キーを配置（npmインストール後に削除）
RUN mkdir ~/.ssh
ADD ./config/ssh_key $HOME/.ssh/id_rsa
RUN echo 'IdentityFile ~/.ssh/id_rsa' > ~/.ssh/config
# sshキーの所有者をhogeuserに
RUN chown -R hogeuser:hogeuser $HOME/.ssh
RUN chown -R hogeuser:hogeuser $HOME/.ssh/*

USER hogeuser
# 一旦疎通確認＆警告を無視する設定
RUN ssh -o StrictHostKeyChecking=no -T git@github.com || true
RUN npm set progress=false && npm install --no-progress --no-cache

# npm install終わったのでSSHキーを削除
USER root
RUN rm -f $HOME/.ssh/*
```
