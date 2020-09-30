# varchar(max) の場合は SqlParameter の size 引数に何を渡すか

-1 です(終了).

```csharp
new SqlParameter("@Param", SqlDbType.VarChar, -1) {
```

SQL Server の1行は基本的には8060バイトが上限で、varchar の場合は varchar(8000) が最大長になります.
ただし、SQL Server 2005 で追加された新機能 varchar(max) を使うとそのカラムは最大 2GB になります.
その中間の10000文字とかで制限したいと思っても出来ません.

Microsoft の .NET API ページの SqlParameter Class の Size の説明に何も書かれていないのがもうね…….
