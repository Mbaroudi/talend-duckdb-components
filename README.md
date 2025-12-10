# Talend DuckDB Components

[![License: Apache 2.0](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)
[![Talend Version](https://img.shields.io/badge/Talend-8.0.1-orange.svg)](https://www.talend.com)
[![DuckDB Version](https://img.shields.io/badge/DuckDB-1.0.0-yellow.svg)](https://duckdb.org)

A comprehensive set of custom Talend components for **DuckDB**, the fast in-process analytical database.

## Why DuckDB for Talend?

DuckDB is an embedded analytical database that excels at:
- **OLAP workloads** - Columnar storage optimized for analytics
- **In-process execution** - No server setup required
- **SQL compatibility** - PostgreSQL-compatible SQL dialect
- **Parquet/CSV native support** - Direct file querying
- **Memory efficiency** - Out-of-core processing for large datasets

## Components

### Core Components

| Component | Description |
|-----------|-------------|
| **tDuckDBConnection** | Establish connection to DuckDB (file or in-memory) |
| **tDuckDBClose** | Close DuckDB connection properly |
| **tDuckDBInput** | Read data from DuckDB tables/queries |
| **tDuckDBOutput** | Write data to DuckDB tables |
| **tDuckDBRow** | Execute SQL statements (DDL/DML) |

### Advanced Components

| Component | Description |
|-----------|-------------|
| **tDuckDBRestInput** | Query REST APIs and load directly into DuckDB |

### Kafka Integration (Beta)

> **Note**: Ces composants Kafka ne sont pas encore testés en production. Vos retours sont les bienvenus !

| Component | Description |
|-----------|-------------|
| **tDuckDBKafkaConnection** | Configure Kafka connection for DuckDB |
| **tDuckDBKafkaInput** | Stream data from Kafka topics to DuckDB |
| **tDuckDBKafkaOutput** | Publish DuckDB query results to Kafka |
| **tDuckDBKafkaList** | List available Kafka topics |

## Installation

### Method 1: Via Talend Preferences (Recommended)

1. Download/clone this repository to a local folder (e.g., `Downloaded` in your Exchange folder)
2. Open **Talend Studio**
3. Go to **Window > Preferences > Talend > Components**
4. In **User component folder**, add the path to your downloaded folder
5. Click **Apply and Close**
6. Restart Talend Studio
7. Components appear in the **Databases > DuckDB** palette category

### Method 2: Manual Installation

1. Download the component folders
2. Copy each component folder to your Talend Exchange folder:
   ```
   # Windows
   TOS_DI-[version]\plugins\org.talend.designer.components.exchange_[version]\Downloaded\
   
   # macOS/Linux
   TOS_DI-[version]/plugins/org.talend.designer.components.exchange_[version]/Downloaded/
   ```
3. Restart Talend Studio
4. Components appear in the **Databases > DuckDB** palette category

## Quick Start

### Basic Usage

```
[tDuckDBConnection] --> [tDuckDBInput] --> [tLogRow]
                                      |
                               [tDuckDBClose]
```

### Example: Query Parquet Files

```sql
SELECT * FROM read_parquet('/data/sales/*.parquet')
WHERE year = 2024
```

## Configuration

### tDuckDBConnection Parameters

| Parameter | Description | Default |
|-----------|-------------|---------|
| Database Path | Path to .duckdb file or `:memory:` | `:memory:` |
| Read Only | Open in read-only mode | false |
| Threads | Number of parallel threads | Auto |
| Memory Limit | Maximum memory usage | 80% RAM |

### tDuckDBInput Parameters

| Parameter | Description |
|-----------|-------------|
| Query | SQL SELECT statement |
| Use Existing Connection | Reuse tDuckDBConnection |
| Schema | Output schema mapping |

## Performance Tips

1. **Use Parquet for large files** - 10x faster than CSV
2. **Enable parallel execution** - DuckDB auto-parallelizes
3. **Use memory-mapped files** - For files larger than RAM
4. **Partition data** - Hive-style partitioning supported

## Requirements

- Talend Open Studio 8.0.1+ (or Talend Data Integration)
- Java 11+
- DuckDB JDBC 1.0.0 (included)

## Contributing

Contributions are welcome! Please:
1. Fork the repository
2. Create a feature branch
3. Submit a Pull Request

## License

Apache License 2.0 - See [LICENSE](LICENSE)

## Author

Created for the Talend community.

## Acknowledgments

- [DuckDB](https://duckdb.org) - The amazing analytical database
- [Talend Community](https://community.talend.com) - For inspiration and support

---

**Note**: These components are community-contributed and not officially supported by Talend or DuckDB Labs.
