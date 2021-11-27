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


### push の際にパスとアカウント名を確認された場合

	// 通信を ssh に変更
	#  (master>)> git remote set-url origin git@github.com

	// https:// -> git@ になっていることを確認
	// -v オプションはリモートリポジトリの名前と url を表示する。
	#  (master>)> git remote -v
	origin  git@github.com (fetch)
	origin  git@github.com (push)

	// リモートリポジトリをセットする。ユーザー名とリポジトリ名は適宜置き換え
	#  (master>)> git remote set-url origin git@github.com:userName/repoName.git

	// リモートリポジトリがセットされているのを確認する。
	#  (master>)> git remote -v
	origin  git@github.com:userName/repoName.git (fetch)
	origin  git@github.com:userName/repoName.git (push)

	#  (master>)> git push

	// push できる
	
### ファイル名の大文字、小文字の変更を無視しない

設定を確認する。

	$ git config -l --local | grep core.ignorecase
	core.ignorecase=true
	
ignorecase を false にセットすれば、大文字小文字の変更も検知される。  
上記コードで再度確認も可能

	$ git config core.ignorecase false