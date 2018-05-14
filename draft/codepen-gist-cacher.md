# CodePenを使うならGistとCacherを合わせれば幸せ

ブラウザ上でさくっとコードを書いて実行できるCodePen。
せっかく使うならGistとCacherを一緒に使って自分専用のコードスニペット集を作ろう。という話です。

# 知らない人のためにサービスの紹介

## CodePen

[CodePen - Front End Developer Playground & Code Editor in the Browser](https://codepen.io/)

ブラウザ上でHTML、CSS、JSを書いて、その場で実行、確認できるWebサービス。
人の書いたコードに「いいね」やForkができたり、ブログやQiitaにも貼り付けることができるのでSNS的側面が強いです。
Babelや主要なフロントエンドライブラリ（自作ライブラリも可）、CSSプリプロセッサ、HTMLテンプレートエンジンがオプションで選べるようになっているため開発環境を構築せずにコードを試せます。

<!-- img -->

使い方はこちらが参考になります。

[codepenでウェブ開発入門 - Qiita](https://qiita.com/InoueDaiki/items/1ee988d8e1f43bc205de)

## Gist

[Discover gists · GitHub](https://gist.github.com/discover)

GitHubが提供するファイル管理サービス。
GitHubはプロジェクト（ディレクトリ）を管理するものですが、Gistはファイル単体を管理します。

こちらの記事が参考になります。

[GitHubについてもう少し知ってみる。その5（Gistでお手軽コード管理）](http://www.paka3.net/github05/)

## Cacher

[Cacher](https://www.cacher.io/)

旧サービス名GistBox。コードスニペット管理ツールです。
100以上の言語をサポートし、Gistとの連携、チーム共有などの機能があります。
メーラーのようなUIやラベル、言語別での管理、検索をすることができます。

# サービスの連携

## 準備

まず、[こちら](https://codepen.io/accounts/signup/user/free) からGitHubアカウントでCodePenにサインアップ。
つぎに、[こちら](https://www.cacher.io/) からアプリをインストールしGitHubアカウントでサインイン。もしくは[こちら](https://app.cacher.io/sign-up)のWebアプリからサインインしてください。

## 使ってみる

CodePen上でCreateからNew Penを選択しエディタ画面を開きコードを書きます。
書き終わったら右下の `Export` から `Save as GitHub Gist` を選択します。

<!-- img -->

別ウィンドウでGistの画面が開きます。
概要が書かれたMarkdown、HTML、CSS、JSそれと読み込んだライブラリのリンクが出力されました。

<!-- img -->

Cacherアプリを開いてみます。

<!-- img -->

自動でコードが同期されています。

## コードの編集

保存されたコードはCodePen、Gist、Cacherのどこからでも編集することができます。しかし、Gist、Cacher間は同期していますが、CodePen、Gist間はCodePenからの出力のみで一方通行です。

このように、CodePenでコードを試す -> Gist に保存する -> Cacherから参照する。の流れで自作スニペット集を活用すると便利。という話でした。

　











