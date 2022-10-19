# Linq

C# のリストに対する操作を行う機能。実態は拡張メソッドらしい。

## GroupBy

    var groupingTags = tags.GroupBy(t => t.CommentId);

その名の通り、リストのグルーピングを行う。上記例では `t.CommentId` を基準としてグルーピングしている。  

わかりにくいが下記のような感じになる。

    // befor
    commentId,  tag 
    1,          one
    3,          three1
    2,          two1
    3,          three2
    2,          two2

    // after
    commentId,  tag 

    // key 1
    1,          one

    // key 2
    2,          two1
    2,          two2

    // key 3
    3,          three1
    3,          three2

つまり二重リストが返ってくる。

`foreach(var tagGroup in groupingTags)` のようにした場合、`tagGroup` は `Key` プロパティを持っている。

更にループをネストして `foreach(var tag in tagGroup)` とすると、`Key` 毎に分類された `tag` にアクセスすることができる。

## Join

要するに内部結合

    public DbSet<Tag> Tags { get; set; }

    public DbSet<TagMap> TagMaps { get; set; }

    var tags = TagMaps.Join(
        Tags,
        tm => tm.TagId,
        t => t.Id,
        (tm, t) => new { tm.Id, tm.CommentId, t.Name, });

`TagMaps` と `Tags` の２つのテーブル(リスト)を内部結合している。  
第一引数として結合するリストを指定する。

上記では `TagMaps.TagId`, `Tags.Id` をキーとして結合を行っている。

`(tm, t)` の行で、両方のテーブルのデータを併せ持った匿名型を生成している。

内部結合なので、結合側と被結合側の両方が同じキーを持っているもののみが結果のリストに含まれる。

片方しかキーを持っていないものも含めて欲しい場合は外部結合を使用するみたいだが、今回は省略。

## Take

先頭から指定数の要素を取り出す

    List<int> list = Enumerable.Range(0,100).ToList();

    list = list.Take(10).ToList();
    // 0 1 2 3 4 5 6 7 8 9

## Skip

先頭から指定数スキップして要素を取り出す

    List<int> list = Enumerable.Range(0,100).ToList();

    list = list.Skip(10).ToList();
    //10 11 12 13 14 15 16 17 18 19 20 21 ...
