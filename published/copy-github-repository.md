GitHubでCloneでもForkでもなくレポジトリの複製が欲しい。
そんな時の方法。

## 1. GitHub上で新規レポジトリを作成

普通に `New Repository` から作っちゃってOK。
仮にこれを `user/new-repository` とする。

## 2. ベアレポジトリの作成

ターミナルでコピー元のレポジトリ（仮に `user/old-repository` とする）をベアクローンする。
（ベアとは作業ディレクトリを持たないレポジトリのこと）

```bash
git clone --bare https://github.com/user/old-repository.git
```

（SSH使ってる方は `https://github.com` を `git@github.com:` に変えてください）

## 3. 新しいレポジトリにミラープッシュする

```bash

cd old-repository.git
git push --mirror https://github.com/user/new-repository.git
```

## 4. 確認&ベアレポジトリの削除

GitHubで新しいレポジトリにファイルが複製されているのを確認したら、ベアレポジトリを削除する。

```bash
cd ../
rm -rf old-repository.git
```

以上。