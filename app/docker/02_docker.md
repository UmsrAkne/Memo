# Docker のインストール

`Windows 64bit pro` に Docker のインストールする上で確認すること。

### Home エディションを使っている場合

`WSL2` が使用可能 = `ver2004` 以降である。  
有効である必要は無い？ 未調査。

### Home エディション以外を使っている場合

`Build 16299` 以降である。

### CPU が SLAT をサポートしているか確認する

`MS` のページから `Coreinfo` というツールをダウンロードする。公式が無料で配布している。

管理者権限の `cmd` を起動して以下を実行する。

    > coreinfo64 -v

    ... 中略 ...

    HYPERVISOR	-	Hypervisor is present
    VMX       	*	Supports Intel hardware-assisted virtualization
    EPT       	*	Supports Intel extended page tables (SLAT)
    URG       	*	Supports Intel unrestricted guest

`VMX`, `EPT` の両方が `*` となっていれば対応している。  

尚、64bit OS で `coreinfo -v` を実行するとブロックされたと言われて実行できないので注意。

### BIOS で virtualization を有効にする。

BIOS に入って以下の項目を有効にする。

    Advanced > CPU Configuration >  Execute Disable Bit > [Enabled]   
    Advanced > CPU Configuration >  Intel(R) VirtualizationTechnology > [Enabled]

### 必要な機能の有効化

下記の順で設定画面を開き、該当項目を有効にする。

    スタートメニュー -> 設定 -> アプリ -> (関連設定)プログラムと機能 -> Windows の機能の有効化

    [v] Linux 用 Windows サブシステム
    [v] 仮想マシンプラットフォーム

設定後に再起動を促されるので再起動する。

### Linux カーネルのアップデート

    https://wsltorestoreage.blob.core.windows.net/wslblob/wsl_update_x64.msi

ダウンロードした `msi` を実行してボタンを押すだけの簡単なお仕事。

### Docker のインストール

公式ページから Docker Desktop (Windows) をダウンロードする。

    https://www.docker.com/get.started/

インストーラーを実行すると最初の画面に下記チェックボックスが出てくる。

    [v] Install required Windows components for WSL 2
    [v] Add shortcut to desktop

２つともチェックした状態で進んで問題ない。

### 起動確認

デスクトップのショートカットや、スタートメニューから Docker を起動できればインストール完了。
