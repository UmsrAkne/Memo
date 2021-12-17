# Ren'py のメモ


## テキストの表示

`lable start:` 以降に記述したデータが表示される。Python のルールに則り、ラベル以下のブロックはスペースx4 によるインデントが必須。  
キャラクターの名前を先に宣言することで、各行に記述するキャラ名を省略できる。

	define s = Character('Sylvie', color="#c8ffc8")
	define m = Character('Me', color="#c8c8ff")

	label start:
		s "Hi there! How was class?"
		m "Good..."
		"I can't bring myself to admit that it all went in one ear and out the other."
		
## 画像の表示

	label start:
		scene bg meadow
		"message1"
		show chara1 tag imageName

画像の表示命令は上記。但し注意が必要。  

`scene bg meadow` の部分。  
`scene` と `bg meadow` に分けられる。更に `bg meadow` は `bg` タグがついた `meadow` となる。  
`bg meadow` はこの一塊で画像ファイル名となる。具体的にはプロジェクト下の `images` の `bg meadow.png` ファイルになる。  
ファイル名に半角スペースが含まれる命名規則となる模様。  
この命名規則に従うことで、画像をグループ分けしたりできるらしい。

`images` にサブディレクトリを作成してそこに画像ファイルを入れた場合では、ディレクトリ名は無視される仕様とのこと。

## 画像の表示(トランジション)

瞬時に画像を表示するのではなく、徐々に表示する命令。

	label start:
		scene bg01
		show chara01
		with dissolve

画像表示命令文の後に `with` を入力する。`dissolve` の他にも `fade` , `move` とか色々あるらしい。  
詳細は ドキュメントの Transitions の項を参照。

## 画像の表示(layeredimage)

	layeredimage limage:
		always:
			"ch a chara01"
		group layer1:
			attribute l1:
				"ch0 b chara02"
				
	label start:
		scene bg bg001 
		show limage
		"msg"
		
		show limage l1
		"msg2"

	return

レイヤーを使って複数枚の画像を重ねたものを一枚の画像のように扱う。必須機能。

`layeredimage limage:` で変数を宣言する。  
`always:` の後に続く画像名が常時表示されるベース画像。下段で `show limage` の際にもデフォルトで表示される。  
`group layer1:` 以下の `attribute l1' に関しては、下段の `show limage l1` のように属性名を併記して `show` することで `ch0 b chara02' が表示される。  
例には無いが、複雑な条件の指定も可能？
