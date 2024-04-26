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

### パターンにマッチするか確認する

マッチしているかどうかを判定できれば良いのであれば、静的メソッドの `Regex.IsMatch(string input, string pattern)` を使える。

    // 第一引数にチェックを行う文字列、第二引数に正規表現パターンを入力する。
    bool isMatch = Regex.IsMatch(input, pattern);

デバッグ中の文字列を VS で確認すると `\d` が `\\d` のようにバックスラッシュがエスケープされているように表示されるが問題ない。  
そのまま入力すれば正規表現として解釈してくれる。

### 特殊文字

記号をエスケープする場合は以下。バックスラッシュを２つ並べて使用する。１つではダメ。

    "\\["
    "\\*"

空白文字、タブ、改行、全角・半角スペースにマッチ

    "\s"
    "\S" // 上記以外の全ての文字にマッチ

文字列の先頭と末尾にマッチ。  
テキストエディタ感覚でいると勘違いするが、 改行の前後にマッチするわけではない。  
よって、複数行の文字列であっても一致するのは各１回ずつ。

    "^" // 先頭
    "$" // 末尾

直前の文字の n回以上の繰り返し

    "a*" // a が 0回以上の繰り返しにマッチ
    "a+" // a が 1回以上の繰り返しにマッチ
    "a?" // a が 0 or 1回以上の繰り返しにマッチ

Regex.Replace 等で置き換えるときとかに使うグループ化。  
指定範囲をグループとして、置き換え後の文字列に使用することができる。

グループ化した文字列は $1 から連番が割り当てられる。 $0 はマッチした文字列全体を表す。

    string r = Regex.Replace("abcdef", "(abc)(def)", "$2, $1, $0");
    Console.WriteLine(r);
    // def, abc, abcdef

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

## 文字列補完

C# の機能。`$` のあとに続くリテラル内で変数等をそのまま使うことができる。  
下記では `variable` の部分では変数の値が入力され、 `method()` ではメソッドの戻り値が入る。`strings` の場所には通常通り文字列が入る。

    $"{variable}{method()}, string"

## 画像ファイルのメタデータの読み込み

AIで生成したイラストに埋め込まれているメタデータを読み取る。

    try
    {
        var frame = BitmapFrame.Create(new Uri(FileInfo.FullName));

        if (frame.Metadata == null)
        {
            return metaData;
        }

        var m = (BitmapMetadata)frame.Metadata;

        if (m.Any(query => query.StartsWith("/tEXt")))
        {
            metaData = new ImageDetail(m.GetQuery("/tEXt/{str=parameters}") as string);
            return metaData;
        }
    }
    catch (Exception e)
    {
        Debug.WriteLine(e);
        Debug.WriteLine("画像の読み込みに失敗しました");
        throw;
    }

プロジェクトのコードを直貼りしているので、余計なコードが入っているが、チェックするべき部分は多くない。

まず `BitmapFrame.Create` で画像を読み取る。このクラスはコンストラクタが `public` ではないため `new()` はできない。

画像を読み取れるクラスとして `WPF` では、`System.Windows.Media.Imaging.BitmapImage` があるが、こちらでは `Metadata` プロパティがサポートされておらず、`NotSupportedException` がスローされるため不可。

その後、`Metadata` が含まれるかを確認し、`BitmapMetadata` にキャストして `m` に格納する。

`m.GetQuery()` するとメタデータを取得できる。サンプルコードでは string にキャストしている。

### 備考

詳しくは確認していないが、メタデータはディレクトリのような構造でアクセスするらしい。

`m.GetQuery("/tEXt")` とすると `{str=parameters}` というデータが取得できる。
  そして更にこの中に生成に使った各種パラメーターの文字列が格納されている。サンプルコードでもこれにアクセスしてデータを取得している。

## その他のメモ

C# では改行コード `"\r"`, `"\n"` は各自１文字扱い。例えば、`"test\r\n".Length` の戻り価は `6` となる。
