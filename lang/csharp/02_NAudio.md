# C#で NAudio を使う

C# のサウンドライブラリ NAudio を使ってみたので、それに関してのメモ。

## 環境

Windows 64bit WPF アプリケーション内で使用。

## 導入

VisualStudio GUI の NuGet から導入成功。

パッケージ名は NAudio (ver 1.10.0)

最新バージョンは 2.x.x となっていたが、導入できず。 詳しくは確認していないが、依存関係にある NAudio.WinForms が怪しい。

バージョン 1.x.x に関しては依存関係の問題はなくインストール成功。

同様に NAudio.Core 等もあるが、こちらだけでは独力でサウンド再生はできなかった（やろうと思えば可能？）。

## サンプル

NAudio を使用して mp3 ファイルを複数同時に再生する。

	using NAudio.Wave;

	void playSample(){
		var reader = new Mp3FileReader("url");
		var waveOut = new WaveOutEvent();
		waveOut.Init(reader);
		waveOut.Play();
	}

上記で実現可能。複数のインスタンス内で同じコードを実行すれば、複数の音声ファイルを再生することも可能。

尚、Mp3FileReader, WaveOutEvent は両方 `Disepose()` を実装しているが、`using` とかで使ったあと速攻破棄した場合はサウンドが再生されない。

このため、音声を再生している間はインスタンスは保持しておく必要がある。

クラスのフィールドに持っておくか、NAudio のサンプルでは using 内で処理を `Thread.Sleep()` で停止させたりしている。

## 音量操作

上記コード上で音量を操作する場合は、waveOut の方の音量を変更するとアプリ全体の音量に影響する。

今回、音量の操作を実現するに当たって、`Mp3FileReader` ではなく、 `AudioFileReader` を使用。  
`AudioFileReader.Volume` を操作して音量のコントロールを行った。これによって、インスタンス毎の音量操作が可能になる。  
尚、`AudioFileReader` でも `mp3` の読み込みは全く問題なかった。

## 曲の終了を検知する

`WaveOut.PlayBackStopped` イベントを使って検知することができる模様。  
ただ、名前が Stop とか End とかじゃないのが少し気になる。ネットの情報を見た感じ、検知できないパターンもあるらしい。  
当方 `.wav` ファイルを再生してみた感じでは問題なく検知できた。

## その他

	今回のコーディングの際、初期は
	
	var waveOut = new WaveOut();
	
を使用。最初はこちらでも問題なかったが、特定の状況でサウンドが再生されない場合があった。 再現性はあるが、原因は検討もつかない。

再生されなかった際、一行ずつステップインしたところ、`waveOut.Init(reader)` の行で処理が終了していた。(続く Play() は実行されなかった)

終了の際、アプリが落ちるわけでもなく、例外も出なかった。わからん。

# NAudio を使用して PC の音量を操作する

再生しているファイルの音量ではなく、システムそのものの音量を変更できる。

`NAudio.CoreAudioApi` は NAudio を `Nuget` から導入すれば利用することができる。  
`MasterVolumeLevelScalar` には `get, set` の両方が実装されている。

	using NAudio.CoreAudioApi;

	private void SetVolume(int value)
	{
		MMDeviceEnumerator devEnum = new();
		MMDevice device = devEnum.GetDefaultAudioEndpoint(DataFlow.Render, Role.Multimedia);
		device.AudioEndpointVolume.MasterVolumeLevelScalar = value / 100.0f;
	}

Enumerator とあるので、多分アプリ別の音量出力の設定もできるかもしれない。  
今回はマスターボリュームだけ設定できれば良かったので未検証。
