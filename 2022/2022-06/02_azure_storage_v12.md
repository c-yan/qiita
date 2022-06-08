# Azure Storage v12 での非互換変更について

もう何度目だよという Azure Storage ライブラリのオーバーホールですが、今回ほどドツボにはまったのは初めてだったので記録を残します. というか、非互換変更がまとまっているページが見つからないので、他にもあるかもしれません.

GitHub のレポジトリに CHANGELOG.md や BreakingChanges.txt があるのですが、v12 以降の情報しか載ってなくて、肝心な v11 から v12 で何が変わったのかは全く分からないという…….

流石に無いわけがないだろと思うので、変更一覧ページの URL を知っている人がいたらコメントしてください. 情報お待ちしております.

## v11 ではデフォルトで上書きされていたのが、v12 ではエラーになるようになった

Microsoft.Azure.Storage.Blob v11 の Upload は既にブロブが存在していた場合に上書きするが、v12 ではデフォルトでは上書きされずに例外が飛ぶようになりました.

[Upload(Stream, Boolean, CancellationToken)](https://docs.microsoft.com/en-us/dotnet/api/azure.storage.blobs.blobclient.upload?view=azure-dotnet#azure-storage-blobs-blobclient-upload(system-io-stream-system-boolean-system-threading-cancellationtoken))

> The Upload(Stream, CancellationToken) operation creates a new block blob or throws if the blob already exists.

[UploadFromStream(Stream, AccessCondition, BlobRequestOptions, OperationContext)](https://docs.microsoft.com/en-us/dotnet/api/microsoft.azure.storage.blob.cloudblockblob.uploadfromstream?view=azure-dotnet-legacy#microsoft-azure-storage-blob-cloudblockblob-uploadfromstream(system-io-stream-microsoft-azure-storage-accesscondition-microsoft-azure-storage-blob-blobrequestoptions-microsoft-azure-storage-operationcontext))

> Uploads a stream to a block blob. If the blob already exists, it will be overwritten.

今まで通り上書きアップロードをしたい場合には、Upload に bool 型の overwrite 引数が追加されているので true を渡してください.

```
containerClient.GetBlobClient(blobName).Upload(fileStream, true);
```

## v11 ではデフォルトで Base64 エンコーディングされていたのが、v12 ではされなくなった

Microsoft.Azure.Storage.Queue v11 では送信メッセージがデフォルトで Base64 エンコーディングされていたのが、v12 ではされなくなりました.

[チュートリアル:.NET で Azure Queue Storage キューを操作する](https://docs.microsoft.com/ja-jp/azure/storage/queues/storage-tutorial-queues)

> v12 より前のバージョンの SDK でメッセージがキューに送信されると、自動的に Base64 でエンコードされます。 v12 以降では、この機能は削除されています。 v12 SDK を使用してメッセージを取得しても、自動的に Base64 デコードされることはありません。 コンテンツは、明示的に Base64 デコードする必要があります。

既に使っている人はバージョンアップでチュートリアルを見直したりなんてしないと思うんですが、他に書かれている場所見つからないんですよね…….

一番ひどいのは Microsoft が提供している Azure Functions や Azure App Service WebJobs の QueueTriggers は Base64 エンコーディングされていることを期待していて、プレーンで送信すると動きません. 普通は、デフォルトは Base64 エンコーディングするままで変えずに、性能が必要な人だけオプションでプレーン送信するよう変更するだろって思うんですけどねえ.

送信データを Base64 エンコーディングするには、`QueueServiceClient` の生成時に以下のオプションを設定します.

```cs
var serviceClient = new QueueServiceClient(connectionStrings, new QueueClientOptions()
{
    MessageEncoding = QueueMessageEncoding.Base64,
});
```
