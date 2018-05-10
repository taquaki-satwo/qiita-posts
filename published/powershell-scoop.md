# PowerShell と Scoop でWindows開発環境を整える

ガッツリLinux環境を構築したい人向け -> [WSLでWindowsにLinux開発環境を構築する](https://qiita.com/taquaki-satwo/items/b379a141492ce8927deb)

# Scoopとは

コマンドラインで操作するパッケージマネージャ。
Homebrewやapt-get的なもの。
WindowsでもLinuxと同じようにツール類を扱いたい人向け。

詳しくはGitHubのWiki （[So What?](ttps://github.com/lukesampson/scoop/wiki/So-What%3F)） を参照。

# 手順

今回はScoopを使ってNode.jsをインストールする。

## 1. Scoopのインストール

power shellを起動し次のコマンドを入力。

```
iex (new-object net.webclient).downloadstring('https://get.scoop.sh')
```

もしエラーが出たら次を入力。

```
Set-ExecutionPolicy RemoteSigned -scope CurrentUser
```

## 2. Node.jsをインストール

Scoopはパッケージをbucketと呼ばれるもので管理している。
まず、Node.jsを `scoop search` コマンドで検索する。

```
scoop search node
'main' bucket:
    eventstore (4.1.0) --> includes 'EventStore.ClusterNode.exe'
    node-chakracore (8.11.1)
    nodejs-lts (8.11.1)
    nodejs (9.11.1)
    sliksvn (1.9.7) --> includes 'svn-populate-node-origins-index.exe'
```

`'main' bucket`に存在する `nodejs-lts (8.11.1)` と `nodejs (9.11.1)` がインストールできることがわかる。

ではNode.jsをインストールする。

```
scoop install nodejs
...中略
'nodejs' (9.11.1) was installed successfully!
```

このようにScoopは常に最新のパッケージがインストールされる。

## 3. versions bucketをインストール

Scoopには~envのようなバージョン管理はなく、そのかわりに `version bucket` をインストールした上でバージョンの違うパッケージをインストールする。

```
scoop bucket add versions
```

もう一度Node.jsを検索する。

```
scoop search node
'main' bucket:
    eventstore (4.1.0) --> includes 'EventStore.ClusterNode.exe'
    node-chakracore (8.11.1)
    nodejs-lts (8.11.1)
    nodejs (9.11.1)
    sliksvn (1.9.7) --> includes 'svn-populate-node-origins-index.exe'

'versions' bucket:
    nodejs010 (0.10.48)
    nodejs012 (0.12.18)
    nodejs4 (4.9.1)
    nodejs6 (6.14.1)
    nodejs7 (7.10.1)
    nodejs8 (8.11.1)
    nodejs9 (9.11.1)
```

`'versions' bucket` に存在するバージョン違いのNode.jsがインストールできるようになる。

```
scoop install nodejs8
...中略
'nodejs8' (8.11.1) was installed successfully!
node -v
v8.11.1
```

## 4. バージョンの変更

`scoop reset` コマンドを使う。

```
scoop reset nodejs
Resetting nodejs (9.11.1).
Linking ~\scoop\apps\nodejs\current => ~\scoop\apps\nodejs\9.11.1
node -v
v9.11.1
```

また、bucketは自作することができ、Scoopに登録されていないパッケージもインストールできるそうな。

# おまけ

### `scoop uninstall` で ` 〜を削除できません: パス '〜' の一部が見つかりませんでした。` と表示されたら

インストールされたパッケージのファイルパスが長いせいかと思われる。下記を参考にディレクトリのリネームを試す。

> [Can't uninstall nodejs; larger problem afoot · Issue #478](https://github.com/lukesampson/scoop/issues/478#issuecomment-135579615)

### bashでScoopが使えるかどうか

使えるけどScoopが実行されるたびにPowerShellが起動されるから効率悪い。

> [Can I use Scoop in bash?](https://github.com/lukesampson/scoop/wiki/Can-I-use-Scoop-in-bash%3F)

# 参考にした記事

* Scoop 公式: [Scoop](http://scoop.sh/)
* Scoop Wiki: https://github.com/lukesampson/scoop/wiki
* [Scoop と PackageManagement を使った Windows 環境の構築](https://qiita.com/kikuchi_kentaro/items/77793e4a21db6ffdb7cd)
* [ScoopでWindowsにおける開発環境構築を最適化しよう | さにあらず](https://blog.satotaichi.info/scoop/)
