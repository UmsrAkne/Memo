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

### .ideavimrc

設定ファイル `.ideavimrc` をユーザールートに作成することである程度の設定を行うことができる。

一度コマンドラインから `:source ~/.ideavimrc` で読み込みを行ったが、多分やらなくても読み込んでくれる。

    " ニーモニックマークの登録ウィンドウを表示
    nnoremap m :action ToggleBookmarkWithMnemonic<CR>

    " ニーモニックマーク一覧ウィンドウを表示
    nnoremap ` :action ShowBookmarks<CR>

独自の機能として、`:action` が存在する。

`Rider` 他、Jetbrains のIDE の機能を呼び出すことができる。  
`ideavim` のキー操作に IDE の機能をマッピング可能。

上記では、ニーモニックマークの操作を vim のマーク操作キーに割り当てている。  
`<CR>` は `Enter` (確定) を表している。

使用可能なアクションはコマンドラインにて `:actionlist` で一覧を出すことが出来る。  
`:actionlist searchPattern` 等とすると、部分一致検索でリスティングすることも出来る。

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

## Vim のコマンドライン上で日本が文字化けする

日本語を扱えるフォントを設定する。

    Setting -> Editor -> ColorScheme -> Fallback font 

こちらは一応設定したが恐らく無関係な項目。

    Setting -> Editor -> Font -> Typograpy Settings -> Fallback font 

`Typograpy`って何？　 両方変更したら文字化けしなくなったので記録しておく。

尚、コンソールの方は同じ設定を用いても相変わらず文字化けした。

## プロジェクトの設定ファイル

`Properties.Settings.Default` でアクセスすることができるあれ。

Rider の場合は、プロジェクトの設定等の画面からは弄れない？

    Search Everywhere -> Settings.settings

上記ファイルを開くと設定画面が出て編集することができる。

但し、今回のケースでは予め設定ファイルが VS で生成されていた。未生成の場合は同じような手順が取れるかは不明。

自分で直接設定ファイルを作ることもできるようだが（やり方を見ただけで未実施）手間がかかりそう。

通常、やむを得ず VS が使用できないような場合を除けばやる理由はないか。

## Debug Output の表示

`Debug` タブの `Debug Output` タブに関して。`Debug.WriteLine` の出力を確認する際に使ったりするやつ。

これがビルドした際に他のタブに切り替わってしまう挙動。毎回タブを切り替えるのが手間すぎる。

`Debug Output` タブの上で右クリックで出てくる項目

    v Focus On Startup
    v Focus On Breakpoint
    v Focus On Finish

上記のように全チェックで切り替わらなくなる。

これ以外にも設定画面の下記項目にもオプションが存在している。

Settings -> Build, Execution, Deployment -> Debugger 

設定時はこちらも確認する。

## Unity で Rider を使う

使おうとするとぞろぞろと通知が出てくる。それぞれ和訳していく。

    Update available - JetBrains Rider package.
    Check for JetBrains Rider package 3.0.24 in Unity Package Manager.

    # Unity の Rider package が利用可能。Rider の方ではなく、Unity のパッケージマネージャーを開いてパッケをインストールする。

---

    Advanced Unity integration is unavailable
    Make sure JetBrains Rider Editor is installed in Unity’s Package Manager and Rider 2023.1.3 is set as the External Editor.

    # Unity の外部エディタを Rider にしてるか確認してきている。
    # 多分エディタが Visual Studio とかに設定されている。
    # ちゃんと指定しないと、ツール間の連携が満足にできないので必ず指定する。

---

    Microsoft Defender configuration

    The IDE has detected Microsoft Defender with Real-Time Protection enabled. It might severely degrade IDE performance. It is recommended to add following paths to the Defender folder exclusion list:

    フォルダパスのリスト
    ...

    Choose "Automatically" to run a script that excludes these paths (note: Windows will ask for administrative privileges). Choose "Manually" to see Defender configuration instructions.

    # Microsoft Defender のリアルタイム保護が有効になっていると出てくる警告。
    # リストのフォルダを保護から除外してくれないとアプリのパフォーマンスが落ちるらしい。
    # 管理者権限があれば、スクリプトを実行して自動で除外リストにフォルダを加えてくれる。ある場合は使う。
    # ない場合は手動で。

## 新しいプロジェクトテンプレートを導入する

コマンドプロンプトを起動して以下のコマンドで導入完了。

    > dotnet new --install "Prism.Templates"

ソースは以下のページを参照。

    https://dotnetnew.azurewebsites.net/template/Prism.Templates/Prism.Wpf.Blank.Core.CSharp

執筆時点で `dotnet` をインストールした覚えはないので、他のアプリを導入する際に一緒に入ったものと思われる。

`dotnet` コマンドについては、MSのドキュメントが検索上位に上がってくる。

ただし、今回はひとまず概要だけ確認できればいいので、下記のページの説明を参照。

ascii の解説によると、`.netSDK` のインストールが必要らしい。

    https://ascii.jp/elem/000/004/082/4082000/

## アプリが起動できなくなった

以下のメッセージが表示され、アプリが起動できなくなった。

    Cannot connect to already running IDE instance.
    
以下のディレクトリの中にある `.lock` ファイルを探して削除することで起動できる。

    C:\Users\UseName\AppData\Roaming\JetBrains\IDEName

`IDEName` では使っている IDE の名前が入る。

複数のバージョンを導入していると、その数だけディレクトリができるので、起動できなくなったバージョンを適宜探す。
    