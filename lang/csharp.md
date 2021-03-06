## 正規表現クラスの利用方法
tag C# Regex 正規表現 抽出

検索2回目（覚えてられないのでメモ）

```
var testString = "testTextString";

// 単一の Match を返す。コレクションを返す Matches もある。
// 第一引数に検索を書ける文字列。第二引数に正規表現テキストを入力する。

Match m = Regex.Match(testString, "test(Text)String");
System.Diagnostics.Debug.WriteLine($"[0] = '{m.Groups[0].Value}'");
System.Diagnostics.Debug.WriteLine($"[1] = '{m.Groups[1].Value}'");

```

以上コード実行で

```
[0] = 'testTextString'
[1] = 'Text'
```

が出力される。  
キャプチャ（一部文字列の抜き出し）を行うには () で対象の正規表現部分を囲い込む。

---

## Processクラスの利用
tag C# Cmd コマンドライン　コマンドプロンプト

C# からコマンドラインを実行する

```
            var process = new Process();
            process.StartInfo.FileName = Environment.GetEnvironmentVariable("COMSPEC");
            process.StartInfo.RedirectStandardOutput = true;

            // 標準出力を使うためには false にセットする必要があるみたい
            process.StartInfo.UseShellExecute = false;

            string commandText = "echo test";
            process.StartInfo.Arguments = "/c " + commandText;
            process.Start();

            System.Diagnostics.Debug.WriteLine(process.StandardOutput.ReadToEnd());
```

出力

```
test
```

### 解説
まずコマンドを実行するにあたって、環境変数の COMSPECT を参照して cmd.exe のパスを取得。  
ここのパスは別に cmd.exe である必要はなく、実行ファイルなら多分何でもいける。ここを差し替えれば別のソフトを起動するコードになる。

process.StartInfo にコマンドの実行にあたっての設定を行い、Arugment にコマンドを詰めて実行する。  
コマンドの先頭に `/c` が付いているが、これが付いている場合、実行直後にウィンドウが閉じる。  

ここの値は　`/c` or `/k` となるっぽい。`/k` を指定すると、コマンド実行後もコマンドプロンプトのウィンドウが閉じずに残る。  
今回のサンプルコードでは、最後に標準出力から値を取り出しているが、`/k` 指定時は、CMDのウィンドウを閉じるまで標準出力が VisualStudio の出力に表示されなかった。
このことから、コマンドプロンプトのクローズを待ってコマンドが実行されている模様。  

あまりウィンドウを残すような用途も思いつかないので、基本的は `/c` でOKだと思われる。  
尚、どちらも書かなかった場合は、何も書かれていないCMDが立ち上がった。この引数の指定は強制のようである。  
`/c` `/k` に関しては Process.Start メソッドのドキュメントに記述があった。

