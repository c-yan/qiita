# Amazon S3 が2020年9月30日以降パス形式のサポートを終了するが AWS SDK for .NET は大丈夫か?

結論: 大丈夫.

ソースコードを見るに、自ら好き好んで ForcePathStyle を true にしない限りは、バケット名が仮想ホスト形式に適合している限り、仮想ホスト形式を使うようになっています.
[aws-sdk-net/AmazonS3PostMarshallHandler.cs at master · aws/aws-sdk-net](https://github.com/aws/aws-sdk-net/blob/master/sdk/src/Services/S3/Custom/Internal/AmazonS3PostMarshallHandler.cs#L130)

参考: [【注意喚起】 2020年9月30日以降、パス形式での S3 API リクエストは受け付けられなくなります。](https://dev.classmethod.jp/cloud/aws/s3-no-longer-support-path-style-requests/)
