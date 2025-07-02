# WPF Project に Material Design を適用する。

## インストール

`Install-Package MaterialDesignThemes` で導入可能。  
手動検索して `NuGet` GUI操作でもOK

## 使い方

`App.xaml` を編集する。  

    <Application x:Class="Example.App"
                xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
                xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
                xmlns:materialDesign="http://materialdesigninxaml.net/winfx/xaml/themes"
                StartupUri="MainWindow.xaml">
        <Application.Resources>
            <ResourceDictionary>
                <ResourceDictionary.MergedDictionaries>
                    <materialDesign:BundledTheme BaseTheme="Light" PrimaryColor="DeepPurple" SecondaryColor="Lime" />
                    <ResourceDictionary Source="pack://application:,,,/MaterialDesignThemes.Wpf;component/Themes/MaterialDesign3.Defaults.xaml" /> 
                </ResourceDictionary.MergedDictionaries>
            </ResourceDictionary>
        </Application.Resources>
    </Application>

`ResourceDictionary` の中身と `xmlns:materialDesign` のところを追加する。  
そして `Window.xaml` で `Style` を指定。

    <Window [...]
        Style="{StaticResource MaterialDesignWindow}"
        [...] >

## その他のコントロール

    <CheckBox
        Style="{StaticResource MaterialDesignLightCheckBox}">

    <ListBox.ItemContainerStyle>
        <Style BasedOn="{StaticResource MaterialDesignListBoxItem}" TargetType="ListBoxItem">

`Window` 以外にも各コントロール用のスタイルが用意されている。インテリセンスで候補も表示される。