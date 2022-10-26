# JetBrains Rider 

試しに導入してみたので所感のメモ

## 感触

プラグインとして `IdeaVim` を追加して利用。  

エディタの動作は非常に軽快。`VS2022` もそれほど重いという感覚ではなかったが、動かした感じ非常に軽く感じる。  
`IDE` なのに `VSCode` くらい軽い。

## プロジェクト作成

`WPF` プロジェクトを実際に作ってみた。プレーンなプロジェクトを作ることはできる。

残念ながら `Prism Template Pack` のようなものは存在しないらしい。

`VS2022` で作成したプロジェクトの読み込みは問題ないみたい。

但し、`.Idea` のようなディレクトリが追加でプロジェクト内に作成される。  
これは仕方ない。`IDE` はそういうものなので。

## StyleCop

設定画面から設定できるものと、`Nuget` から導入できるものを両方 ON にしてみると、ひとまず動く。

しかし細々と、`VS2022` のものとの齟齬が目立つ。

* インスタンス・フィールドの命名規則として、 `_` がプレフィックスになっている。StyleCop と衝突する。

* `using` を名前空間の外に配置するスタイルが採用されている
    * `VS2022` の StyleCop では名前空間の内側に配置するのがデフォルト 
    * これはむしろ `VS2022` の方のスタイルを寄せる方がベストか

* インデントされた行で改行すると、次の行も自動でインデントされるが、この空白行に警告が表示される。  
    * (不要なスペースで行が終了していると言われる。スペースしかないんだけど……)

## IdeaVim

`Shift-{` , `Shift-}` のショートカット(空白行、段落移動) が上手く動作しない。

先述のように、改行時に、現在の行のインデントに合わせて自動でスペースが入力される。  
このスペースが原因でこの行が空白行扱いされないらしい。

セーブの時に自動フォーマットで行のスペースが削除されれば空白行扱いされるが、些か手間。

コマンドが中括弧だから検索も上手くできず情報が集まらない。何か設定があるのか？

## Xaml Styler

Xaml を自動でフォーマットしてくれる。VS と同名のプラグインが Rider にも用意されている。  
プラグインからインストール可能。

## キーマップ

ターミナルタブの出し入れ

    Ctrl + `

に設定されているがバッククォートは `Shift + @` の入力が必要なので入力できないし、  
では `Ctrl + Shift + @` を入力すれば良いのかというと反応しない。  
どうやら英語キーボードではバッククォートを単体入力できるという豆知識を得た。

駄目じゃん……。

## プロジェクトのバージョン設定

VS ではプロジェクトのプロパティから設定できるが、`Rider` では見つからない。  
ファイルを直接編集することで対応可能。

    ProjectName/Properties/AssemblyInfo.cs

上記ファイルを編集する。

    [assembly: AssemblyFileVersion("1.0.0.0")] // この部分を編集

保存すればそのまま反映される。  
因みに上記を取得するためのコードは以下の通り。

    FileVersionInfo.GetVersionInfo(System.Reflection.Assembly.GetExecutingAssembly().Location).FileVersion;