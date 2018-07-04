# 速攻で Flow の書き方を学ぶ

Flowの型注釈に関するドキュメントを速攻で読んで、速攻でまとめて、速攻で書き方を学びました。
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

// 動作する
method({y: undefined});
method({});

// しかし、nullはエラーになる
method({y: null});
```

#### 関数のオプション引数

```:javascript
function method(x?: string)  {
  // ...
}

// 動作する
method(undefined);
method();

// しかし、nullはエラーになる
method(null);
```

#### デフォルト引数

```:javascript
function method(x: string = 'foo')  {
  // ...
}

// 動作する
method(undefined);
method();

// しかし、nullはエラーになる
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

// 動作する
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

// すべて動作する
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

型チェックを動作させないようにできる。
ランタイムエラーになりそうな記述でさえ通してしまうようになるので、可能な限り使用を控えたほうが良いとのこと。  
また、any型のオブジェクトを与えた場合、そのオブジェクトのプロパティや処理結果もany型と解釈されるようなので、適切にキャストしてあげないとコードがどんどんany型に侵食される。

```:javascript
function fn(obj: any) /* 返り値がany型になっちゃう */ {
  let foo /* プロパティもany型 */ = obj.foo;
  let bar /* 演算結果もany型 */ = foo * 2;
  return bar;
}

let bar /* 外に漏洩したany型 */ = fn({ foo: 2 });
let baz /* とまらないany型 */ = "baz:" + bar;
```

```
function fn(obj: any) /* number型と推論される */ {
  let foo: number = obj.foo; // キャストしてにany型を食い止める
  let bar /* number型と推論される */ = foo * 2;
  return bar;
}

let bar /* number型と推論される */ = fn({ foo: 2 });
let baz /* string型と推論される */ = "baz:" + bar;
```

### 4. Maybe型

nullやundefinedを許容する場合 `?` を型の頭につける。

```:javascript
function method(x: ?number) {...}

// すべて動作する
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

```
const a: number = 1;
let b: string = '2';
```

変数の再代入時にFlowは型推論してくれる。

```
let x = 1;
x = '1';

// 動作する
let y: string = x;
```

### 6. 関数の型

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
var obj: {
  x: string,
  y: number,
  z: boolean
} = {
  x: "1",
  y: 1,
  z: true
};
```

Flowではundefinedのプロパティにアクセスするとエラーになる。

```
var obj: {x: string} = {x: "1"};
obj.y // エラー
```















































