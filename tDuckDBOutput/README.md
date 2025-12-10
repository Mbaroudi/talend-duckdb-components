# tDuckDBOutput

Write data from Talend flow into DuckDB tables with multiple action modes.

## Features

- INSERT, CREATE_IF_NOT_EXISTS, DROP_CREATE actions
- Share connections with other DuckDB components via USE_EXISTING_CONNECTION
- Automatic table creation from schema
- High-performance bulk inserts

## Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| USE_EXISTING_CONNECTION | Check | Reuse connection from another component |
| CONNECTION_COMPONENT | Text | Name of connection component (e.g., "tDuckDBRow_1") |
| DB_PATH | File | Path to DuckDB database file |
| TABLE | Text | Target table name |
| ACTION | List | INSERT, CREATE_IF_NOT_EXISTS, DROP_CREATE |
| SCHEMA | Schema | Input schema definition |

## Actions

| Action | Description |
|--------|-------------|
| INSERT | Insert rows into existing table |
| CREATE_IF_NOT_EXISTS | Create table if missing, then insert |
| DROP_CREATE | Drop table, recreate, then insert |

## Usage

```
tFileInputDelimited_1 --> tDuckDBOutput_1
```

### Sharing Connection
```
tDuckDBRow_1 (creates in-memory DB)
     |
     +--> tDuckDBOutput_1 (USE_EXISTING_CONNECTION=true)
     |
     +--> tDuckDBOutput_2 (USE_EXISTING_CONNECTION=true)
```

## Global Variables

| Variable | Type | Description |
|----------|------|-------------|
| NB_LINE | Integer | Number of rows inserted |

## Dependencies

- `duckdb_jdbc-0.9.2.jar` (auto-downloaded via Maven)

## Version

- **Version:** 0.2
- **Status:** ALPHA
- **Author:** Talend/DuckDB
- **Compatibility:** Talend 8.0.1+

## License

Apache License 2.0
