# XAML のメモ

## Trigger

指定したプロパティの変更を察知して、ターゲットのプロパティを変更してくれる。便利だが書き方が冗長。以下が基本的な使用例

	<TextBlock>
		<TextBlock.Style>
			<Style TargetType="TextBlock">
				<Style.Triggers>

					<DataTrigger Binding="{Binding Editor.Saved}" Value="false">
						<Setter Property="Text" Value="(modified)" />
					</DataTrigger>

					<Trigger Property="Text" Value="t">
						<Setter Property="Foreground" Value="Red" />
					</Trigger>

				</Style.Triggers>
			</Style>
		</TextBlock.Style>
	</TextBlock>

Trigger は少なくとも２種類ある。  
単純なプロパティでの条件分岐には `Trigger` 、Binding を利用する場合は `DataTrigger` を使わなければならない。

`DataTrigger` の書式は `Binding="{Binding targetProperty}" っていう回りくどい書式になっているので注意

## Behavior, EventTrigger

Event 発生に対して処理をしたい場合は `Behavior` や `EventTrigger` が使える。

`Behavior` ではイベントハンドラを、`EventTrigger` ではコマンドをそれぞれ実行することができる。

	<Window
		xmlns:i="http://schemas.microsoft.com/expression/2010/interactivity"
		... >
		<-- namespace i を宣言 -->

		<-- Behavior-->
		<i:Interaction.Behaviors>
			<m:TextInputLimitBehavior />
		</i:Interaction.Behaviors>

		<-- Trigger-->
		<i:Interaction.Triggers>
			<i:EventTrigger EventName="SelectionChanged">
				<i:InvokeCommandAction Command="{Binding Command}" />
			</i:EventTrigger>
		</i:Interaction.Triggers>

## Image コントロール

画像を表示するためのコントロール。Source に画像ファイルのフルパスを入力することで利用できる。  
Source にパスをバインドすれば、パスの変化に合わせて画像が切り替わる。  
手軽に画像が表示できるが、幅と高さが正常に表示されない画像があった。  
(画像の情報で表示される画像サイズとコントロールに配置した際の画像のサイズが異なる。)

要点は Width, Height をバインドしている点。  
コントロール上で幅と高さを、`System.Drawing.Bitmap` 等として読み込んだ情報に合わせる。  

	<Image
		Canvas.Left="{Binding ImageLoader.CurrentImageFileA.X}"
		Canvas.Top="{Binding ImageLoader.CurrentImageFileA.Y}"
		Width="{Binding ImageLoader.CurrentImageFileA.Width}"
		Height="{Binding ImageLoader.CurrentImageFileA.Height}"
		Source="{Binding ImageLoader.CurrentImageFileA.FileInfo.FullName}"
		Visibility="{Binding DrawingA, Converter={StaticResource BooleanToVisibilityConverter}}">
		<Image.LayoutTransform>
			<TransformGroup>
				<ScaleTransform ScaleX="{Binding Scale}" ScaleY="{Binding Scale}" />
			</TransformGroup>
		</Image.LayoutTransform>
	</Image>

サンプルでは Canvas を使用しているがこれは、`Image` の `TranslateTransform` で `X`,`Y` を上手く設定できなかったため、上記のコードになっている。

## WPF, XAML でインテリセンス

既存の `Window` 要素に下記の属性を追記する。  
`xmlns:viewModels` , `ViewModel`, `projectName` に関しては適宜プロジェクトに合わせて名前を修正する。

	<Window 
			xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
			xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
			mc:Ignorable="d"
			xmlns:viewModels="clr-namespace:projectName.ViewModels"
			d:DataContext="{d:DesignInstance viewModels:MainWindowViewModel}"
			/>

また、`ControlTemplate` 配下でも通常インテリセンスは有効にならない。  
しかし、以下のように `Window` と同様に記述することで、型を指定し、インテリセンスを使うことができる。

型名を修飾する名前空間の宣言は省いてあるので、必要に応じて追加する。

	<ControlTemplate
		d:DataContext="{d:DesignInstance models:Folder}"
		mc:Ignorable="d"
		>

`DataTemplate` の場合は `DataType` プロパティを使用すれば良い。

## 型に対して適用したスタイルを継承する

Style をキー無しで宣言した場合、型に対してデフォルトでセッターを適用できる。  
この状態のスタイルを継承するには以下のようにする。

	<Style BasedOn="{StaticResource {x:Type TextBox}}" TargetType="TextBox" />
	<!-- 型が TextBox の場合 -->

## 基本型をリソースで定義する

	<!-- トップレベルにて xmlns を宣言する -->
	xmlns:system="clr-namespace:System;assembly=System.Runtime"

	<!-- Double を使った例>
	<system:Double x:Key="DateTimeColumnWidth">110.0</system:Double>

## キャスト（型注釈）

	<MenuItem
		Command="{Binding RelativeSource={RelativeSource AncestorType=ContextMenu},
						  Path=PlacementTarget.(ListViewItem.Tag).(viewModels:MainWindowViewModel.StartTodoCommand)}"

		CommandParameter="{Binding RelativeSource={RelativeSource AncestorType=ContextMenu},
						  Path=PlacementTarget.(ListViewItem.Content)}"
		/>

なんらかの理由で、IDEが型が認識しない場合や、確定ができない場合、

	(TypeName.Property)
	(NameSpace:TypeName.Property)

この書式を使うことで、型を明示することができる。  
名前空間の宣言はその場で行えば問題ない。
