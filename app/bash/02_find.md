# find

ファイルを検索するコマンド。再帰的に指定ディレクトリ内を検索してくれる。

### 標準的使用例

	find -name "*.txt"

`.txt` のファイルを検索している。名前で検索するには -name 引数が必要になる模様。  
ファイルサイズや更新日時等のあらゆる条件で検索が可能なコマンドであるため、条件を明示する必要があるためと思われる。  
また、当方環境ではファイル名指定は二重引用符なしでは反応しない？

以下は検索ディレクトリを指定する場合の書式。[startPoint] が検索を開始するディレクトリ。  
これを省略した場合はカレントディレクトリを開始点と見做すとのこと。

	find [startPoint] -name "*.txt" 

### 他のコマンドと組み合わせて使用した例

	find -name "*.txt" | grep -e '[0-9]' | xargs -I{} mv {} {}suffix

以下説明

1. 複数のディレクトリ内に散らばるテキストファイルを検索。
2. 標準出力を `grep` に渡して、数字が含まれるファイル名を抽出。
3. `grep` の出力を `xargs` に渡し、`mv` を実行。  
4. `grep` で抽出されたファイル名の後ろに suffix が加わる。

複数フォルダに散らばっている特定の名前のファイルを集めるために使用。(ファイル名に文字列を加えてエクスプローラーで検索)  
二度と使うことはない気がするけど記念に記録しておく。

find で正規表現による検索が可能ならもう少し簡略化できたが、このコマンドで正規表現を利用するのはどうにも面倒そう。

曰く、正規表現のタイプの設定が必要だとか、部分一致は不可だとか（部分一致に関しては `.*` とか使えばでも問題はなさそうだが……）

結論は `grep` を使ったほうが早い。