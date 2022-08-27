# Docker を使う

## version 確認

コマンド操作でバージョンを確認することができる。

    $ docker version

このコマンドも含めて `Docker` のコマンドは全て `docker` から始まる。

## コマンドの基本書式

    $ docker [command] [option] [...arg]

## コマンド

基本的なコマンドのみ記述。

### コンテナを操作するコマンド

    $ docker container [command] # 基本書式

    start       # コンテナを起動する
    stop        # コンテナを停止する
    create      # イメージからコンテナを作成する
    run         # イメージのダウンロード、コンテナ作成、起動をまとめて行う。
    exec        # コンテナの中でプログラムを実行する
    ls          # 作成したコンテナの一覧を表示する
    cp          # コンテナとホスト PC の間でファイルをコピーする
    commit      # コンテナをイメージに変換する。

### イメージを操作するコマンド

    $ docker image [command] # 基本書式

    pull        # Docker Hub からイメージをダウンロード
    run         # イメージを削除
    ls          # ダウンロードしたイメージ一覧
    build       # イメージを作成

## コンテナの作成と起動

`Docker` が起動済みなのを確認し、以下のコマンドを実行する。

    $ docker container run --name myContainer -d httpd

    # httpd は Apache のイメージ名
    # --name myContainer でコンテナの名前を指定している。こちらは自由に命名。
    # -d はコンテナをバックグラウンドで動作させるオプション
    # -d を付けないと起動しても終了してしまう。と資料に書いてあるが付けないで起動しても普通に起動した。??

## コンテナ一覧の確認

    $ docker container ls -a    # -a を付けない場合、起動していないコンテナは表示されない
    $ docker -a ps

    # どちらのコマンドも同じ動作

作成したコンテナ一覧が表示される。

## コンテナ削除

    $ docker container rm myContainer

一覧を確認すると削除されていることが確認できる。

## Apatch のコンテナと通信

    $ docker container run --name myContainer -d -p 8080:80 httpd
    # -p 8080:80 はローカルポート 8080 にアクセスするとコンテナ内部のポート 80 のアプリに接続できる。
    # 80 番ポートは HTTP で通信するのに使用するポートとなっている。

`Windows` の方でブラウザを開き下記にアクセス

    http://localhost:8080
    # It works! が表示され、 Apatch が動作中なのを確認することができる。
