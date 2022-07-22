# AutoHotkey Memo

スクリプトエンジン（ソフト？） "AutoHotkey" に関するメモ

TimeStamp を入力するスクリプト。ahk ファイルの適当なところに貼り付ければ動く。

	::ts;::
	FormatTime,TimeString,,yyyyMMdd_HHmmss
	Send,%TimeString%_%A_MSec%
	Return

`::ts;::` でキー入力を受け取っている。この場合は `ts;;` と入力すると続く形式で日時が入力される。

FormatTime の仕様は以下

	FormatTime, OutputVar [, TimeStamp, Format] 

* `OutputVar` で変数に格納
* `TimeStamp` は必要無さそうなので把握してない
* `Format` で出力形式を設定する。yyyyMMdd_HHmmss -> 20210721_002535 という具合。

## スクリプトのリロード

	#z::Reload

スクリプトをリロード。編集中は必須。上記は `Win + z` で発動。

---

## Alt + tab

自動操作の中で `alt + tab` を利用した際の挙動。

	F12::
		Send, !{Tab}
		Sleep 20
		Send, !{Tab}
		return

このようにスクリプトを記述した際に通常の `alt + tab` を２回押した際とは違う動作をする。

通常、キー操作によって上記の操作を行うと、操作終了時点で、操作開始時と同じウィンドウがアクティブになっているはずだが……ならない。  
Sleep はキー操作がスルーされないように挟んでいるが、無くても動くかもしれない。

キー操作と同じ状態を実現するコマンドは以下のようにする。

	F12::
		Send, !{Tab}
		Sleep 20
		Send, !+{Tab}
		return
	
二回目のタブ切り替えを `alt + shift + tab` で逆方向に移動すると元のウィンドウをアクティブにできる。  
理由は未検証。自分の環境だけかもしれない。