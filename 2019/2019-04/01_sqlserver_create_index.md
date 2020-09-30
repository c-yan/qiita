# SQL Server で性能不足で新しくインデックスを作ることを検討する

備忘録として.

まずは現状の実行プランを確認する.
SQL Server Management Studio (SSMS) で「実際の実行プランを含める」をオンにして、対象のクエリを実行する.
そうすると実行プランと不足しているインデックスがサジェストされる.

サジェストを参考にインデックスを作成.

```sql
CREATE INDEX IDX_COL1 ON TBL1(COL1)
```

インデックスが出来たら性能変化の確認.

```sql
DBCC DROPCLEANBUFFERS
DBCC FREEPROCCACHE
```

でキャッシュを飛ばし、WITH 句で元のクエリで使われていたインデックスを指定して、クエリを実行.

```sql
SELECT ... FROM TBL1 WITH (INDEX(PK_...)) WHERE ...
```

再度キャッシュを飛ばし、WITH 句で新しく作ったインデックスを指定して、クエリを実行.

```sql
SELECT ... FROM TBL1 WITH (INDEX(IDX_COL1)) WHERE ...
```

それぞれの実行時間と読み取り行数を比較して、性能が向上したかを確認.

メールにコピペするために実行計画をテキストで欲しい場合は以下.

```sql
SET SHOWPLAN_TEXT ON
GO

SELECT ... FROM TBL1 WHERE ...
GO

SET SHOWPLAN_TEXT OFF
GO
```
