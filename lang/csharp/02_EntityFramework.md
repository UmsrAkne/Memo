# EntityFramework の使用

## 概要

DB をいい感じに利用できるようにする EntityFramework を使用するサンプル。(sqlite)

## 導入

まずは、Nuget からパッケージをインストールする。今回は SQlite を使用するので `.Sqlite` もインストールする。  
こちらを入れておかないと、後に出てくる拡張メソッドが使用できない。

	Microsoft.EntityFrameworkCore 
	Microsoft.EntityFrameworkCore.Sqlite
	System.Data.Sqlite

`EntityFrameworkCore` と `EntityFrameworkCore.Sqlite` はそれぞれ導入する。  
`.Sqlite` の方をインストールしたら `Core`  の方もくっついて来そうに見えるがそんなことはない。

テーブルとして作成するデータを定義する。
プロパティの上部についている `[Key]` などの文字列はアノテーション。下記 using 宣言が必要になる。

	using System.ComponentModel.DataAnnotations;
	using System.ComponentModel.DataAnnotations.Schema;

下記のサンプルでは、`Id` を主キーに設定。カラム名を `id` に設定している。

	namespace PracticeEntityFW.Models
	{
		using System.ComponentModel.DataAnnotations;
		using System.ComponentModel.DataAnnotations.Schema;

		public class Person
		{
			[Key] // ID などの名前のプロパティがあるときは自動で主キーに指定されるらしい。
			[Required]
			[Column("id")]
			public int Id { get; set; }

			public string Name { get; set; }
		}
	}
	
コンテキストを作成する。あまり理解してないけど、DB との橋渡し的な役割のクラス？  
理解したら追記する。

特に重要な部分は `OnConfiguring()` の中身。ここに SQLite のデータベース作成のコードを記述する。  

SQLite 以外を使う場合は当然別のコードを書くことになると思われる。  
因みに、`UseSqlite() `は `EntityFrameworkCore.Sqlite` を using しないと使えない拡張メソッドらしい。

特筆するようなことでもないが、接続文字列に関しては、 `EFWTest.sqlite` の名称でDBファイルを生成しているだけ。
	
	namespace PracticeEntityFW.Models
	{
		using Microsoft.EntityFrameworkCore.Sqlite;
		using Microsoft.EntityFrameworkCore;
		using System.Data.SQLite;
		using System.IO;
		using Microsoft.Data.Sqlite;

		public class PersonDbContext : DbContext
		{
			public DbSet<Person> Persons { get; set; }

			protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
			{
				if(!File.Exists()){
					SQLiteConnection.CreateFile("EFWTest.sqlite"); // ファイルが存在している場合は問答無用で上書き。
				}

				var connectionString = new SqliteConnectionStringBuilder { DataSource = @"EFWTest.sqlite" }.ToString();
				optionsBuilder.UseSqlite(new SQLiteConnection(connectionString));
			}
		}
	}

最後に MainWindowViewModel のコンストラクタで DB ファイルを生成する。  
正しく進んでいれば `Person` で指定したテーブル構造を持った DB ファイルが生成される。

SQLite では必要ないかもしれないが、 EnsureCreated() は非同期版もあるっぽい。

	namespace PracticeEntityFW.ViewModels
	{
		using PracticeEntityFW.Models;
		using Prism.Mvvm;

		public class MainWindowViewModel : BindableBase
		{
			public MainWindowViewModel()
			{
				using (var db = new PersonDbContext())
				{
					db.Database.EnsureCreated();
				}
			}
		}
	}

## 導入時に遭遇した色々

導入した際にトラブルまみれだったのでその辺のメモ。

まずテストプロジェクトでビルド時にエラーが出る。細かい内容は覚えてないが、大体意味がわからないやつ。  
dll が見つからないけど大丈夫？ みたいな文言が並ぶ。

いくつかの例外に関しては、導入するバージョンを下げることで解決。かなり古いバージョン `2.0.0` でなら無事に実行できることが判明。  
テストの方で使わなくていいなら、`3.3` くらいまで上げても大丈夫そう？  
力技だしあまり良くない方針な気はする。

最終的には `EntityFrameworkCore.Sqlite` の方だけを、プロジェクトとテストプロジェクトの両方にインストールした。  
試す気力はなかったけど、`EntityFrameworkCore` のパッケージも入っているとまずいような気がした。

テストの方にパッケージを導入しなかった場合、初回のビルド時のみ `e_sqlite3` が見つからないとかでコケる。どうしてなのか……。  
テスト側にもパッケージをインストールすることで上記が解決したみたい。

# Insert

導入が済んだので、次はデータの挿入を行う。データの操作は DbContext クラスを経由して行う。  
具体的な記述は以下。

	using (var db = new PersonDbContext())
	{
		db.Persons.Add(new Person() { Id = 10, Name = "名前" });
		db.SaveChanges();
	}

上述で DbContext クラスに宣言したリストにデータを挿入する。  
但し、`db.Persons.Add()` を実行した段階ではまだデータベースにデータは挿入されない。  
`db.SaveChanges()` を実行した際に初めて変更が適用される。

Add() されたオブジェクトは `db` に保持されることになる。  
大量にオブジェクトを挿入する場合は、SaveChanges() をコールしてオブジェクトは破棄するといった対応が必要。

# Select 

データを読み出すのに特別な操作は必要ない。下記のようにすれば全てのデータを取り出すことができる。

	using (var db = new PersonDbContext())
	{
		foreach (var p in db.Persons)
		{
			System.Diagnostics.Debug.WriteLine(p.Name);
		}
	}

条件を指定してデータを取り出すような場合は Linq で。  
以下は Where で条件に合致するデータのリストを取得している。

	using (var db = new PersonDbContext())
	{
		var list = db.Persons.Where(p => p.Id == 11).ToList();
	}

## Update

データの更新は、取り出したオブジェクトのプロパティを書き換える。  
`SaveChanges` を実行しなければ変更が確定されないので注意。

	using (var db = new PersonDbContext())
	{
		foreach (var p in db.Persons)
		{
			p.prop = xxx;
		}
		
		SaveChanges();
	}
	

## Delete

データの削除は、データを格納しているリストの `DbSet` の `Remove(item)`, `RemoveAt(index)` を呼び出す。  
まとめて削除する Range つきのメソッドも提供されている。`SaveChanges()` の呼び出しを忘れないように。

## アノテーション

	using System.ComponentModel.DataAnnotations;
	[Key]				// 主キー
	[Required]			// 付記されたプロパティを必須にする。NotNull
	[Column("id")]		// カラム名は通常プロパティ名が採用されるが、自分で指定する際に使用する。
	
	using System.ComponentModel.DataAnnotations.Schema;
	[NotMapped] // アノテーションが付記されたプロパティをデータベースが無視する。

## Inmemory DB の使用

どうにかして DBContext クラスのテストをやろうとした結果、InmemoryDB ならテストが行えそうな気がした。

DBContext を継承したクラスに以下のようなコンストラクタと DbSet を定義する。  
OnCofiguring は後ほどの説明のために記述に含める。

    public class TextDBContext : DbContext
    {
        public TextDBContext(DbContextOptions<TextDBContext> options) : base(options)
        {
        }

        protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        {
        }
    }

テストプロジェクトの方には以下のようなコードを記述する。  

`DbContextOptionBuilder<T>` 使用するためには `Microsoft.EntityFrameworkCore` パッケージの導入が必要。  
また、`UseInMemoryDatabase(arg)` を使用するためには、`Microsoft.EntityFrameworkCore.Inmemory` の導入が必要。

	[TestMethod()]
	public void AddTitleTest()
	{
		var option = new DbContextOptionsBuilder<TextDBContext>()
		.UseInMemoryDatabase(databaseName: "MyMemDb")
		.Options;

		var db = new TextDBContext(option);
		db.Database.EnsureCreated();
	}

テストで `EnsureCreated()` を実行した際、`OnConfiguring(arg)` が実行されるが、ここに DB ファイルの生成コードが記述されている場合、テストの実行に失敗する……。

## EF で PostgreSQL を使う

`Nuget` でインストールするパッケージ

	Microsoft.EntityFrameworkCore
	Npgsql.EntityFrameworkCore.PostgreSQL

DB のレコードを表すクラスを作成

	using System.ComponentModel.DataAnnotations;

	public class Comment
	{
		[Key]
		[Required]
		public int Id { get; set; }

		public string Text { get; set; }
	}

DBコンテキストクラスを作成する。以下の `using` が必要となる。  
接続に必要な情報は `OnConfiguring` の中に記述する。

    using Microsoft.EntityFrameworkCore;
    using Npgsql;

    public class CommentDbContext : DbContext
    {
        public DbSet<Comment> Comments { get; set; }

        protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        {
            NpgsqlConnectionStringBuilder builder = new NpgsqlConnectionStringBuilder();

            builder.Port = 5433;
            builder.Username = "postgres";
            builder.Password = "passw0rd";
            builder.Host = "localhost";
            builder.Database = "testdb";

            optionsBuilder.UseNpgsql(builder.ToString());
        }
    }

任意の場所でコンテキストクラスを `new` して `EnsureCreated()` を呼び出す。

	public MainWindowViewModel()
	{
		var context = new CommentDbContext();
		context.Database.EnsureCreated();
	}

### DateTime が使えない問題

いくつか対策があるようだが、ひとまずメモしておく。

`DbContext` のコンストラクタに以下の処理を記述する。

	System.AppContext.SetSwitch("Npgsql.EnableLegacyTimestampBehavior", true);

`System.AppContext` に関して

	アプリケーションのコンテキストについてのデータを設定したり取得したりするためのメンバーを提供します。

よくわからない。アプリのコアな部分の設定とか切り替えるクラスなのかな？

## 接続失敗後に再接続できない

`ConnectionStringBuilder` 等で接続を試行した際、その後同じ `DbContext` インスタンスを使用して再接続できなかった。

`DbContext.Database.EnsureCreated()` の初回実行時に以下のメソッドが実行される。

	protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
	{
		optionsBuilder.UseNpgsql(ConnectionStringBuilder.ToString());
	}

一回目の実行で `ConnectionStruiBuilder` に不正な内容が含まれていた場合、当然接続に失敗する。ここまでは良い。  
しかし、再度正しい接続情報を `ConnectionStruiBuilder` に入力し、`optionBuilder.UserNpgsql` を実行しても接続できなかった。

* `EnsureCreated()`
* `OnConfiguring()`
* `UseNpgsql()`

いずれのメソッドを実行してみても再接続はできなかった。  
今回は `DbContext` をシングルトンで運用していたため、できれば再生成は避けたかったのだが。

`DbContext` インスタンスを生成し直して最初から設定をやり直せば問題はないけど、何かやり方が間違っていた？
