# Docker導入手順

# 用語

* Docker
    * コンテナ型仮想環境技術
* コンテナ
    * 実行環境や命令など複数のファイルシステムをひとまとめにしたもの
* Docker レジストリ
    * Docker イメージ配信サーバ
* Docker Hub
    * Docker公式のDocker レジストリ
* Docker イメージ
    * コンテナをファイル化したもの
* Dockerfile
    * dockerコンテナイメージを生成するmakefileのようなもの
* リポジトリ
    * レジストリ上のイメージの集合
* タグ
    * リポジトリ上に登録されたイメージの別名
* イメージID
    * レジストリ上のイメージに割り振られる固有のID
    * イメージIDは複数のリポジトリ/タグと紐づく
    * レジストリ上のイメージは、ユーザ/リポジトリ:タグ の形式で表記される
    * mor/someApp:centos7
* Docker デーモン
    * runやbuildというコマンドを受け取ったDockerデーモンが実際の処理を行っている。
* Docker Machine
   * Docker Machineは、Dockerの実行環境を構築、管理をするツール。EC2やVirtualBoxなどの対象を指定して、そこにDockerが動作する環境を作ることができる。現在は各プラットフォームにネイティブ対応しているため、ローカル実行の際にはDocker Machineは使われない。ただし、古いOSを利用している場合にはDocker Machineを使うことになる。
* Docker Compose
    * 複数のDockerコンテナからなるサービスを構築、実行する機能
    * docker-compose.ymlで構成を定義する
    * https://docs.docker.com/compose/

# Dockerの流れ

1. Docker イメージ取得
2. Docker コンテナ作成
3. Docker コンテナ実行
4. Docker コンテナ停止
5. Docker コンテナ再実行
6. Docker コンテナ削除
7. Docker イメージ削除

* インストール

https://docs.docker.com/install/
https://docs.docker.com/docker-for-mac/install/

Get Docker for Mac をダウンロード

# 公式スタートガイド

https://docs.docker.com/get-started/
