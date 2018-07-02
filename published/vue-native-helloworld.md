さっそくVue NativeでHello Worldしてみた

[Vue Native](https://vue-native.io/) が話題になっていたので、さっそく試してみました。

# Vue Native とは

[What is Vue Native?](https://vue-native.io/docs/index.html#What-is-Vue-Native)

Vue.jsでネイティブアプリが作れるツール。中身的にはReact NativeのAPIをラップしているらしい。

# 前提条件

* node 6.0以上
* npm 4.0以上
* React Native CLIがグローバルにインストールされていること

React Native CLIのインストール

```
npm install -g react-native-cli
npm install -g create-react-native-app
```

# Vue Native CLIを使ってプロジェクトを作る

```
npm install -g vue-native-cli
```

インストール後はターミナルの再起動を忘れずに。

```
vue-native init <プロジェクト名> // Create React Native App を使う
vue-native init <プロジェクト名> --no-crna //　Create React Native App を使わない
```

Create React Native Appを使うとExpo.ioというアプリで動作確認ができる

**参考**

* [Create React Native app](https://github.com/react-community/create-react-native-app) 
* [Expo](https://expo.io/)
* [React Native アプリの開発で create-react-native-app を使うべきかどうか - IT探検記](http://itexplorer.hateblo.jp/entry/20170902-create-react-native-app)

今回は `helloVueNative` という名前でCreate React Native Appを使いプロジェクトを作成しました。

## 生成されるファイル

```
helloVueNative
├── .babelrc
├── .gitignore
├── .watchmanconfig
├── App.vue
├── node_modules
├── README.md
├── app.json
├── package.json
├── rn-cli.config.js
├── vueTransformerPlugin.js
└── yarn.lock
```

## 起動

```
cd helloVueNative
npm start
```

ここでエラー発生。

```
> helloVueNative@0.1.0 start <プロジェクトディレクトリのパス>
> react-native-scripts start

12:56:26: Unable to start server
See https://git.io/v5vcn for more information, either install watchman or run the following snippet:
  sudo sysctl -w kern.maxfiles=5242880
  sudo sysctl -w kern.maxfilesperproc=524288

npm ERR! code ELIFECYCLE
npm ERR! errno 1
npm ERR! helloVueNative@0.1.0 start: `react-native-scripts start`
npm ERR! Exit status 1
npm ERR!
npm ERR! Failed at the helloVueNative@0.1.0 start script.
npm ERR! This is probably not a problem with npm. There is likely additional logging output above.

npm ERR! A complete log of this run can be found in:
npm ERR!     /Users/<ユーザー名>/.npm/_logs/2018-06-13T03_56_26_545Z-debug.log
```

メッセージ通りだけど、watchmanをインストールするかカーネルパラメータを変更すれ。とのこと。

```
brew install watchman
```

```
npm start

> helloVueNative@0.1.0 start <プロジェクトディレクトリのパス>
> react-native-scripts start

13:25:38: Starting packager...
Packager started!
```

無事起動。

# 確認方法

起動したターミナル画面で以下を入力。

1. `a` を入力 -> Android エミュレータの起動
2. `i` を入力 -> iOS エミュレータの起動
3. `s` を入力 -> メールでアプリのURLを送る
4. `q` を入力 -> QRコードを表示する
5. `r` を入力 -> 再起動
6. `R` を入力 -> キャッシュを消して再起動
7. `d` を入力 -> developモードにトグルする

今回は環境の問題でエミュレータで確認。

<img width="442" alt="iPhone X - 11.4 2018-06-13 13-54-23.png" src="https://qiita-image-store.s3.amazonaws.com/0/45634/7beee0a0-a4e8-1954-c990-9931c650dc5f.png">

確認できました。

# ライブリロード

`App.vue` を編集してみる。

```App.vue
<template>
  <view class="container">
-   <text class="text-color-primary">My Vue Native App</text>
+   <text class="text-color-primary">Hello Vue Native App</text>
    </view>
</template>
```

<img width="435" alt="iPhone X - 11.4 2018-06-13 14-01-33.png" src="https://qiita-image-store.s3.amazonaws.com/0/45634/f7ceda67-af69-2be6-2342-789526369c74.png">

ライブリロードされました。
以上です！
