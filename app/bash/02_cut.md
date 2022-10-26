# Cut

入力した各行の指定部分を切り出すことができるコマンド。汎用性は高い。

## オプション

    -b [byte]       # バイト数を指定して切り出す
    -c [number]     # 文字数を指定して切り出す
    -d [char]       # 区切り文字を指定して切り出す。デフォルトの区切り文字は tab
    -f [field]      # フィールド数で切り出す
    -s	            # 区切り文字を含まない文字列は出力に表示しない

## 数値の指定方法

    N	    # N行目
    N-	    # N行目から最後まで
    N-M	    # N行目からM行目
    -M	    # 先頭行からM行目

## 書式

    $ cut [option] [number] [file]

## 使い方

簡単に例を示すため標準出力を使用。  
ファイルの入力方法に関しては前述の書式を参照。

    # ３文字目以降を切り出している
    $ echo abc:defg | cut -c 3-
    c:defg

    # -b は半角文字で使う場合一文字につき 1byte なので -c と同様の動作
    $ echo abc:defg | cut -b 6-
    efg

    # 区切り文字(デリミタ)として ':' を指定し、２番目のフィールドを切り出す
    # echo abc:defg | cut -d ':' -f 2
    defg