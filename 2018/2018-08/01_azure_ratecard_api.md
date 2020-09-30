# Azure Resource RateCard API を叩く

備忘録として.

1. Auzre AD に`Web アプリ/API`型のアプリケーションを登録し、サブスクリプションの IAM から`所有者`の権限を付けておく.

2. トークンを取得する.

```sh
curl -X POST https://login.microsoftonline.com/{TenantID}/oauth2/token \
   -F grant_type=client_credentials \
   -F resource=https://management.core.windows.net/ \
   -F client_id={Application ID} \
   -F client_secret={Key} \
```

3. 実際に API を叩く.

```sh
curl -L \
   "https://management.azure.com/subscriptions/{SubscriptionID}/providers/Microsoft.Commerce/RateCard?api-version=2016-08-31-preview&%24filter=OfferDurableId+eq+'MS-AZR-0003p'+and+Currency+eq+'USD'+and+Locale+eq+'en-US'+and+RegionInfo+eq+'US'" \
   -H 'ContentType: application/json' \
   -H 'Authorization: Bearer {Token}'
```
