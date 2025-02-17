## Select
``` sql
SELECT * FROM tab;
SELECT col1, col2 FROM tab;
```

## Sort
```sql
SELECT * FROM tab ORDER BY col1 ASC;
SELECT * FROM tab ORDER BY col DESC;
```

## Filter

```sql title='By condition'
SELECT * FROM tab WHERE col1 = 11;
SELECT * FROM tab WHERE col1 = 11 AND col2 > 5;
```

```sql title='Check null'
SELECT * FROM tab WHERE col1 IS NULL;
SELECT * FROM tab WHERE col1 IS NOT NULL;
```

```sql title='Compare between columns'
SELECT * FROM tab WHERE col1 > col2;
```

## Add new column by calculation
```sql
SELECT col1, col2, col1/col2 AS new_col FROM tab;
```

## Aggregate

```sql title='Group sum'
SELECT col1, SUM(col2) FROM tab GROUP BY col1;
```

```sql title='Group average'
SELECT col1, AVG(col2) FROM tab GROUP BY col1;
```

```sql title='Multiple'
SELECT col1, SUM(col2), MAX(col3) FROM tab GROUP BY col1;
```

## Joining

```sql title='Inner'
SELECT * FROM tab1 INNER JOIN tab2 ON tab1.id = tab2.id;
```

```sql title='Left'
SELECT * FROM tab1 LEFT JOIN tab2 ON tab1.id = tab2.id;
```
