# UPDATE 文と DELETE 文でインデックスヒントを指定する

Transact-SQL リファレンス見ても分からなかったので記録. 特に FROM が2回出てくる DELETE 文. ちなみに1つめの FROM は消してもいい(オプショナル).

```sql
UPDATE
    <table_name>
  SET
    <column_name1> = <expression1>,
    <column_name2> = <expression2>,
    ...
  FROM
    <table_name>
    WITH
      (INDEX(<index_name>))
  WHERE
    ...

DELETE
  FROM
    <table_name>
  FROM
    <table_name>
    WITH
      (INDEX(<index_name>))
  WHERE
    ...
```
