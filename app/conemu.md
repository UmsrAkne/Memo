# ConEmu Memo

端末を便利に使える高機能コンソールエミュレーター。

## ConEmu 起動失敗のメモ

### 環境

	ConEmu 210627 (64) on Windows 10 64bit

複数のアプリを同時に立ち上げるためのバッチファイルを作成する過程で ConEmu が起動しなくなった。  
Settings - Startup - Startup options の項目が Auto save/restore opened tabs になっていたのが原因と思われる。  

	文法的にまずい bat ファイルを実行 -> ConEmu 起動失敗 -> その時の状況を再起動時にリストア -> 起動できない。  

という流れで起動できなくなったと考えられる。復旧した手順は以下。

	Auto save/restore opened tabs -> Specified named task {Bash::Git bash}

に変更したことで正常に起動できるようになった。