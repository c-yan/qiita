# Web リクエストがエラーになったときのレスポンスボディを取得する (.NET)

特に Web API 呼び出しだと、エラー応答に修正に重要な情報が載っていることが多いので.
例外に Response オブジェクトが入っていて、後は正常時と同様に GetResponseStream を呼べば OK.

```csharp
catch (WebException e)
{
    using (var rs = new StreamReader(e.Response.GetResponseStream())) {
        Console.WriteLine(rs.ReadToEnd());
    }
}
```
