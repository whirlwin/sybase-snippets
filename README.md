# Sybase snippets

Useful queries in Sybase used for monitoring.

----

## 1. Metadata

### Finding number of rows for all tables

#### Alternative #1, using `systabstats`:
```sql
SELECT LOWER(so.name) as table_name, sum(sts.rowcnt) AS estimated_rowcount_total
FROM sysobjects so
    LEFT OUTER JOIN systabstats sts
    ON so.id = sts.id AND sts.indid IN (0,1)
WHERE so.type='U'
GROUP BY so.name
```

#### Alternative #2, using `sysobjects`:
```sql
USE {{db_name}}
SELECT so.name AS table_name, CONVERT(BIGINT,row_count(db_id(),so.id)) AS estimated_row_count
FROM sysobjects so WHERE so.type = 'U' GROUP BY so.name
```



