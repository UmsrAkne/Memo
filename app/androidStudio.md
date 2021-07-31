# Android Studio Memo

### API のバージョンを下げる

	Menu Bar File -> Project Structure -> Modules -> app folder -> Default Config tab 0> Min SDK Version

因みに初期状態では、選択したバージョン以外は表示されなかった。  
選択したバージョン以外の SDK はインストールされていないらしい。

### 上記のリスト内に希望の API が表示されない場合

	Menu Bar Tools -> SDK Manager -> (Left colomn) Appearance & Behavior -> System Settings -> Android SDK 
	必要なバージョンにチェック -> OK
	
上記でインストールが開始される。