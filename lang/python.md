# Python Memo

## インストール (Windows)

python 公式サイトからインストーラー (64 or 32bit) をダウンロード。インストールでOK。  
"Disable path length limit" の項目は、インストール時のパスの長さの制限を解除するものらしい。無視しても問題無さそう  

## 使用方法
コマンドプロンプトから使う場合  `py` に続いてコードを入力。  
bash(当方ConEmuを使用)の場合は `winpty py` と入力。詳細な理由は調べたけど理解できなかった。  
とりあえず .bashrc に `alias py='winpty py'` を追加しておく。いちいち入力するのは面倒すぎる。  

## コーディング

### 終了方法 
	exit()
	
### 出力
	print('hello world')
	print("hello world")
	
### 四則演算
	# 足し算
	>>> 1 + 2
	3
	
	# 商を整数で求める（切り捨て？）
	>>> 7 // 4
	1
	
	# 冪乗
	>>> 3 ** 3
	27
	
	# 商と余剰を求める
	>>> divmod(11,4)
	(2,3)

### 変数
	# 数値
	>>> a = 3
	>>> b=5
	>>> a + b
	8
	
	# 文字列
	>>> t = "text"
	>>> print(t)
	text

### 関数の呼び出し
	# 標準で用意されている関数 max()
	>>> max(2, 6)
	6
	
	# 型を表示する type()
	>>> type(6)
	<class 'int'>
	
	>>> type(0.1)
	<class 'float'>
	
	# 真偽値に関しては小文字表記 true は不可の模様
	>>> type(True)
	<class 'bool'>