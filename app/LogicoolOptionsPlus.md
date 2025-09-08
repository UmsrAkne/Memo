# Logicool Options+ ソフトウェア に関するメモ

Windows 10 にて、起動時に画面右下に通知が出る。

    Logicool Options + ソフトウェア

という表示。要は広告。通知を消すには以下。

## レジストリエディタを使用する方法

レジストリエディタからスタートアップアプリを編集する。

    HKEY_LOKAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Run

のアドレスにジャンプ

    Logitech Download Assistant

の値があるのでこれを削除。

CMD から行いたい場合は以下のコマンドを使用する。

執筆時点で下記は試し打ちしていないので、コマンドは失敗するかもしれない。実行後要確認。

    reg delete HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Run /v "Logitech Download Assistant"

## Windows の設定を使用する方法

スタートメニューで `スタートアップ` で検索。

スタートアップアプリの中から `Logitech Download Assistant` を Off にする。

なお、スタートアップアプリは

    設定 -> アプリ -> スタートアップアプリ

の順でアクセスしても開ける。

スタートアップアプリに、同ソフトが存在することを確認しただけで、実際に実施してはいない。

これを行っても、通知が消えない可能性もある。要確認。
