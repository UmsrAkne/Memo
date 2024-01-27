# IDisposable を実装する

System.Timers.Timer 等の Disposable なフィールドやプロパティを宣言すると IDE から警告が出る。

警告が出たクラスには IDisposable を実装する。

    public class MainWindowViewModel : BindableBase, IDisposable

ただし、実装した（空のメソッド）だけではダメで、以下のように中身も実装する。

    public void Dispose()
    {
        Dispose(true);
        GC.SuppressFinalize(this);
    }

    protected virtual void Dispose(bool disposing)
    {
        timer.Dispose();
    }

`protected virtual void` の方でリソースの Dispose() を実行。 `public void` の方から 呼び出す。

正しいかどうかは知らんけど上記で警告は消える。

自分が開発するアプリの規模だと、まぁ気にする必要はなさそうだけど、警告が出るのは気分が良くないので対処しておく。