---
title: "Cognitoパスワードのパスワード強制終了を解除する手順"
templateKey: blog-post
date: 2020-07-15T17:38:00+09:00
tags: [Cognito]
draft: true
---

AWS コンソールにて、  
`ユーザプール＞全般設定＞アプリクライアント＞ ADMIN_NO_SRP_AUTH を ON`  
で作成しておくことが前提。

<!--more-->

```bash
export USER_POOL_ID=[任意]
export APP_CLIENT_ID=[任意]
export COGNITO_USERNAME=[任意]
export COGNITO_PASSWORD=[任意]
export COGNITO_NEW_PASSWORD=[任意]
export COGNITO_SESSION=`aws cognito-idp admin-initiate-auth --user-pool-id $USER_POOL_ID --client-id $APP_CLIENT_ID --auth-flow ADMIN_NO_SRP_AUTH --auth-parameters USERNAME=${COGNITO_USERNAME},PASSWORD=${COGNITO_PASSWORD} | jq -r .Session`
aws cognito-idp admin-respond-to-auth-challenge --user-pool-id $USER_POOL_ID --client-id $APP_CLIENT_ID --challenge-name NEW_PASSWORD_REQUIRED --challenge-responses NEW_PASSWORD=$COGNITO_NEW_PASSWORD,USERNAME=$COGNITO_USERNAME --session $COGNITO_SESSION
```

Cognito ユーザー追加

```bash
# 環境設定
export USER_POOL_ID=[任意]
export APP_CLIENT_ID=[任意]

#ユーザー設定
export COGNITO_USERNAME=[任意]
export COGNITO_TMP_PASSWORD=[任意]
export COGNITO_NEW_PASSWORD=[任意]

export TMP1=`aws cognito-idp admin-create-user --user-pool-id $USER_POOL_ID --username $COGNITO_USERNAME --temporary-password ${COGNITO_TMP_PASSWORD}`
echo $TMP1

export TMP2=`aws cognito-idp admin-initiate-auth --user-pool-id $USER_POOL_ID --client-id $APP_CLIENT_ID --auth-flow ADMIN_NO_SRP_AUTH --auth-parameters USERNAME=${COGNITO_USERNAME},PASSWORD=${COGNITO_TMP_PASSWORD}`
```
