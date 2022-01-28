# Unity に関するメモ

## 外部のファイルを読み込む

Unity プロジェクトの `Resources` フォルダに入っているファイルが読めるのは当然だが、それ以外のPC 内のファイルは読めるか？

実際に読めた。でもやらないで済むならやるべきではなさそう。自分はやります。

	using System.IO;
	using UnityEngine;
	using UnityEngine.UI;

	string path = Directory.GetCurrentDirectory() + "/texts/testtext.txt";
	GameObject textWindow = GameObject.Find("TextField");

	string data = File.ReadAllText(path);
	textWindow.GetComponent<Text>().text = data;

ゲームの実行ファイルのパスは `Directory.GetCurrentDirectory()` で取得することができる。  
他にも、設定ファイル等が入っていると思われるフォルダ(AppDataとか)を取得するメソッドもあるらしい。  
今回は関係ないのでスルー。

実際の読み込みは `System.IO.File.ReadAllText(string path)` で可能。驚くほど普通に読める。

### 注意

コードを見れば明らかなことだけど、確実に存在しているわけでもないフォルダの中のファイルを参照している。  
行儀は良くない。というか、コードを実装した人間でないとこのソフトは使えない（当然）。  
一般公開するソフトには使えないことに注意。