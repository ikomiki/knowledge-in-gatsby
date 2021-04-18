---
title: "GraphQLメモ"
templateKey: blog-post
date: 2020-08-19T18:43:00+09:00
tags: ["GraphQL"]
draft: false
---

## スキーマ

<!--more-->

```graphql
type Query {
  me: User
}
type User {
  id: ID
  name: String
}
```

## 例 1

### クエリ

```graphql
{
  hero {
    name
    # コメント可能
    friends {
      name
    }
  }
}
```

### レスポンス

```json
{
  "data": {
    "hero": {
      "name": "Luke Skywalker"
      friends: [
        {
          "Name": "Luke"
        },
        {
          "name": "Han solo"
        }
      ]
    }
  }
}
```

スキーマに示されている内容に基づいて、単一アイテムかアイテムリストのどちらが期待されているかを認識しています。

## クエリの機能

### Argument(引数)

```graphql
{
  human(id: "1000") {
    name
    height(unit: FOOT)
  }
}
```

サーバで 1 階だけデータ変換を実装することができます。(unit: FOOT)

```json
{
  "data": {
    "human": {
      "name": "Luke Skywalker",
      "height": 5.6430448
    }
  }
}
```

### Aliases エイリアス

```graphql
{
  empireHero: hero(episode: EMPIRE) {
    name
  }
  jediHero: hero(episode: JEDI) {
    name
  }
}
```

同じフィールドを異なる引数で直接クエリできないので、エイリアスで任意の名前に変更する

```json
{
  "data": {
    "empireHero": {
      "name": "Luke Skywalker"
    },
    "jediHero": {
      "name": "R2-D2"
    }
  }
}
```

### フラグメント

フラグメントと呼ばれる再利用可能なユニット

```graphql
{
  leftComparison: hero(episode: EMPIRE) {
    ...comparisonFields
  }
  rightComparison: hero(episode: JEDI) {
    ...comparisonFields
  }
}

fragment comparisonFields on Character {
  name
  appearsIn
  friends {
    name
  }
}
```

フラグメントは、クエリまたはミューテーションで宣言された変数にアクセス出来る

```graphql
query HeroComparison($first: Int = 3) {
  leftComparison: hero(episode: EMPIRE) {
    ...comparisonFields
  }
  rightComparison: hero(episode: JEDI) {
    ...comparisonFields
  }
}

fragment comparisonFields on Character {
  name
  friendsConnection(first: $first) {
    totalCount
    edges {
      node {
        name
      }
    }
  }
}
```

### オペレーション名

```graphql
query HeroNameAndFriends {
  hero {
    name
    friends {
      name
    }
  }
}
```

オペレーションタイプは query, mutation, or subscription。

### 変数

```graphql
query HeroNameAndFriends($episode: Episode) {
  hero(episode: $episode) {
    name
    friends {
      name
    }
  }
}
```

トランプポート固有の(通常は json)変数ディクショナリを別に渡す

```json
{
  "episode": "JEDI"
}
```

### デフォルト変数

```graphql
query HeroNameAndFriends($episode: Episode = JEDI) {
  hero(episode: $episode) {
    name
    friends {
      name
    }
  }
}
```

### ディレクティブ

```graphql
query Hero($episode: Episode, $withFriends: Boolean!) {
  hero(episode: $episode) {
    name
    friends @include(if: $withFriends) {
      name
    }
  }
}
```

```json
{
  "episode": "JEDI",
  "withFriends": false
}
```

@include(if: Boolean)引数が true の場合にのみ、このフィールドを結果に含めます。
@skip(if: Boolean)引数が tre の場合、このフィールドをスキップします。

## ミューテーション

```graphql
mutation CreateReviewForEpisode($ep: Episode!, $review: ReviewInput!) {
  createReview(episode: $ep, review: $review) {
    stars
    commentary
  }
}
```

```json
{
  "ep": "JEDI",
  "review": {
    "stars": 5,
    "commentary": "This is a great movie!"
  }
}
```

クエリフィールドは並列に実行されますが、ミューテーションフィールドは次々に直列に実行されます

## インラインフラグメント

```graphql
query HeroForEpisode($ep: Episode!) {
  hero(episode: $ep) {
    name
    ... on Droid {
      primaryFunction
    }
    ... on Human {
      height
    }
  }
}
```

```json
{
  "ep": "JEDI"
}
```

```json
{
  "data": {
    "hero": {
      "name": "R2-D2",
      "primaryFunction": "Astromech"
    }
  }
}
```

## メタフィールド

```grphql
{
  search(text: "an") {
    __typename
    ... on Human {
      name
    }
    ... on Droid {
      name
    }
    ... on Starship {
      name
    }
  }
}
```

```json
{
  "data": {
    "search": [
      {
        "__typename": "Human",
        "name": "Han Solo"
      },
      {
        "__typename": "Human",
        "name": "Leia Organa"
      },
      {
        "__typename": "Starship",
        "name": "TIE Advanced x1"
      }
    ]
  }
}
```
