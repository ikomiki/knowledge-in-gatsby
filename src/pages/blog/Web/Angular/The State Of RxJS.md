---
title: "ng-conf 2020  The State Of RxJS"
templateKey: blog-post
date: 2020-07-09T11:43:00+09:00
tags: [Angular, typescript, RxJS]
draft: true
---

ng-conf 2020 [The State Of RxJS](https://www.youtube.com/watch?v=OC6g4yFsw0w&t=1s)

<!--more-->

バージョン 7(Beta)

## Improved Stability

安定性の改善

## Better TypeScript Support

### N-argument interface

of(new A(), ...new J())に対応できる

### Fixed bad types

```typescript
const names$: Observable<string[]> = getNames();
const names = await name$.toPromise();
const names: string[] | undefined;
に対応;
```

toPromise()が非推奨(deprecation)

Union type return are bettter;

## New Functionnality

animationFrames()
アニメーションフレームでの発行

New Promise Conversion

```typescript
lastValueFrom - replacing ToPromise();
`await lastValueFrom(source$);
firstValueFrom(source$);
```

AsyncIterable support

rxjs-for-await
https://github.com/benlesh/rxjs-for-await

eachValueFrom
bufferdValuesFrom
latestValueFrom

IxJs (rxjs, bt for AsyncIteratable)
https://github.com/ReactiveX/IxJS

rety({restOnSuccess: true})

zipWith, cmbineatetWith, mergeWith, concaWith（pipe に渡すよう)

## Deprecation(But no removals!)

resultSelectors
scheduler arguments
→ use scheduled(), observeOn()

Deprecaed subscription signatures
OK {next, error, complete}
DEPRECATED (next, error, complete)

scheduler は 4k くらい使っている

TimestampProvider

```typescript
const subject = new ReplaySubject(10, 10.400, perforｍａｎｃｅ）
```

## 7.1 and beyond

ESLint トランスフォーム
