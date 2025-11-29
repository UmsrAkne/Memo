# **テストプロジェクト関連ビルドエラー対応メモ**

## ■ 症状①

**NU1605（パッケージのダウングレード）エラーが大量発生し、テストプロジェクトのビルドが失敗する。**

### ● 原因

古い `Microsoft.NET.Test.Sdk`（例：17.1.0）が古い依存ライブラリを引きずり、
その依存関係が解決できず NU1605 が発生する。

### ● 解決方法

**エラーが出ているテストプロジェクト直下で Test SDK を最新にアップデートする。**

```bash
dotnet add ImgPlacer.Tests package Microsoft.NET.Test.Sdk
```

（プロジェクト名は適宜変更）
これで依存関係がリフレッシュされ NU1605 が解消される。

---

## ■ 症状②

以下のようなエラーでビルドが停止する：

```
error : Microsoft.NET.Test.Sdk doesn't support net6.0-windows and has not been tested with it.
Consider upgrading your TargetFramework to net8.0 or later.
```

### ● 原因

新しい Test SDK（18.x 以降）が **.NET 6 のテストプロジェクトを非サポート**にしたため。
`.NET 6` のテストプロジェクトで `Microsoft.NET.Test.Sdk 18.x` を使用すると必ずエラーになる。

### ● 解決方法

**該当テストプロジェクトの `.csproj` の TFM（TargetFramework）を .NET 8 以上へ引き上げる。**

例：

```xml
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
      <TargetFramework>net8.0-windows</TargetFramework>
  </PropertyGroup>
</Project>
```

これで Test SDK の対応範囲に入り、ビルドエラーが解消される。

---

# ■ 補足メモ

* `dotnet publish` は **ソリューション全体をビルドする**ため、
  **テストプロジェクトが落ちても publish 全体が失敗する**。
* 本体プロジェクト（ImgPlacer）は警告だけならビルドできるが、
  テストのビルドエラーがあると絶対に先へ進めない。
* `net6.0-windows` は既に **EOL**。余裕があれば本体も 8.0 に上げると幸せ。

---

