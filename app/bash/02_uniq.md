# Uniq

重複している行を取り除くコマンド。

このコマンドで比較されるのは、隣接行のみなので、使用するには事前にソートされている必要がある。

## オプション

    -c --count          # 重複した行の回数のカウントを表示
    -u --unique         # 重複していない行だけ出力する
    -d --repeated       # 重複した行だけ出力
    -i --ignore-case    # 比較時に大文字小文字を無視する
    -w [number]         # 行の比較を先頭 N 文字に関してのみ行う

## 備考

`sort -u` でも、重複行を除いて出力することができる模様（未確認）