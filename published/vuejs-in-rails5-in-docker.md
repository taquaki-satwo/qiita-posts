# Docker環境のRails 5にVue.jsを導入した手順

# 検証環境

* 検証日: 2018/05/14
* macOS: High Sierra 10.13.4
* Docker: version 18.03.1-ce
* Rails: 5.1.5

# Node.jsとYarnのインストール

```:Dockerfile
...

# Node.js
RUN curl -sL https://deb.nodesource.com/setup_8.x | bash - && \
    apt-get install -y nodejs

# Yarn
# 1行目は外部パッケージをインストールするために、HTTPSに対応したapt methodsをインストールする処理
RUN apt-get update && apt-get install -y curl apt-transport-https wget && \
    curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add - && \
    echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list && \
    apt-get update && apt-get install yarn

...
```

## コンテナの起動

```
docker-compose build
dcoker-compose up -d
```

## 参考

* [パッケージマネージャを利用した Node.js のインストール](https://nodejs.org/ja/download/package-manager/#debian-and-ubuntu-based-linux-distributions-debian-ubuntu-linux) 
* [インストール | Yarn](https://yarnpkg.com/lang/ja/docs/install/#debian-stable)

# Webpackerの導入

[rails/webpacker: Use Webpack to manage app-like JavaScript modules in Rails](https://github.com/rails/webpacker)

```rb:Gemfile
...

# 現在の最新を使う場合
gem 'webpacker', '~> 3.5'

# または、masterを使う場合
gem 'webpacker', git: 'https://github.com/rails/webpacker.git'

# または、プレリリース版を使う場合
gem 'webpacker', '>= 4.0.x'

...
```

## インストール

```
docker-compose run web bundle install
docker-compose run web bundle exec rails webpacker:install
```

## 生成ファイル

| ファイル名 | 概要 |
|:--|:--|
| .babelrc | babel設定ファイル |
| .postcssrc.yml | PostCSS設定ファイル |
| app/javascript/packs/application.js | エントリーポイントファイル |
| bin/webpack | webpackのビルドスクリプト |
| bin/webpack-dev-server | ビルドサーバーの起動スクリプト |
| config/webpack/development.js | 開発環境設定ファイル |
| config/webpack/environment.js | 共通環境設定ファイル |
| config/webpack/production.js | プロダクション環境設定ファイル |
| config/webpack/test.js | テスト環境設定ファイル |
| config/webpacker.yml | webpacker設定ファイル |


## ヘルパーの修正

```rb:/app/views/layouts/application.html.erb
-  <%= javascript_include_tag 'application', 'data-turbolinks-track': 'reload' %>
+  <%= javascript_pack_tag 'application', 'data-turbolinks-track': 'reload' %>
```

## ビルド

ここで一旦ビルド

```
# webpack dev server
docker-compose exec web ./bin/webpack-dev-server

# watcher
docker-compose exec web ./bin/webpack --watch --colors --progress

# standalone build
docker-compose exec web ./bin/webpack
```

# Vue.jsの導入

```
docker-compose run web bundle exec rails webpacker:install:vue
```

## 生成ファイル

| ファイル名 | 概要 |
|:--|:--|
| app/javascript/app.vue | Vueコンポーネントファイル |
| app/javascript/packs/hello_vue.js | エントリーポイントファイル |
| config/webpack/loaders/vue.js | Vue.jsの環境設定ファイル |

## ヘルパーの修正

```rb:/app/views/layouts/application.html.erb
-  <%= stylesheet_link_tag 'application', media: 'all', 'data-turbolinks-track': 'reload' %>
-  <%= javascript_pack_tag 'application', 'data-turbolinks-track': 'reload' %>
+  <%= stylesheet_pack_tag 'hello_vue', media: 'all', 'data-turbolinks-track': 'reload' %>
+  <%= javascript_pack_tag 'hello_vue', 'data-turbolinks-track': 'reload' %>
```

## ビルド

```
docker-compose exec web ./bin/webpack
```
