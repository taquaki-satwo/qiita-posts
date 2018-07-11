# 速攻で Flow の型注釈を学ぶ

Flowのドキュメントを速攻で読んで、速攻でまとめて、速攻で型注釈を学びました。
https://flow.org/en/docs/types/

そこそこ時間かかりました。

## まずはFlowを使うための準備

### インストール

Flowのインストール方法はこちら
https://flow.org/en/docs/install/

### 初期化

設定ファイル `.flowconfig` を作成

```
flow init
```

### バックグランドプロセスで実行

```
flow status

# 停止
flow stop
```

### 監視ファイルを設定

JavaScriptファイルのファイルの先頭に以下を記述する。

```:javascript
// @flow
```

### コードをチェックする

```
# flow status と同じ動作
flow
```

### サクッと試したい人は

ブラウザで動作確認
https://flow.org/try


## 型注釈

ここから本題。  
基本の書き方

```
変数や引数や関数: 型
```

### 1. プリミティブ型

JavaScriptのプリミティブ型は以下。

* Booleans
* Strings
* Numbers
* null
* undefined
* Symbols

#### リテラルとグローバルオブジェクト

```:javascript
// リテラル -> 小文字
function method(x: number)  {
  // ...
}
method(1);

// グローバルオブジェクト -> 頭文字大文字
function method(x: Number) {
  // ...
}
method(new Number(1));
``` 

#### 文字列の連結

オブジェクトや配列は文字列化してから連結する。

```:javascript
// エラー
"foo" + {};
"foo" + [];

// 文字列にする
"foo" + String({});
"foo" + [].toString();
"" + JSON.stringify({});
```

#### オブジェクトのオプションプロパティ

オブジェクトのプロパティをオプション（= 省略可能）にする。

```:javascript
function method(x: {y?: string}) {
  // ...
}

// 動く
method({y: undefined});
method({});

// しかし、nullはエラー
method({y: null});
```

#### 関数のオプション引数

```:javascript
function method(x?: string)  {
  // ...
}

// 動く
method(undefined);
method();

// しかし、nullはエラー
method(null);
```

#### デフォルト引数

```:javascript
function method(x: string = 'foo') {
  // ...
}

// 動く
method(undefined);
method();

// しかし、nullはエラー
method(null);
```

### 2. リテラル型

以下のリテラル値で型を指定することができる。

* `true` や　`false` のようなBooleans
* `3.14` のようなNumbers
* `"foo"` のようなStrings

```:javascript
function method(x: 1) {
  // ...
}

// 動く
method(1);

// エラー
method(2);
```

### 3. Mixed型

未知の型を許容する型。

```:javascript
function method(x: mixed) {
  // ...
}

// すべて動く
method('foo');
method(1);
method(null);
method({});
```

でも型を特定する処理をいれないとエラーになる。

```:javascript
function method(x: mixed) {
  return x + ''; // 型を特定していないのでエラー
}

function method(x: mixed) {
  if (typeof x === 'string') {
    return '' + x; // 型を特定しているので動く
  } else {
    return '';
  }
}
```

### 3. Any型

Flowの型チェックをスルーできる。
ランタイムエラーになりそうな記述でさえ通してしまうので、可能な限り使用を控えたほうが良いとのこと。  
また、any型のオブジェクトを与えた場合、そのオブジェクトのプロパティや処理結果もany型と解釈されるようなので、適切にキャストしてあげないとコードがどんどんany型に侵食される。

```:javascript
function method(obj: any) /* 返り値がany型になっちゃう */ {
  let a = obj.a; // プロパティもany型
  let b = a * 2; // 演算結果もany型
  return b;
}

let b = method({ a: 2 }); // 外に漏れちゃったany型
let c = "c:" + b; // 止まらないany型
```

```
function method(obj: any) /* number型と推論される */ {
  let a: number = obj.a; // キャストしてにany型を食い止める
  let b = a * 2;
  return b;
}

let b = method({ a: 2 }); // number型と推論される
let c = "c:" + b; // string型と推論される
```

### 4. Maybe型

nullやundefinedを許容する場合 `?` を型の頭につける。

```:javascript
function method(x: ?number) {...}

// すべて動く
method(1);
method(null);
method(undefined);
method();
```

Maybe型使用時のnullチェック

```:javascript
function method(x: ?number) {
  if (x !== null && x !== undefined) {
    // ...
  }
}

function method(x: ?number) {
  if (x != null) {
    // ...
  }
}

function method(x: ?number) {
  if (typeof x === 'number')  {
    // ...
  }
}
```

### 5. 変数の型

#### const

```
const a = 1; // number型と推論される
const b: number = 1; // 型注釈
```

#### let、var

```
let a = 1; // number型と推論される
a = '1' ; // これは動く

let b: number = 1;
b = '1'; // 型を指定して違う型の値を代入するとエラー
```

#### 変数を別の変数へ代入する

代入された変数の型は指定可能なものすべてになる

```
let a = 1;
if (Math.random()) a = '1';
if (Math.random()) a = 'true';
 
let b: number | string | boolean = a; 
```

記述によっては、代入された変数の型が確実なものもある

```
let a = 1;
let b: number = a;
```

処理を通すと型が把握できないこともある

```
let a = 1;

function func(x) {
  a = '1';
  a = true;
}

func();

let b: boolean = a; // エラー
```

### 6. 関数の型指定

#### 関数宣言

```
function method(str: string, bool?: boolean, ...nums: Array<number>): void {
  // ...
}
```

#### アローファンクション

```
let method = (str: string, bool?: boolean, ...nums: Array<number>): void => {
  // ...
};
```

#### 関数型

関数そのものを型にできる。以下はコールバックで使う例。

```
function method(callback: (error: Error | null, value: string | null) => void) {
  // ...
}
```

#### 可変長引数

可変長引数はArray型じゃないとだめ。

```
function method(...args: Array<number>) {
  // ...
}
```

#### 述語関数

返り値をtrueまたはfalseで返す関数を述語関数と呼ぶ。  
`%checks` 構文を使うそうです。

```
// これだとエラー
function isString(x): boolean { 
  return typeof x === 'string';
}

function method(x: ?string): string {
  if (isString(x)) {
     return x;
   } else {
     return '';
   }
}
```

```
// これだと動く
function isString(x): boolean %checks {
  return typeof x === 'string';
}

function method(x: ?string): string {
  if (isString(x)) {
     return x;
   } else {
     return '';
   }
}
```

### 7. オブジェクトの型指定

```
let obj: {
  a: string,
  b: number,
  c: boolean
} = {
  a: "1",
  b: 1,
  c: true
};
```

undefinedのプロパティにアクセスするとエラーになる。

```
let obj: {a: string} = {a: "1"};
obj.b // エラー
```

#### Sealedオブジェクト

プロパティを指定してオブジェクトを生成した場合、Sealedオブジェクトになる。  
Flowはプロパティの型を認識してくれる。

```
let obj = {a: 1};

let a: number = obj.a; // 動く
let b: string = obj.a; // エラー
```

このオブジェクトはプロパティを追加するとエラーになる。

```
let obj = {a: 1};
obj.b = 2; // エラー
```

#### Unsealedオブジェクト

プロパティを指定せずにオブジェクトを生成した場合、Unsealedオブジェクトになる。  
これはプロパティを追加してもエラーにならない。

```
let obj = {};
obj.a = 1; // 動く
```

#### オブジェクトのプロパティを変数へ代入する

変数の型は指定可能なものすべてになる

```
let obj = {};

if (Math.random()) obj.a = 1;
if (Math.random()) obj.a = '1';

let b: number | string = obj.a; 
```

記述によっては、代入された変数の型が確実なものもある

```
let obj = {};
obj.a = 1;
obj.a = '1';

let b: string = obj.a;
```

仕様上、定義されていないプロパティを変数に代入できてしまう。  
これは将来改善される可能性があるとのこと。

```
let obj = {};

let b: string = obj.a;
``` 

#### オブジェクトを厳密に定義する

余分なプロパティがあってもエラーにならない。

```
let obj: {a: number} = {a: 1, b: '1'};
```

厳密にするには以下のように記述する。

```
let obj: {|a: number|} = {a: 1, b: '1'}; // エラー
```

#### Mapオブジェクト

Mapオブジェクトのキーを型定義するためには、 `インデクサプロパティ` を使用する。

```
let obj: {[string]: number} = {};
obj['foo'] = 1;
let foo: number = o['foo'];
```

インデクサに名前をつける。

```
let obj: {[id: number]: string} = {};
obj[1] = 'foo';
obj[2] = 'bar';
```

　


































































