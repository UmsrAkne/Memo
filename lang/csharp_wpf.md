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

## 一つのウィンドウを分割表示する
tag WPF, XAML, GridSplitter, 分割

```
<Grid>
    <GridSplitter Grid.Row="3"
                    Height="3"
                    HorizontalAlignment="Stretch"
                    />
</Grid>
```

### 解説
ウィンドウを分割したい。あとユーザー側で分割の表示をドラッグで変えられるようにしたい場合、 <Grid> タグで <GridSplitter> を挟み込んで使用する。  
注意点として、このスプリッター単体でグリッドのスペースを一つ専有する。既にコントロールが存在するスペースに置くと中央で重なってしまう。  
あと HorizontalAlignment="Stretch" は必須。無いとスプリッターを摘むことができない。

---

## ListView のマウスオーバー時の効果を消す

```
    <Style x:Key="ListViewItemContainerStyle" TargetType="ListViewItem">
        <Setter Property="SnapsToDevicePixels" Value="true" />
        <Setter Property="OverridesDefaultStyle" Value="true" />
        <Setter Property="Template">
            <Setter.Value>
                <ControlTemplate TargetType="ListViewItem">
                    <Border x:Name="Border"
                            SnapsToDevicePixels="true"
                            BorderBrush="Transparent"
                            BorderThickness="1"
                            Background="{TemplateBinding Background}" >

                        <VisualStateManager.VisualStateGroups>
                            <VisualStateGroup x:Name="CommonStates">
                                <VisualState x:Name="Normal" />
                                <VisualState x:Name="MouseOver" />
                                <VisualState x:Name="Disabled" />
                            </VisualStateGroup>

                            <VisualStateGroup x:Name="SelectionStates">
                                <VisualState x:Name="Unselected" />
                                <VisualState x:Name="Selected">
                                    <Storyboard>
                                        <ColorAnimationUsingKeyFrames Storyboard.TargetName="Border"
                                                Storyboard.TargetProperty="(Border.BorderBrush).(SolidColorBrush.Color)">
                                            <EasingColorKeyFrame KeyTime="0:0:0.3"
                                         Value="Blue" />
                                        </ColorAnimationUsingKeyFrames>
                                    </Storyboard>
                                </VisualState>

                                <VisualState x:Name="SelectedUnfocused">
                                    <Storyboard>
                                        <ColorAnimationUsingKeyFrames Storyboard.TargetName="Border"
                                                Storyboard.TargetProperty="(Border.BorderBrush).(SolidColorBrush.Color)">
                                            <EasingColorKeyFrame KeyTime="0"
                                         Value="LightBlue" />
                                        </ColorAnimationUsingKeyFrames>
                                    </Storyboard>
                                </VisualState>
                            </VisualStateGroup>
                        </VisualStateManager.VisualStateGroups>
                        <ContentPresenter/>
                    </Border>
                </ControlTemplate>
            </Setter.Value>
        </Setter>
    </Style>
```

### 解説
いつもやるやつ。MSDNからコピってちょっとイジったやつ。上記をリストビューのスタイルに適用すれば可。

### 備考
`<ControlTemplate>` 直下の `<Border.Background>` が {TemplateBinding} となっているが、これはテンプレートバインディングでないとダメ。無指定でも、直接色指定でも不可。これをしないとスタイルを適用したリストビューのリストビューアイテムの背景色を設定することができない。

```
<Storyboard>
    <ColorAnimationUsingKeyFrames Storyboard.TargetName="Border"
            Storyboard.TargetProperty="(Border.BorderBrush).(SolidColorBrush.Color)">
        <EasingColorKeyFrame KeyTime="0:0:0.3"
        Value="Blue" />
    </ColorAnimationUsingKeyFrames>
</Storyboard>
```

### アニメーションに関して
上記の `<StoryBoard>` に関しても記述しておく。  
まず、`<ColorAnimationUsingKeyFrames>` は複数種あるアニメーションの一つで、名前の通り色変更を行う。  
TargetName="Border" は対象オブジェクトを表すが、ここで言う Border というのは x:Name="Border" のことで型名ではない。

次に、TargetProperty="(Borader.BorderBrush).(SolidColorBrush.Color)" は変更の対象となるプロパティを指定している。完全に理解しているわけではないが、  

`(Border.BorderBrush) は Border 型の BorderBrush プロパティ,`

`(SolidColorBrush.Color) は SolidColorBrush 型の Color プロパティ`

をそれぞれ表現しているっぽい。ただ、この妙な記述方法の意味はちょっとわからない。
最後に色変更のアニメなので、`Value` には普通に色を表す文字列を入れればOK

### その他
今回、上記をやろうとして、定義済みの ItemContainerStyle と被ってうまくスタイルが適用されなかった。未だにこの辺の階層関係がいまいち理解できない。複雑すぎない？

---
## Resouce を読み込む
tag, WPF, XAML, Resource, ResourceDictionary

リソースファイル（VisualStudioに生成してもらうやつ)

```
<ResourceDictionary xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
                    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
                    >
    <Style x:Key="ListViewItemContainerStyle" TargetType="ListViewItem">
        <Setter Property="SnapsToDevicePixels" Value="true" />
        <Setter Property="OverridesDefaultStyle" Value="true" />
    </Style>
</ResouceDictionary>
```

リソースを使う側の記述

```
<Window.Resources>
    <ResourceDictionary>
        <ResourceDictionary.MergedDictionaries>
            <ResourceDictionary Source="../res/Dictionary1.xaml"/>
        </ResourceDictionary.MergedDictionaries>
    </ResourceDictionary>
</Window.Resources>
```

今回、Source に書くパスは相対指定にしてある。1階層上にある別のディレクトリを指定。  
一応、Windowのリソースに書いてあるけど、多分他の要素のリソースにも書けると思われる（未検証）

---
## 特定のコントロールにデータコンテキストを適用する。
tag DataContext, Style, Setter

現状の設計方法だと、MainWindowViewModel （もしくはそこに乗っているモデル）のコードがガンガン増えていくため、ちょっと良くないなと思い検証。
機能が多くなりそうなコントロールのビューモデルを Window のビューモデルから分離出来ないかと考えた。

### xaml.cs 一部抜粋
```
<Window xmlns:vm="clr-namespace:BlankApp1.ViewModels">

<ListView>
    <ListView.Style>
        <Style TargetType="ListView">
            <Style.Setters>

                <Setter Property="DataContext">
                    <Setter.Value>
                        <vm:ListViewModel/>
                    </Setter.Value>
                </Setter>

                <Setter Property="ItemsSource" Value="{Binding Strings}"/>

            </Style.Setters>
        </Style>
    </ListView.Style>

</ListView>
```

### ListViewModel.cs
```
namespace BlankApp1.ViewModels {
    class ListViewModel : BindableBase{
        private List<string> strings = new List<String>();
        public List<string> Strings {
            get => strings;
            set => SetProperty(ref strings, value);
        }
    }
}
```

### 解説
ListView の DataContext に、Window で定義されているデータコンテキストとは別のデータコンテキスト(ListViewModel)を適用するサンプル。

最初、ListView 要素内で直接 ItemsSource="{Binding Strings}" し、ListView.DataContext 要素内に vm:ListViewModel 要素でデータコンテキストを代入というやり方をしたところ、バインディングエラーが発生。見た目上は動作するが、エラーが発生している以上、正道ではなさそう。  

これは推測だが、ListView が作成された時点では、データコンテキストは Window.DataContext を参照しているためだと思われる。  
なので、少しやり方を変えて、Style.Setters を使用し、ListView が作成されてから DataContext, ItemsSource をセットする方式に変更することでエラーを消した。その時のソースが上記となっている。

もっとスマートなやり方があればそちらを使いたいがちょっと思いつかない。

---

## アイコンを変更する

タグ アイコン, 画像, Icon

### wpf アプリケーションのアイコンを自分で用意したアイコンファイルに差し替える。
まず、任意のプロジェクトに画像を格納するためのフォルダを作成する。今回は "Images" フォルダを作成。
画像ファイルのプロパティは
```
ビルドアクション : Resource
出力ディレクトリにコピー : コピーしない
```

以上で設定。ひとまず今回はこれで使用できた。

MainWindow.xaml は Icon プロパティに値を入力
```
<Window Icon="{Binding IconImagePath}" >
```

ファイル名は、プロジェクト内に画像ファイルを入れてあれば以下で可能。 png の透過はデフォルトで有効。
```
/Images/fileName.png
```

## TextBox.CaretIndex

テキストの入力に便利なテキストボックスだが、CaretIndex が依存関係プロパティではないためバインディングできない。  
キャレットの位置を変更したい場合はコードビハインドかビヘイビアあたりを使って実装することになりそう。

---

## WPF アプリで別ウィンドウ

設定ウィンドウとか、警告ダイアログとか、基本のウィンドウ以外のウィンドウを出す際のメモ。

### 前提

    Prism を導入済み

### 必要なこと。概要

    1.MainWindowViewModel のコンストラクタの引数に IDialogService を入力する。 (Prism.Service.Dialogs)
    2.新規で表示したいウィンドウのファイルと、そのビューモデルを作成する。(ウィンドウではなくページで作る)
    3.新規作成したビューモデルに IDialogAware を実装 (Prism.Service.Dialogs)
    4.App.xaml.cs.RegisterType(IContaineRegsiter containerRegister) を書き換える。
    5.MainWindowViewModel 上で、dialogService.ShowDialog() を実行する。

### 詳細

(1) MainWindowViewModel のコンストラクタを書き換える。  
これによりプログラム実行時には、自動で `IDialogService` オブジェクトが渡されようになる。  
それをフィールドとかに代入して保持しておく。これを(5)で使う。

(2) が、ウィンドウではなくページなのは、ウィンドウの子としてウィンドウを持つことができないため。  
できなくはなさそうだが、がんばってやる理由もないので、ページを使用する。

(3) 普通に実装する。実装したメソッドはダイアログが開いた際に実行される。  
デフォルトで未実装例外のスローが記載されている場合は注意。

(4)

    containerRegistry.RegisterDialog<WindowClass, WindowViewModelClass>();

のような感じで追記する 。(3) をやっていない場合は型不一致になる。

(5)

    dialogService.ShowDialog(nameof(WindowClass), param, (IDialogResult result) => { });

ViewModel 中のプロパティなりメソッドなりでこれを実行すれば晴れて新規ウィンドウが表示される。
