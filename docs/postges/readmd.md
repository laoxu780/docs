# Postgres

# 表添加字段
```sql
ALTER TABLE api_config
    add COLUMN created_at    timestamptz,
    add COLUMN created_by    timestamptz,
    add COLUMN updated_at    timestamptz,
    add COLUMN updated_by    timestamptz;
```