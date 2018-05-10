## Windows Subsystem for Linux（WSL）とは

Windwos 10またはWindows Server上でLinuxのツールを実行するためのサブシステムです。
嬉しいことにWindowsでBashが使えるようになります。

## WSLが解決できるもの

Windowsで開発していたがツールの関係上仕方なくMacで開発している人。
Macで開発していたが様々な事情で仕方なくWindowsで開発している人。
両方の悩みを解決します。

## 導入手順

今回はWSLのセットアップからNode.jsをインストールしHELLO WORLDまでを行います。

## 1. 機能の有功化

まず、コントロールパネルから `プログラムと機能` を選択し、 `Windowsの機能の有功化または無効化` をクリック。
`Windows Subsystem for Linux` にチェックを入れてPCを再起動します。

<!-- img -->
<img width="526" alt="01.png" src="https://qiita-image-store.s3.amazonaws.com/0/45634/e9c8510d-56b3-f4cd-4ab6-635c8777601c.png">

## 2. ディストリビューションの入手

次にMicrosoft Storeを開き、検索フォームに `linux` と入力。 `WindowsでLinuxを実行する` をクリックし好きなディストリビューションを選びましょう。

<!-- img -->
<img width="1042" alt="03.png" src="https://qiita-image-store.s3.amazonaws.com/0/45634/405784fd-f856-e3ff-4368-db8a9de1e327.png">

入手をクリックしダウンロード。

<!-- img -->
<img width="1045" alt="04.png" src="https://qiita-image-store.s3.amazonaws.com/0/45634/925cca96-7b0d-70e7-707a-fbc8e4e502d3.png">

ダウンロードが完了すると起動するか尋ねられるので `起動` をクリック。
するとコンソールが立ち上がり、名前とパスワードの設定を求められます。

<!-- img -->
<img width="994" alt="05.png" src="https://qiita-image-store.s3.amazonaws.com/0/45634/5e8a548a-23fa-3919-c373-639719f7e6ca.png">

設定が終わったら `exit` でコンソールを閉じちゃいましょう。

## 3. anyenvとnodenvのインストール

ここからはBashで作業してきます。
Bashを検索し起動します。

<!-- img -->
<img width="405" alt="06.png" src="https://qiita-image-store.s3.amazonaws.com/0/45634/68aab413-ee77-7752-3709-7cfff2327358.png">


開発言語のバージョン管理は好みがありそうですが、今回はanyenvとnodenvを導入します。

### anyenv

```
git clone https://github.com/riywo/anyenv ~/.anyenv
echo 'export PATH="$HOME/.anyenv/bin:$PATH"' >> ~/.bashrc
echo 'eval "$(anyenv init -)"' >> ~/.bashrc
exec $SHELL -l
```

### nodenv

```
anyenv install nodenv
...中略
exec $SHELL -l
```

## 4. Node.jsのインストール

現時点の最新版とLTSを調べてインストールします。
グローバルには最新版を設定しておきます。
（現在の最新版はv9.11.1、LTSがv8.11.1でした）

```
nodenv install --list
...中略
nodenv install 9.11.1
...中略
nodenv install 8.11.1
...中略
nodenv global 9.11.1
node -v
v9.11.1
```

## 5. Windwos上のファイル操作

さて、Node.jsをインストールしたまでは良いですが、どうやってBashからWindows上のファイルを操作したら良いでしょうか。
実は `/mnt/c` 配下は、WindowsのCドライブをマウントしています。
ですので、

```
cd /mnt/c/Users/<USER_NAME>
```

で、ユーザーフォルダに移動できます。
いちいち打つのは面倒なので適当なaliasを設定しておくと良いでしょう。
（ちなみにWindowsからもLinux上のファイルを操作できるそうですが、壊れる可能性があるので注意してください。）
[Bash on WindowsでWindows側からUbuntu側のファイルをいじると壊れることがあるので注意](https://www.kaitoy.xyz/2016/11/19/bow-do-not-change-linux-files-from-windows/)

では、ユーザーフォルダに `workspace` フォルダを作り、Node.jsのLTS版（v8.11.1）を設定してみます。

```
cd /mnt/c/Users/<USER_NAME>
mkdir workspace
cd workspace
nodenv local 8.11.1
node -v
v8.11.1
```

続いてHELLO WORLDを出力してみます。

```
echo 'console.log("HELLO WORLD")' > helloWorld.js
node helloWorld
HELLO WORLD
```

以上です。あとは好きな言語やツールを入れてWindowsでの快適な開発を楽しみましょう。
