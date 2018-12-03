# Flowを使うための準備

## インストール

Flowのインストール方法はこちら
https://flow.org/en/docs/install/

## 初期化

設定ファイル `.flowconfig` を作成

```
flow init
```

## バックグランドプロセスで実行

```
flow status

# 停止
flow stop
```

## 監視ファイルを設定

JavaScriptファイルのファイルの先頭に以下を記述する。

```:javascript
// @flow
```

## コードをチェックする

```
# flow status と同じ動作
flow
```

## サクッと試したい人は

ブラウザで動作確認
https://flow.org/try
