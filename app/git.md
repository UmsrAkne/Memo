# Git Memo

## Http でクローンして SSH で通信する。

前提 : SSH生成済み、Github に 秘密鍵を登録済み

github から http で `git clone` した場合、`push` する際にユーザー名とパスを求められる。  
該当リポジトリに入って、以下コマンドでOK

 	git remote set-url origin git@github.com:userName/repo

`git remote set-url` に関しての詳細は後ほど調べる。以上走り書き。