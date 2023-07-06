# Windows 10 を最初からセットアップするときのメモ

## アプリ導入

## ConEmu 

導入後に以下の設定を行う。

    Setting -> General -> Choose your startup task -> Bash::Git bash

    Setting -> General -> Minimize/Restore hotkey : -> 消去 or 当たり障りの無いキーを割り当てる
        Ctrl + @ で ConEmu が立ち上がってくる。他のアプリと衝突するため、このキーは無効にした方が良い。

## VSCode

他の PC から設定を持ってこられる場合は以下のロケーションに設定ファイルを置く。

設定ファイルは以下

    %APPDATA%\Code\User\settings.json

プラグインは以下に保存されている。

    %USERPROFILE%\.vscode\extensions

デフォルトターミナルの切り替え。コマンドパレットを開いて以下を入力することで選択可能

    > Terminal Select Default Profile -> Git Bash

## Google 日本語入力

設定ファイルをエクスポートするには

    IME のボタンを右クリック -> プロパティ -> キー設定 -> キー設定の選択 -> 編集 -> 新しいページの左下のプルダウン -> エクスポート

インポートも同様の場所に配置されている。

### デフォルト IME の切り替え

デフォルト設定を MSIMl Eからグーグル日本語入力に切り替える

    設定 -> 時刻と言語 -> 言語 -> 日本語の項目をクリック -> オプション 
    "言語オプション : 日本語" のページに移動 -> キーボードの追加 -> グーグル日本語入力を追加 -> MicrosoftIME を削除

## Git

以下のコマンドでコミット編集用のアプリを指定する。

ウィンドウズでのファイル指定だが、バックスラッシュではなくスラッシュで通る。

    > git config --global core.editor 'C:/NonRegistry/vim82/gvim.exe'

## Vivaldi

アドオンを導入する。

    AdBlock
    uBlacklist
    Vimium
    Stylus

履歴はデフォルトでは 3ヶ月 で消滅する設定なので、必要なら変更する。

Stylus の css には以下の内容を記述。画像全消し。

    img,
    iframe {
        visibility: hidden !important;
        display: none !important;
    }

### データの復元

復元元のメニュー

    File -> ブックマークのエクスポート

復元先のメニュー

    File -> アプリケーションまたはファイルからインポート -> エクスポートしたファイルを選択

ブックマークの画面には `インポートしたブックマーク` というフォルダに入れられて追加される。

## Windows 自体の設定

設定 -> システム -> マルチタスク

    [ ] ウィンドウをスナップしたときに横に配置できるものを表示する

チェックを外す。Windowsキーでウィンドウを整列した際、画面半分にウィンドウのサムネイルが並ぶ機能をオフにする。

