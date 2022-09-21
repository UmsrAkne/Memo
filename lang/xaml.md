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
`xmlns:local` , `ViewModel` に関しては適宜プロジェクトに合わせて名前を修正する。

	<Window xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
			xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
			xmlns:local="clr-namespace:WpfTest"
			mc:Ignorable="d"
			d:DataContext="{d:DesignInstance local:ViewModel}"
			/>
