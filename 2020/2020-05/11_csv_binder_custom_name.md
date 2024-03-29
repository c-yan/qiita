# CSV をクラスにバインドする カラム名自由編 (C#)

\# 以下の記事が書かれた時の版数は .NET Core 3.1 (3.1.201) となります.

[CSV をクラスにバインドする (C#)](https://qiita.com/c-yan/items/aa3025642b32201454c8) は有用だが、CSV のヘッダには日本語やカッコなどの識別子に使えないカラム名を使いたいということがあるのでそれに対応する.

例えば

```
RowId,契約ID,開始日,終了日,顧客ID
0,C0001,2020-03-01T00:00:00.0000000,2020-04-01T00:00:00.0000000,0
1,C0002,2020-03-01T00:00:00.0000000,,1
```

みたいな CSV があるとします.

バインド対象のクラスのプロパティーに ` System.Runtime.Serialization.DataMember` 属性を使ってカラム名をアノテーションします.

```csharp
public class Contract
{
    public int RowId { get; set; }
    [DataMember(Name = "契約ID")]
    public string Id { get; set; }
    [DataMember(Name = "開始日")]
    public DateTime StartDate { get; set; }
    [DataMember(Name = "終了日")]
    public DateTime? StopDate { get; set; }
    [DataMember(Name = "顧客ID")]
    public int CustomerId { get; set; }
}
```

後は Load を以下に変更すると、`var contracts = Load<Contract>(fstream).ToArray();` でバインドできます.

```csharp
public static IEnumerable<T> Load<T>(Stream stream, Encoding encoding = null)
    where T : class, new()
{
    var headerNameMap = typeof(T).GetProperties().Select(e =>
    {
        var a = e.GetCustomAttributes(typeof(DataMemberAttribute), true)
            .Cast<DataMemberAttribute>()
            .SingleOrDefault();
        if (a == null || a.Name == null)
        {
            return new KeyValuePair<string, string>(e.Name, e.Name);
        }
        return new KeyValuePair<string, string>(a.Name, e.Name);
    }).ToDictionary(e => e.Key, e => e.Value);

    if (encoding == null) encoding = Encoding.UTF8;
    using (var reader = new StreamReader(stream, encoding))
    {
        var header = reader.ReadLine().Split(",");
        var headerMap = new Dictionary<string, int>();
        for (var i = 0; i < header.Length; i++)
        {
            headerMap[headerNameMap[header[i]]] = i;
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

関連記事: [クラスを CSV にデバインドする カラム名自由編 (C#)](https://qiita.com/c-yan/items/753c16198dff01c27f18)
