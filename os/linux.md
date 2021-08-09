# linuxに関するメモ

## 初期設定諸々

## ユーザーの作成
    # sudo useradd newUserName
    
    # ls /home 
        作成したユーザーの確認 (sudoが必要？)
    
    # sudo passwd newUserName
        先程のユーザーのパスワードを設定
        
    # su newUserName
        ユーザーを切り替え
    
## ユーザーに sudo コマンドの実行権限を与える。
    # sudo usermod -aG wheel newUserName
        -aG は --apend --groups グループ に相当
        
    # less /etc/group | grep wheel
        wheel に所属しているか確認する。
        
## ssh の設定
    # sudo cp -R ~/.ssh/ /home/newUserName/.ssh
        sshフォルダをコピー -R はサブディレクトリも含めてコピーするオプション
        
    # sudo chown -R newUserName:newUserName ~newUserName/.ssh
        ディレクトリの所有者を変更する。
        
    # sudo chmod -R go-rwx ~newUserName/.ssh
    
## sshでログインできるか確認する。
    #ssh -i ~/.ssh/newUserName newUserName@aaa.bbb.ccc.ddd
        ssh ログイン
    
## ssh root ログインを禁止する
    #sudo su -
        ssh ではなく普通にログイン
        
    # ssh -i keyName root@aaa.bbb.ccc.ddd
        ssh で root ログインを試行。ec2-user でログインしろと言われた。
        ec2 はデフォルトで root に ssh ログインはできないらしいので OK か。
    
## ssh のポートを変更する
    #sudo lsof -i:22
        指定した番号のポートが使用されているか調べる。表示がなければ未使用ポート
        
    #sudo vim /etc/ssh/sshd.config
        #Port 22 の記述を探す。

        いきなりポートを変更する前に

        Port 22
        Port xxxx
        
        のように記述し、任意のポートでログインできるか確認する。
        
    #sudo systemctl restart sshd.service
        sshdを再起動
        
    #ssh -i keyName userName@aaa.bbb.ccc.ddd -p xxxxx
        -p に続いて番号を指定することでポートを指定できる。
        EC2 の場合はセキュリティルール（インバウンド）の設定も必要なので注意
        
    
## ユーザーを削除する
    #sudo userdel -r userName
        -r オプションをつけるとホームディレクトリも一緒に削除
        
## ユーザーの一覧を確認する
    #sudo less /etc/passwd
      
## 自動でアップデートさせる

OS を使えるように鳴ったらまず

    sudo yum update
    
OS を最新の状態にしたら

    sudo yum install yum-cron

自動更新を行ってくれるパッケージが入る。更に設定。
    
    sudo vi /etc/yum/yum-cron.conf
    -> vim で /apply_updates
    
    - apply_updates = no
    + apply_updates = yes

カーネルの自動アップデートをしないようにする。  
ネットの説明を流し読みした感じだと、カーネルは通常のアップデートと一緒にガンガンやるようなものでもないらしい。  
不用意にやっているとサーバーが落ちたりする場合もあるとのこと。  
たまにログインすた時に手動でやる程度でいい？

    sudo vi /etc/yum.conf
    [main] より下に追加
    exclude=kernel*

上記設定は `#exclude=kernel*` のように `#` を行頭につけてコメントアウトするか行自体を削除すれば無効にできる。  

装備しただけでは意味がない。起動

    sudo systemctl start yum-cron
    sudo systemctl status yum-cron
    
２つ目のコマンドに対して `Active:active` の表示があれば起動している。

更に OS起動時に自動で起動するようにする。

    $ sudo systemctl enable yum-cron
    $ sudo systemctl is-enabled yum-cron
    enabled

１つ目のコマンドで自動起動を有効に。２つ目のコマンドで `enabled` が返ってくれば起動済み。