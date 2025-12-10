# tDuckDBInput

Execute SQL queries on DuckDB databases and output rows to the Talend flow.

## Features

- Execute any SQL SELECT query on DuckDB databases
- Support for in-memory (`:memory:`) and file-based databases
- Automatic type mapping to Talend schema
- Returns row count via `NB_LINE` global variable

## Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| DB_PATH | File | Path to DuckDB database file (use `:memory:` for in-memory) |
| QUERY | SQL | SQL SELECT query to execute |
| SCHEMA | Schema | Output schema definition |

## Usage

```
tDuckDBInput_1 --> tLogRow_1
```

### Example Query
```sql
SELECT id, name, created_at FROM users WHERE active = true
```

## Global Variables

| Variable | Type | Description |
|----------|------|-------------|
| NB_LINE | Integer | Number of rows returned |
| ERROR_MESSAGE | String | Error message if any |

## Dependencies

- `duckdb_jdbc-0.9.2.jar` (auto-downloaded via Maven)

## Version

- **Version:** 0.3
- **Status:** ALPHA
- **Author:** Talend/DuckDB
- **Compatibility:** Talend 8.0.1+

## License

Apache License 2.0
