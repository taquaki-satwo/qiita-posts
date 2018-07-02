# Firebase でテスト用APIサーバを作る

* firebaseでプロジェクトを作る
    * https://console.firebase.google.com
    * プロジェクトを追加
    * プロジェクト名を入力
    * プロジェクトを作成をクリック
* firebase cliのインストール
    * https://firebase.google.com/docs/cli/
    * npm install -g firebase-tools
* firebase login
    * Allow Firebase to collect anonymous CLI usage and error reporting information?
    * エラー情報収集の許可
    * ブラウザが開きログイン完了
* firebase listでプロジェクト一覧を見る
```
┌─────────────────────────────┬───────────────────────────────┬─────────────┐
│ Name                        │ Project ID / Instance         │ Permissions │
├─────────────────────────────┼───────────────────────────────┼─────────────┤
│ example-api                 │ example-api-xxxxx             │ Owner       │
└─────────────────────────────┴───────────────────────────────┴─────────────┘
```
* firebase hostingの設定
    * mkdir project name
    * cd project name
    * firebase init hostingで初期化
        * プロジェクトを選択
```
➜  chat-server firebase init hosting

     🔥🔥🔥🔥🔥🔥🔥🔥 🔥🔥🔥🔥 🔥🔥🔥🔥🔥🔥🔥🔥  🔥🔥🔥🔥🔥🔥🔥🔥 🔥🔥🔥🔥🔥🔥🔥🔥     🔥🔥🔥     🔥🔥🔥🔥🔥🔥  🔥🔥🔥🔥🔥🔥🔥🔥
     🔥🔥        🔥🔥  🔥🔥     🔥🔥 🔥🔥       🔥🔥     🔥🔥  🔥🔥   🔥🔥  🔥🔥       🔥🔥
     🔥🔥🔥🔥🔥🔥    🔥🔥  🔥🔥🔥🔥🔥🔥🔥🔥  🔥🔥🔥🔥🔥🔥   🔥🔥🔥🔥🔥🔥🔥🔥  🔥🔥🔥🔥🔥🔥🔥🔥🔥  🔥🔥🔥🔥🔥🔥  🔥🔥🔥🔥🔥🔥
     🔥🔥        🔥🔥  🔥🔥    🔥🔥  🔥🔥       🔥🔥     🔥🔥 🔥🔥     🔥🔥       🔥🔥 🔥🔥
     🔥🔥       🔥🔥🔥🔥 🔥🔥     🔥🔥 🔥🔥🔥🔥🔥🔥🔥🔥 🔥🔥🔥🔥🔥🔥🔥🔥  🔥🔥     🔥🔥  🔥🔥🔥🔥🔥🔥  🔥🔥🔥🔥🔥🔥🔥🔥

You're about to initialize a Firebase project in this directory:

  <PROJECT_DIRECTORY>


=== Project Setup

First, let's associate this project directory with a Firebase project.
You can create multiple project aliases by running firebase use --add,
but for now we'll just set up a default project.

? Select a default Firebase project for this directory:
  gohanBook (gohanbook)
  paymentAPI (paymentapi-de226)
  firstProject (firstproject-1a510)
❯ react-native-firestore-test (react-native-firestore-test)
  fetch-can-i-use (fetch-can-i-use)
  cloud-firestore-example (cloud-firestore-example-3f34d)
  [create a new project]
(Move up and down to reveal more choices)
```
    * ? What do you want to use as your public directory? (public)
        * ？ あなたのパブリックディレクトリとして何を使いたいですか？ -> enter
    * Configure as a single-page app (rewrite all urls to /index.html)
        * SPAにするかどうか -> N
```
✔  Wrote public/404.html
✔  Wrote public/index.html

i  Writing configuration info to firebase.json...
i  Writing project information to .firebaserc...

✔  Firebase initialization complete!
```

```
.
├── .firebaserc
├── firebase-debug.log
├── firebase.json
└── public
    ├── 404.html
    └── index.html
```


* firebase functionsの設定
    * firebase init functions

```

    🔥🔥🔥🔥🔥🔥🔥🔥 🔥🔥🔥🔥 🔥🔥🔥🔥🔥🔥🔥🔥  🔥🔥🔥🔥🔥🔥🔥🔥 🔥🔥🔥🔥🔥🔥🔥🔥     🔥🔥🔥     🔥🔥🔥🔥🔥🔥  🔥🔥🔥🔥🔥🔥🔥🔥
    🔥🔥        🔥🔥  🔥🔥     🔥🔥 🔥🔥       🔥🔥     🔥🔥  🔥🔥   🔥🔥  🔥🔥       🔥🔥
    🔥🔥🔥🔥🔥🔥    🔥🔥  🔥🔥🔥🔥🔥🔥🔥🔥  🔥🔥🔥🔥🔥🔥   🔥🔥🔥🔥🔥🔥🔥🔥  🔥🔥🔥🔥🔥🔥🔥🔥🔥  🔥🔥🔥🔥🔥🔥  🔥🔥🔥🔥🔥🔥
    🔥🔥        🔥🔥  🔥🔥    🔥🔥  🔥🔥       🔥🔥     🔥🔥 🔥🔥     🔥🔥       🔥🔥 🔥🔥
    🔥🔥       🔥🔥🔥🔥 🔥🔥     🔥🔥 🔥🔥🔥🔥🔥🔥🔥🔥 🔥🔥🔥🔥🔥🔥🔥🔥  🔥🔥     🔥🔥  🔥🔥🔥🔥🔥🔥  🔥🔥🔥🔥🔥🔥🔥🔥

You're about to initialize a Firebase project in this directory:

  <PROJECT_DIRECTORY>

Before we get started, keep in mind:

  * You are initializing in an existing Firebase project directory


=== Project Setup

First, let's associate this project directory with a Firebase project.
You can create multiple project aliases by running firebase use --add,
but for now we'll just set up a default project.

i  .firebaserc already has a default project, skipping

=== Functions Setup

A functions directory will be created in your project with a Node.js
package pre-configured. Functions can be deployed with firebase deploy.

? What language would you like to use to write Cloud Functions? JavaScript
? Do you want to use ESLint to catch probable bugs and enforce style? No
✔  Wrote functions/package.json
✔  Wrote functions/index.js
? Do you want to install dependencies with npm now? Yes
WARN notice [SECURITY] protobufjs has 1 moderate vulnerability. Go here for more details: https://nodesecurity.io/advisories?search=protobufjs&version=5.0.3 - Run `npm i npm@latest -g` to upgrade your npm version, and then `npm audit` to get more info.

> grpc@1.11.3 install <PROJECT_DIRECTORY>/functions/node_modules/grpc
> node-pre-gyp install --fallback-to-build --library=static_library

[grpc] Success: "<PROJECT_DIRECTORY>/functions/node_modules/grpc/src/node/extension_binary/node-v59-darwin-x64-unknown/grpc_node.node" is installed via remote

> protobufjs@6.8.6 postinstall <PROJECT_DIRECTORY>/functions/node_modules/google-gax/node_modules/protobufjs
> node scripts/postinstall


> protobufjs@6.8.6 postinstall <PROJECT_DIRECTORY>/functions/node_modules/google-proto-files/node_modules/protobufjs
> node scripts/postinstall


> firebase-functions@1.0.3 postinstall <PROJECT_DIRECTORY>/functions/node_modules/firebase-functions
> node ./upgrade-warning


======== WARNING! ========

This upgrade of firebase-functions contains breaking changes if you are upgrading from a version below v1.0.0.

To see a complete list of these breaking changes, please go to:

https://firebase.google.com/docs/functions/beta-v1-diff

npm notice created a lockfile as package-lock.json. You should commit this file.
added 497 packages in 15.768s

i  Writing configuration info to firebase.json...
i  Writing project information to .firebaserc...

✔  Firebase initialization complete!
```

```
.
├── .firebaserc
├── firebase.json
├── functions
│   ├── index.js
│   ├── package-lock.json
│   └── package.json
└── public
    ├── 404.html
    └── index.html
```

* 関連モジュール
    * npm i express cors
* SDKインポート

```js:/functions/index.js
const admin = require('firebase-admin');
admin.initializeApp(functions.config().firebase);
```

* expressのインスタンス化

```js:/functions/index.js
const express = require('express');
const app = express();
```

* corsモジュール

```js:/functions/index.js
const cors = require('cors')({ origin: true });
app.use(cors);
```

* ユーザー情報のスクリプトを記述
* APIの記述
* RESTFUL APIを使えるようにする
```
exports.v1 = functions.https.onRequest(app);
```
* APIサーバーのテスト
```
firebase serve --only functions

=== Serving from '<PROJECT_DIRECTRY>'...

i  functions: Preparing to emulate functions.
✔  functions: v1: http://localhost:5000/<PROJECT_ID>/us-central1/v1
info: User function triggered, starting execution
info: Execution took 18 ms, user function completed successfully
info: User function triggered, starting execution
info: Execution took 177 ms, user function completed successfully
info: User function triggered, starting execution
info: Execution took 222 ms, user function completed successfully
```
* URL
```
http://localhost:5000/<PROJECT_ID>/us-central1/v1
```
* APIの疎通テスト
```
curl -H 'Content-Type:application/json' -d '{"cname": "general"}' http://localhost:5000/<PROJECCT_ID>/us-central1/v1/channels
curl http://localhost:5000/<PROJECCT_ID>/us-central1/v1/channels
```
* Functionsの公開
```
firebase deploy --only functions

=== Deploying to '<PROJECT_ID>'...

i  deploying functions
i  functions: ensuring necessary APIs are enabled...
✔  functions: all necessary APIs are enabled
i  functions: preparing functions directory for uploading...
i  functions: packaged functions (39.96 KB) for uploading
✔  functions: functions folder uploaded successfully
i  functions: creating function v1...
✔  functions[v1]: Successful create operation.
Function URL (v1): https://us-central1-<PROJECT_ID>.cloudfunctions.net/v1

✔  Deploy complete!
```








