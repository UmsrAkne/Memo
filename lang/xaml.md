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