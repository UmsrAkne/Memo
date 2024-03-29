# HUAWEI MediaPad M3 Lite 10 (BAH-W09) に関するメモ

## 不具合情報

### 症状

音楽再生中に操作不能になる。

### 環境

HUAWEI 製の Android タブレット
再生した音楽はいずれも 64GB の SD カード内のファイル。  
ハード内蔵のファイルは使用していない。

### 詳細

音楽を再生開始した直後からタッチ操作、スワイプを受け付けない。  
ハードウェアボタンの動作は問題無し。指紋センサーは未確認。

標準の音楽アプリ、目覚まし、タイマーによる音楽の再生、非公式音楽アプリによる再生、全てで再現性あり。  

音楽再生終了後は操作可能な状態に遷移。左記確認手順は以下  

	時計アプリ、目覚ましの再生音楽選択画面で音楽を視聴
	 -> 操作不能 -> 電源ボタンによる画面ロック -> 音楽停止 -> 操作可能

### 原因

現状では心当たり無し。特別な操作をしたわけではないはず。  
あるタイミングで発症し、それ以降の再現率は100%。  
SD カードが原因の可能性は否定できない。カードの容量はかなりギリギリの運用(凡そ9割使用)での発症だが関係ある？
一度目の発症時、アップデートが問題かとも思ったが、二度目の発症時にはオフライン運用だったためその可能性は低いと思われる。

### 対処 (1度目)

音楽アプリ等の強制終了 -> 改善せず  
ハードの再起動 -> 改善せず  
ハード初期化 -> *改善*

### 対処 (2度目)

初期化後に数日で一度発症。

音楽アプリ等の強制終了 -> 改善せず  
ハードの再起動 -> *改善*