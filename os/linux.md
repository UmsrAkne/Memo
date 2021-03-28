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
    
## ユーザーを削除する
    #sudo userdel -r userName
        -r オプションをつけるとホームディレクトリも一緒に削除
        
## ユーザーの一覧を確認する
    #sudo less /etc/passwd
      