---
title: "TypeScriptの型注釈"
templateKey: blog-post
date: 2020-07-16T00:35:00+09:00
tags: [TypeScript]
draft: false
---

## 型注釈

型注釈にて静的型付を行うことで、コンパイラが整合性をチェックし、適合しているかどうかを検査してくれます。

### シグニチャ

<!--more-->

```typescript
interface Person {
  readonly name: string; // 読み取り専用
  age?: number; // 省略可能
}
// コールシグニチャ
interface Calculate {(x:number, y:number): number; }
let add: Calculate = function(a:number, b:number): number { ... };
// メソッドシグニチャ
interface Calculate {add(x:number, y:number): number; }
let obj: Calculate = { add(a:number, b:number): number { ... } };
// インデックスシグニチャ
interface NumberAssoc {[index: string: nunber];}
// コンストラクターシグニチャ
interface Figure {new(width: number, height: number): Tringle;}
class Triangle { constructor(private width: number, private height: number)}
let Tri: Figure = Triangle;
```

### 型情報の切り出し

```typescript
interface Product {
  name: string;
  price: number;
}
type ProductKeys = keyof Product; // "name" | "price"に等しい
```

### Lookup types

```typescript
interface Product {
  name: string;
  price: number;
}
type NameType = keyof Product["name"]; // string;
```

### Mapped types

```typescript
type ReadonlyProduct = {
  readonly [K in keyof Product]: Product[K];
};
type OptionalProduct = {
  [K in keyof Product]?: Product[K];
};

// TypeScript2.8以降、設定解除ができるように
type Product = {
  -readonly [K in keyof ReadonlyProduct]: ReadonlyProduct[K];
};
type Product = {
  [K in keyof OptionalProduct]?: OptionalProduct[K];
};
```

### ユーティリティ型(Utility Types)

TypeScript には、一般的な型変換を容易にするためのユーティリティタイプがいくつか用意されています。これらのユーティリティはグローバルで利用できます。

#### Partial<T> 全てのプロパティを任意に

#### Required<T> 全てのプロパティを必須に

#### Readonly<T> プロパティを読み取り専用にする

#### Record<K, T> 指定の型を持つプロパティ群を生成する

```typescript
let mySite: Record<'top'|'contact'|'about', ContentInfo> = {
  'top': {略},
  'contact': {略},
  'about': {略},
};
```

#### 既存の型から特定のプロパティだけを抽出する Pick<T,K>, Omit<T,K>

```typescript
type SubBook = Pick<Book, "title" | "price">; // title/priceだけを抜き出す
type SubBook = Omit<Book, "isbn" | "published">; // isbn/publishedを除去する
```

#### 共用型から特定の型を抽出する Exclude<T,U>,Extract<T,U>, NonNullable<T>

```typescript
type Type1 = string | number | boolean;
type NewType1 = Exclude<Type1, string | boolean>; // 除外 nunber

type NewType3 = Extract<Type1, string | object>; // 共通 string

type Type2 = string | null | undefined;
type NewType5 = NonNullable<Type2>; // nullとundefinedを除外 string
```

#### 関数の引数／戻り値を元に型を生成する Parameters<T>, ReturnType<T>, ConstructorParameters<T>

```typescript
function hoge<arg1: string, arg2?: boolean>: string | number {...}
type TypeP = Parameters<typeof hoge>; // 引数型を元にタプル [string, boolean] | [string]
type TypeR = ReturnType<typeof hoge>; // 戻り値型をもとに string | number

class MyClass {
  constructor(arg1: string, arg2?: boolean) {}
}
type TypeC = ConstructorParameters<typeof MyClass>; // 引数型を元にタプル [string, boolean] | [string]
```

<!--
## 変数の宣言

let 変更可能な宣言。 `let name: type = initial`
var (非推奨。JavaScript 互換のための存在)
const 定数。再代入できない。ただしオブジェクトの中身を書き換えることは出来る

型を省略した場合

- 初期値も省略した場合、any と見なされる
- null や undefined を指定した場合も any と見なされれる
  上記の場合でも、noImplicitAny オプションが有効な場合は型が推論される。

### 型

- number
- string
- boolean
- symbol
- any
- オブジェクト型(配列型、タプル型、クラス型、インターフェース型、関数型...)
- void (値なし。関数の戻り値を返さない場合など)
- never(到達しない。関数が終端に到達しない(常に例外、常に無限ループ)の関数の戻り値)
- unknown(なんでもあり。以降のメソッド矢演算子などの呼び出しは許されない。たとえば`let str:any='Hoge';のあとに str.trim()を実行するとエラーになる。使う場合は型ガードで型を特定する)
- this(戻し型にすることで、メソッドチェーンのような記述が可能になる。)

### リテラル

- 数値は 10 進数(108)、16 進数(0xff)、8 進数(0o666)、2 進数(0b1110001)、指数(1.14E-3)が使える
- 数値は数値セパレータ(\_)が使える。(1_000_000、159_00)
- 文字列は’’、""、``(テンプレート文字列)が使える。テンプレート文字列は\${変数}で埋め込める

<型名>を変数/リテラルの前に付与することで明示的に変換できる

### 配列

```
let name: type[] = [1,2]
let name: Array<type> = [1,2]
let name: type[][] = [[1,2], [3,4]]
let name: readonly type[][] = [1,2] // 読み取り専用(一次元目のみ)。`name[1] = 3`がエラーになる
let x!: string; // 初期化チェック(strictPropertyInitializationがtrueの時)を一次的に回避できる
```

### 連想配列

```
let name: {index: i_type}: v_type] = {foo: 'bar'}
```

### 定数

```
enum Sex { MALE, FEMALE, UNKNOWN};
let m: Sex = Sex.Male;
console.log(m) // 0
console.log(Sex[m]) // MALE

enum Sex { MALE=1, FEMALE, UNKNOWN};
enum Sex { MALE='male', FEMALE, UNKNOWN}; // TypeScript 2.4以降
```

### タプル

```
let data: [string, number, boolean] = ['hoge', 10.355, false];
let data: readonly [string, number, boolean] = ['hoge', 10.355, false]; // TypeScript 3.4以降
```

### 関数

```
function name(param: ptype, 略): rtype {} // function命令
(param: ptype): rtype => {} // アロー関数（ラムダ式）

let func: (param: ptype, 略 => rtype // 関数型

// 関数のオーバロード
function show(value:number): void;
function show(value:boolean): void;
function show(value: any): void { // あるいは value: number | boolean
  if (typeof value === 'number') {
    // numberの処理
  } else {
    // booleanの処理
  }
}
```

### 共用型

引数や戻り値として使える

```
let data: string | boolean;
data = 'hoge'
data = false
data = 1 // エラー
```

### 型の判定

```
typeof value === 'number'
obj instanceof Person
'name' in obj
```

### 型ガード関数

```
function isBook(inf: Book | Magazine): inf is Book {
  return (inf as Book).isbn != undefined;
}
if (isBook(i)) {
  // Book型
} else  {
  // Magazine型
}
```

### null 非許容型

tsconfig.json で strict ないし strictNullChecks オプションが true に設定されると、全ての型で null/undefined を禁止できる

この場合は明示的に型指定を行う

```
let data1: string | undefined = undefined;
let data2: string | null = null;
```

### null/undefined のチェック演算子

```
let hoge: string | null = null;
hoge?.trim() // hogeがnullの場合は、nullを返して終わる。trim()を呼びだしてエラーになることはない

hoge?.length ?? 0 // hoge?.lengthがnull/undefineの場合、??の右(0)を返す
```

### 型エイリアス

```
type FooType = [string, number, boolean];
```

## 文字列リテラル型

```
type Season = 'spring' | 'summer' | 'autumn' | 'winter'
```

## クラス

```
class name {
  pname: ptype;
  constructor(param; type) {

  }
  mname(param: type); rtyp {

  }
}
```

アクセス修飾子は public(規定),protected,private

```
private name: string; // this.name='hoge';
public show(): string {} // this.show();

#data = 10  // this.#data 。TypeScript3.8以降、プライベートフィールド。
c['name'] は読み込めるが、
c['#data'] はundefinedが返る。(ハードプライバシー)
```

プロパティに初期化がなく、コンストラクターでも明示的に代入されていない場合はエラーとなる。（strict ないし strictPropertyInitialization が true の場合）
ただし、`age!: number`により初期化チェックを一次的に回避できる

### コンストラクタの省略構文

```
constructor(private name:string) {}
```

### getter/setter アクセサー

private \_age!: number;
get age(): number { return this.\_age; }
set age(value:number) {this.\_age = value; }

### 静的メンバー

```
class Figure {
public static PI: number = 3.14159; // 静的メンバー
// 静的メソッド
pubic static circle(radius: number): number {...}
}
Figure.PI
Figure.circle(5)
```

### 継承

```
class name extends parent {
  constructor(v: t) {
    super(v);
  }
  show(): string  {
    return super.show()
  }
}
```

### 抽象クラス

派生クラスで定義する

```
abstract class Figure {
  abstract getArea(): number;
  abstract msg: string // 抽象プロパティ
}
```

### インターフェース

```
interface Figure{
  getArea(): number;
}
class Triangle implements Figure {
  getArea(): number {
    hogehoge
  }
}

// インターフェースの継承
interface Figure2 extends Figure {}
```

## モジュール
-->
