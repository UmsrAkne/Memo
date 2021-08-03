# バッチファイル

コマンドプロンプト上で使うコマンドを１つ以上記述したテキストファイル  
*.bat ファイル。

## バッチファイルのテンプレ

	@echo off
	
	rem バッチの説明を記述
	
	exit


## コマンドの説明

-----
	@echo off

バッチファイルに書いたコマンドを cmd に表示しないようにする。  
特に理由がなければ表示する必要は無さそう。デフォルトは on になっている模様。

-----
	rem コメント
	: コメント

コメントアウト。 rem に続く文字列はコメントとみなされる。  
コロンによるコメントアウトは本来の使い方ではないようなので非推奨。  
知識として持っておくに留める。

-----
	exit

cmd を終了する。以下オプション。

	/B 現在実行しているバッチファイルを終了する 

-----
	pause

cmd を待機させる。バッチファイルを実行後に結果を確認したい時とかに使う？

-----
	xxx.exe

exe ファイルを実行。普通にファイル名を打ち込めば実行できる。

-----
	start xxx.exe
	
	rem ConEmu を指定ディレクトリで開く
	start C:\PROGRA~1\ConEmu\ConEmu64.exe -Dir path

単体ファイルの実行だけならファイル名を指定するだけでOKだが、複数の実行ファイルの実行は start を使う。  
start を使わないと順次アプリが実行されていくので、複数のソフトを起動したいだけならこっち。

---
## 触ってみた感想。

複数のアプリを一括で起動できるようにしたかったため、バッチファイルを作成しようとしたが無理だった。  
今回やろうとして引っかかった点を以下に記述。

### 自前の C# アプリ起動時の問題 (解決済み)

バッチファイルから呼び出しを行った際、実行ファイルのロケーションをアプリ内で使用しているアプリに関しては正常に動作しなかった。  
詳細は調べてないが、アプリの実装を整えればなんとかなりそう？

上記問題点は、実行前に実行ファイルのディレクトリに `cd` で移動してから実行することで回避可能。

### exit でウィンドウが閉じない問題 (解決済み)

ネットの情報を見ると exit で実行後にウィンドウが閉じてくれそうだが、閉じない。
一応 `exit /B` とかも試してみたけど黒い窓が残る。

バッチファイル内に記載していた `start "" code` が問題だったらしい。  
vscode を起動するためのコマンドだが、この場合は start は不要。
`code` とだけバッチファイルに記述すれば起動する。