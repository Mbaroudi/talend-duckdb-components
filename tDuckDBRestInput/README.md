# tDuckDBRestInput

Fetch data from REST APIs and load directly into DuckDB for SQL processing.

## Features

- **Multiple Auth Types:** Bearer, Basic, API Key, OAuth2 Client Credentials
- **Response Formats:** JSON, NDJSON, CSV, Parquet
- **Pagination:** Offset, Page, Cursor, Link Header
- **Caching:** Built-in response caching with TTL
- **Post-Processing:** Transform API response with SQL

## Parameters

### API Configuration

| Parameter | Type | Description |
|-----------|------|-------------|
| API_URL | Text | REST API endpoint URL |
| RESPONSE_FORMAT | List | JSON, NDJSON, CSV, PARQUET |
| JSON_PATH | Text | JSONPath to data array (e.g., `$.results`) |

### Authentication

| Parameter | Type | Description |
|-----------|------|-------------|
| USE_AUTHENTICATION | Check | Enable authentication |
| AUTH_TYPE | List | BEARER, BASIC, API_KEY, OAUTH2_CLIENT |
| BEARER_TOKEN | Password | Bearer token value |
| BASIC_USERNAME | Text | Basic auth username |
| BASIC_PASSWORD | Password | Basic auth password |
| API_KEY_HEADER | Text | Custom header name (default: X-API-Key) |
| API_KEY_VALUE | Password | API key value |
| OAUTH2_TOKEN_URL | Text | OAuth2 token endpoint |
| OAUTH2_CLIENT_ID | Text | OAuth2 client ID |
| OAUTH2_CLIENT_SECRET | Password | OAuth2 client secret |

### Pagination

| Parameter | Type | Description |
|-----------|------|-------------|
| USE_PAGINATION | Check | Enable pagination |
| PAGINATION_TYPE | List | OFFSET, PAGE, CURSOR, LINK_HEADER |
| PAGE_SIZE | Integer | Records per page |
| MAX_PAGES | Integer | Maximum pages (0 = unlimited) |
| OFFSET_PARAM | Text | Offset parameter name |
| LIMIT_PARAM | Text | Limit parameter name |
| PAGE_PARAM | Text | Page parameter name |
| CURSOR_PARAM | Text | Cursor parameter name |
| CURSOR_JSON_PATH | Text | JSONPath to next cursor |

### Caching

| Parameter | Type | Description |
|-----------|------|-------------|
| USE_CACHE | Check | Enable response caching |
| CACHE_TTL_SECONDS | Integer | Cache TTL in seconds |
| CACHE_KEY | Text | Custom cache key |

### Post-Processing

| Parameter | Type | Description |
|-----------|------|-------------|
| POST_SQL | SQL | SQL to transform/filter API response |

### Advanced

| Parameter | Type | Description |
|-----------|------|-------------|
| TIMEOUT_SECONDS | Integer | Request timeout (default: 60) |
| RETRY_COUNT | Integer | Retry attempts (default: 3) |
| RETRY_DELAY_MS | Integer | Delay between retries |
| USER_AGENT | Text | Custom User-Agent header |
| CUSTOM_HEADERS | Table | Additional HTTP headers |

## Usage Examples

### Simple API Call
```
tDuckDBRestInput_1 --> tLogRow_1
```
Settings:
- API_URL: `https://api.example.com/users`
- RESPONSE_FORMAT: JSON
- JSON_PATH: `$.data`

### API with Bearer Auth and Pagination
```
tDuckDBRestInput_1 --> tFileOutputDelimited_1
```
Settings:
- API_URL: `https://api.example.com/orders`
- USE_AUTHENTICATION: true
- AUTH_TYPE: BEARER
- BEARER_TOKEN: `${context.api_token}`
- USE_PAGINATION: true
- PAGINATION_TYPE: OFFSET
- PAGE_SIZE: 100

### Transform API Response with SQL
```
tDuckDBRestInput_1 (with POST_SQL) --> tMap_1 --> tDBOutput_1
```
POST_SQL:
```sql
SELECT 
    id,
    UPPER(name) as name,
    CAST(created_at AS DATE) as date
FROM api_response
WHERE status = 'active'
```

## Global Variables

| Variable | Type | Description |
|----------|------|-------------|
| NB_LINE | Integer | Total rows returned |
| NB_API_CALLS | Integer | Number of API calls made |
| NB_PAGES | Integer | Pages fetched (pagination) |
| TOTAL_RESPONSE_SIZE | Long | Total bytes received |
| PROCESSING_TIME_MS | Long | Total processing time |
| CACHE_HIT | Boolean | True if response from cache |
| NB_ERRORS | Integer | Error count |

## Dependencies

- `duckdb_jdbc-1.0.0.jar`
- `httpclient-4.5.14.jar`
- `httpcore-4.4.16.jar`
- `gson-2.10.1.jar`
- `commons-io-2.11.0.jar`

(All auto-downloaded via Maven)

## Version

- **Version:** 1.0
- **Status:** PROD
- **Author:** ESDB
- **Compatibility:** Talend 8.0.1+

## License

Apache License 2.0
