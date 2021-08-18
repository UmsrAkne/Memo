# Git Memo

## Http でクローンして SSH で通信する。

前提 : SSH生成済み、Github に 秘密鍵を登録済み

github から http で `git clone` した場合、`push` する際にユーザー名とパスを求められる。  
該当リポジトリに入って、以下コマンドでOK

 	git remote set-url origin git@github.com:userName/repo

`git remote set-url` に関しての詳細は後ほど調べる。以上走り書き。

## リモートリポジトリの追加

	git remote add origin git@github.com:UserName/repoName.git
	git remote add origin https://github.com/UserName/repoName
	
いずれかの形式でも追加可能。

### 登録されているリモートリポジトリの確認

	git remote
	
登録されているリポジトリが出力される。未登録の場合は何も表示されない。

### リモートリポジトリの登録解除

	git remote remove targetName

対象の名前は通常は `origin` で登録されている。 `git remote` で確認することも可能。	