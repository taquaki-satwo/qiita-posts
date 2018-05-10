# Windows × Shift-JIS 環境に（やや）最新のフロントエンド環境を構築する

# 1. Node.jsの開発環境を整える

Windows Subsystem for Linuxを使う場合
https://qiita.com/taquaki-satwo/items/b379a141492ce8927deb

Poser Shell + Scoopを使う場合
https://qiita.com/taquaki-satwo/items/f815159b918bfe2a2a92

インストーラーを使う場合
https://nodejs.org/ja/download/current/

以下からは Windows Subsystem for Linux を使った場合。

# 2. yarn のインストールと初期化

[インストール | Yarn](https://yarnpkg.com/ja/docs/install#debian-stable)

```
curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -
echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list

sudo apt-get install --no-install-recommends yarn

yarn --version
1.5.1
```

package.jsonを作成する。

```
cd <プロジェクトディレクトリ>
yarn init -y
```







