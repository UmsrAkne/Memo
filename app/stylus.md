# Chrome の拡張機能 Stylus のメモ

## 画像を非表示にするスタイル

	img,
	iframe {
		visibility: hidden !important;
		display: none !important;
	}

次で始まるURL -> `https://www.google.com` で使用  
適用する URL を指定しないと画像が一切表示されないので注意  
正確には、グーグルの検索ページで上記を適用すると、画像が表示されるべき部分に単色の矩形が表示されるようになる。