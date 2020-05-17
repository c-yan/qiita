# クラスを CSV にデバインドする (C#)

例えば

```csharp
public class Contract
{
    public int RowId { get; set; }
    public string Id { get; set; }
    public DateTime StartDate { get; set; }
    public DateTime? StopDate { get; set; }
    public int CustomerId { get; set; }
}
```

みたいなクラスがあって

```
RowId,Id,StartDate,StopDate,CustomerId
0,C0001,2020-03-01T00:00:00.0000000,2020-04-01T00:00:00.0000000,0
1,C0002,2020-03-01T00:00:00.0000000,,1
```

みたいな、CSV にデバインドしたいという話です. CSV 変換は今回は話題にしたくないので、`string.Join(",", v)` で片付くということにしましょう.

以下みたいなコードを書くと、`Save(fstream, contracts);` でデバインドできます.

```csharp
public static string[] Debind<T>(Dictionary<string, int> headerMap, T obj)
    where T : class
{
    var result = new string[headerMap.Count];
    foreach (var p in typeof(T).GetProperties())
    {
        if (!headerMap.ContainsKey(p.Name)) continue;
        var i = headerMap[p.Name];

        var type = Nullable.GetUnderlyingType(p.PropertyType);
        if (type == null)
        {
            type = p.PropertyType;
        }
        else
        {
            if (p.GetValue(obj) == null)
            {
                result[i] = "";
                continue;
            }
        }

        if (type == typeof(DateTime))
        {
            result[i] = ((DateTime)p.GetValue(obj)).ToString("o");
        }
        else
        {
            result[i] = (p.GetValue(obj) ?? "").ToString();
        }
    }
    return result;
}

public static void Save<T>(Stream stream, IEnumerable<T> content, Encoding encoding = null)
    where T : class
{
    if (encoding == null) encoding = Encoding.UTF8;
    using (var writer = new StreamWriter(stream, encoding))
    {
        var header = typeof(T).GetProperties().Select(e => e.Name).ToList();
        var headerMap = new Dictionary<string, int>();
        for (var i = 0; i < header.Count; i++)
        {
            headerMap[header[i]] = i;
        }
        writer.WriteLine(string.Join(",", header));

        foreach (var o in content)
        {
            writer.WriteLine(string.Join(",", Debind(headerMap, o)));
        }
    }
}
```

なお、プロパティではなくフィールドにしたい場合には `.GetProperties()` が `.GetFields()` になり、`.PropertyType` が `.FieldType` になります.

また、class ではなく struct にしたい場合には、`where T : class` が `where T : struct` になり、`p.GetValue(obj)` が `p.GetValueDirect(__makeref(obj)))` になります.

関連記事: [CSV をクラスにバインドする (C#)](https://qiita.com/c-yan/items/aa3025642b32201454c8)
