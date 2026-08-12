---
title: 'XAML animations'
description: XAML でアニメーションを記述するサンプル集です。
date: '2026-08-13 01:02:11'
tags: 
  - csharp
  - xaml
  - wpf
---

### 処理概要
 - 完全カスタムの `ToggleButton` 
 - データトリガーにより、背景色と枠線の太さがアニメーションで変化する。
 - ラベル、コマンド、チェック状態を依存関係プロパティでバインディングしている。
 - ボーダーの外側にドロップシャドウを付加。
 - 内部に表示する文字列を背景色に馴染むようにステータスで切り替え（アニメーションなし）

#### CustomDesignToggleButton.xaml.cs
```
<UserControl
    x:Class="WebFeedReader.Controls.CustomDesignToggleButton"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    x:Name="Root"
    d:DesignHeight="70"
    d:DesignWidth="200"
    mc:Ignorable="d">

    <ToggleButton
        Command="{Binding ElementName=Root, Path=Command}"
        Cursor="Hand"
        IsChecked="{Binding ElementName=Root, Path=IsChecked}">
        <ToggleButton.Style>
            <Style TargetType="ToggleButton">
                <Setter Property="Template">
                    <Setter.Value>
                        <ControlTemplate TargetType="ToggleButton">
                            <Border
                                x:Name="ToggleButtonBorder"
                                Background="DarkSlateGray"
                                BorderThickness="1"
                                CornerRadius="6">
                                <Border.Effect>
                                    <DropShadowEffect
                                        BlurRadius="5"
                                        Direction="270"
                                        Opacity="0.2"
                                        ShadowDepth="2"
                                        Color="Black" />
                                </Border.Effect>

                                <ContentPresenter HorizontalAlignment="Center" VerticalAlignment="Center">
                                    <ContentPresenter.Content>
                                        <TextBlock
                                            FontSize="15"
                                            FontWeight="SemiBold"
                                            Text="{Binding ElementName=Root, Path=Label}">
                                            <TextBlock.Style>
                                                <Style TargetType="TextBlock">
                                                    <Style.Triggers>
                                                        <DataTrigger Binding="{Binding ElementName=Root, Path=IsChecked}" Value="False">
                                                            <Setter Property="Foreground" Value="LightSteelBlue" />
                                                        </DataTrigger>

                                                        <DataTrigger Binding="{Binding ElementName=Root, Path=IsChecked}" Value="True">
                                                            <Setter Property="Foreground" Value="White" />
                                                        </DataTrigger>
                                                    </Style.Triggers>
                                                </Style>
                                            </TextBlock.Style>
                                        </TextBlock>
                                    </ContentPresenter.Content>
                                </ContentPresenter>
                                <Border.Style>
                                    <Style TargetType="Border">
                                        <Setter Property="BorderBrush" Value="SeaGreen" />
                                    </Style>
                                </Border.Style>
                            </Border>

                            <ControlTemplate.Triggers>
                                <Trigger Property="IsMouseOver" Value="True">
                                    <Trigger.EnterActions>
                                        <BeginStoryboard>
                                            <Storyboard>
                                                <ColorAnimation
                                                    Storyboard.TargetName="ToggleButtonBorder"
                                                    Storyboard.TargetProperty="(Border.BorderBrush).(SolidColorBrush.Color)"
                                                    To="LightGreen"
                                                    Duration="0:0:0.2" />

                                                <ThicknessAnimation
                                                    Storyboard.TargetName="ToggleButtonBorder"
                                                    Storyboard.TargetProperty="BorderThickness"
                                                    To="3"
                                                    Duration="0:0:0.2" />

                                            </Storyboard>
                                        </BeginStoryboard>
                                    </Trigger.EnterActions>

                                    <Trigger.ExitActions>
                                        <BeginStoryboard>
                                            <Storyboard>
                                                <ColorAnimation
                                                    Storyboard.TargetName="ToggleButtonBorder"
                                                    Storyboard.TargetProperty="(Border.BorderBrush).(SolidColorBrush.Color)"
                                                    To="LightSeaGreen"
                                                    Duration="0:0:0.2" />

                                                <ThicknessAnimation
                                                    Storyboard.TargetName="ToggleButtonBorder"
                                                    Storyboard.TargetProperty="BorderThickness"
                                                    To="0"
                                                    Duration="0:0:0.2" />
                                            </Storyboard>
                                        </BeginStoryboard>
                                    </Trigger.ExitActions>
                                </Trigger>

                                <Trigger Property="IsChecked" Value="True">
                                    <Trigger.EnterActions>
                                        <BeginStoryboard>
                                            <Storyboard>
                                                <ColorAnimation
                                                    Storyboard.TargetName="ToggleButtonBorder"
                                                    Storyboard.TargetProperty="(Border.Background).(SolidColorBrush.Color)"
                                                    To="LightSeaGreen"
                                                    Duration="0:0:0.1" />
                                            </Storyboard>
                                        </BeginStoryboard>
                                    </Trigger.EnterActions>
                                    <Trigger.ExitActions>
                                        <BeginStoryboard>
                                            <Storyboard>
                                                <ColorAnimation
                                                    Storyboard.TargetName="ToggleButtonBorder"
                                                    Storyboard.TargetProperty="(Border.Background).(SolidColorBrush.Color)"
                                                    Duration="0:0:0.1" />
                                            </Storyboard>
                                        </BeginStoryboard>
                                    </Trigger.ExitActions>
                                </Trigger>
                            </ControlTemplate.Triggers>
                        </ControlTemplate>
                    </Setter.Value>
                </Setter>
            </Style>
        </ToggleButton.Style>
    </ToggleButton>
</UserControl>
```

#### CustomDesignToggleButton.xaml.cs
``` 
using System.Windows;
using System.Windows.Input;

namespace WebFeedReader.Controls
{
    public partial class CustomDesignToggleButton
    {
        public readonly static DependencyProperty IsCheckedProperty =
            DependencyProperty.Register(
                nameof(IsChecked),
                typeof(bool?),
                typeof(CustomDesignToggleButton),
                new FrameworkPropertyMetadata(false, FrameworkPropertyMetadataOptions.BindsTwoWayByDefault));

        public readonly static DependencyProperty LabelProperty =
            DependencyProperty.Register(
                nameof(Label),
                typeof(string),
                typeof(CustomDesignToggleButton),
                new PropertyMetadata("Default Label"));

        public readonly static DependencyProperty CommandProperty =
            DependencyProperty.Register(
                nameof(Command),
                typeof(ICommand),
                typeof(CustomDesignToggleButton));

        public CustomDesignToggleButton()
        {
            InitializeComponent();
        }

        public bool? IsChecked { get => (bool?)GetValue(IsCheckedProperty); set => SetValue(IsCheckedProperty, value); }

        public string Label { get => (string)GetValue(LabelProperty); set => SetValue(LabelProperty, value); }

        public ICommand Command { get => (ICommand)GetValue(CommandProperty); set => SetValue(CommandProperty, value); }
    }
}
```