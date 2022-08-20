# Android Studio Memo

## インストール

公式ページから実行ファイルをダウンロードしてインストールする。

最初にしておきたい設定等は下記。

---

## 初期設定

標準で日本語化することはできないらしい。確認してみたが `Setting` にそれらしい項目は見当たらなかった。

### プラグインのインストール

	MenuBar -> File -> Settings -> (SideBar) Plugin -> MarketPlace

1. `IdeaVim` 
2. `Save Actions`

### SaveActions の設定

ファイルの保存時に自動整形をさせるために導入する。導入後、以下の設定を行う。

	File -> Settings -> (SideBar) Save Ations

にて設定画面を表示。以下の項目をチェック。

	General ---
	[v] Activate save actions on save

	Formatting Actions ---
	[v] Optimaize imports
	[v] Reformat File

---

## バージョン管理

	MenuBar -> VCS -> (プルダウンメニュー) Git

Git を選んだ時点で自動的に該当プロジェクトのローカルリポジトリが作成される。

このとき、`.gitignore` も自動生成される。ひとまずこれを使っておけば良さそう。

---

## ビルドとデバッグ

USB ケーブルを使って PC とスマホを繋ぐ。繋いだ時点でスマホ側にダイアログを表示するのでデバッグを許可する。

---

## XML の編集

レイアウトの `.xml` を開くとレイアウト編集画面が開く。

デフォルトでは GUI の画面とプレビューが表示されている。画面の右上か左下のタブ。バージョンによっては位置が異なる？

	| Code | Split | Design |

と並んでいるので好きなのを選ぶ。`Split` しか使わない気がする。

---

### API のバージョンを下げる

	Menu Bar File -> Project Structure -> Modules -> app folder -> Default Config tab 0> Min SDK Version

因みに初期状態では、選択したバージョン以外は表示されなかった。  
選択したバージョン以外の SDK はインストールされていないらしい。

### 上記のリスト内に希望の API が表示されない場合

	Menu Bar Tools -> SDK Manager -> (Left colomn) Appearance & Behavior -> System Settings -> Android SDK 
	必要なバージョンにチェック -> OK
	
上記でインストールが開始される。

### キーボードショートカット

	shift -> shift : プロジェクト内を検索して移動。ファイル名指定で直に移動とかする
	
### その他

	オーバーライドメソッドを書く場合は、記述可能な場所でメソッドの先頭数文字を入力して候補から選択が早くて確実。
