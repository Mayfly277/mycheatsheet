# SQLI

## Mysql

- List Databases
```sql
SELECT schema_name FROM information_schema.schemata;
```
- List Columns
```
SELECT table_schema, table_name, column_name FROM information_schema.columns WHERE table_schema != 'mysql' AND table_schema != 'information_schema'
```

- List Tables
```
SELECT table_schema,table_name FROM information_schema.tables WHERE table_schema != 'mysql' AND table_schema != 'information_schema'
```


