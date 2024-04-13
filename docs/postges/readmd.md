# Postgres

## 连接数据库



## 表添加字段

```sql
ALTER TABLE api_config
    add COLUMN created_at    timestamptz,
    add COLUMN created_by    timestamptz,
    add COLUMN updated_at    timestamptz,
    add COLUMN updated_by    timestamptz;
```

## 获取磁盘空间占用
```sql
SELECT
    table_schema || '.' || table_name AS table_full_name,
   pg_size_pretty(pg_total_relation_size('"' || table_schema || '"."' || table_name || '"')) AS size
FROM information_schema.tables
ORDER BY
    pg_total_relation_size('"' || table_schema || '"."' || table_name || '"') DESC;
```