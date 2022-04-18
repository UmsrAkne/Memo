# aws EC2 に関するメモ

https://aws.amazon.com/jp/premiumsupport/knowledge-center/rds-connect-ec2-bastion-host/

### 上記のページを参考に、EC2インスタンス経由でRDSのDBインスタンスへの接続を行う。

EC2 のダッシュボードにアクセス。まず作成ってメニューがない。  
 -> ページ内に記載があるが　インスタンス起動から作成可能らしい。作成って書いて……。
 
 とりあえず無料枠 Amazon Linux 2 AMI , インスタンスタイプ : t2.micro  
 -> インスタンスの詳細の設定ボタン
 
 以下詳細設定項目
 
ネットワーク : DBインスタンスの vpc を選択。 RDSのダッシュボードから確認可能

サブネット : 不明。とりあえずデフォルトで進める。

次にストレージの追加、タグの追加と進む。ここも現状わからないのでデフォルトで行く。続いてルールの追加へ進む。

ルールの追加では、上記の説明ページのとおりに入力。ソースの IP に関しては、今回は マイIP で行く。

そのまま起動まで行ったらキーペアの作成を求められた。秘密鍵と公開鍵のペアを作成しろとのこと。  
まだ作っていないので新しいキーペアを作成することとする。AWSのドキュメントを確認した所、キーペア名はわかりやすい名前でOKらしい。

### 初期設定

コンソールから ssh コマンドを実行する。秘密鍵は ec2 のインスタンスを生成する段階でダウンロードしているはず。
下記 keyPair の部分にはファイル名を入れる。

OS に Amazon Linux を使用している場合はデフォルトユーザーは `ec2-user` でアクセスする。

`ssh -i keyPair userName@aaa.bbb.ccc.ddd`

ログインしたらまずはアプデ。

`sudo yum install`

続いてユーザーを追加する

`sudo adduser newuser`

sudoersファイルを編集する。  
sudoers とは sudo コマンドを使えるユーザー、また実行可能なコマンドを記述するファイルのこと……らしい  
sudoers を編集するためには `visudo` コマンドを使用する。  
このコマンドは sudoers を安全に編集するためのコマンド。編集中にファイルをロック。  
 構文のエラーチェックもやってくれて、異常があると変更を反映しない。

`sudo visudo`

    # %wheel ALL=(ALL) NOPASSWD:ALL
    %newuser ALL=(ALL) NOPASSWD:ALL

%wheel を探して下段に追加したユーザーを追記。パス無しで sudo の使用が可能。

作成したユーザーにログイン

`sudo su - newuser`

newuser でログインした状態で作業を続行する。 newuserのホームディレクトリに.sshディレクトリを作成

    $ mkdir .ssh

    # ファイルパーミッションを700（所有者のみ、読み取り、書き込み、削除が可能）に変更
    $ chmod 700 .ssh

    # authorized_keysを作成
    $ touch .ssh/authorized_keys

    # ファイルパーミッションを600（所有者のみ、読み取りおよび書き込みが可能）に変更
    $ chmod 600 .ssh/authorized_keys

Amazon Linux では、既に鍵ファイルが `ec2-user` のディレクトリに存在しているはずなため

    # authorized_keys ファイルをコピーする。
    # cp -p はファイルの所有者等の属性を含めてコピーするオプション
    $ sudo cp -p /home/ec2-user/.ssh/authorized_keys /home/newuser/.ssh/

    # ファイルの所有者を newuser に変更する。
    $ sudo chown newuser /home/newuser/.ssh/authorized_keys

この段階で `newuser` に ssh でログイン可能となっている。  
よって、必ず ssh でログインできるか確認する。これ以上進んでミスがあるとサーバーにログインできなくなる。

/etc/ssh/sshd_config を編集する。

    # 該当ファイルの末尾に追記する。
    DenyUsers ec2-user

sshd を再起動する。

    $ sudo systemctl restart sshd

一度ログアウトした後にもう一度 ssh で接続を試してみてログインできなくなっていれば成功。

### RDS の方の設定

こっちの方はインスタンス作成済みだったので、セキュリティグループのルールの追加をマニュアル通りに行う。  
EC2 のプライベートIP はEC2インスタンスのページから確認可能  

### 接続
今回成功した接続手段は TeraTerm で行けた。EC2 に SSH を使用してログイン。  
多分デフォルトでは postgresql のデータベースに接続できないと思うのでパッケージを探す。

```
yum search postgresql
```

を実行してパッケージを検索。候補がたくさん出てるけど今回は

```
sudo yum install postgresql.x96_64
```

を選択。インストール。無事成功（よかった）  
最後にデータベースへの接続を試行する。

```
psql -h [hostName] -U [dbUserName] -p[portNumber]
```

コマンドを実行。パスワードは上記コマンド実行後に聞かれるので入力すればログイン成功した。多分変更してない場合はポートは不要。  
あとパスワードは入力しても画面には表示されない。焦るわ。  
接続の確認ができたら 

```
\q
```

postgres の切断は上記コマンドで行ける模様。