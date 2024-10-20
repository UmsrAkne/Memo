# Windows 10 を最初からセットアップするときのメモ

## Windows のアップデート

ネットが開通したら、まず最初に Windows のバージョンを最新版にする。  
ウェブ検索から MS 公式ページに飛び、`今すぐアップデート` ボタンを押す。

## アプリ導入

無事にアップデートが完了したら `winget.md` を参照して、CLI からインストール可能なものは `winget` で導入する。

`winget` で無理なものは、手動でインストール。秀丸ファイラとかはないっぽいので、公式からダウンロード。

インストールせずに使えるソフトは、専用のスペースに隔離してあるので、別の PC からコピってくる。

前環境のユーザフォルダに置いてある以下のファイルも持ってくる。絶対に１からやろうとするな。

    .bashrc
    .gvimrc
    .ideavimrc
    .vimrc
    .vsvimrc
    .bash_profile
    .gitconfig

個人的なファイルだが、同じフォルダに置かれている `shellScripts` も一緒にコピー。

## アプリ設定

一通りのインストールが済んだら、次に最低限の設定を施していく。

２，３項目設定すれば完了する場合は別として、通常は前環境から設定ファイルを出力して適用する。

## ChangeKey

非インストールソフトを集めたフォルダに入っている。

メニューバーから設定ファイルを出力可能。読み込みも同じくメニューバーから。

## ConEmu 

導入後に以下の設定を行う。

    Setting -> General -> Choose your startup task -> Bash::Git bash
        Git bash がない場合は Git をインストールしているフォルダにある bash.exe を直接指定する。

    Setting -> General -> Minimize/Restore hotkey : -> 消去 or 当たり障りの無いキーを割り当てる
        Ctrl + @ で ConEmu が立ち上がってくる。他のアプリと衝突するため、このキーは無効にした方が良い。

導入後に気がついたが、設定ファイルの出力が可能であるようだ。そっちのほうが早そう。

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

エディタのロケーションが変わっている場合は、指定しなおす。

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

## AutoHotkey

アプリそのものは別途インストールして、作ってあるスクリプトをスタートアップに加える。

エクスプローラーのパスバーに以下のアドレスを入力する。

    shell:startup

スタートアップフォルダが開くので、.ahk へのショートカットを追加する。

今回、`winget` から AHK を導入し、スクリプトを起動したところ、警告が出た。  
詳しい文面は省くが、AHK のバージョンの違いに関する警告っぽい。  
そのまま進むと、アプリの方で自動的に必要なバージョンをインストールしてくれた。

## Windows 自体の設定

設定 -> システム -> マルチタスク

    [ ] ウィンドウをスナップしたときに横に配置できるものを表示する

チェックを外す。Windowsキーでウィンドウを整列した際、画面半分にウィンドウのサムネイルが並ぶ機能をオフにする。

## レジストリ変更用バッチ

初期設定時に設定する内容（主に無効にする項目）をレジストリの編集によって実現するバッチが以下。

但し、現時点で動作未確認のため、まずいやつが混じっているかも。

試す機会があったら、最初は確認しながら実行したほうが良い。

`.sh` として実行する場合は、`rem` を `#` に置き換える。

    rem 以下全部不要な機能なので、無効・非表示にする。

    rem 登録されている拡張子は表示しない
    reg add "HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Explorer\Advanced" /v "HideFileExt" /t REG_DWORD /d "0" /f

    rem アクションセンターアイコンの非表示 (デフォルトで画面右端に表示される吹き出しアイコン) (自作)
    reg add "HKEY_CURRENT_USER\SOFTWARE\Policies\Microsoft\Windows\Explorer" /v "DisableNotificationCenter" /t REG_DWORD /d "0" /f

    rem 通知を許可してサウンドを再生する
    reg add "HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Notifications\Settings" /v "NOC_GLOBAL_SETTING_ALLOW_NOTIFICATION_SOUND" /t REG_DWORD /d "0" /f

    rem サインイン時の動作 デスクトップモードを使用
    reg add "HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\ImmersiveShell" /v "SignInMode" /t REG_DWORD /d "1" /f

    rem デバイスがタブレット モードのオンとオフを自動的に切り替えるとき
    rem 確認せず、切り替えも行わない : 0
    reg add "HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\ImmersiveShell" /v "ConvertibleSlateModePromptPreference" /t REG_DWORD /d "0" /f

    rem PC、タブレット、電話と明示的にペアリングする必要のないワイヤレス デバイスとの間で、アプリが自動的に情報の共有や同期を行えるようにする
    reg add "HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows\CurrentVersion\DeviceAccess\Global\LooselyCoupled" /v "Value" /t REG_SZ /d "Deny" /f

    rem アプリがメッセージ (SMS または MMS) の読み取りや送信を行うことを許可する
    reg add "HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows\CurrentVersion\DeviceAccess\Global\{992AFA70-6F47-4148-B3E9-3003349C1548}" /v "Value" /t REG_SZ /d "Deny" /f

    rem 新機能とおすすめを確認するために、更新の後と、サインイン時にときどき、[Windows へようこそ] の情報を表示する
    reg add "HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\ContentDeliveryManager" /v "SubscribedContent-310093Enabled" /t REG_DWORD /d "0" /f

    rem アプリが通話履歴にアクセスすることを許可する
    reg add "HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows\CurrentVersion\DeviceAccess\Global\{8BC668CF-7728-45BD-93F8-CF2B3B41D7AB}" /v "Value" /t REG_SZ /d "Deny" /f

    rem カレンダー
    reg add "HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows\CurrentVersion\DeviceAccess\Global\{D89823BA-7180-4B81-B50C-7E471E6121A3}" /v "Value" /t REG_SZ /d "Deny" /f

    rem アカウント情報
    reg add "HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows\CurrentVersion\DeviceAccess\Global\{C1D23ACC-752B-43E5-8448-8D0E519CD6D6}" /v "Value" /t REG_SZ /d "Deny" /f

    rem マイク
    reg add "HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows\CurrentVersion\DeviceAccess\Global\{2EEF81BE-33FA-4800-9670-1CD474972C3F}" /v "Value" /t REG_SZ /d "Deny" /f

    rem カメラ
    reg add "HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows\CurrentVersion\DeviceAccess\Global\{E5323777-F976-4f5b-9B55-B94699C46E44}" /v "Value" /t REG_SZ /d "Deny" /f

    rem 連絡先
    reg add "HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\DeviceAccess\Global\{7D7E8402-7C54-4821-A34E-AEEFD62DED93}" /v "Value" /t REG_SZ /d "Deny" /f

    rem 通知
    reg add "HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\DeviceAccess\Global\{52079E78-A92B-413F-B213-E8FE35712E72}" /v "Value" /t REG_SZ /d "Deny" /f

    rem アプリに診断の情報へのアクセスを許可する
    reg add "HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\DeviceAccess\Global\{2297E4E2-5DBE-466D-A12B-0F8286F0D9CA}" /v "Value" /t REG_SZ /d "Deny" /f

    rem アプリのバックグラウンド実行を許可する
    reg add "HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\BackgroundAccessApplications" /v "GlobalUserDisabled" /t REG_DWORD /d "1" /f

    rem タスク ビュー ボタンを表示
    reg add "HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\Advanced" /v "ShowTaskViewButton" /t REG_DWORD /d "0" /f

    rem Cortana アイコンを表示する
    reg add "HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows\CurrentVersion\Search" /v "SearchboxTaskbarMode" /t REG_DWORD /d "0" /f

    rem ときどきスタート画面におすすめを表示する
    reg add "HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\ContentDeliveryManager" /v "SystemPaneSuggestionsEnabled" /t REG_DWORD /d "0" /f

    rem ロック画面にトリビアやヒントなどの情報を表示する
    reg add "HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows\CurrentVersion\ContentDeliveryManager" /v "RotatingLockScreenOverlayEnabled" /t REG_DWORD /d "0" /f

    rem システム-マルチタスク-スナップされたウィンドウのサイズを変更するときに、隣接するスナップ ウィンドウのサイズも同時に変更する
    reg add "HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\Advanced" /v "JointResize" /t REG_DWORD /d "0" /f

    rem ウィンドウをスナップしたときに横に配置できるものを表示する」を有効にする
    reg add "HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\Advanced" /v "SnapAssist" /t REG_DWORD /d "0" /f

    rem Windows を使う上でのヒントやお勧めの方法を取得する
    reg add "HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\ContentDeliveryManager" /v "SubscribedContent-338389Enabled" /t REG_DWORD /d "0" /f

    rem Windows 追跡アプリの起動を許可して、スタート画面と検索結果の質を向上する
    reg add "HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Explorer\Advanced" /v "Start_TrackProgs" /t REG_DWORD /d "0" /f

    rem 常にメニューを表示する(エクスプローラー)
    reg add "HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Explorer\Advanced" /v "AlwaysShowMenus" /t REG_DWORD /d "1" /f

    rem [今すぐ会議] アイコンを削除
    reg add "HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Policies\Explorer" /v "HideSCAMeetNow" /t REG_DWORD /d "1" /f

    rem 天気を無効にする (自作)
    reg add "HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Feeds" /v "ShellFeedsTaskbarViewMode" /t REG_DWORD  /d "2" /f

    rem ニュースと関心事を無効
    reg add "HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\Windows Feeds" /v "EnableFeeds" /t REG_DWORD /d "0" /f

    rem People を非表示 (自作コマンド)
    reg add "HKEY_CURRENT_USER\Software\Policies\Microsoft\Windows\Explorer" /v "HidePeopleBar" /t REG_DWORD /d "1" /f

    rem Windows Inkワークスペースボタンを表示(0=表示しない,1=表示する)
    reg add "HKCU\Software\Microsoft\Windows\CurrentVersion\PenWorkspace" /v "PenWorkspaceButtonDesiredVisibility" /t REG_DWORD /d 0 /f

    rem タスクバーの結合をしない
    reg add "HKCU\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\Advanced" /v "TaskbarGlomLevel" /t REG_DWORD /d "2" /f

    rem 詳細な分析が必要な場合はファイルのサンプルを送信する
    reg add "HKEY_LOCAL_MACHINE\Software\Policies\Microsoft\Windows Defender\Spynet" /v "SubmitSamplesConsent" /t REG_DWORD /d "2" /f

    rem スタートメニューからのウェブ検索無効
    reg add "HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows\CurrentVersion\Search" /v "BingSearchEnabled" /t REG_DWORD /d "0" /f
    reg add "HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows\CurrentVersion\Search" /v "CortanaConsent" /t REG_DWORD /d "0" /f

    rem One Drive を無効にする
    reg add "HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\OneDrive" /v "DisableFileSyncNGSC" /t REG_DWORD /d "1" /f

    rem Edge のデスクトップ検索バー (Win + Shift + f) を無効にする。 
    rem Power Shell から実際に実行し、有効であることを確認。
    rem ただし、実行後即時有効にはならず、Edge を起動後、同アプリのアドレスバーに Edge://restart/ を入力、確定後に有効となった。
    reg add "HKLM\SOFTWARE\Policies\Microsoft\Edge" /v WebWidgetAllowed /t REG_DWORD /d 0 /f

    rem 全ての処理が終了したあと、待機する。
    pause
