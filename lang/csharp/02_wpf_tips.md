# WPF に関するメモ-2

## Xaml 関連

### 初期フォーカスを設定する

    <StackPanel FocusManager.FocusedElement="{Binding ElementName=firstButton}">
      <Button Name="firstButton" />
    </StackPanel>

基本的にこれで。

因みに、`DialogService` を使ってウィンドウを開いた際に、`Page` の属性として `FocusManager...` を記述しても反応しない。

この場合、`Page` ではなく、一段下の `Grid` とかに仕掛けると反応してくれる。なぜ？

`MainWindow` で使う場合は、`Window` の属性で記述しても大丈夫だと思う。

---

### Enum の値を属性値に記述する

dialogs は任意の名前空間名。

    <Button CommandParameter="{x:Static dialogs:ButtonResult.OK}" />

省略せずに記述すると以下。長い。

    <Button>
        <Button.CommandParameter>
            <dialogs:ButtonResult>
                OK
            </dialogs:ButtonResult>
        </Button.CommandParameter>
    </Button>

---

### StringFormat

固定値の桁数合わせだけならコンバーターは不要。以下3桁の例。

    <TextBlock Text="{Binding Value, StringFormat=商品A {0:D3} 円}"/>
    
`StringFormat={0:D3}` という表記は不可。フォーマット指定子よりも前に `{}` を入れる必要がある。

    <TextBlock Text="{Binding Value, StringFormat={}{0:D3} 円}"/>

下記の場合はそのままの数値を表示する。

    <TextBlock Text="{Binding Value, StringFormat=商品A {0} 円}"/>

---

### ItemsControl を横並びにする

    <ListBox>
        <ListBox.ItemsPanel>
            <ItemsPanelTemplate>
                <StackPanel Orientation="Horizontal"/>
            </ItemsPanelTemplate>
        </ListBox.ItemsPanel>
    </ListBox>

並び方の変更は `ItemsPresenter` 的なものは不要。
