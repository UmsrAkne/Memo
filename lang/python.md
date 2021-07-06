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