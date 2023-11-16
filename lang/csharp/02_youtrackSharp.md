# YoutrackSharp

C# で Youtrack にアクセスするためのパッケージ。

## 環境

`.net 6.0` `YoutrackSharp 2022.3.1` `Youtrack インスタンスを作成済み`

## 使い方

### アクセスキーの生成

最初にアクセスキーを生成、取得する。

    Youtrack のブラウザ画面 -> 右上のネジマーク -> アクセス管理 -> ユーザー

画面が切り替わるので、アクセスキーを生成できるユーザーを選択する。（私の場合は自分のアカウント）

    上部のタブ -> アカウントのセキュリティ -> 新規トークンボタン

権限に関しては `Youtrack administrator` をセットしないとプロジェクトの読み込みができない。

### コーディング

アクセスキーを取得したら実際にコーディングする。

以下のコードは公式リポジトリから写した、プロジェクトのリストを取得するコード。

    var connection = new BearerTokenConnection("https://ytsharp.myjetbrains.com/youtrack/", "perm:xxx...");

    // youtrack cloud の場合は URL のフォーマットが異なるが問題ない。  
    // この場合 URL は https://username.youtrack.cloud/ のようになるが、これをこのまま入力すれば良い。

    // 第二引数として、アクセスキーの入力が必要だが、このアクセスキーは `perm:` の部分まで含めて一塊となっている。  
    // アクセスキー生成時にコピーされる内容をそのまま貼り付ければ良い。

    var projectsService = connection.CreateProjectsService();
    var projectsForCurrentUser = await projectsService.GetAccessibleProjects();
    var projects = projectsForCurrentUser.ToList();

