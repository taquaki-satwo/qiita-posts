# Railsの変数をJSで使う（gon、Rails 5）

gazay/gon: Your Rails variables in your JS
https://github.com/gazay/gon

# 検証環境

検証日: 2018/05/10
debian 9.4
rails 5.1.5
Bundler version 1.16.1
gon v6.1.0 

# 手順

## 1. gemの追加

```rb:Gemfile
gem 'gon'
```

```
bundle install
```

## 2. テンプレートに記述

**JSファイルよりも前に記述する**

```erb:app/views/layouts/application.html.erb
<%= Gon::Base.render_data %>
```

## 3. Controllerに変数をセット

```rb
def index
  gon.str = 'Hello World'
end
```

## 4. JSから読み込む

```application.js
console.log(gon.str);
```

# 仕組み

gonがHTMLにグローバル変数を宣言してくれてました。

```HTML
<script>
//<![CDATA[
window.gon={};gon.str="Hello World";
//]]>
</script>
```
