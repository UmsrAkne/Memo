# 秀丸エディタに関するメモ

## 置き換え時、一致部分を引用する。

    よく忘れるのでメモ。 後方参照と言うらしい。
    
    検索対象
    abc(.*)hij
    
    置き換え後の文字列 \1 のように バックスラッシュ + 数字を入力。
    検索対象の正規表現に複数の () を使用した場合、\2,\3 のようにする。
    置き換え時の()内の文字列を引用できる。
    例
        \1klmn
        
    vim では \(\) のように () 自体にエスケープが必要だったが、秀丸エディタでは必要ない模様。
        