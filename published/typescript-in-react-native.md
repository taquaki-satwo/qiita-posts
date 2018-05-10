React NativeにはデフォルトでFlowなる静的型付けライブラリが入っておりますが、TypeScriptを使いたい人もいるでしょう。
導入にあたり、Microsoftの公式ドキュメントがあるので早速試してみました。

こちらです。

> [Microsoft/TypeScript-React-Native-Starter](https://github.com/Microsoft/TypeScript-React-Native-Starter)

と思ったのですが、この資料は2017年5月ごろのReact Nativeを元にしているので、現在のバージョンとは少々差があります。
なので、今の構成に合わせて書き換えてみました。なお、流れはそのままですが訳はかなり端折ってます。

また、今回のコードはこちらにあります。

> [taquaki-satwo/TypescriptInReactNative](https://github.com/taquaki-satwo/TypescriptInReactNative)

# 前提

* Node.jsおよびnpmまたはYarnがインストールされていること
* TypeScriptなしでReact Nativeアプリが実行できる環境が整っていること

基本的なReact Nativeアプリの作り方は [こちら](https://facebook.github.io/react-native/docs/getting-started.html) を参照してください。

# 初期設定

まずはReact Nativeプロジェクトを作成します。

```
react-native init MyAwesomeProject
```

## ファイル構成を変える

React Nativeのjsファイルは、React Native Packagerが用いるBabelで変換されバンドルされます。
つまり、TypeScriptから出力されたファイルをReact Native Packagerに渡す形になります。

まずはエントリーポイントの `index.js` と中身の `App.js` を `src` ディレクトリに移動させます。

```
mkdir src
mv index.js App.js src
```

新たにエントリーポイントとなるファイル `index.js` を作成し、 `src/index.js` を読み込みます。

```
echo "import './src/index';" > index.js
```

テストディレクトリもsrc配下に移動します。

```
mv ./__tests__/ ./src/__tests__/
```

ここで一旦動作確認をします。

```
react-native run-android
react-native run-ios
```

テストが通るのも確認します。

```
yarn test
```

ここまで確認できたら、gitなどのバージョン管理ツールでコミットしておきましょう。

## TypeScriptの導入

さきほどのエントリーポイントのimport元を `src` から `lib` に変更します。

```index.js
import './lib/index';
```

### 設定ファイルの作成

`tsconfig.json` を作成します。

```
yarn add --dev typescript
yarn tsc --init --pretty --sourceMap --target es2015 --outDir ./lib --module commonjs --jsx react
```

出力されたファイルに `include` の項目を追加します。

```tsconfig.json
{
    "compilerOptions": {
        // other options here
    },
    "include": ["./src/"]
}
```

### テストの設定

ts-jestをインストールします。

```
yarn add --dev ts-jest
```

package.jsonのjestの項目を以下のように書き換えます。

```package.json
"jest": {
    "preset": "react-native",
    "moduleFileExtensions": [
        "ts",
        "tsx",
        "js"
    ],
    "transform": {
        "^.+\\.(js)$": "<rootDir>/node_modules/babel-jest",
        "\\.(ts|tsx)$": "<rootDir>/node_modules/ts-jest/preprocessor.js"
    },
    "testRegex": "(/__tests__/.*|\\.(test|spec))\\.(ts|tsx|js)$",
    "testPathIgnorePatterns": [
        "\\.snap$",
        "<rootDir>/node_modules/",
        "<rootDir>/lib/"
    ],
    "cacheDirectory": ".jest/cache"
}
```

### 型定義ファイル

Jest、React、React Native、React Test Rendererで使用するTypeScriptの型定義ファイルをインストールします。

```
yarn add --dev @types/jest @types/react @types/react-native @types/react-test-renderer
```

定義ファイルについて詳しくは [こちら](https://www.typescriptlang.org/docs/handbook/declaration-files/consumption.html)

### TypeScriptファイルの作成

`src/index.js` , `src/App.js`  を `src/index.tsx`, `src/App.tsx` にリネームします。

TypeScriptで解釈できるように `App.tsx` の以下の部分を書き換えます。

```App.tsx
// import React, { Component } from 'react'; 
import * as React from 'react';

...中略

// export default class App extends Component<Props> {
export default class App extends React.Component<object, object> {
```

また、flowは使わないのでファイルのコメントから `@flow` を削除します。

次にテストファイルも同様に変更していきます。

`src/__test__/App.js` を `src/__test__/App.tsx` にリネームして以下を書き換えます。

```src/__test__/App.tsx
// import React, {Component} from 'react';
import * as React from 'react';

...中略

// import renderer from 'react-test-renderer';
import * as renderer from 'react-test-renderer';
```

ここでTypeScriptを実行します。

```
yarn tsc
```

正しくlibディレクトリにファイルが出力されているのが確認できたら、ここでコミットしておきます。

>
実際はここで
error TS2300: Duplicate identifier 'require'. や
error TS2717: Subsequent property declarations must have the same type.
といったエラーが出るはずです。解決策を記事の終わりに記載しました。

### ignoreファイルを追加する

.gitignoreに以下を追記します。

```
# TypeScript
#
lib/

# Jest
#
.jest/
```

## コンポーネントの作成

`src/components/` ディレクトリを作成し以下の内容で `Hello.tsx` を作ります。

```Hello.tsx
// src/components/Hello.tsx
import * as React from 'react';
import { Button, StyleSheet, Text, View } from 'react-native';

export interface Props {
  name: string;
  enthusiasmLevel?: number;
  onIncrement?: () => void;
  onDecrement?: () => void;
}

function Hello({ name, enthusiasmLevel = 1, onIncrement, onDecrement }: Props) {
  if (enthusiasmLevel <= 0) {
    throw new Error('You could be a little more enthusiastic. :D');
  }

  return (
    <View style={styles.root}>
        <Text style={styles.greeting}>
        Hello {name + getExclamationMarks(enthusiasmLevel)}
        </Text>
        <View style={styles.buttons}>
            <View style={styles.button}>
            <Button title="-" onPress={onDecrement || (() => {})} accessibilityLabel="decrement" color='red' />
            </View>
            <View style={styles.button}>
                <Button title="+" onPress={onIncrement || (() => {})}  accessibilityLabel="increment" color='blue' />
            </View>
        </View>
    </View>
  );
}

export default Hello;

// styles

const styles = StyleSheet.create({
    root: {
        alignItems: "center",
        alignSelf: "center"
    },
    buttons: {
        flexDirection: "row",
        minHeight: 70,
        alignItems: "stretch",
        alignSelf: "center",
        borderWidth: 5,
    },
    button: {
        flex: 1,
        paddingVertical: 0,
    },
    greeting: {
        color: "#999",
        fontWeight: "bold"
    }
});

// helpers

function getExclamationMarks(numChars: number) {
  return Array(numChars + 1).join('!');
}
```

## コンポーネントのテスト

上記のコンポーネントのテストを作成します。
まず、Reactコンポーネント用テストユーティリティのEnzymeをインストールします。

```
yarn add --dev enzyme @types/enzyme react-addons-test-utils react-dom
```

`src/components/__tests__/` ディレクトリを作成し、以下の内容で `Hello.tsx` を作成します。

```src/components/__tests__/Hello.tsx
import * as React from 'react';
import { Text } from 'react-native';
import { shallow } from 'enzyme';

import Hello from '../Hello';

it('renders correctly with defaults', () => {
    const hello = shallow(<Hello name="World" />);
    expect(hello.find(Text).render().text()).toEqual("Hello World!");
})
```

# 次のステップ

* [React-TypeScript-Tutorial](https://github.com/DanielRosenwasser/React-TypeScript-Tutorial)
* [Redux](https://redux.js.org/)
* [ReactXP](https://microsoft.github.io/reactxp/)


# TypeScriptコンパイル時のエラー解決

## error TS2300: Duplicate identifier 'require'.

定義が重複した場合のエラーだそうです。対策としては `tsconfig.json` の `types` の項目に以下を追加します。

```
{
  "types": ["react", "react-native", "jest"],
} 
```

> [Is there a conflict between @types/node and @types/react-native? · Issue #15960 · DefinitelyTyped/DefinitelyTyped](https://github.com/DefinitelyTyped/DefinitelyTyped/issues/15960#issuecomment-354403930)


## error TS2717: Subsequent property declarations must have the same type.

こちらのIssueで討論されています。

> [[@types/react-native] Property 'geolocation' must be of type 'Geolocation', but here has type 'GeolocationStatic'. · Issue #24573 · DefinitelyTyped/DefinitelyTyped](https://github.com/DefinitelyTyped/DefinitelyTyped/issues/24573)

差し当たりの解決策は `tsconfig.json` に `"skipLibCheck": true` を追加します。
これは型定義ファイルの整合性を無視する設定です。
正しい解決策がわかり次第追記します。
