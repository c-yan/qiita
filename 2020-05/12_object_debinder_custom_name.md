# クラスを CSV にデバインドする カラム名自由編 (C#)

[クラスを CSV にデバインドする (C#)](https://qiita.com/c-yan/items/74345e0aad795cc23929) は有用だが、CSV のヘッダには日本語やカッコなどの識別子に使えないカラム名を使いたいということがあるのでそれに対応する.

```csharp
[AttributeUsage(AttributeTargets.Property)]
public class HeaderNameAttribute : Attribute
{
    public string Name { get; private set; }
    public HeaderNameAttribute(string name) { Name = name; }
}

public class Contract
{
    public int RowId { get; set; }
    [HeaderName("契約ID")]
    public string Id { get; set; }
    [HeaderName("開始日")]
    public DateTime StartDate { get; set; }
    [HeaderName("終了日")]
    public DateTime? StopDate { get; set; }
    [HeaderName("顧客ID")]
    public int CustomerId { get; set; }
}
```

みたいなクラスがあって

```
RowId,契約ID,開始日,終了日,顧客ID
0,C0001,2020-03-01T00:00:00.0000000,2020-04-01T00:00:00.0000000,0
1,C0002,2020-03-01T00:00:00.0000000,,1
```

みたいな、CSV にデバインドしたいという話です. CSV 変換は今回は話題にしたくないので、`string.Join(",", v)` で片付くということにしましょう.

Save を以下のコードに置き換えると、`Save(stream, contracts);` でデバインドできます.

```csharp
public static void Save<T>(Stream stream, IEnumerable<T> content, Encoding encoding = null)
    where T : class
{
    if (encoding == null) encoding = Encoding.UTF8;
    using (var writer = new StreamWriter(stream, encoding))
    {
        var headerName = typeof(T).GetProperties().Select(e =>
        {
            var a = Attribute.GetCustomAttributes(e, typeof(HeaderNameAttribute));
            if (a.Length == 0)
            {
                return e.Name;
            }
            else
            {
                return (a[0] as HeaderNameAttribute).Name;
            }
        }).ToList();
        writer.WriteLine(string.Join(",", headerName));

        var header = typeof(T).GetProperties().Select(e => e.Name).ToList();
        var headerMap = new Dictionary<string, int>();
        for (var i = 0; i < header.Count; i++)
        {
            headerMap[header[i]] = i;
        }

        foreach (var o in content)
        {
            writer.WriteLine(string.Join(",", Debind(headerMap, o)));
        }
    }
}
```

関連記事: [CSV をクラスにバインドする カラム名自由編 (C#)](https://qiita.com/c-yan/items/e4d553aac74e845c407a)
