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