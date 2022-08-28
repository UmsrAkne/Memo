# Postgresql on Docker

Docker の上の Postgresql を構築し、`Windows` からアクセスする。

以下のページのやり方。

(【Docker】postgresqlの構築)[https://zenn.dev/re24_1986/articles/b76c3fd8f76aec]

## docker-compose.yml の準備

カレントディレクトリに `docker-compose.yml` というファイル名でファイルを作成し、以下を記述する。

    version: '3'

    services:
    db:
        image: postgres:14
        container_name: postgres
        command: -c 'config_file=/etc/postgresql/postgresql.conf'
        ports:
        - 5433:5432 # デフォルトのままでは接続できなかった。
        volumes:
        - db-store:/var/lib/postgresql/data # db-store(volume) とコンテナ内の DB を接続。データを永続化。
        - ./config/postgresql.conf:/etc/postgresql/postgresql.conf
        environment:
        - POSTGRES_PASSWORD=passw0rd
    volumes:
    db-store:

ここに記述された情報を元に自動設定してコンテナを作ってくれる。

大体は最初に挙げたページと同じ内容だが、ポートの部分は書き換えてある。

    - - 5432:5432 
    + - 5433:5432

これは、`Windows` から `psql.exe` を利用した接続ができなかったため。  
詳細は不明だが、ポートが同じなせいか、`Windows` 側のデータベースに接続されてしまっていた。

このため、今回はポートの番号を別の番号に振っている。

書式は `ホスト側のポート:Docker側のポート` となっている。

因みに `.yml` はタブ文字禁止とのこと。

## postgresql.conf の準備

カレントディレクトリに以下のディレクトリ、ファイルを作成する

    ./config/postgresql.conf

内容は以下のようにする。

    listen_addresses = '*'

こっちは必要ないかも。　未検証。

## コンテナを作成

    $ docker-compose up -d
    # コンテナが作成される。すごい。


## コンテナにログイン

コンテナが作成されたら、まずはログインして　`postgresql` が動かせるか確認したい。

    $ winpty docker exec -it postgres bash

ログインしたら `psql` を実行。

    $ psql -h localhost -U postgres
    # データベースにログインできることを確認する。
    # psql の終了は exit コマンド

## ホストから接続

いよいよホストから接続する。クライアントソフトが必要なので何か入れる。  
今回は `winget` からインストールしておいた `PostgreSQL14` に入っている `psql.exe` を使用する。

それと、ここでは `コマンドプロンプト` を使用する。何故か `git bash` だとパスワードの入力を受け付けてくれない……。

    > "C:\Program Files\PostgreSQL\14\bin\psql.exe" -h localhost -p 5433 -U postgres
    # 接続できる

ポート番号を指定する `-p` は先程変更しておいた `5433` を忘れずに指定。  
コンテナ内に接続するのにも `localhost` でOK

パスワードの入力を求められるので、`docker-compose.yml` で指定したパスを入力。
