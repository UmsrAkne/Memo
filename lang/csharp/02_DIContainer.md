# DI コンテナ

C# の DI コンテナに関するメモ

Prism Blank App (WPF) でプロジェクトを作った際の DI コンテナの使い方

    public partial class App
    {
        protected override void RegisterTypes(IContainerRegistry containerRegistry)
        {
            containerRegistry.RegisterDialog<DetailPage, DetailPageViewModel>();

            IUnityContainer container = containerRegistry.GetContainer();

            // DI する対象が具象クラスである場合は RegisterType の必要はないかも？　(未検証)
            container.RegisterType(typeof(TodoDbContext));

            // 前述の RegisterType を削除しても Singleton に登録は可能。
            container.RegisterSingleton(typeof(TodoDbContext));
            container.RegisterSingleton(typeof(TodoLists));

            var dbContext = container.Resolve<TodoDbContext>();
        }
    }

DI コンテナに関するコードは、プロジェクトの "最初に" "確実に実行される" ような箇所に書くのがセオリー

よって、今回はプロジェクトルートの `App.RegisterTypes()` 内に記述している。

メソッド先頭の `RegisterDialog` はダイアログ（サブウィンドウを出すための記述）

次の `container = containerRegistry.GetContainer()` では、コンテナインスタンスを取得している。  
※ `GetContainer()` は使用するために `using` の追加が必要だった。  

ここから取得したコンテナに対して処理をしないと、その後の処理に結果が反映されない。  
従って `new UnityContainer()` 等のようにして生成しても意味がない。

`container.RegisterType()` はインターフェースに対して生成する具象クラスを指定する。  
最初から具象クラスの場合は（つまり例のようなケース）では必要ないかも。

`container.RegisterSingleton()` では、登録した型をシングルトンで生成するようにコンテナに指定する。  
これを指定することによって、同じインスタンスを複数のビューで使い回すといったことが簡単にできる。  
DIコンテナを使わない場合に比べて圧倒的にわかりやすくスマート。

`container.Resolve<Type>()` でコンテナからインスタンスを取得することもできる。  
例ではシングルトン指定がされているので、常に共通のインスタンスが取得できる。
