## avif を一括でデコードするコマンド

`$ls -1 | xargs -I{} ./avifdec.exe {} {}.png`

### 解説

`ls -1` によって出力されるファイル名一覧を `xargs` に渡す。  
`xargs` は渡された文字列を `{}` という変数に格納。(変数名は任意)。  
`avifdec.exe` を実行する。引数は先程定義した変数部分に`ls -1`の出力が入力される。  

この場合、実行されるコマンドは

	./avifdec.exe fileName0 fileName0.png
	./avifdec.exe fileName1 fileName1.png
	./avifdec.exe fileName2 fileName2.png
	... 以下略

のようになる。

avif のデコーダーは公式リポジトリのリリースタグにて配布されているのでそれを取得する。
