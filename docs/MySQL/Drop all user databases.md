---
title: Drop all user databases
summary: Drop all user databases.
authors:
  - Levent Ogut
tags:
  - mysql
---

```sql
-- Prevent truncation
SET SESSION group_concat_max_len = 1000000;

SELECT GROUP_CONCAT(
  DISTINCT CONCAT('DROP DATABASE ', table_schema, ';')
  SEPARATOR ''
)
FROM information_schema.tables
WHERE table_schema NOT IN ('mysql', 'information_schema');
```
