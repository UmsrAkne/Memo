# Visual Studio Code

大体何でもできる便利なエディタ。

## ターミナルを変更する

settings.json ファイルを編集する。  
コマンドパレットから "settings.json" で入力。  
今回は "bash.exe" に変更。

    "terminal.integrated.shell.windows": "C:\\Program Files\\Git\\bin\\bash.exe"

現在では上記は非推奨の警告が出た。推奨されるフォーマットは下記。

    "terminal.integrated.defaultProfile.windows": "Git Bash",
    "terminal.integrated.profiles.windows": {
        "Git Bash": {
            "path": "C:\\Program Files\\Git\\bin\\bash.exe",
            "args": [
                "-l"
            ]
        },
        "PowerShell": {
            "source": "PowerShell"
        }
    }

settings.json は３種類ある。
* 無表記(グローバル) 
* ワークベンチ(ワークスペース)
* デフォルト設定確認用（読み取り専用）

ワークスペース限定のテーマ変更とかの設定はワークスペース。  
そうでなければグローバル設定で。

デフォルトではターミナルコマンドはパワーシェルが起動。  
コマンドプロンプトでも構わないならば設定しなくても変更可能。  
どっちも使い方がわからなかったため（特にパワーシェルさん）バッシュに変更した。

## プレビューモードをオフにする。

プレビューモードでファイルを開いてくれる便利機能をオフにする設定。  
プレビューモードでファイルを開いている場合、新しくファイルを開くとタブが上書きされたりする。  
不要なのでオフにする。

    "editor.rename.enablePreview": false,
    "workbench.editor.enablePreview": false,
    "workbench.editor.enablePreviewFromQuickOpen": false

## キーボードショートカットの変更

vscode では、キーボードショートカットの変更も .json ファイルに記入することで変更する。  
GUI からでも変更可能なようだが、テキストファイルに直接編集した方が（複数環境での利用の場合等）汎用性が高そう。

編集方法はコマンドパレット -> `Preferences : Open Keyboard Shortcuts (JSON)`  
コマンドパレットに Keyboard とか打てばマッチする項目が列挙されるので上記の項目を選ぶ。
下記に設定の一例を示す。

    [
        {
            "key": "ctrl+t",
            "command": "workbench.action.terminal.focus",
            "when": "editorTextFocus"
        },
        {
            "key": "ctrl+t",
            "command": "workbench.action.focusFirstEditorGroup",
            "when": "terminalFocus"
        }
    ]

ctrl + t でターミナル <-> エディタ間でフォーカスを切り替える動作を設定している。  
vscode に bash を設定した際、ターミナルが開いてもフォーカスが移ってくれなかったため設定した。  
詳細は試していないが、 when の項目でフォーカスが当たっている箇所毎に動作を指定できると思われる。

## 無理なこと

こういう設定ないの？　こういうことできないの？　みたいに考えて自分で調べて実現"できなかったこと"を以下に書く。

### 十字カーソル

    カーソルx座標の位置を示す縦線を表示したかった。
    行のハイライトは可能だが、列のハイライト方法は発見できず。
    また、特定位置に固定された縦線を描画する方法もあるようだが、今回実現したい機能とは異なる。
    グーグルの検索、拡張(vertical line, column line で検索) を確認。
    vim や サクラエディタ、秀丸エディタ等には同等の機能が存在。
