# MacからVS CodeでVM上のWindows内のファイルを編集する

---

タイトルがやたら冗長ですが…。

# あらすじ

作業マシンはMacだけど、IISサーバを使用する必要があったためVM Fusionをインストールし、そこにWindows環境を構築した。
しかし、VMのWindowsでファイルを編集すると非常に動作が遅い…。またVMのファイル共有機能はホストマシンのファイルが対象でありゲストのファイルは対象外だった。
そこで、今年の春に正式リリースされたWindows10のOpenSSH機能を使うことにした。


# 前提

Windows 10 April 2018 Update (1803) 以降がインストールされていること


# 1. OpneSSH サーバのインストール

`スタートメニュー` から `設定` をクリックし `アプリ` を選択。

![01.png](https://qiita-image-store.s3.amazonaws.com/0/45634/5ec26f7a-dbb3-53af-2269-179ff40221b8.png)

`オプション機能の管理` をクリック。

![02.png](https://qiita-image-store.s3.amazonaws.com/0/45634/958ed23c-ac84-4dbd-7923-ab14bca83343.png)

`機能の追加` をクリック。

![03.png](https://qiita-image-store.s3.amazonaws.com/0/45634/74d53c51-90d4-20a4-8634-d5146213c016.png)

`OpenSSH サーバー` をインストール。

![04.png](https://qiita-image-store.s3.amazonaws.com/0/45634/0e1f8da6-f38a-29b3-e677-bf17e4d81461.png)

# 2. SSHサーバの起動

`コントロールパネル` から `管理ツール` を開き、 `サービス` を表示する。

![05.png](https://qiita-image-store.s3.amazonaws.com/0/45634/fa7a0bfe-7212-306e-64ca-d75ebbd77341.png)

`OpenSSH SSH Server` の項目で右クリックしてプロパティを開く。
`スタートアップの種類` の項目で `自動` を選択し再起動する。

![06.png](https://qiita-image-store.s3.amazonaws.com/0/45634/e6c165ca-d6ad-2aef-74a9-9619b285b579.png)

# 3. macからSSHログイン

Macでターミナルを開き以下のコマンドを実行する。

```:Shell
ssh user_name@IP
```

`user_name` はWindowsのログインユーザー名
`IP` はWindowsのIPアドレス

```:Shell
Are you sure you want to continue connecting (yes/no)?
```

と聞かれるので `yes` と入力。  
その後パスワードを聞かれるのでWindowsのログインパスワードを入力する。


# 4. Visual Studio Code に SSH FS をインストール

こちらの記事を参考に、Visual Studio Codeの設定をする。


> Visual Studio CodeでSSHごしにファイルを編集する
> https://qiita.com/informationsea/items/5c9f05c81a41fb885460

設定ファイルは以下のようにした。

```:JSON
{
    "label": "Local Windows",
    "root": "プロジェクトファイルの場所",
    "host": "WindowsのIPアドレス",
    "port": 22,
    "username": "Windowsのログインユーザー名",
    "password": true
}
```

以上で、Macから編集できるようになりました。めでたしめでたし。


