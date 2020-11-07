# WMP,XAML に関するメモ

## コンテキストメニュー内からウィンドウのデータコンテキストにアクセス。
2020-11-07  
tag WPF, XAML, ContextMenu, ListView, DataContext

```
<ListView ItemsSource="{Binding DatabaseHelper.TodoList}" >

    <ListView.Resources>
        <ContextMenu x:Key="listViewContextMenu">
            <MenuItem Header="header text"
                        Command="{Binding Path=DataContext.Command,
                        RelativeSource={RelativeSource AncestorType=Window}}"
                        CommandParameter="{Binding ListViewItemProperty}"
                        />

        </ContextMenu>
    </ListView.Resources>

    <ListView.ItemContainerStyle>
        <Style TargetType="ListViewItem">
            <Setter Property="ContextMenu" Value="{StaticResource listViewContextMenu}"/>
        </Style>
    </ListView.ItemContainerStyle>
</ListView>
```

### やろうとしたこと
コンテキストメニューの項目をクリックで MainWindowViewModel 内のコマンドを実行する。

### 解説
当初は、ListVIew.ItemContainerStyle.ContextMenu に直接コンテキストメニューを記述。  
が、その方法ではデータコンテキストへアクセスできず、上記を実現することができなかった。  
コンテキストメニューは XAML のビジュアルツリーに含まれないため、ウィンドウやリストビューにアクセスすることができないのが原因っぽい。

なので、ListView.Resouces の上で ContextMenu を定義。そこからなら RelativeSouce を使えばデータコンテキストにアクセスできる。（ただ、どうしてそうなるのかはわからなかった。Resouces 上に定義した場合はビジュアルツリーに含まれているということだろうか？)

更に ListView.ItemContainerStyle の Setter ではリソースキーを参照して ContextMenu をセットする。  
ContextMenu と Setter が離れているのが少し納得行かないが、目的を達成。正直できただけで理解できていないのでメモしておく。

---