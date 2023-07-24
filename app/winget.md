# winget

CLI からソフトのインストールが可能。  
なんでも配布されているわけではないが、有名なソフトなら大抵は対応可能。

## Require

 Windows 10 1709 (ビルド 16299) 以降が必要。それ以降は標準でバンドルされている。

	$ winget

でアプリの使い方が表示されればインストールされている。

ConEmu で使おうとするとで `permission denied` が表示される。要検証。

## サブコマンド

`winget` に続けて以下のサブコマンドを入力して使う。公式ページより一部抜粋。

	install		# 指定されたアプリケーションをインストールします。
	show		# 指定されたアプリケーションの詳細を表示します。
	search		# アプリケーションを検索します。
	list		# インストール済みパッケージを表示します。
	upgrade		# 指定したパッケージをアップグレードします。
	uninstall	# 指定したパッケージをアンインストールします。
	settings	# 設定を開きます。
	export		# インストール済みパッケージの一覧をエクスポートします。
	import		# ファイル内のすべてのパッケージをインストールします。


## 使い方

	$ winget search packagename

で検索。複数の候補が引っかかる場合は、インストールができないので絞り込む。  
フィルタリングするには以下のようにする。

	$ winget search --id -e packagename

`id` を使って検索するのが手取り早い。`-e` は検索を完全一致とするオプション。

インストールは結果が唯一なのを確認した上で下記。

	$ winget install --id -e packagename

## 一括で導入したいソフト一覧

## 全マシンに導入

	winget install -e --id Microsoft.VisualStudioCode
	winget install -e --id Apple.iTunes
	winget install -e --id CodeJelly.Launchy
	winget install -e --id Git.Git
	winget install -e --id Google.Chrome
	winget install -e --id Google.JapaneseIME
	winget install -e --id Lexikos.AutoHotkey
	winget install -e --id Maximus5.ConEmu
	winget install -e --id Mozilla.Firefox
	winget install -e --id VivaldiTechnologies.Vivaldi
	winget install -e --id voidtools.Everything
	winget install -e --id VideoLAN.VLC
	winget install -e --id TheDocumentFoundation.LibreOffice
	winget install -e --id 7zip.7zip
	winget install -e --id Microsoft.PowerToys

## 開発マシン専用

	winget install -e --id GitHub.cli

	winget install -e --id Microsoft.VisualStudio.2019.Community
	winget install -e --id Microsoft.VisualStudio.2022.Community
	winget install -e --id Microsoft.OpenJDK.17
	winget install -e --id Microsoft.UpdateAssistant
	winget install -e --id Microsoft.DotNet.SDK.5
	winget install -e --id Microsoft.DotNet.SDK.6

	winget install -e --id Google.AndroidStudio

	winget install -e --id DBBrowserForSQLite.DBBrowserForSQLite
	winget install -e --id SnoopWpf.Snoop
	winget install -e --id Twilio.Authy
	winget install -e --id UnityTechnologies.UnityHub
	winget install -e --id vim.vim
	winget install -e --id ImageMagick.ImageMagick

	winget install -e --id TeraTermProject.teraterm
	winget install -e --id Docker.DockerDesktop
	winget install -e --id JetBrains.Toolbox
	winget install -e --id OpenJS.NodeJS.LTS
	winget install --id PostgreSQL.PostgreSQL

## 導入に失敗したもの

インストール直前までは行くものの、エラーで失敗したもの。

	winget install -e --id CrystalDewWorld.CrystalDiskMark
	winget install -e --id RARLab.WinRAR 
