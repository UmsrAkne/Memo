# JSON

`JavaScript Object Notation` の略。

`JavaScript` のオブジェクトの表記方法に由来するデータ記述言語の一種。

## 基本書式

    {“key” : “value”}

キーと値を中括弧で囲い、`:` で区切って表現する。

キーは文字列型となる。従ってダブルクォーテーションで囲む必要がある。

環境にもよるが、キーの値に大文字を使うのは避けた方が良いような記述が見つかる。  
少し調べた感じだと、具体的に明言しているソースがないが……。

中括弧で囲まれたオブジェクトの中では、カンマ区切りで複数のキー、バリューを入れることもできる。

    {
        “key1” : “value1”,
        “key2” : “value2”
    }

### 末尾のカンマ

`JavaScript` では付けても問題ないが、`JSON` では不可。  
よって以下のような記述はできない。

    {
        “key” : “value”,
    }

ソースは `MDN web docs`

    https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Trailing_commas

---
## データ型

### 文字列

前述と同様。value にダブルクォーテーションが必須。

    {"key" : "value"}

### 数値

値が数値である場合はダブルクォーテーションはつけない。  
付けた場合は文字列型として認識される。

    {"key" : 100}

### bool値

値として `true` or `false` を指定する。  
ダブルクォーテーションはつけない

    {"y" : true, "n" : false}

### オブジェクト

オブジェクトの中にオブジェクトを入れることができる。  
値を記述する部分にオブエジェクトを記述する。二重以上にネストすることも勿論可能。

    {
        "id": 1,
        "name": "tanaka",
        "attribute": {
            "gender": "male",
        }
    }

### 配列

値を `[]` で囲い込むことで宣言できる。`JSON` で使用可能な全ての型が格納できる。  
また、`JavaScript` と同様に複数の型を混ぜた配列を作ることができる。

    {
        "color_list": [ "red", "green", "blue" ],
        "num_list": [ 123, 456, 789 ],
        "mix_list": [ "red", 456, null, true ],
        "array_list": [ [ 12, 23 ], [ 34, 45 ], [ 56, 67 ] ],
        "object_list": [
            { "name": "Tanaka", "age": 26 },
            { "name": "Suzuki", "age": 32 }
        ]
    }

### null

設定するべき値がない場合に使用する。詳細は未調査。

    {“key” : null}
