# C# で Json を扱う

選択肢は複数ある。ヘビーな使い方をする場合は機能の比較が必要か？

今回の使用目的は、アプリケーションの設定項目の保存と読み込みのみ。

    using System.IO;
    using Newtonsoft.Json;
    using Newtonsoft.Json.Serialization;

    namespace TimeRecorder.Models;

    public class ApplicationSetting
    {
        public static string AppSettingFileName => "applicationSettings.json";

        public bool VisibleActivatedLog { get; set; } = true;

        public static void WriteApplicationSetting(ApplicationSetting setting)
        {
            var jsonSerializeSetting = new JsonSerializerSettings()
            {
                ContractResolver = new CamelCasePropertyNamesContractResolver(),
                Formatting = Formatting.Indented,
            };

            string data = JsonConvert.SerializeObject(setting, jsonSerializeSetting);

            using StreamWriter sw = File.CreateText(AppSettingFileName);
            sw.Write(data);
        }

        public static ApplicationSetting ReadApplicationSetting(string jsonFilePath)
        {
            using var reader = new StreamReader(jsonFilePath);
            return new JsonSerializer().Deserialize<ApplicationSetting>(new JsonTextReader(reader));
        }
    }

例のソースコードでは、クラスそのものが Json ファイルで保存される。

保存される対象は自動実装プロパティのみとなる。フィールドや非自動実装プロパティは保存されない。

よって `BindableBase` を継承してプロパティに処理を仕込みたい場合は工夫する必要がある

例の通り、`string`, `bool` は確実に動作する。整数型も動くだろうけど、未確認。
