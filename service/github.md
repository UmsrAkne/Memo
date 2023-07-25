
# Github

## SSH で接続する

新しい環境で、新規リポジトリを push しようとしたらできない。

いきなりは無理っぽい。以下の鍵登録を実施することで、無事に push できた。

    $ cd ~/.ssh

`.ssh` フォルダに移動する。デフォルトでは何も入っていない。

`id_rsa` が存在する状態でこのまま進めると、上書きされるので注意する。

次のコマンドで鍵を生成する。

    $ ssh-keygen -t rsa

保存場所の確認とパスフレーズの設定について聞かれるので、`Enter` を押下。

尚、パスフレーズは未設定でも進めることができる。

`github` にログインした状態で以下のページにアクセス。設定画面 -> SSH and GPG Keys のページが開く。

    https://github.com/settings/ssh

SSH Key の New ボタンがあるので探して押す。

`Title`, `Key` をそれぞれ入力する画面に切り替わる。

`Title` には、鍵名。識別子っぽいので、わかりやすい名前をつけられると思われる。

`Key` の内容は、`.ssh` ディレクトリの中で、以下のコマンドを実行すればクリップボードにコピーされる。

    $ clip < ~/.ssh/id_rsa.pub

コピーされたものを、そのままテキストボックスに貼り付ける。

続いて、作業時には２段階認証による確認が挟まった。コードを入力すればキーが登録される。

最後に以下のコマンドを入力。

    $ ssh -T git@github.com

ポジティブな出力が返ってきたら成功。

## push の時に警告が出る
    
    > git push
    @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
    @    WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!     @
    @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
    IT IS POSSIBLE THAT SOMEONE IS DOING SOMETHING NASTY!
    Someone could be eavesdropping on you right now (man-in-the-middle attack)!
    It is also possible that a host key has just been changed.
    The fingerprint for the RSA key sent by the remote host is
    **:**:**:**:**:**:**:**:**:**:**:**:**:**:**:**.
    Please contact your system administrator.
    Add correct host key in /Users/hnw/.ssh/known_hosts to get rid of this message.
    Offending RSA key in /Users/hnw/.ssh/known_hosts:133
    RSA host key for example.com has changed and you have requested strict checking.
    Host key verification failed.

ip が変わったりした時に出るらしい？

    Offending RSA key in /Users/hnw/.ssh/known_hosts:133

のところで問題になっている行を確認できるので、該当行を確認して削除する。

本当に攻撃されてるかもしれない場合は……どうするんだろう。
