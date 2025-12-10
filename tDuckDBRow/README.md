# tDuckDBRow

Swiss-army knife component for DuckDB: execute SQL, transform data, load/export files, and work with cloud storage.

## Features

- **4 Operation Modes:** QUERY, TRANSFORM, LOAD_FILE, EXPORT
- **Cloud Storage:** S3, Azure Blob, GCS, Cloudflare R2
- **File Formats:** Parquet, CSV, JSON, NDJSON
- **Extensions:** httpfs, spatial, FTS, Iceberg, Delta Lake
- **In-memory Processing:** Fast transformations with batch processing

## Parameters

### Basic

| Parameter | Type | Description |
|-----------|------|-------------|
| MODE | List | QUERY, TRANSFORM, LOAD_FILE, EXPORT |
| DATABASE_PATH | File | Path to database (default: `:memory:`) |
| SQL_QUERY | SQL | SQL to execute (for QUERY/TRANSFORM modes) |
| SCHEMA | Schema | Output schema definition |

### Cloud Storage

| Parameter | Type | Description |
|-----------|------|-------------|
| USE_CLOUD_STORAGE | Check | Enable cloud storage access |
| CLOUD_PROVIDER | List | S3, AZURE, GCS, R2 |
| S3_ACCESS_KEY | Text | AWS/R2 access key |
| S3_SECRET_KEY | Password | AWS/R2 secret key |
| S3_REGION | Text | AWS region (e.g., eu-west-1) |
| S3_ENDPOINT | Text | Custom endpoint for R2/MinIO |

### File Operations

| Parameter | Type | Description |
|-----------|------|-------------|
| FILE_PATH | Text | Path for LOAD_FILE mode |
| FILE_FORMAT | List | PARQUET, CSV, JSON, NDJSON |
| EXPORT_PATH | Text | Output path for EXPORT mode |
| EXPORT_PARTITION_BY | Text | Partition column for exports |

### Advanced

| Parameter | Type | Description |
|-----------|------|-------------|
| MEMORY_LIMIT | Text | DuckDB memory limit (default: 4GB) |
| THREADS | Integer | Number of threads (default: 4) |
| BATCH_SIZE | Integer | Rows per batch (default: 10000) |
| INIT_SQL | SQL | Initialization SQL (SET, PRAGMA, etc.) |
| PRE_SQL | SQL | SQL before each batch |
| POST_SQL | SQL | SQL after each batch |

### Extensions

| Parameter | Type | Description |
|-----------|------|-------------|
| USE_HTTPFS | Check | Enable HTTP/S3 file access |
| USE_SPATIAL | Check | Enable geospatial functions |
| USE_FTS | Check | Enable full-text search |
| USE_ICE | Check | Enable Iceberg table format |
| USE_DELTA | Check | Enable Delta Lake format |

## Usage Examples

### Transform Data
```
tFileInputDelimited_1 --> tDuckDBRow_1 (MODE=TRANSFORM) --> tLogRow_1
```
SQL:
```sql
SELECT 
    UPPER(name) as name,
    ROUND(amount, 2) as amount,
    DATE_TRUNC('month', created_at) as month
FROM INPUT_BUFFER
WHERE amount > 100
```

### Load Parquet from S3
```
tDuckDBRow_1 (MODE=LOAD_FILE) --> tLogRow_1
```
Settings:
- FILE_PATH: `s3://bucket/data/*.parquet`
- USE_CLOUD_STORAGE: true
- CLOUD_PROVIDER: S3

### Export to Partitioned Parquet
```
tFileInputDelimited_1 --> tDuckDBRow_1 (MODE=EXPORT)
```
Settings:
- EXPORT_PATH: `/output/data`
- FILE_FORMAT: PARQUET
- EXPORT_PARTITION_BY: `year, month`

## Global Variables

| Variable | Type | Description |
|----------|------|-------------|
| NB_LINE_INPUT | Integer | Rows received |
| NB_LINE_OUTPUT | Integer | Rows produced |
| NB_BATCHES | Integer | Batches processed |
| PROCESSING_TIME_MS | Long | Total processing time |
| NB_FILES_LOADED | Integer | Files loaded (LOAD_FILE mode) |
| NB_FILES_EXPORTED | Integer | Files exported (EXPORT mode) |
| NB_ERRORS | Integer | Error count |

## Dependencies

- `duckdb_jdbc-1.0.0.jar` (auto-downloaded via Maven)

## Version

- **Version:** 1.2
- **Status:** PROD
- **Author:** ESDB
- **Compatibility:** Talend 8.0.1+

## License

Apache License 2.0
