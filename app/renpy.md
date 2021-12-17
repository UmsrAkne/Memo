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
