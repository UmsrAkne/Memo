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