# C# で Json を入出力

C# で `Prompt` クラスを JSON に変換（シリアライズ）したり、JSON から復元（デシリアライズ）するには、`System.Text.Json` が使いやすくておすすめです。

---

## 必要な using ディレクティブ

```csharp
using System.Text.Json;
using ImageRenameHelper.Models;
```

.NET 5 以降なら `System.Text.Json` が標準で使えます。

---

## JSON に変換してファイルに保存（シリアライズ）

`Prompt` はカスタムクラス。クラスの方には特に設定は必要ない。そのまま渡すだけで良い。

```csharp
public static void SavePromptToJsonFile(Prompt prompt, string filePath)
{
    var options = new JsonSerializerOptions
    {
        WriteIndented = true // 読みやすいように整形
    };

    string json = JsonSerializer.Serialize(prompt, options);
    File.WriteAllText(filePath, json);
}
```

---

## JSON ファイルから読み込んで Prompt を復元（デシリアライズ）

```csharp
public static Prompt LoadPromptFromJsonFile(string filePath)
{
    string json = File.ReadAllText(filePath);
    var prompt = JsonSerializer.Deserialize<Prompt>(json);
    
    return prompt ?? new Prompt(); // null の場合に空のインスタンスを返す
}
```

---

## 使用例

```csharp
var prompt = new Prompt
{
    Positive = "masterpiece, best quality",
    Negative = "lowres, blurry"
};

string path = "prompt.json";

// 保存
SavePromptToJsonFile(prompt, path);

// 復元
var loadedPrompt = LoadPromptFromJsonFile(path);
Console.WriteLine($"Positive: {loadedPrompt.Positive}");
Console.WriteLine($"Negative: {loadedPrompt.Negative}");
```

---

## ファイル不要で文字列だけ使いたいなら？

```csharp
string json = JsonSerializer.Serialize(prompt);
Prompt parsed = JsonSerializer.Deserialize<Prompt>(json);
```

---