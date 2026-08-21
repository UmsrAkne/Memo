---
title: 'Xaml の応用的な Tips'
date: '2026-08-21 22:17:37'
tags: [wpf, xaml]
published: false
---

## 動的な DataTemplate の表示

### 概要

ビューモデルの型によって、適切なデータテンプレートを自動で選択して適切な表示を行う方法を記述する。

### 1. DataTemplate を定義する
まず、`App.xaml` か、あるいは `MainWindow.xaml` のリソースに、「ViewModel」と「View」の対応を定義する。

```xaml
<Window.Resources>
    <!-- ViewModelの型に合わせてViewを指定する -->
    <DataTemplate DataType="{x:Type viewModels:ToolAViewModel}">
        <views:ToolAView />
    </DataTemplate>
    
    <DataTemplate DataType="{x:Type viewModels:ToolBViewModel}">
        <views:ToolBView />
    </DataTemplate>
</Window.Resources>
```

### 2. 表示したい場所に ContentControl を置く

```xaml
<ItemsControl ItemsSource="{Binding ToolAreaViewModels}">
    <ItemsControl.ItemTemplate>
        <DataTemplate>
            <ContentControl Content="{Binding}" />
        </DataTemplate>
    </ItemsControl.ItemTemplate>
</ItemsControl>
```