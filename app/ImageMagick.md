# ImageMagick に関するメモ

画像を表示、操作を行うソフト。画像に関する操作を CLI から行うことができるのが強み。

## 導入

最新版をインストールするには winget

    > winget install ImageMagick.ImageMagick

## 使い方

インストール後、CLI から利用。`magick` コマンドに続いて、必要な情報を入力していく。

    ~ > magick
    Usage: magick.exe tool [ {option} | {image} ... ] {output_image}
    Usage: magick.exe [ {option} | {image} ... ] {output_image}
        magick.exe [ {option} | {image} ... ] -script {filename} [ {script_args} ...]
        magick.exe -help | -version | -usage | -list {option}

主要なコマンドは以下。

* composite
* display
* animate
* import
* montage
* identify
* mogrify
* convert

多彩な処理が可能。それぞれの解説は省略。

個人的に必要になりそうな範囲だけ記述する。

### identify

画像の情報を表示するサブコマンド。以下のように使用する。

    ~ > magick identify *.png
    A0101.png PNG 1280x720 1280x720+0+0 8-bit sRGB 485575B 0.000u 0:00.000
    A0201.png PNG 1280x720 1280x720+0+0 8-bit sRGB 245224B 0.000u 0:00.000

`*.png` の部分は、ファイル名を指定しても良い。  
ファイル単体の情報が欲しい場合は、普通にプロパティを確認した方が早い。  

CLI を使う必要があるのは、大量の画像の情報を一覧したい場合だと思うので、上記の指定方法が良い。

この場合は、画像の情報が揃っているので、整った出力になっているが、サイズ違い、フォーマット違いの画像が入り混じっている場合は、出力はかなり見難い。

`column -t` とかに渡して情報を整形して出力すると良い。

汚くても構わないならそれでも良し。

    ~ > magick identify *.png | column -t
    A0101.png  PNG  1280x720  1280x720+0+0  8-bit  sRGB  485575B  0.000u  0:00.000
    A0201.png  PNG  1280x720  1280x720+0+0  8-bit  sRGB  245224B  0.000u  0:00.000

### convert

画像の編集を行う。以下は画像を指定のサイズに切り取るコマンド

    ~ > magick convert base.png -crop 100x100+50+50 out.png

コマンドの意味としては、左上基準で `(50,50) - (150,150)` までの範囲を切り取って出力するというふうになる。

結果は `100x100` の画像が出力される。以下は詳細な解説。

#### 解説

`-crop`` の後に続く数値は

    # (w)x(h)+(x)+(y)

のようなフォーマットを取っている。各数値の意味は

    w : 切り取った後の画像の幅
    h : 切り取った後の画像の高さ
    x : 切り取りを開始する位置 (x座標)
    y : 切り取りを開始する位置 (y座標)

    # 切り取りを開始する座標の基準点はデフォルトでは左上。
    # 先述のコマンドの場合は元画像の左上から、xy +50px の位置から切り取りを開始する。
    # この位置はオプションで変更可能な模様(未確認)
