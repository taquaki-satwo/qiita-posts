# Flow を速攻で学ぶ

Flowのドキュメントを速攻で読んで学んだまとめ。

https://flow.org/en/docs/

## Flowを使うための準備

### 1. インストール

Flowのインストール方法はこちら
https://flow.org/en/docs/install/

### 2. 初期化

設定ファイル `.flowconfig` を作成

```
flow init
```

### 3. バックグランドプロセスで実行

```
flow status

# 停止
flow stop
```

### 4. 監視ファイルを設定

JavaScriptファイルのファイルの先頭に以下を記述する。

```:javascript
// @flow
```

### 5. コードをチェックする

```
# flow status と同じ動作
flow
```

### サクッと試したい人は

ブラウザで動作確認
https://flow.org/try

## 型注釈

基本の書き方

```
x: 型
```

### プリミティブ型

JavaScriptのプリミティブ型は以下

* Booleans
* Strings
* Numbers
* null
* undefined
* Symbols

#### リテラルとグローバルオブジェクト

```:javascript
// リテラル -> 小文字
function method(x: number) {...}
method(1);

// グローバルオブジェクト -> 頭文字大文字
function method(x: Number) {...}
method(new Number(1));
``` 

#### 文字列の連結

オブジェクトや配列は文字列化してから連結する。

```
// エラー
"foo" + {};
"foo" + [];

// 文字列にする
"foo" + String({});
"foo" + [].toString();
"" + JSON.stringify({});
```