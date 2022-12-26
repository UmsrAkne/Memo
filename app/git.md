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
	
### ファイル名を小文字から大文字に変更した後の挙動

ファイル名を大文字 aaa.* -> Aaa.* に変更した際、小文字ファイルと大文字ファイルの両方が git の変更ファイルに表示されるという事態が発生。  
詳細な原因まではわからないが、不要な方のファイルに対して `git rm` コマンドを実行。

	$ git rm aaa.* 

`git status` を実行すると該当ファイルが `deleted` に変化。その変更をコミットでファイルが一つになる。

上記やったらマージできなくなった。 `git mv -f oldName newName` をするべきだった？

## コミットを復活させる

	$ git reflog

git に対して行った変更、操作が記録されている。   
このコマンドを打って復活させたいコミットの ID を確認する。

ID が確認できたら、変更を適用するブランチに移動する。続いて

	$ git cherry-pick commitID

これにより、現在チェックアウトしているブランチに対して、指定したコミットが追加される。

注意点として、変更がステージングされたりするわけではないこと。  
cherry-pick を実行した時点で**チェックアウトしているブランチにコミットが追加される。**  
当然、main とかで行うと、直接コミットしてしまうことになる。（というかやった）

## git commit のエディタを設定する

`git config --global core.editor "C:/PROGRA~2/Vim/vim82/gvim.exe"`

`core.editor` が既に設定されている場合は、引数で上書きであることを明示する。

`git config --global --replace-all core.editor "C:/PROGRA~2/Vim/vim82/gvim.exe"`

自分の環境では基本ないけど、 `vim` にパスが通っている場合はフルパスで指定する必要はもちろんない。

## エラーとか

	Warning: Permanently added the RSA host key for IP address 'xxx.xxx.xxx.xxx' to the list of known hosts.

しばらくアクセスしていなかったリポジトリから pull とかした時に表示された。
エラーは表示されるが、それによって操作が行えない等の弊害は無い様子。

	ssh-keygen -R xxx.xxx.xxx.xxx

コマンドで対処できるらしい。時間がないので未検証。

## 削除したファイルを復活させる

以下を使用する。

	git checkout filePath

以下実証。※コマンド入力行以外はフォーマットしてある。

	# git に file.txt が登録されている状態。現在は変更無し
	~/Desktop/gitTest  (main)> git status
		nothing to commit, working tree clean

	# file.txt を削除する
	~/Desktop/gitTest  (main)> rm file.txt
	~/Desktop/gitTest  (main *)> git status
		(use "git restore <file>..." to discard changes in working directory)
				deleted:    file.txt

	# 削除したファイルを復元する
	~/Desktop/gitTest  (main *)> git checkout file.txt
		Updated 1 path from the index

	~/Desktop/gitTest  (main)> git status
		nothing to commit, working tree clean

## ブランチを別のコミットにつける

根本を付け替えるブランチにチェックアウトして以下を実行する。

	git rebase 根本に指定するコミットハッシュ

または

	# こちらで指定する場合はブランチの先頭に付け替えか？
	git rebase 根本となるブランチ名

以下検証。

	# 現在 main の Commit2 から branch1 が生えている
	# branch1 には２つのコミット　Commit2, COmmit3 がある。
	~/Desktop/gitTest  (branch1)> git log --oneline --graph
	* b0cbf2e (HEAD -> branch1) Commit 3 (branch1)
	* f73d0f8 Commit 2 (branch1)
	* c97e539 (main) Commit 1
	* 6b37170 init

	# main には Commit1 の後に既に Commit4 が存在する。
	~/Desktop/gitTest  (main)> git log --oneline --graph
	* d1ae886 (HEAD -> main) Commit 4 (main)
	* c97e539 Commit 1
	* 6b37170 init

	~/Desktop/gitTest  (main)> git checkout branch1
	Switched to branch 'branch1'

	# branch1 にチェックアウトした状態でリベース
	# リベース先にコミットハッシュを指定する
	~/Desktop/gitTest  (branch1)> git rebase d1ae886
	Successfully rebased and updated refs/heads/branch1.

	# git log を叩くと branch1 の根本が Commit1 から Commit4 になっていることがわかる。
	~/Desktop/gitTest  (branch1)> git log --oneline --graph
	* 50b7b20 (HEAD -> branch1) Commit 3 (branch1)
	* 7c3e717 Commit 2 (branch1)
	* d1ae886 (main) Commit 4 (main)
	* c97e539 Commit 1
	* 6b37170 init

### 備考

Rebase でブランチを付け替えた場合、ブランチ内のコミットは全て新規のものに置き換わるので頭に入れておく。

## 以前のコミットの差分を確認する

変更したファイル名ではなく、変更内容を確認するコマンド。  
差分確認を行う場合は `git diff`

	# 直前のコミットとの差分
	$ git diff head^
	$ git diff head~1

	# それよりも前のコミットとの差分
	$ git diff head^^
	$ git diff head~2

コミットの変更内容の確認は `git show`

	$ git show head^
	$ git show head~2

