# CSV をクラスにバインドする (C#)

例えば

```
RowId,Id,StartDate,StopDate,CustomerId
0,C0001,2020-03-01T00:00:00.0000000,2020-04-01T00:00:00.0000000,0
1,C0002,2020-03-01T00:00:00.0000000,,1
```

みたいな CSV があって

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

にバインドしたいという話です. CSV パーサは今回は話題にしたくないので、`string.Split(",")` で片付くということにしましょう.

以下みたいなコードを書くと、`var contracts = Load<Contract>(fstream).ToArray();` でバインドできます.

```csharp
public static T Bind<T>(Dictionary<string, int> headerMap, string[] values)
    where T : class, new()
{
    var result = new T();
    foreach (var p in typeof(T).GetProperties())
    {
        if (!headerMap.ContainsKey(p.Name)) continue;
        var value = values[headerMap[p.Name]];

        var type = Nullable.GetUnderlyingType(p.PropertyType);
        if (type == null)
        {
            type = p.PropertyType;
        }
        else
        {
            if (value == "")
            {
                p.SetValue(result, null);
                continue;
            }
        }

        if (type == typeof(int))
        {
            p.SetValue(result, int.Parse(value));
        }
        else if (type == typeof(bool))
        {
            p.SetValue(result, bool.Parse(value));
        }
        else if (type == typeof(string))
        {
            p.SetValue(result, value);
        }
        else if (type == typeof(DateTime))
        {
            if (!DateTime.TryParseExact(value, "o", null, DateTimeStyles.RoundtripKind, out var _))
            {
                value = DateTime.Parse(value).ToString("o");
            }
            p.SetValue(result, DateTime.ParseExact(value, "o", null, DateTimeStyles.RoundtripKind));
        }
        else
        {
            throw new ApplicationException($"Unsupported type: {typeof(T).Name}");
        }
    }
    return result;
}

public static IEnumerable<T> Load<T>(Stream stream, Encoding encoding = null)
    where T : class, new()
{
    if (encoding == null) encoding = Encoding.UTF8;
    using (var reader = new StreamReader(stream, encoding))
    {
        var header = reader.ReadLine().Split(",");
        var headerMap = new Dictionary<string, int>();
        for (var i = 0; i < header.Length; i++)
        {
            headerMap[header[i]] = i;
        }

        while (true)
        {
            var line = reader.ReadLine();
            if (line == null) break;
            yield return Bind<T>(headerMap, line.Split(","));
        }
    }
}
```

なお、プロパティではなくフィールドにしたい場合には `.GetProperties()` が `.GetFields()` になり、`.PropertyType` が `.FieldType` になります.

aまた、class ではなく struct にしたい場合には、`where T : class, new()` が `where T : struct` になり、`var result = new T();` が `var result = default(T);`  になり、`p.SetValue(result, v);` が `p.SetValueDirect(__makeref(result), v);` になります.

関連記事: [クラスを CSV にデバインドする (C#)](https://qiita.com/c-yan/items/74345e0aad795cc23929)
