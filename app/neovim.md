# NeoVim

vim のすごいやつ。

ある日、`vscode` (のVIMプラグイン) がぶっ壊れてまともに日本語入力できなくなったため、仕方なく乗り換えを行った。

## 前提環境

特に明記しない限り、 `Windows 10` を前提とする。

## 導入

ソフトの導入は `winget` で。

`winget` での導入でトラブっているような記事も見かけたが、今のところは問題なさそう。

## 設定ファイルの場所

    C:\Users\[UserName]\AppData\Local\nvim\init.vim
    
ディレクトリやファイルが存在しない場合は作れば OK

中身は、 `.vimrc` と同じ要領でコマンドを書いていけば良い。

互換性があるらしいが、まだ詳細な設定までは行っていないので、どの程度互換性があるのかは要調査。
