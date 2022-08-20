# Visual studio

## VS2022 で Prism Template pack

2019 ではあったのに拡張機能の検索結果にプリズムテンプレートパックがない。  
しかしインストールはできる（！？）  

ブラウザで Prism Template pack で検索。 Visual studio Marketplace のページが出る。  
vsix ファイルをダウンロードしたらそれをクリック。デフォルトで VS で開くようになっているため関連付けはいらなかった。  
VS の拡張機能マネージャーが開くのでそのままインストール。

これで使えるようになる。

## コードスニペット(C#)の格納場所

いつも忘れる VS のコードスニペットのアドレス。

    C:\Users\%USERNAME%\Documents\Visual Studio 2019\Code Snippets\Visual C#\My Code Snippets

---

## NUnit

元から入っている純正のフレームワーク(MSTest)以外のFWを使うことが可能。使い方は次の通り。

1. テストプロジェクトを作成する

ひとまず、MSTest をフレームワークに指定してテストを作成すれば良い。  
テストプロジェクト作成ダイアログのコンボボックスに項目を追加する方法もあるかも？　でも今回は使わない。

2. Nuget 経由で必要なパッケージを導入

必要なパッケージは２つ

    NUnit
    NUnit Test Adapter

両方バージョンが存在するので、適宜バージョンを合わせてインストールする。  
本当にテストしたいだけなら `NUnit` だけで良いと思われる。(未検証)  
しかし、`Visual Studio` のテストエクスプラーラーからテストを実行するためには `Test Adapter` が必要。

事実上必須。ないとやってられない。

3. テストファイルの using を書き換える

`using` から MS の項目を削除し、`NUnit.Framework` を追加。これをしないと `Assert` が衝突する。

以上。  

`NUnit` は属性として　`[Test]` や `[TestCase()]` を使用する。

最後に記述例を示す。(名前空間は省略)

    using NUnit.Framework;

    public class SoundProviderTests
    {
        [Test]
        public void テスト１()
        {
            // 単純はテストは [Test] で良い
        }

        [TestCase(true)]
        public void テスト２(bool isLoopPlay)
        {
            // 複数の検証値を使いたい場合は [TestCase] が便利
        }
    }

## 設定ファイルの移行

`VS 2022` で実施。

`メニューバー` -> `ツール` -> `設定のインポートとエクスポート` 

からエディター設定等の設定ファイルを出力、入力することができる。見つけるのに苦労した……。

## 改行コード

エディタの右下に改行コードの表示がある。(VS2022)  
VSでファイルを作成した場合はデフォルトで `CRLF` が改行コードとして採用されるみたい。
