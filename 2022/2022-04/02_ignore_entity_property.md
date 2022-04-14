# ITableEntity の一部のプロパティを無視する方法

\# 以下の記事が書かれた時の版数は .NET Core 3.1 (3.1.24)、Azure.Data.Tables 12.5.0 となります.

`Microsoft.Azure.Cosmos.Table` の時にはエンティティの一部のプロパティを無視するための `Microsoft.Azure.Cosmos.Table.IgnorePropertyAttribute` クラスが存在しました. `Azure.Data.Tables` になって一見無視するための手段がなくなってしまったように見えますが、標準で存在する `System.Runtime.Serialization.IgnoreDataMemberAttribute` クラスが使えるようになっています.

以下、使用例.

```csharp
using Azure;
using Azure.Data.Tables;
using System;
using System.Runtime.Serialization;

namespace WebApplication1.Models
{
    public sealed class BillingSummary : ITableEntity
    {
        [IgnoreDataMember]
        public string SubscriptionId { get; set; }
        public int BillingAmount { get; set; }
        public bool IsFinalized { get; set; }
        [IgnoreDataMember]
        public DateTime YearMonth { get; set; }

        public string PartitionKey { get => SubscriptionId; set => SubscriptionId = value; }
        public string RowKey { get => $"{YearMonth:yyyyMM}"; set => YearMonth = DateTime.ParseExact(value, "yyyyMM", null); }
        public DateTimeOffset? Timestamp { get; set; }
        public ETag ETag { get; set; }
    }
}
```

情報源は `Azure.Data.Tables` の [CHANGELOG](https://github.com/Azure/azure-sdk-for-net/blob/Azure.Data.Tables_12.5.0/sdk/tables/Azure.Data.Tables/CHANGELOG.md#features-added) です. `12.2.0-beta.1 (2021-08-10)` で機能追加されたようです.

cf: [[FEATURE REQ] Allow specific properties to be ignored on ITableEntity implementation #19782](https://github.com/Azure/azure-sdk-for-net/issues/19782)
