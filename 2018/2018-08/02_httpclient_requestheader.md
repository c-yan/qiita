# HttpClient でリクエストヘッダを設定する (C#)

備忘録として.
また、ググったら`HttpClient.DefaultRequestHeaders.Add`だらけでイラッとしたので.

ざっくり言えば`HttpRequestMessage`を作り`HttpRequestMessage.Headers.Add`して、`HttpClient.SendAsync`に渡す.

以下コード例.

```csharp
var client = new HttpClient();
var request = new HttpRequestMessage(HttpMethod.Get, url);
request.Headers.Add("ContentType", "application/json");
request.Headers.Add("Authorization", $"Bearer {accessToken}");
var response = await client.SendAsync(request);
```
