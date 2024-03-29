# Windows に関するメモ

## Windows がアップデートできない

OS : Windows 10 (インストール直後)

症状 : Windows がアップデートできない。具体的にはインストール後にバージョン 1511 までは自動的にアップデート適用となるが、  
それよりも先のバージョンに上がらない。

また、手動での更新チェックは最新バージョンだと表示される。

### 解決方法

`Windows 10 ダウンロード` でググって Microsoft の公式ページに飛び、更新アシスタントをダウンロードする。  

同じく MSの公式ページにアップデートカタログなるページがあるが、今回に関してはこちらは罠。  
闇雲に試してもダウンロードはできるが、適用不可と表示される。

---

## IMEの操作を Mac 準拠にする

日本語キーボードのスペースキーの左右にあるキーで、IME の設定切り替えができるようにする。

結論から言うとグーグル日本語入力を使う。無料。

Windows 10 の 1511 の時点では MS の純正 IME でも同等の機能が実現可能。  
尚、現行バージョンにアップデートした場合は設定がシンプル（！）になり不可能な模様（何故なのか？）  
少なくとも 21H2 では同等の機能を実現する設定項目が見つからなかった。

まず Google日本語入力をインストールすると、デフォルト設定時は画面右下に `あ` 等の文字とアプリのアイコンが表示される。  
思わずアプリアイコンの方を触りたくなるが、そこはグッと堪えて `あ` の文字の方を右クリックしてプロパティを出す。

Google 日本語入力プロパティが表示されるので、一般タブのキー設定の選択を編集。

モード : 直接入力 key1 IME を無効化  
モード : 入力文字なし key1 IME を無効化

モード : 直接入力 key2 IME を有効化  
モード : 入力文字なし key2 IME を有効化

を設定して編集完了。

## Windowsメニューの Web検索を無効にする

### gpedit を使って検索機能をブロック

`Windows Home` では使えないらしい(未確認)

Windowsキーを押して入力

    gpedit

(多分)左側に表示されていると思われるツリービューで項目を選択していく

    [コンピューターの構成] -> [管理用テンプレート] -> [Windows コンポーネント] -> [検索] 

画面右側に表示されている設定リストの中から次の項目を選択して右クリックで編集

    Web を検索したり [検索] に Web の検索結果を表示したりしない

未構成・有効・無効 を選べるラジオボタンがあるので `有効` を選択。

こいつが仕事しないせいでこんな面倒な手順を踏まなければならない。

これで Windows メニューにテキストを入れても勝手に検索されなく……ならない。gpedit 仕事しろ。

### レジストリエディタを使って検索機能をブロック

無心になって　`Windowsキー + R` を押し、ファイル名を指定して実行を出す。

    regedit

レジストリエディタが立ち上がったらまたまた左側のツリービューを以下の順で展開する。

    HKEY_LOCAL_MACHINE -> SOFTWARE -> Microsoft -> Windows -> CurrentVersion -> Search

Search の中でコンテキストメニューを開き以下。

    新規 -> DWARD(32bit) 値
    作成した値の名称は BingSearchEnabled

更に作成した値のコンテキストメニューから修正で編集が可能。値のデータが `0` になっているかを確認する。

更に同じく `Search` 内に `CortanaConsent` を探す。ない場合は `BingSearchEnabled` と同じ手順で値を作成する。

`CortanaConsent` の値のデータを `0` に設定。

Windowsメニューの検索機能は死ぬ。長く苦しい戦いだった。  

Windows の検索機能は本当に役に立たない。Web検索とか余計な機能つける前にまともな精度でファイル検索できるようにしてね。  
間違って検索した時に出てくるトピックの画像とかでダメージを受けるから一発でオフにできるようにして欲しいですね本当に。

---

## Windows Defender の無効化

    [Windows + R] -> regedit 

レジストリエディタが起動したら以下の階層を辿る。

    HKEY_LOCAL_MACHINE -> SOFTWARE -> Policies -> Microsoft -> Windows Defender

新しく値を作成する。

    右クリック -> (コンテキストメニュー)新規 -> DWORD(32bit) -> (値の名前を変更)DisableAntiSpyware

修正から値のデータを変更する。

    0 -> 1

### 備考

ネットに繋がっている普通のPCでするべきではない。

## セーフモードで起動する

以下は `Windows 10` での手順。

コマンドプロンプトを開く。（多分）管理者として開かなくても問題ない。

    Win + R -> cmd

シャットダウンコマンドを以下の引数付きで実行して待機。

    > shutdown /r /o /t 0

再起動すると青い背景にボタンが幾つか並んだ画面で立ち上がる。

`オプションの選択` という見出しが出ているはず。

    トラブルシューティング -> 詳細オプション -> その他の修復オプション -> スタートアップ設定 -> 再起動

続いて、スタートアップ設定から番号でオプションを選択する画面に移るので、必要な項目を選択する。

何をするために使用するかにもよるが、大体は `セーフモードとコマンドプロンプトを有効にする` だけで良さそう。

## マザーボードの型番確認

毎回忘れるので。コマンドプロンプトで試したが `powershell` とかでも問題ないと思う。

Win + R -> cmd -> systeminfo -> wmic baseboard get product

管理者権限でコマンドプロンプトを立ち上げる必要がある、との情報があるが、当環境では別に通常権限での立ち上げでも問題無かった。
